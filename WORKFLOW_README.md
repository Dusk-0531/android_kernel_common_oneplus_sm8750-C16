# OnePlus SM8750 Kernel Build Workflow

这个项目包含了一个自动化的 GitHub Actions 工作流，用于为 OnePlus SM8750 (骁龙 8 Elite) 设备编译内核，并打包成 AnyKernel3 格式的刷机包。

## 特性

### ✅ 保留的特性
- **AIOS/ADIOS IO 调度器**: 提升 IO 读写性能的高级调度器
- **LZ4 1.10.0 & zstd 1.5.7 压缩**: 优化的压缩算法支持
- **网络功能增强**: 
  - IP_SET 支持 (用于高效的大规模 IP 地址管理)
  - NETFILTER 扩展 (支持 iptables 等高级网络功能)
  - IPv6 NAT 支持
  - BPF 流解析器
- **TCP 拥塞控制算法**: 
  - BBR (Bottleneck Bandwidth and RTT)
  - CUBIC (默认)
  - Vegas
  - Westwood+
  - HTCP
  - Brutal
- **O2 编译优化**: 提升内核性能

### ❌ 移除的特性
- **KernelSU**: 不包含任何 KernelSU 分支 (SukiSU, ReSukiSU, KernelSU Next, MKSU, 原版 KSU)
- **SUSFS**: 不包含 SUSFS 隐藏功能支持
- **KPM**: 不包含 KernelPatch Manager
- **LZ4KD**: 不包含 LZ4KD 补丁

## 使用方法

### 自动构建

工作流会在以下情况自动触发：
1. 推送代码到 `main` 或 `master` 分支时
2. 创建 Pull Request 时
3. 也可以手动触发 (在 GitHub Actions 页面点击 "Run workflow")

### 手动触发构建

1. 进入仓库的 "Actions" 标签页
2. 选择 "Build OnePlus SM8750 Kernel" 工作流
3. 点击 "Run workflow" 按钮
4. 选择分支后点击绿色的 "Run workflow" 按钮

### 下载构建产物

构建完成后：
1. 在 Actions 页面找到对应的工作流运行记录
2. 在 "Artifacts" 部分找到 "Kernel_AK3_Package"
3. 点击下载 ZIP 文件

如果是推送到 main/master 分支触发的构建，还会自动创建一个 Release，可以直接从 Releases 页面下载。

## 安装说明

### 方法 1: 使用 TWRP/自定义 Recovery
1. 下载 AnyKernel3 刷机包
2. 重启手机进入 Recovery 模式
3. 选择刷入下载的 ZIP 包
4. 刷入完成后重启设备

### 方法 2: 使用 HorizonKernelFlasher (需要 root 权限)
1. 在手机上安装 [HorizonKernelFlasher](https://github.com/libxzr/HorizonKernelFlasher/releases)
2. 在应用中选择下载的 AnyKernel3 刷机包
3. 刷入并重启

## ⚠️ 重要提示

**刷写内核有风险！** 在刷入新内核前，请务必：
1. 备份重要数据
2. 使用 [KernelFlasher](https://github.com/capntrips/KernelFlasher) 等工具备份 boot 分区
3. 确保电池电量充足 (建议 50% 以上)

如果刷入后无法开机：
1. 重启进入 Recovery
2. 恢复之前备份的 boot 分区
3. 或者重新刷入官方系统包

## 技术细节

### 构建环境
- 运行平台: Ubuntu Latest (GitHub Actions)
- 编译器: LLVM Clang 18 (r510928)
- 构建工具: Android Kernel Build Tools
- 优化级别: O2
- 使用 ccache 加速编译

### 内核版本
- 基础版本: Linux 6.6.89
- 适用设备: OnePlus SM8750 (骁龙 8 Elite)
- Android 版本: Android 15

### 工作流说明

工作流包含以下步骤：
1. **环境准备**: 安装必要的依赖包
2. **工具链下载**: 下载 Clang 18 和构建工具
3. **配置 ccache**: 设置编译缓存以加速后续构建
4. **内核配置**: 启用 AIOS 调度器、网络增强等特性
5. **编译内核**: 使用多线程并行编译
6. **打包**: 使用 AnyKernel3 打包成刷机包
7. **上传**: 上传编译产物和创建 Release

## 贡献

欢迎提交 Issue 和 Pull Request！

## 许可证

遵循原始 Linux 内核的 GPL-2.0 许可证。

## 参考

- 参考项目: [oppo_oplus_realme_sm8750](https://github.com/cctv18/oppo_oplus_realme_sm8750)
- 工具链: [oneplus_sm8650_toolchain](https://github.com/cctv18/oneplus_sm8650_toolchain)
- AnyKernel3: [AnyKernel3](https://github.com/cctv18/AnyKernel3)
