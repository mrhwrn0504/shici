# 构建与发布

## 本仓库与源码

此仓库是安装包发布和使用指南入口，不是上游仓库的镜像。每个Release提供对应版本源码ZIP，包含app、OCR SDK源文件、模型与词库来源、Gradle配置及许可证；不包含本机SDK路径、构建缓存、用户学习资料或签名密钥。

## 构建条件

Android SDK36、Gradle9.4.1、兼容AGP9.2.1的JDK（建议21或以上），Java源码目标17。在本机配置SDK路径后执行：

```
gradle -PtargetAbi=arm64-v8a :app:assembleRelease
```

源码中的调试签名配置仅用于开发。公开分发者应妥善保存自己的签名证书，后续更新保持一致。仓库不发布私钥，源码构建不会自动得到原发布者证书。

## 翻译组件的R8保留规则

ML Kit通过manifest元数据反射创建Firebase组件注册器。旧firebase-components消费者规则可能只保留类名而删除公开无参构造方法。0.10.1显式保留这些构造方法与接口；不要为了瘦身移除此规则。JNI与模型运行时的既有保留规则同样需要保留。

## 更新协议

正式Release中同时上传APK、update.json、SHA256.txt与源码包。版本号可变，但versionCode必须递增。当前应用检查：

https://github.com/mrhwrn0504/shici-updates/releases/latest/download/update.json

JSON字段：versionCode、versionName、packageName、minSdk、downloadUrl、sha256、notes。downloadUrl指向本仓库Release的对应APK，SHA-256与实际文件一致。应用不静默安装，点击下载交给浏览器与Android安装器。

发行包须完成编译、签名、版本与资产核对；修改构建规则时还需核对反射构造方法是否保留。实机验证范围应如实写在发布说明中，不用“编译成功”替代识别效果证据。
