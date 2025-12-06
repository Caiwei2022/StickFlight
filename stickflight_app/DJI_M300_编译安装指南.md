# DJI M300 遥控器 - StickFlight APK 编译安装指南

## 📋 前置要求

- **电脑系统**: Windows/macOS/Linux（需联网）
- **遥控器**: DJI M300 遥控器（Android 7.1.2）
- **软件**: Android Studio 或 命令行工具（Gradle）
- **网络**: 首次编译需要下载依赖（约200MB+）

---

## 🔨 方法一：使用 Android Studio（推荐新手）

### 1. 安装 Android Studio
- 下载地址: https://developer.android.com/studio
- 安装时确保勾选 Android SDK、Android SDK Platform、Android Virtual Device

### 2. 打开项目
```
1. 启动 Android Studio
2. File → Open
3. 选择文件夹: StickFlight/stickflight_app
4. 等待 Gradle 同步完成（首次需要10-20分钟下载依赖）
```

### 3. 配置 SDK
```
1. File → Settings → Appearance & Behavior → System Settings → Android SDK
2. 确保已安装:
   - Android SDK Platform 31
   - Android SDK Build-Tools 30.0.3
   - Android SDK Platform-Tools
```

### 4. 编译 APK
```
1. Build → Build Bundle(s)/APK(s) → Build APK(s)
2. 等待编译完成（首次较慢）
3. 点击提示中的 "locate" 找到APK文件
```

### 5. APK 输出位置
```
StickFlight/stickflight_app/app/build/outputs/apk/debug/app-debug.apk
```

---

## 🚀 方法二：使用命令行（推荐熟手）

### Windows系统
```cmd
cd StickFlight\stickflight_app
gradlew.bat clean assembleDebug
```

### macOS/Linux系统
```bash
cd StickFlight/stickflight_app
chmod +x gradlew
./gradlew clean assembleDebug
```

### 编译成功后
APK位置: `stickflight_app/app/build/outputs/apk/debug/app-debug.apk`

---

## 📱 安装到 DJI M300 遥控器的方法

### 方法 A: 通过微信/QQ传输（最简单）

1. **将APK传到手机**
   - 电脑上找到 `app-debug.apk`
   - 通过微信/QQ/邮件发送给自己

2. **在遥控器上接收**
   - 遥控器连接WiFi
   - 登录微信/QQ（可以用网页版）
   - 下载APK文件
   - 点击安装

### 方法 B: 通过U盘/SD卡（最可靠）

1. **复制APK到U盘**
   ```
   将 app-debug.apk 复制到U盘根目录
   ```

2. **在遥控器上安装**
   ```
   1. U盘插入遥控器
   2. 打开文件管理器
   3. 找到 app-debug.apk
   4. 点击安装
   ```

### 方法 C: 通过ADB无线调试（技术流）

1. **遥控器设置**
   ```
   设置 → 关于设备 → 连续点击版本号7次（开启开发者模式）
   设置 → 开发者选项 → 开启USB调试
   设置 → 开发者选项 → 开启ADB无线调试
   设置 → WiFi → 查看IP地址（如 192.168.1.100）
   ```

2. **电脑连接遥控器**
   ```bash
   # 电脑和遥控器连接同一WiFi
   adb connect 192.168.1.100:5555

   # 安装APK
   adb install app-debug.apk
   ```

### 方法 D: 通过局域网共享（办公室场景）

1. **电脑共享文件夹**
   - Windows: 右键APK所在文件夹 → 共享
   - 记住共享路径: `\\电脑名\共享文件夹`

2. **遥控器访问**
   - 使用ES文件浏览器或其他文件管理器
   - 访问局域网共享
   - 下载并安装APK

---

## ⚠️ 安装时的注意事项

### 1. 开启未知来源安装
```
设置 → 安全 → 允许安装未知来源的应用
或
设置 → 应用和通知 → 特殊应用权限 → 安装未知应用
```

### 2. 权限授予
首次运行会请求以下权限，**请全部允许**：
- ✅ 位置权限（GPS定位）
- ✅ 存储权限（保存航线）
- ✅ 相机权限（视频流）
- ✅ 录音权限（音频记录）
- ✅ 电话权限（设备识别）

### 3. DJI SDK 激活
- 首次运行需要联网激活DJI SDK
- 确保遥控器连接互联网
- 等待SDK注册成功提示

---

## 🐛 常见问题排查

### 问题1: 编译失败 - 无法下载依赖
```
解决方案：
1. 检查网络连接
2. 配置国内镜像（阿里云Maven镜像）
3. 使用VPN或代理
```

### 问题2: 安装失败 - 解析包错误
```
原因: APK损坏或不兼容
解决方案：
1. 重新下载/传输APK
2. 确认遥控器系统版本（需Android 4.4+）
3. 清理遥控器缓存后重试
```

### 问题3: 安装成功但无法打开
```
解决方案：
1. 检查DJI API Key是否有效
2. 查看日志: adb logcat | grep DJI
3. 确保飞机已连接遥控器
```

### 问题4: 连接飞机失败
```
解决方案：
1. 确保飞机和遥控器已对频
2. 重启应用
3. 检查USB连接模式（需要是MTP或调试模式）
```

---

## 📦 APK 签名（可选 - 用于发布）

如果需要正式发布版本：

```bash
# 生成签名密钥
keytool -genkey -v -keystore release.keystore -alias stickflight -keyalg RSA -keysize 2048 -validity 10000

# 签名APK
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore release.keystore app-release-unsigned.apk stickflight

# 优化APK
zipalign -v 4 app-release-unsigned.apk app-release.apk
```

---

## 📞 技术支持

- **DJI SDK文档**: https://developer.dji.com/mobile-sdk/documentation/
- **Android 开发文档**: https://developer.android.com/docs
- **问题反馈**: 查看项目的 GitHub Issues

---

## 📝 更新日志

**最近更新** (2025-12-06):
- ✅ 优化debug构建配置
- ✅ 禁用代码混淆和资源压缩，便于调试
- ✅ 确认DJI M300（Android 7.1.2）兼容性

---

**祝编译顺利！飞行安全！** 🚁
