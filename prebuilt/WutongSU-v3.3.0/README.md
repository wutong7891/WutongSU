# WutongSU v3.3.0 预编译成品

本目录包含与仓库当前 WutongSU 专用包名和 RSA2048 管理器证书绑定的完整预编译成品。

## 文件

- `manager/WutongSU-v3.3.0-manager.apk`：WutongSU Root 管理器，包名 `com.ksuwei.wutong`；
- `image-patchers/WutongSU镜像工坊.exe`：内嵌 8 个 ARM64 KMI 的 Windows 镜像修补工具；
- `image-patchers/WutongSU-image-patcher-arm64.apk`：内嵌 8 个 ARM64 KMI 的 Android 镜像修补工具；
- `kmi/`：8 个可以单独使用的 ARM64 `kernelsu.ko`；
- `SHA256SUMS.txt`：本目录主要成品的 SHA-256 校验值。

## KMI 范围

- Android 12 / Linux 5.10
- Android 13 / Linux 5.10、5.15
- Android 14 / Linux 5.15、6.1
- Android 15 / Linux 6.6
- Android 16 / Linux 6.12
- Android 17 / Linux 6.18

选择错误的 Android/KMI 组合可能导致设备无法启动。刷写前必须保存原厂镜像并准备可用的救砖方式。

本目录不包含签名私钥、JKS 密码、GitHub Secrets、设备原厂镜像或修补后的设备镜像。
