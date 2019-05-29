---
title: Node.js之mysql增删改查
date: 2019-03-27 15:37:45
tags: "Node.js"
---

## 1.安装库
```
npm install mysql

```

## 2.编写db.js(用作公共模块)

```
//连接MySQL数据库
var mysql = require("mysql");

var pool = mysql.createPool({
    host:"127.0.0.1",
    user:"root",
    password:"123456",
    database:"wordpress",
	port:3306
});


function query(sql,callback){
    pool.getConnection(function(err,connection){
        connection.query(sql, function (err,rows) {
            callback(err,rows);
            connection.release();
        });
    });
}

exports.query = query;

```
<!--more-->
## 3.编写user.js(与数据库交互并对外开放接口)
顺便说下req.params、req.body、req.query的应用

req.body通常用于解析post请求数据

req.query通常用于解析get请求数据,如http://wwww.youcongtech.com/blog/user?username=youcong

req.params通常用于解析rest请求方式，如http://www.youcongtech.com/blog/user/youcong



```
var express = require('express');
var router = express.Router();
var URL = require('url');
var db = require('./db');
var jsonData = require('./jsonData');
var bodyParser = require('body-parser');

//定义一个post输出接口
router.post('/post', function(req, res) {

    var obj = {
        a: 1,
        b: 2
    };

    res.json(obj); //以json格式输出
});



//增删改查

//查询所有信息
router.get('/queryAll', function(req, res, next) {
    
  var userLogin = req.userLogin
  
  var querySql = 'SELECT * FROM wp_users';
  
  db.query(querySql,function (err, rows) {
	  
	  
        if(err){
          console.log('[SELECT ERROR] - ',err.message);
          return;
        }

        //把搜索值输出
       res.send(rows);
	   console.log('The solution is: ', rows[0].ID);
	   
    });
	

});

//添加用户信息
router.post("/add",function(req,res){
	var params = URL.parse(req.url, true).query;
	  
	var addSql = "INSERT INTO wp_users (user_login) VALUES('"+params.userLogin+"')";
	
	
    var addSqlParams = [params.userLogin];
	 
	 db.query(addSql,function(err,rows){
        if(err){
            res.send("添加失败 " + err);
        }else {
            res.send(jsonData.addInfo);
        }
    });
	  
});

//更新用户信息
router.put("/update",function(req,res,next){
	 
	 var params = URL.parse(req.url, true).query;
	 
	 var displayName = params.displayName;
	 
	 var id = params.id;

	 var sql = "update wp_users set display_name = '"+ displayName +"' where ID = " + id;
	 
	  db.query(sql,function(err,rows){
        if(err){
            res.send("修改失败 " + err);
        }else {
            res.send(jsonData.updateInfo);
        }
    });
	
});

//根据ID获取用户信息
router.get("/getById/:id",function(req,res){
	
	var id = req.params.id;
	
	var sql = 'select * from wp_users where ID = '+id;
	
	  
   db.query(sql,function (err, result) {
	  
        if(err){
          console.log('[SELECT ERROR] - ',err.message);
          return;
        }   
		
   console.log(result);
   res.send(result);
	   
    });
	
});


//删除用户信息
router.delete('/delete/:id', function(req, res, next) {
  
  
 var id = req.params.id;
  
  var delSql = 'delete from wp_users where id = '+id;
  
  db.query(delSql,function (err, result) {
	  
        if(err){
          console.log('[INSERT ERROR] - ',err.message);
          return;
        }   
	    res.send(jsonData.deleteInfo);
	   
    });
});






module.exports = router;




```


## 4.不要忘记在app.js配置路由

```
var usersRouter = require('./routes/users');

app.use('/users', usersRouter);

```

参考资料如下:
node.js取参四种方法req.body,req.params,req.param,req.body:https://www.cnblogs.com/jkingdom/p/8065202.html