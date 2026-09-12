# WutongSU

WutongSU 是基于 KernelSU `v3.3.0` 整理的个人分支，管理器名称为 **WutongSU**，Android 包名为 `com.ksuwei.wutong`。

本仓库包含：

- WutongSU Android Root 管理器源码；
- `ksud`、`ksuinit` 与内核模块源码；
- Android 12～17、Linux 5.10～6.18 的 ARM64 KMI 构建工作流；
- 内嵌 KMI 的 Windows 镜像修补工具源码；
- 内嵌 KMI 的 Android 镜像修补器源码。

## 已验证版本

当前专用签名绑定参数：

- 管理器包名：`com.ksuwei.wutong`
- 管理器证书 DER 长度：`0x336`（822 字节）
- 管理器证书 SHA-256：`f5720eb1effc96e619165ed6a4503946f69c0f7a8680fd3619d93cc1ce0bd91d`

该 RSA2048 证书长度符合内核中管理器证书校验器的限制。私钥和 JKS 文件不包含在本仓库中，也不应上传到公开仓库。

## KMI 支持范围

| Android 版本 | Linux KMI |
| --- | --- |
| Android 12 | 5.10 |
| Android 13 | 5.10、5.15 |
| Android 14 | 5.15、6.1 |
| Android 15 | 6.6 |
| Android 16 | 6.12 |
| Android 17 | 6.18 |

注意：这里的“5.1”实际应为 Linux **5.10**。修补镜像时必须同时匹配 Android 版本和 KMI，不能只看 Linux 主版本。

## GitHub Actions 构建

仓库地址：<https://github.com/yzhl3771/WutongSU>

### 构建管理器与 Android 16 / 6.12 KMI

打开 GitHub 仓库的 **Actions** 页面，运行：

`Build WutongSU manager and Android 16 6.12 KMI`

对应文件：

`.github/workflows/build-wutong-android16-6.12.yml`

### 构建全部 KMI 和两个镜像修补器

在 Actions 页面运行：

`Build WutongSU KMI and image patchers`

对应文件：

`.github/workflows/build-wutong-image-patchers.yml`

该工作流会完成：

1. Rust check、Clippy 和格式检查；
2. 构建 8 个证书绑定的 ARM64 `kernelsu.ko`；
3. 构建带 8 个内置 KMI 的 `WutongSU镜像工坊.exe`；
4. 构建包名为 `com.ksuwei.wutong.patcher` 的 Android 修补器；
5. 验证 Windows 内置 KMI 列表、APK 签名、包名、原生引擎及 KMI 数量。

## GitHub Actions 签名 Secrets

构建签名 APK 前，需要在仓库的 **Settings → Secrets and variables → Actions** 中配置：

- `KEYSTORE`：JKS 文件的 Base64 内容；
- `KEYSTORE_PASSWORD`：JKS 密码；
- `KEY_ALIAS`：密钥别名；
- `KEY_PASSWORD`：密钥密码。

不要把 JKS、私钥、密码或 Secrets 明文提交到仓库。遗失签名密钥后，新 APK 无法覆盖安装旧版本。

## Windows 镜像修补器

Windows 成品名称：

`WutongSU镜像工坊.exe`

双击运行后：

1. 输入 KMI 序号；
2. 通过文件选择窗口选择原始 `boot.img` 或 `init_boot.img`；
3. 工具在原镜像同目录创建 `年.月.日.时.分` 文件夹；
4. 输出 `WutongSU_patched_对应KMI.img` 和 `SHA256.txt`。

工具只在本地生成镜像，不会连接设备，也不会自动调用 ADB、Fastboot 或刷写分区。

## Android 镜像修补器

Android 工具源码位于：

`android-image-patcher/`

安装包名：`com.ksuwei.wutong.patcher`

使用时选择 KMI 和原始镜像，输出保存在：

`/storage/emulated/0/Download/WutongSU/`

## 模块目录

WutongSU 沿用 KernelSU 模块安装逻辑：

- 已启用模块：`/data/adb/modules/`
- 安装或更新暂存：`/data/adb/modules_update/`

模块安装脚本不应把最终启用目录改为管理器应用的私有目录。

## 主要目录

- `manager/`：Android Root 管理器；
- `kernel/`：KernelSU 内核模块；
- `userspace/ksud/`：用户空间守护进程及 Windows 修补器；
- `userspace/ksuinit/`：启动阶段组件；
- `android-image-patcher/`：独立 Android 镜像修补器；
- `.github/workflows/`：GitHub Actions 构建流程。

## 安全提醒

- 刷写前备份当前设备的原厂镜像；
- 确认镜像分区类型、Android 版本和 KMI 完全匹配；
- 保留可用的 Bootloader、Fastboot 或其他救砖方式；
- 不要在未经确认的情况下直接刷写生成镜像；
- 本仓库不包含私人签名材料，也不包含用户设备的原厂镜像。

## 当前验证记录

全套 KMI、Windows 工具与 Android 工具已由以下工作流成功构建：

<https://github.com/yzhl3771/WutongSU/actions/runs/34699929923>
