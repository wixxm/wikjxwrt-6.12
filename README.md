

<h1 align="center">WikjxWrt-24.10 - <a href="https://kernel.org" target="_blank" >Linux 6.12 LTS</a></h1>

<p align="center">
  <b>基于原生 <a href="https://github.com/openwrt/openwrt" target="_blank" >OpenWrt</a> 更改与优化的固件，提供高效、稳定的使用体验！</b>
</p>

<p align="center">
  <b>感谢 <a href="https://github.com/sbwml/r4s_build_script" target="_blank" >sbwml</a> 大佬开源仓库 </b>
</p>

---------------

## 基于 Linux 6.12 LTS 固件下载:


#### X86_64: https://op.055553.xyz/


## 登录信息
```
地址：192.168.88.1（修改代码：vi /etc/config/network）
用户：root
密码：空
```
---------------


---------------

## 本地编译环境安装（根据 debian 11 / ubuntu 22）
```shell
sudo apt-get update
sudo apt-get install -y build-essential flex bison g++ gawk gcc-multilib g++-multilib gettext git libfuse-dev libncurses5-dev libssl-dev python3 python3-pip python3-ply python3-distutils python3-pyelftools rsync unzip zlib1g-dev file wget subversion patch upx-ucl autoconf automake curl asciidoc binutils bzip2 lib32gcc-s1 libc6-dev-i386 uglifyjs msmtp texinfo libreadline-dev libglib2.0-dev xmlto libelf-dev libtool autopoint antlr3 gperf ccache swig coreutils haveged scons libpython3-dev jq
```

---------------

### 启用 [Clang/LLVM](https://docs.kernel.org/kbuild/llvm.html) 构建内核
##### 脚本支持使用 Clang/LLVM 构建内核，NanoPi & X86_64 设备将同时启用 LLVM LTO 链接时优化，这会增加编译的时间，但会获得更优的性能
##### 只需在构建固件前执行以下命令即可启用 Clang/LLVM 构建内核与内核模块

```
export KERNEL_CLANG_LTO=y
```

### 启用 [GCC13](https://gcc.gnu.org/gcc-13/)/[GCC14](https://gcc.gnu.org/gcc-14/)/[GCC15](https://gcc.gnu.org/gcc-15/) 工具链编译
##### 只需在构建固件前执行以下命令即可启用 GCC13/GCC14/GCC15 交叉工具链

```
# GCC13
export USE_GCC13=y
```

```
# GCC14
export USE_GCC14=y
```

```
# GCC15
export USE_GCC15=y
```

### 启用 [LTO](https://gcc.gnu.org/onlinedocs/gccint/LTO-Overview.html) 优化
##### 只需在构建固件前执行以下命令即可启用编译器 LTO 优化

```
export ENABLE_LTO=y
```

### 启用 [MOLD](https://github.com/rui314/mold) 现代链接器（需要启用 `USE_GCC13=y` 或 `USE_GCC14=y` 或 `USE_GCC15=y`）
##### 只需在构建固件前执行以下命令即可启用 MOLD 链接，如果使用它建议同时启用 LTO 优化

```
export ENABLE_MOLD=y
```

### 启用 [eBPF](https://docs.kernel.org/bpf/) 支持
##### 只需在构建固件前执行以下命令即可启用 eBPF 支持

```
export ENABLE_BPF=y
```

### 启用 [LRNG](https://github.com/smuellerDD/lrng)
##### 只需在构建固件前执行以下命令即可启用 LRNG 内核随机数支持

```
export ENABLE_LRNG=y
```

### 启用 [Glibc](https://www.gnu.org/software/libc/) 库构建 （实验性）
##### 启用 glibc 库进行构建时，构建的固件将会同时兼容 musl/glibc 的预构建二进制程序，但缺失 `opkg install` 安装源支持
##### 只需在构建固件前执行以下命令即可启用 glibc 构建

```
export ENABLE_GLIBC=y
```

### 启用本地 Kernel Modules 安装源 （For developers）
##### 启用该标志时，将会拷贝全部 target packages 到 rootfs 并替换 openwrt_core 源为本地方式，以供离线 `opkg install kmod-xxx` 安装操作
##### 这会增加固件文件大小（大约 70MB），对项目内核版本、模块、补丁 有修改的需求时，该功能可能会有用
##### 只需在构建固件前执行以下命令即可启用本地 Kernel Modules 安装源

```
export ENABLE_LOCAL_KMOD=y
```

### 启用 [DPDK](https://www.dpdk.org/) 支持
##### DPDK（Data Plane Development Kit）是一个开源工具集，专为加速数据包处理而设计，通过优化的数据平面技术，实现高性能、低延迟的网络应用
##### 只需在构建固件前执行以下命令即可启用 DPDK 工具集支持

```
export ENABLE_DPDK=y
```

### 快速构建（仅限 Github Actions）
##### 脚本会使用 [toolchain](https://github.com/sbwml/toolchain-cache) 缓存代替源码构建，与常规构建相比能节省大约 60 分钟的编译耗时，仅适用于 Github Actions `ubuntu-24.04` 环境
##### 只需在构建固件前执行以下命令即可启用快速构建

```
export BUILD_FAST=y
```

### 构建 Minimal 版本
##### 不包含第三方插件，接近官方 OpenWrt 固件
##### 只需在构建固件前执行以下命令即可构建 Minimal 版本

```
export MINIMAL_BUILD=y
```

### 更改 LAN IP 地址
##### 自定义默认 LAN IP 地址
##### 只需在构建固件前执行以下命令即可覆盖默认 LAN 地址（默认：10.0.0.1）

```
export LAN=10.0.0.1
```

### 使用 uhttpd 轻量 web 引擎
##### 固件默认使用 Nginx（quic） 作为页面引擎，只需在构建固件前执行以下命令即可使用 uhttpd 取代 nginx
##### Nginx 在具备公网的环境下可以提供更丰富的功能支持

```
export ENABLE_UHTTPD=y
```

### 禁用全模块编译（For developers）
##### 启用该标志时，固件仅编译 config 指定的软件包和内核模块，但固件不再支持安装内核模块（opkg install kmod-xxx），强制安装模块将会导致内核崩溃
##### 最大的可能性降低 OpenWrt 的编译耗时，适用于开发者调试构建

```
export NO_KMOD=y
```

---------------

### 特别致谢：

#### 开源项目
- [OpenWrt](https://github.com/openwrt/openwrt)
- [r4s_build_script](https://github.com/sbwml/r4s_build_script/tree/openwrt-23.05)
