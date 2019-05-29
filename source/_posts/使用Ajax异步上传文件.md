---
title: 使用Ajax异步上传文件
date: 2019-05-14 10:37:00
tags: [javascript,Java]
---

使用Ajax上传文件的应用场景颇多，比如上传用户头像、博客文章中插入图片、对认证用户相关身份进行校验等等很多很多。
<!--more-->
下面贴相关代码示例:
html代码片段:
```
                         <form class="layui-form" action="#" id="uploadForm">
							<div class="layui-form-item">
								<label class="layui-form-label">名称</label>
								<div class="layui-input-block">
									<input type="text" id="config_name" placeholder="请输入配置名称" autocomplete="off"
										class="layui-input">
								</div>
							</div>


							<div class="layui-form-item layui-form-text">
								<label class="layui-form-label">描述</label>
								<div class="layui-input-block">
									<textarea id="config_desc" placeholder="请输入配置描述" class="layui-textarea"></textarea>
								</div>
							</div>

							<div class="layui-form-item">
								<label class="layui-form-label">文件</label>
								<div class="layui-input-block">
									<input type="file" name="file">
									<p class="help-block">请选择配置文件</p>
								</div>
							</div>




							<div class="layui-form-item">
								<div class="layui-input-block">
									<button class="layui-btn" id="save_config_file">立即提交</button>
									<button type="reset" class="layui-btn layui-btn-primary">重置</button>
								</div>
							</div>

						</form>

```

js代码片段:
```
//上传配置文件
$("#save_config_file").click(function () {

	var name = $("#config_name").val();
	var desc = $("#config_desc").val();
	var userId = $("#userId").val();

	var formData = new FormData($("#uploadForm")[0]);

	formData.append("name",name);
	formData.append("desc",desc);
	formData.append("userId",userId);

		$.ajax({
			url: 'http://localhost:8090/bfi-web/api/ide/settings/uploadFiles',
			type: 'POST',
			data: formData,
			async: false,
			cache: false,
			contentType: false,
			processData: false,
			success: function (returndata) {

				layui.use('layer', function () {
					var layer = layui.layer;

					layer.msg(returndata.returnMsg, {
						icon: 1
					});
				});

				setTimeout(() => {

					closeLayui();

				}, 300);

			},
			error: function (returndata) {
				console.log("====================Error==========================");
			}
		});





});

```

Java代码片段(这里是SpringMVC+腾讯云对象存储,可将其更换为其它对象存储，如七牛云、ftp或者是其它对象存储):
```
    /**
     * 上传文件
     * @param request
     * @param file
     * @return
     */
	@PostMapping(value="/uploadFiles",produces="application/json;charset=utf-8")
	public JSONObject upModify(HttpServletRequest request, MultipartFile file) {
		
		JSONObject json = new JSONObject();

		try {
			
			COSClientUtil cosClientUtil = new COSClientUtil(); 

			if(!file.isEmpty()) {
				
				String name = cosClientUtil.uploadFile2Cos(file); 
                String desc = request.getParameter("desc");
                String names = request.getParameter("name");
                String userId = request.getParameter("userId");
                
                logger.info("desc:"+desc);
                logger.info("names:"+names);
                logger.info("userId:"+userId);
                
				//图片名称
				logger.info("name = " + name);
				
				//上传到腾讯云
				String imgUrl = cosClientUtil.getImgUrl(name); 

				logger.info("imgUrl = " + imgUrl);
				
				//数据库保存图片地址
				String dbImgUrl = imgUrl.substring(0,imgUrl.indexOf("?"));
				logger.info("dbImgUrl = " + dbImgUrl);
			
				IdeSettings ide = new IdeSettings();
				ide.setName(names);
				ide.setContent(dbImgUrl);
				ide.setUserId(userId);
				ide.setUpdateTime(DateUtil.date().toString());
				ide.setUploadTime(DateUtil.date().toString());
				ide.setDescription(desc);
				
				boolean isAddConfig = ideSettingsService.insert(ide);
				
				logger.info(isAddConfig);
				
				if(isAddConfig) {
					json.put(CommonEnum.RETURN_CODE, "000000");
					json.put(CommonEnum.RETURN_MSG, "上传成功");
				}else {
					json.put(CommonEnum.RETURN_CODE, "222222");
					json.put(CommonEnum.RETURN_MSG, "上传失败");
				}
				
		
				
			}else {
				json.put(CommonEnum.RETURN_CODE, "111111");
				json.put(CommonEnum.RETURN_MSG, "参数异常");
			}
			
		} catch (Exception e) {
			e.printStackTrace();

			json.put(CommonEnum.RETURN_CODE, "333333");
			json.put(CommonEnum.RETURN_MSG, "特殊异常");
	
		}
        
        return json;
	}

```