---
title: Openresty之Lua实战系列
date: 2022-07-21 16:35:28
tags: ["Linux","Lua","中间件"]
---

本文分享，旨在使读者朋友通过实践了解如何在Openresty中使用脚本语言Lua自定义或定制化自己的文件服务、缓存服务、DB服务等。
<!--more-->

## 一、构建文件存储服务

### 1.upload.lua
```
local upload = require "resty.upload"
local cjson = require "cjson"

local chunk_size = 4096
local form, err = upload:new(chunk_size)
if not form then
    ngx.log(ngx.ERR, "failed to new upload: ", err)
    ngx.exit(ngx.HTTP_INTERNAL_SERVER_ERROR)
end

form:set_timeout(1000)

-- 字符串 split 分割
string.split = function(s, p)
    local rt= {}
    string.gsub(s, '[^'..p..']+', function(w) table.insert(rt, w) end )
    return rt
end

-- 支持字符串前后 trim
string.trim = function(s)
    return (s:gsub("^%s*(.-)%s*$", "%1"))
end

-- 文件保存的根路径
local saveRootPath = "/usr/local/openresty/nginx/upload/"

-- 保存的文件对象
local fileToSave

--文件是否成功保存
local ret_save = false

while true do
    local typ, res, err = form:read()
    if not typ then
        ngx.say("failed to read: ", err)
        return
    end

    if typ == "header" then
        -- 开始读取 http header
        -- 解析出本次上传的文件名
        local key = res[1]
        local value = res[2]
        if key == "Content-Disposition" then
            -- 解析出本次上传的文件名
            -- form-data; name="testFileName"; filename="testfile.txt"
            local kvlist = string.split(value, ';')
            for _, kv in ipairs(kvlist) do
                local seg = string.trim(kv)
                if seg:find("filename") then
                    local kvfile = string.split(seg, "=")
                    local filename = string.sub(kvfile[2], 2, -2)
                    if filename then
                        fileToSave = io.open(saveRootPath .. filename, "w+")
                        if not fileToSave then
                            ngx.say("failed to open file ", filename)
                            return
                        end
                        break
                    end
                end
            end
        end
    elseif typ == "body" then
        -- 开始读取 http body
        if fileToSave then
            fileToSave:write(res)
        end
    elseif typ == "part_end" then
        -- 文件写结束，关闭文件
        if fileToSave then
            fileToSave:close()
            fileToSave = nil
        end

        ret_save = true
    elseif typ == "eof" then
        -- 文件读取结束
        break
    else
        ngx.log(ngx.INFO, "do other things")
    end
end

if ret_save then
    ngx.say("save file ok")
end


```

### 2.配置
```
location /upload_file {
    client_max_body_size 10m;
	content_by_lua_file /usr/local/openresty/nginx/upload.lua;
}

location /download {
	autoindex on;
	autoindex_localtime on;
	alias /usr/local/openresty/nginx/upload;

}


```


## 二、缓存控制

### 1.cache.lua
```
local redis = require "resty.redis"
local red = redis:new()
local resty_lock = require "resty.lock"
local ngx_cache = ngx.shared.ngx_cache

function set_to_cache(key, value, exptime)
        if not exptime then
                exptime = 0
        end
        local succ, err, forcible = ngx_cache:set(key, value, exptime)
        return succ
end

function get_from_cache(key)
        local ngx_cache = ngx.shared.ngx_cache;
        local value = ngx_cache:get(key)
        if not value then       -- cache miss
                local lock = resty_lock:new("cache_lock")
                local elapsed, err = lock:lock(key)
                if not elapsed then
                        return fail("failed to acquire the lock: ", err)
                end

                value = get_from_redis(key)
                if not value then
                        local ok, err = lock:unlock()
                        if not ok then
                                return fail("failed to unlock: ", err)
                        end
                        ngx.say("no value found")
                        return
                end

                local ok, err = ngx_cache:set(key, value, 1)
                if not ok then
                        local ok, err = lock:unlock()
                        if not ok then
                                return fail("failed to unlock: ", err)
                        end
                        return faile("failed to update ngx_cache: ", err)
                end

                local ok, err = lock:unlock()
                if not ok then
                        return faile("failed to unlock: ", err)
                end

                return value
        end

        ngx.say("get from cache.")
        return value
end

function get_from_redis(key)
        red:set_timeout(1000)

        local ok, err = red:connect("127.0.0.1", 6379)
        if not ok then
                ngx.say("failed to connect: ", err)
                return
        end

        local res, err = red:get(key)
        if not res then
                ngx.say("failed to get doy: ", err)
                return ngx.null
        end

        ngx.say("get from redis.")
        return res
end



function set_to_redis(key, value)
        red:set_timeout(1000)
        local ok, err = red:connect("127.0.0.1", 6379)
        if not ok then
                ngx.say("failed to connect: ", err)
                return
        end

        local ok, err = red:set(key, value)
        if not ok then
                ngx.say("failed to set to redis: ", err)
                return
        end
        return ok
end

set_to_redis('key', "Lua And Openrestry")
local rs = get_from_cache('key')
ngx.say(rs)

```

### 2.配置
```
location /cache {
	content_by_lua_file /usr/local/openresty/nginx/cache.lua;
}


```

## 三、数据库操作

### 1.mysql.lua
```
local request_method = ngx.var.request_method
local cjson = require("cjson")
local mysql = require("resty.mysql")
local quote = ngx.quote_sql_str
local db,err = mysql:new()
local function close_db(db)
    if not db then
        return
    end
    db:close()
end
if not db
then
    nax.say("new mysql error",err)
end

local args = nil
local username = nil
local pwd = nil
if "GET" == request_method
then
    args = ngx.req.get_uri_args()
elseif "POST" == ngx.request_method
then
    ngx.req.read_body()
    args = ngx.req.get_post_args()
end
username = tostring(args["username"])
pwd = tostring(args["pwd"])
if username == nil then
    ngx.say("username not nil")
    return
elseif username == '' then
    ngx.say("username not nil")
    return
elseif pwd == nil then
    ngx.say("password not nil")
    return;
elseif pwd == '' then
    ngx.say("password not nil")
    return;
end

db:set_timeout(1000)

local props = {
    host = "xxxx.com",
    port = 3306,
    database = "xxxx",
    user = "xxxx",
    password = "xxxx"
}

local res, err, errno, sqlstate = db:connect(props)
if not res then
   ngx.say("connect to mysql error : ", err, " , errno : ", errno, " , sqlstate : ", sqlstate)
   return close_db(db)
end

local select_name = "select username from users where username="..quote(username)
res, err, errno, sqlstate = db:query(select_name)
if not res then
   ngx.exec("/view/404.html")
   return close_db(db)
end
local result = {}
result.success = true;
result.info = "login success"
ngx.say(cjson.encode(result))

```

### 2.配置
```
location /mysql {
   access_by_lua_file /usr/local/openresty/nginx/mysql.lua;
}


```