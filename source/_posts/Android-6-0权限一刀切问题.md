title: Android 6.0权限一刀切问题
author: TryCatch
tags:
  - android
categories:
  - android
date: 2017-05-17 14:43:00
---
#### 今天使用m4的时候突然发现程序抛出异常了，原因是因为升级了小米的最新系统MIUI7.3稳定版本，仔细一看发现是Android6.0.1的操作系统，瞬间想到Android6.0的Requesting Permissions at Run Time特性。于是找到了google的官方文档和demo进行脑补

官方文档：
<https://developer.android.com/training/permissions/requesting.html/>

官方demo：

<https://github.com/googlesamples/android-RuntimePermissions/blob/master/Application/src/main/java/com/example/android/system/runtimepermissions/MainActivity.java/>
#### 首先必须在AndroidManifest.xml文件里面声明需要用到的权限，下面是一个权限例子
```
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
```
#### 至于代码实现如下：
```
int REQUEST_READ_PHONE=101;
 public void initPermission(){
        if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_PHONE_STATE) != PackageManager.PERMISSION_GRANTED) {
            // Should we show an explanation?
            if (ActivityCompat.shouldShowRequestPermissionRationale(this, Manifest.permission.READ_PHONE_STATE)) {

                // Show an expanation to the user *asynchronously* -- don't block
                // this thread waiting for the user's response! After the user
                // sees the explanation, try again to request the permission.

            } else {

                // No explanation needed, we can request the permission.

                ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.READ_PHONE_STATE},
                        REQUEST_READ_PHONE);

                // MY_PERMISSIONS_REQUEST_READ_CONTACTS is an
                // app-defined int constant. The callback method gets the
                // result of the request.
            }
        }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
        if (requestCode == REQUEST_READ_PHONE){
            // If request is cancelled, the result arrays are empty.
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                //PERMISSION GRANTED

            } else {
                //PERMISSION DENIED permission denied
                Toast.makeText(this, "您拒绝了权限请求，应用程序无法正常工作，请手动设置权限为允许", Toast.LENGTH_LONG).show();
            }
        } else {
            super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        }
    }
```