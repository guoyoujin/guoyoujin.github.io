title: ng-file-upload使用
author: TryCatch
tags:
  - angularjs
categories:
  - js
date: 2017-05-17 14:41:00
---
### <a href="https://github.com/danialfarid/ng-file-upload">github地址</a>

### 1.引入js文件
```
//= require adminAngular/lib/ng-file-upload-shim.min
//= require adminAngular/lib/ng-file-upload.min
```

### 2.注入
```
var controllers = angular.module('controllers', ['services', 'directives','ngFileUpload','xeditable']);

```

### 3.注入controller
```
controllers.controller('ProductGroupManagementCtrl',['$scope', '$uibModal', 'mallOrderHttp', 'get_params','$uibModalInstance', '$state','Upload',function($scope,$uibModal,mallOrderHttp,get_params,$uibModalInstance,$state,Upload){
    $scope.upload = function (files,product_group_id) {
        if (files && files.length) {
            for (var i = 0; i < files.length; i++) {
                var file = files[i];
                Upload.upload({
                    url: '/api/admin/v1/mall_orders/update_product_group',
                    data: { 'id': product_group_id },
                    file: file
                }).progress(function (evt) {
                    //进度条
                    var progressPercentage = parseInt(100.0 * evt.loaded / evt.total);
                    console.log('progess:' + progressPercentage + '%' + evt.config.file.name);
                }).success(function (data, status, headers, config) {
                    //上传成功
                    console.log('file ' + config.file.name + 'uploaded. Response: ' + data);
                    $scope.uploadImg = data;
                    $scope.select_list_change();
                }).error(function (data, status, headers, config) {
                    //上传失败
                    console.log('error status: ' + status);
                });
            }
        }
    };
    //查看大图
    $scope.show_big_image = function(objValue){
        $(this).ekkoLightbox({
            remote:objValue.image.url
        });
    }
}]);
```

### 4.html文件
```
<td>
<a href="javascript:void();" ng-click="show_big_image(product_group)">
<img style="display: inline-block;width:30px;height:30px"  ng-src="{{product_group.image.url}}" />
</a>
<span class="btn btn-success fileinput-button btn-xs">
<i class="glyphicon glyphicon-plus"></i>
<span>文件上传</span>
<input ngf-select ngf-change="upload($files,product_group.id)" ngf-multiple="true" type="file"/>
</span>
</td>
```