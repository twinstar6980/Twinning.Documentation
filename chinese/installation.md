# 安装步骤

- [平台支持](#平台支持)

- [模块组成](#模块组成)

- [安装准备](#安装准备)

- [编译或下载二进制分发](#编译或下载二进制分发)

- [使用捆绑包](#使用捆绑包)

- [安装 `Kernel`](#安装-Kernel)

- [安装 `Shell`](#安装-Shell)

- [安装 `Shell GUI`](#安装-Shell-GUI)

- [安装 `Script`](#安装-Script)

- [安装 `Forwarder For Windows`](#安装-Forwarder-For-Windows)

- [安装 `Forwarder For Macintosh`](#安装-Forwarder-For-Macintosh)

- [安装 `Forwarder For Android`](#安装-Forwarder-For-Android)

- [安装 `Helper`](#安装-Helper)

- [外部程序](#外部程序)

- [多语言支持](#多语言支持)

## 平台支持

* 操作系统：`Windows 7+` 、`Linux ~` 、`Macintosh 12~` 、`Android 9+` 、`iPhone 16~` 。

* 处理器架构：`x86 32|64` 、`arm 32|64` 。

## 模块组成

工具由多个模块组成，不同的模块提供了不同的功能：

* `Kernel`
	
	内核，负责内部功能的实现。

* `Shell`
	
	外壳，提供命令行界面。

* `Shell GUI`
	
	外壳，提供图形界面。

* `Script`
	
	脚本，控制工具的运行流程。

* `Forwarder For Windows`
	
	转发器，使用户可以通过 Windows Explorer 右键菜单将文件转发至工具。

* `Forwarder For Macintosh`
	
	转发器，使用户可以通过 Macintosh Finder 右键菜单将文件转发至工具。

* `Forwarder For Android`
	
	转发器，使用户可以通过 Android 文件分享功能将文件转发至工具。

* `Helper`
	
	助手，提供额外的高级功能。

## 安装准备

在安装各个组件前，你需要做一些准备工作：

1. 创建 **主目录**
	
	在存储空间中创建一个空目录，它用于存放工具运行所需的所有文件，称为主目录。
	
	`Kernel` 、`Shell` 、`Script` 是便携式的，请将它们放置在主目录中，这样不会在其他位置残留数据。
	
	`Shell GUI` 、`Windows Explorer Extension` 、`Helper` 是需要安装的应用，需要用户自行卸载，并清除应用数据。
	
	> 主目录的位置可以随意，但要确保用户有对该目录及其内容的 **读写执行权限** 。

## 编译或下载二进制分发

你可以克隆本项目并自行编译，本项目的 [Release](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Latest) 中也提供了预编译的二进制分发。

> Release 中只提供 `windows x86_64` 与 `android arm_64` 的二进制分发，如有面向其他平台的需要，请自行克隆并编译本项目。

> Release 中只分发了捆绑包，如果你只需要其中的个别模块，可以在我的 [个人网盘](https://1drv.ms/f/s!AkIzoME-1oU-fB6En185husw59Q?e=EKJ1e9) 中找到，其中也提供了历史版本的下载。

## 使用捆绑包

捆绑包已经组织好了 `Kernel` 、`Shell` 、`Script` 与 `Shell` 的启动脚本，使用捆绑包就不必依照下面的详细步骤进行安装。

1. 查看 [Release](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Latest) ，下载适用于你设备的分发。

2. 将下载的压缩包文件解压至主目录内。
	
	> @ `Android` \
	> 你还需要进行以下额外配置：安装 **C++ 共享库** 至系统库目录。\
	> 具体操作步骤参见下文。
	> 
	> @ `iPhone` \
	> 你还需要进行以下额外配置：对 `kernel` 与 `shell` 进行签名。\
	> 具体操作步骤参见下文。

3. 执行其中的 `launch.[sh|cmd]` 脚本以启动应用，工具应当能够正常启动与运行。
	
	> @ `Windows` \
	> 可以直接双击 `launch.cmd` 以运行，或将文件对象拖拽至其上并释放。

捆绑包中还包含了 `Shell GUI` 、`Forwarder` 模块的安装包，如有需要请参照下文手动安装。

## 安装 `Kernel`

内核，负责内部功能的实现。

这是必需安装项，分发为动态库。

1. 编译或下载适用于你设备的分发。

2. 将所得文件移动到主目录内，并重命名为 `kernel` 。

## 安装 `Shell`

外壳，提供命令行界面。

这是可选安装项，分发为可执行程序。

> @ `Android` `iPhone` \
> 在 Android 与 iPhone 上使用 `Shell` 需要 ROOT 权限，若你的设备未获取 ROOT ，请使用 `Shell GUI` 。

1. 编译或下载适用于你设备的分发。

2. 将所得文件移动到主目录内，并重命名为 `shell` 。
	
	> @ `Windows` \
	> Windows 不支持以非编程方式调用无 `exe` 扩展名的可执行文件，因此，需要在终端中执行 `> mklink shell.exe shell` 以创建指向 `- shell` 的软链接 `- shell.exe` ；或者直接重命名 `- shell` 为 `- shell.exe` 。

3. 赋予 `shell` 可执行权限。
	
	**Windows** 上一般不需操作，其他系统的用户可能要在终端中执行 `> chmod +x shell` 为 `- shell` 添加可执行权限。

对于 `Android` 系统，还需要执行以下操作：

1. 安装 **C++ 共享库** 至系统库目录。
	
	出于安全性与减小体积的考虑，程序使用的 C++ 运行时为 c++_shared 。因此，用户还需要将对应处理器架构的 `libc++_shared.so` 文件复制到系统库目录中。
	
	> 系统库目录在 32 位系统中为 `+ /system/lib` ，在 64 位系统中为 `+ /system/lib64` 。
	> 
	> 该文件可从 Android NDK 工具链中找到，捆绑包中也预置了与其中的 `kernel` 版本相对应的 `libc++_shared.so` 。

对于 `iPhone` 系统，还需要执行以下操作：

1. 对 `kernel` 与 `shell` 进行签名。
	
	在 iPhone 上运行可执行文件需要通过签名验证，而分发的程序文件（ **kernel | shell** ）未经过签名，用户需要自行对其进行签名，才能在 iPhone 中运行。
	
	> 可以在 **Macintosh** 上使用 **codesign** 工具进行签名。

## 安装 `Shell GUI`

外壳，提供图形界面。

这是可选安装项，分发为应用包。

1. 编译或下载适用于你设备的分发。

2. 安装应用包。
	
	> @ `Windows` \
	> 安装前需要先信任 MSIX 中的签名证书。\
	> 右键查看 `.msix` 的属性，切换到 ⌈ 数字签名 ⌋ 页，选择列表中第一项，再点击 ⌈ 详细信息 ⌋ ，在弹出的窗口中依次选择 ⌈ 查看证书 ⌋ - ⌈ 安装证书 ⌋ - ⌈ 本地计算机 ⌋ - ⌈ 将所有证书都放入下列存储 ⌋ - ⌈ 受信任人 ⌋ ，完成证书的安装。

对于 `Android` 系统，还需要执行以下操作：

1. 打开应用，点击右下方 ⌈ Setting ⌋ 进入设置页，点击 ⌈ Storage Permission ⌋ 一栏以申请存储空间访问权限。
	
	如果应用未取得这一权限，将只能访问应用的专属存储空间，而无法访问设备的共享外部存储空间。

## 安装 `Script`

脚本，控制工具的运行流程。

这是必需安装项，发为脚本包。

1. 编译或下载适用于你设备的分发。

2. 将所得文件移动到主目录内，创建 `script` 目录，将压缩包的所有内容解压至其中。

## 安装 `Forwarder For Windows`

转发器，使用户可以通过 Windows Explorer 右键菜单将文件转发至工具。

> 该模块专用于 Windows 系统，其他系统无法使用。

这是可选安装项，分发为应用包。

1. 编译或下载适用于你设备的分发。

2. 安装应用包。
	
	> 安装前需要先信任 MSIX 中的签名证书。\
	> 右键查看 `.msix` 的属性，切换到 ⌈ 数字签名 ⌋ 页，选择列表中第一项，再点击 ⌈ 详细信息 ⌋ ，在弹出的窗口中依次选择 ⌈ 查看证书 ⌋ - ⌈ 安装证书 ⌋ - ⌈ 本地计算机 ⌋ - ⌈ 将所有证书都放入下列存储 ⌋ - ⌈ 受信任人 ⌋ ，完成证书的安装。

3. 启动应用，运行成功后将弹出一个 Explorer 窗口，它指向了应用的私有数据目录，其中有名为 `forward.cmd` 的脚本文件，以文本形式打开并编辑该文件。
	
	用户每次选择该模块的扩展菜单项时，模块会将所选文件的路径作为参数启动该脚本，该脚本负责将接收到的参数转发至工具。
	
	以下示例能够将参数转发至 `launch.cmd` 与 `Helper` 模块提供的快速转发窗口中：
	
	```cmd
	@echo off
	"C:\TwinStar.ToolKit\launch.cmd" %*
	"C:\Program Files\WindowsApps\TwinStar.ToolKit.Helper_15.0.0.0_x64__7qfdsg797hj0p\Helper.exe" ^
		-WindowSize        496 968 ^
		-WindowAlwaysOnTop true ^
		-ModuleType        ResourceForwarder ^
		-ModuleOption ^
			-AutomaticClose  true ^
			-Input           %*
	```
	
	> `Helper.exe` 的路径需要自行在你的设备上检索。

4. 现在，你可以在任意文件或目录的右键菜单项中看到 `⌈ TwinStar ToolKit - Forwarder ⌋` ，并通过它将文件对象快速转发至工具。
	
	如果没有看到该选项，请尝试重启资源管理器 `explorer.exe` ，或重启计算机。

## 安装 `Forwarder For Macintosh`

转发器，使用户可以通过 Macintosh Finder 右键菜单将文件转发至工具。

> 该模块专用于 Macintosh 系统，其他系统无法使用。

这是可选安装项，分发为应用包。

1. 编译或下载适用于你设备的分发。

2. 安装应用包。

3. 启动应用，运行成功后将弹出一个 Finder 窗口，它指向了应用的私有数据目录，其中有名为 `forward.sh` 的脚本文件，以文本形式打开并编辑该文件。
	
	用户每次选择该模块的扩展菜单项时，模块会将所选文件的路径作为参数启动该脚本，该脚本负责将接收到的参数转发至工具；但需要注意，脚本的执行处于应用的沙盒环境中。
	
	以下示例能够将参数转发至 `Shell GUI` ：
	
	```sh
	#!/bin/bash
	"/Applications/TwinStar ToolKit - Shell GUI.app/Contents/MacOS/TwinStar ToolKit - Shell GUI" \
		"-additional" \
		"$@"
	```

4. 进入 ⌈ 系统设置 ⌋ - ⌈ 隐私与安全性 ⌋ - ⌈ 扩展 ⌋ - ⌈ 添加的扩展 ⌋ - ⌈ TwinStar ToolKit - Forwarder ⌋ ，勾选其中的 ⌈ “访达”扩展 ⌋ 。

5. 现在，你可以在任意文件或目录的右键菜单项中看到 `⌈ TwinStar ToolKit - Forwarder ⌋` ，并通过它将文件对象快速转发至工具。
	
	如果没有看到该选项，请尝试重新勾选扩展设置，或重启计算机。

## 安装 `Forwarder For Android`

转发器，使用户可以通过 Android 文件分享功能将文件转发至工具。

> 该模块专用于 Android 系统，其他系统无法使用。

这是可选安装项，分发为应用包。

1. 编译或下载适用于你设备的分发。

2. 安装应用包。

3. 现在，你可以在其他第三方文件管理器的文件分享目标内看到 `⌈ TwinStar ToolKit - Forwarder ⌋` ，并通过它将文件对象快速转发至工具。

> 由于 Android 系统的限制，转发器无法直接获取所转发文件的绝对路径，而是将所转发文件的 Content URI 传递至 Shell GUI 作为其启动的命令参数，Shell GUI 会尝试解析 Content URI 。具体参见 [Android Content URI 处理方式](./question.md#Android-Content-URI-处理方式) 。

## 安装 `Helper`

助手，提供额外的高级功能。

> 该模块专用于 Windows 系统，其他系统无法使用。

这是可选安装项，分发为应用包。

1. 编译或下载适用于你设备的分发。

2. 安装应用包。
	
	> 安装前需要先信任 MSIX 中的签名证书。\
	> 右键查看 `.msix` 的属性，切换到 ⌈ 数字签名 ⌋ 页，选择列表中第一项，再点击 ⌈ 详细信息 ⌋ ，在弹出的窗口中依次选择 ⌈ 查看证书 ⌋ - ⌈ 安装证书 ⌋ - ⌈ 本地计算机 ⌋ - ⌈ 将所有证书都放入下列存储 ⌋ - ⌈ 受信任人 ⌋ ，完成证书的安装。

## 外部程序

工具的某些功能需要调用外部程序，用户需自行下载并安装这些程序，并放置于主目录下的 `external` 目录内。

* [ffmpeg](https://ffmpeg.org/download.html)
	
	用于 **WEM 音频解码** 。
	
	下载完成后，将文件 `ffmpeg[.exe]` 重命名为 `ffmpeg` 并放置于 `+ <home>/external/ffmpeg` 目录下。

* [ww2ogg](https://github.com/hcs64/ww2ogg/releases/tag/0.24)
	
	用于 **WEM 音频解码** 。
	
	下载完成后，将文件 `ww2ogg[.exe]` 重命名为 `ww2ogg` 并放置于 `+ <home>/external/ww2ogg` 目录下。
	
	再将文件 `packed_codebooks_aoTuV_603.bin` 放置于该目录下。
	
	> 该项目的作者只分发了适用于 Windows 的可执行程序，其他系统的用户须自行下载源码构建。

* [adb](https://developer.android.com/studio/command-line/adb)
	
	用于 **远程安卓辅助** 。
	
	下载 [`Android SDK Platform Tools`](https://developer.android.com/studio/releases/platform-tools) ，解压并配置 `PATH` 环境变量中。

## 多语言支持

工具提供多语言支持，目前支持中文与英文。

默认情况下，工具将使用中文进行交互，如果需要切换为其他语言，应修改配置文件，步骤如下：

1. 进入工具的主目录，找到 `- <home>/script/Entry/Entry.json` 文件，使用文本编辑器打开。

2. 文本中的第二行为 `"language": "Chinese"` ，表示目前所使用的交互语言为中文，若需切换为英文，请将 `Chinese` 改为 `English` 。

> 多语言文本通过 `- <home>/script/Language/<Language-ID>.json` 定义，如果需要修正文本错误，或增加对其他语言的支持，请修改该文件；方便的话，也请提交 PR 为本项目做贡献。
