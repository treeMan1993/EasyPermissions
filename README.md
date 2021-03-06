# EasyPermissions
**声明**：本库参考了rxpermissions（[https://github.com/tbruyelle/RxPermissions](https://github.com/tbruyelle/RxPermissions "go to rxpermissions"))<br>
本库旨在简化安卓6.0及以上版本运行时权限申请流程，特点如下：
* 链式操作
* 相较于rxpermissions，无需依赖rxjava
* 若请求的权限未在manifest中注册，将抛出明确的异常
* 请求间互不影响，即使每次请求的是相同权限

## Gradle依赖

1.在项目根build.gradle中添加

```gradle
allprojects {
	repositories {
		maven { url 'https://jitpack.io' }
	}
}
```

2.在需要依赖本库的build.gradle中添加

```gradle
dependencies {
    implementation 'com.github.Ficat:EasyPermissions:v1.0.1'
}
```
## 使用
1.创建easypermissions对象

```java
EasyPermissions easyPermissions = new EasyPermissions(activity);
```

2.请求权限

```java
//request方式，请求的所有权限被用户授权后返回true，否则返回false  
easyPermissions
       .request(Manifest.permission.CAMERA,Manifest.permission.CALL_PHONE)
       .subscribe(new RequestSubscriber<Boolean>() {
           @Override
           public void onPermissionsRequestResult(Boolean granted) {
               if (granted) {
                   //摄像头、拨打电话都被用户授权
               } else {
                   //摄像头、拨打电话任意一项被拒绝或两者都被拒绝
               }
           }
       });

//requestEach方式
easyPermissions
       .requestEach(Manifest.permission.CAMERA,Manifest.permission.CALL_PHONE)
       .subscribe(new RequestSubscriber<Permission>() {
           @Override
           public void onPermissionsRequestResult(Permission permission) {
               String name = permission.name;
               if (permission.granted) {
                   //name权限被授予
               } else {
                   if (permission.shouldShowRequestPermissionRationale) {
                       //name权限被拒绝但用户未勾选不再询问框
                   } else {
                       //name权限被拒绝且用户勾选了不再询问框
                       //此时就不能再次请求name权限了，而需要user前往设置界面授权
                   }
               }
           }
       });
```
## License

```
Copyright 2018 Ficat

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```


