# SecureChat macOS 版

这是 [SecureChat](https://github.com/kiritoX0914/SecureChat) 的非官方 **Mac Catalyst** 构建。功能和 iOS 版一样：局域网端到端加密聊天。可在 **macOS 13 及以上**（包括 **macOS 15.x**）运行，同时支持 **Apple 芯片** 和 **Intel**。

本仓库是 iOS 工程的副本，只做了为 Mac 构建所需的最小改动，原来的 iOS 仓库没有修改。

## 下载和安装

从最新一次 [Actions](https://github.com/kiritoX0914/SecureChat-macOS/actions) 运行结果里下载 `SecureChat-macOS.zip`（产物名一般是 `SecureChat-macOS-macos-15-intel`），或到 [Releases](https://github.com/kiritoX0914/SecureChat-macOS/releases) 下载。

1. 解压后得到 `SecureChat.app`。
2. 这个包是 **临时签名**（ad-hoc），没有经过 Apple 公证，第一次打开可能被系统拦截。可以：
   - 右键 `SecureChat.app` → **打开** → 打开
   - 或在终端执行：`xattr -cr SecureChat.app && open SecureChat.app`
3. 如果系统询问本地网络权限，请允许，否则局域网发现会失败。

安装包是通用二进制（`arm64` + `x86_64`），最低系统版本 **macOS 13.0**。


## 用 IP 连接（v1.1）

如果自动发现失败（例如这台 Mac 正在把网线网络共享给通过巨魔安装的 iPhone），打开「设置」或点工具栏的链接图标「用 IP 连接」。这里会显示本机 IPv4 地址和监听端口（默认 **47823**）。在另一台设备上输入该地址和端口再点连接，完成握手后对方会出现在聊天列表里。

互联网共享：从手机连 Mac 时请用 Mac 的 `bridge100` 地址；从 Mac 连手机时请用 iPhone 在共享网络上的地址。
