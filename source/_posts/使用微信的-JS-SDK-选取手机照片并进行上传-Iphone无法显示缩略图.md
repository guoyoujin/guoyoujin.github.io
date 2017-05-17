title: '使用微信的 JS SDK 选取手机照片并进行上传,Iphone无法显示缩略图'
author: TryCatch
tags:
  - js
  - weixin
categories:
  - js
date: 2017-05-17 14:05:00
---
#### 前言
最近从三月初开始就发现有用户反应微信浏览器选择图片显示不了预览，仔细查找发现跟微信最近升级浏览器内核有关，发现需要升级weixin js sdk了，并且需要修改一些方法，以及对一些老版本的兼容。


#### weixin选取图片代码(老版本jweixin-1.2.0.js之前的版本)
```
wx.chooseImage({
  count: 1, 
  sizeType: ['original', 'compressed'],
  sourceType: ['album', 'camera'],
  success: function (res) {
    var localId = res.localIds[0];
    $('img.avatar-temp').attr('src', localId);
  }
)};
```
使用如上代码发现图片在iphone上无法显示，Android上可以无差别显示，那肯定是浏览器内核的问题了，解决办法就是升级weixin js sdk喽，直接升级微信js sdk
```
<script src="https://res.wx.qq.com/open/js/jweixin-1.2.0.js"></script>
```
#### 接下来就是修改选显示图片的步骤了
```
wx.chooseImage({
  count: 1, 
  sizeType: ['original', 'compressed'],
  sourceType: ['album', 'camera'],
  success: function (res) {
    var localId = res.localIds[0];
    if(window.__wxjs_is_wkwebview){
		wx.getLocalImgData({
    		localId: auth_image.localId,
    		success: function (res) {
        		var localData = res.localData;
                localData = localData.replace('jgp', 'jpeg');
                $('img.avatar-temp').attr('src', localData);
    		},
            fail:function(res){
                alert(res.errMsg);
            }
		});
    }else{
    	$('img.avatar-temp').attr('src', localId);
    }
  }
)};
```
使用getLocalImgData方法即可在wkwebview浏览器内核也可以正常显示图片了。注意记得一定要判断浏览器内核，不然总有一个出问题的，并且在else里面做你该做的事情，千万别忘了！！！！


#### weixin上传图片
我选择直接上传到微信服务器上面，然后在利用反回的图片地址，让自己服务器去异步下载图片
```
wx.uploadImage({
    localId: localId,
    isShowProgressTips: 1,
    success: function (res) {
        auth_image.serverId = res.serverId;
    },
    fail: function (res) {
        alert(JSON.stringify(res));
    }
});
```