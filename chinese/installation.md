# 安装步骤

- [平台支持](#平台支持)

- [模块组成](#模块组成)

- [安装准备](#安装准备)

- [编译或下载预编译分发](#编译或下载预编译分发)

- [使用捆绑包](#使用捆绑包)

- [安装 `Core`](#安装-Core)

- [安装 `Shell`](#安装-Shell)

- [安装 `Shell GUI`](#安装-Shell-GUI)

- [安装 `Script`](#安装-Script)

- [安装 `Windows Explorer Extension`](#安装-Windows-Explorer-Extension)

- [安装 `Helper`](#安装-Helper)

- [外部程序](#外部程序)

- [多语言支持](#多语言支持)

## 平台支持

* 操作系统：`Windows 7+` 、`Linux ?` 、`Macintosh 13.1~` 、`Android 9~` 、`iPhone 16.2~` 。

* 处理器架构：`x86 32|64` 、`arm 32|64` 。

## 模块组成

工具由多个模块组成，不同的模块提供了不同的功能：

* `Core`
	
	核心，负责内部功能的实现。

* `Shell`
	
	外壳，提供命令行界面。

* `Shell GUI`
	
	外壳，提供图形界面。

* `Script`
	
	脚本，控制工具的运行流程。

* `Windows Explorer Extension`
	
	Windows Explorer 扩展，将工具集成至 Windows Explorer 右键菜单中。

* `Helper`
	
	助手，提供额外的高级功能。

## 安装准备

在安装各个组件前，你需要做一些准备工作：

1. 创建 **主目录**
	
	在存储空间中创建一个空目录，它用于存放工具运行所需的所有文件，称为主目录。
	
	`Core` 、`Shell` 、`Script` 是便携式的，请将它们放置在主目录中，这样不会在其他位置残留数据。
	
	`Shell GUI` 、`Windows Explorer Extension` 、`Helper` 是需要安装的应用，需要用户自行卸载，并清除应用数据。
	
	> 主目录的位置可以随意，但要确保用户有对该目录及其内容的 **读写执行权限** 。

## 编译或下载预编译分发

你可以克隆本项目并自行编译，本项目的 [Release](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Latest) 中也提供了预编译的二进制分发。

> Release 中只提供 `windows x86_64` 与 `android arm_64` 的二进制分发，如有面向其他平台的需要，请自行克隆并编译本项目。

> Release 中只分发了捆绑包，如果你只需要其中的个别模块，可以在我的 [个人网盘](https://1drv.ms/f/s!AkIzoME-1oU-fB6En185husw59Q?e=EKJ1e9) 中找到，其中也提供了历史版本的下载。

## 使用捆绑包

捆绑包已经组织好了 `Core` 、`Shell` 、`Script` 与 `Shell` 的启动脚本，使用捆绑包就不必依照下面的详细步骤进行安装。

1. 查看 [Release](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Latest) ，下载适用于你设备的分发。

2. 将下载所得的压缩包文件解压至主目录内。
	
	> @ `Android` \
	> 你还需要进行以下额外配置：安装 **C++ 共享库** 至系统库目录。\
	> 具体操作步骤参见下文。
	> 
	> @ `iPhone` \
	> 你还需要进行以下额外配置：对 `core` 与 `shell` 进行签名。\
	> 具体操作步骤参见下文。

3. 执行其中的 `launch.[sh|cmd]` 脚本以启动应用，工具应当能够正常启动与运行。
	
	> @ `Windows` \
	> 可以直接双击 `launch.cmd` 以运行，或将文件对象拖拽至其上并释放。

捆绑包中还包含了 `Shell GUI` 、`Windows Explorer Extension` 模块的安装包，如有需要请参照下文手动安装。

## 安装 `Core`

核心，负责内部功能的实现。

这是必需安装项，分发为动态库。

1. 编译或下载适用于你设备的分发。

2. 将下载所得文件移动到主目录内，并重命名为 `core` 。

## 安装 `Shell`

外壳，提供命令行界面。

这是可选安装项，分发为可执行程序。

> @ `Android` `iPhone` \
> 在 Android 与 iPhone 上使用 `Shell` 需要 ROOT 权限，若你的设备未获取 ROOT ，请使用 `Shell GUI` 。

1. 编译或下载适用于你设备的分发。

2. 将下载所得文件移动到主目录内，并重命名为 `shell` 。
	
	> @ `Windows` \
	> Windows 不支持以非编程方式调用无 `exe` 扩展名的可执行文件，因此，需要在终端中执行 `> mklink shell.exe shell` 以创建指向 `- shell` 的软链接 `- shell.exe` ；或者直接重命名 `- shell` 为 `- shell.exe` 。

3. 赋予 `shell` 可执行权限。
	
	**Windows** 上一般不需操作，其他系统的用户可能要在终端中执行 `> chmod +x shell` 为 `- shell` 添加可执行权限。
	
	> @ `Android` \
	> 在采用了 FUSE 方案的系统中，`+ /storage/emulated/<id>` 实际上是 `+ /data/media/<id>` 的映射，但 FUSE 的限制使用户无法通过 `+ /storage/emulated/<id>` 进行文件的权限修改与执行，必须通过真实路径 `+ /data/media/<id>` 进行访问。

对于 `Android` 系统，还需要执行以下操作：

1. 安装 **C++ 共享库** 至系统库目录。
	
	出于安全性与减小体积的考虑，程序使用的 C++ 运行时 为 c++_shared 。因此，用户还需要将对应处理器架构的 `libc++_shared.so` 文件复制到系统库目录中。
	
	> 系统库目录在 32 位系统中为 `+ /system/lib` ，在 64 位系统中为 `+ /system/lib64` 。
	> 
	> 该文件可从 Android NDK 工具链中找到，捆绑包中也预置了与其中的 `core` 版本相对应的 `libc++_shared.so` 。

对于 `iPhone` 系统，还需要执行以下操作：

1. 对 `core` 与 `shell` 进行签名。
	
	在 iPhone 上运行可执行文件需要通过签名验证，而分发的程序文件（ **core | shell** ）未经过签名，用户需要自行对其进行签名，才能在 iPhone 中正常运行。
	
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

1. 在系统设置中为应用授予完全的外部存储空间读写权限。
	
	如果应用未取得这一权限，将在启动控制台时出现 ⌈ Permission denied ⌋ 错误。
	
	> 在不同版本与厂商定制的系统中，该权限一般会被表述为 ⌈ 读写存储空间 ⌋ 、⌈ 管理所有文件 ⌋ 等。\
	> 原生系统中，可以在应用设置中找到授权页面；\
	> 对于一些厂商定制系统，可能需要在别处进行授权，比如 MIUI 需要在 ⌈ 保护隐私 ⌋ 中授予应用 ⌈ 所有文件访问权限 ⌋ 。

## 安装 `Script`

脚本，控制工具的运行流程。

这是必需安装项，发为脚本包。

1. 编译或下载适用于你设备的分发。

2. 将下载所得文件移动到主目录内，创建 `script` 目录，并将压缩包的所有内容解压至其中。

## 安装 `Windows Explorer Extension`

Windows Explorer 扩展，将工具集成至 Windows Explorer 右键菜单中。

> 该模块专用于 Windows 系统，其他系统无法使用。

这是可选安装项，分发为应用包。

1. 编译或下载适用于你设备的分发。

2. 安装应用包。
	
	扩展分为两个子版本，需按个人需要进行选择：
	
	* `0_...` 只提供普通的文件转发入口；在新、旧式右键菜单中均显示为单个菜单项。
	
	* `1_...` 额外提供了各功能的快捷转发入口，但对右键菜单的调出速度有较大影响；在新式右键菜单中显示为单个二级菜单，在旧式右键菜单中显示为多个一级菜单。
	
	> 安装前需要先信任 MSIX 中的签名证书。\
	> 右键查看 `.msix` 的属性，切换到 ⌈ 数字签名 ⌋ 页，选择列表中第一项，再点击 ⌈ 详细信息 ⌋ ，在弹出的窗口中依次选择 ⌈ 查看证书 ⌋ - ⌈ 安装证书 ⌋ - ⌈ 本地计算机 ⌋ - ⌈ 将所有证书都放入下列存储 ⌋ - ⌈ 受信任人 ⌋ ，完成证书的安装。

3. 以文本方式打开 `create_setting.reg` 文件，在其中编辑配置项，完成后右键选择注册表编辑器打开该文件以合并注册表。
	
	扩展将从系统注册表中读取扩展配置数据，所有配置均位于 `HKEY_CURRENT_USER\Software\TwinStar\ToolKit\WindowsExplorerExtension` ，以下是配置项：
	
	* `launch_script`
		
		字符串，启动脚本的路径，默认为空串。
		
		扩展程序将执行指定的启动脚本，并将所选文件的路径传递给它。因此，必须创建一个启动脚本，在其中启动外壳并传递参数。
		
		> 启动脚本应当正确地启动工具的 `Shell` 或 `Shell GUI` ，你需要自行编写启动脚本 `*.cmd` ，或使用捆绑包中预置的启动脚本 `launch.cmd` 。
	
	* `launch_limit`
		
		双字，所需文件对象的数量限制，默认为 `0` 。
		
		为 `0` 时，无数量限制，此时所有文件对象一并转发到工具，需要顺序处理；否则限制为不超过所设数字，此时每项文件对象分别转发到工具，用户可以再不同窗口分别处理它们。
	
	* `language`
		
		字符串，扩展使用的语言，默认为 `Chinese` 。
		
		可为 `Chinese` 或 `English` 。
	
	* `hidden_group_when_unavailable`
		
		双字，当某个功能组内各功能项都不适用于所选文件对象时是否隐藏该功能组，默认为 `0` 。
		
		值为 `0` 时显示，否则隐藏。
	
	* `hidden_item_when_unavailable`
		
		双字，当某个功能项不适用于所选文件对象时是否隐藏该功能项，默认为 `0` 。
		
		值为 `0` 时显示，否则隐藏。
	
	* `visible_<group>`
		
		双字，控制是否显示各功能组，默认为 `0` 。
		
		值为 `0` 时隐藏，否则显示。
		
		可以将不常用的功能组隐藏，以减少对右键菜单空间的占用、降低对右键菜单调出速度的影响。

4. 现在，你可以在任意文件或目录的右键菜单内看到 `⌈ TwinStar ToolKit - Extension ⌋` 选项，并通过这些选项将文件对象快速转发至工具。
	
	如果没有看到该选项，请尝试重启资源管理器 `explorer.exe` ，或重启计算机。
	
	> 请注意，由于 Windows 的限制，每次最多只能选择 16 项文件对象，超出该数目则无法成功转发文件对象。

5. 卸载扩展程序后，可以使用 `remove_setting.reg` 文件以清除系统注册表中残留的扩展配置数据。

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
