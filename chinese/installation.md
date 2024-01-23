# 安装步骤

- [平台支持](#平台支持)

- [模块组成](#模块组成)

- [安装步骤](#安装步骤)

- [外部程序](#外部程序)

## 平台支持

工具支持以下平台：

* 操作系统：`Windows 7+` 、`Linux ~` 、`Macintosh 13~` 、`Android 9+` 、`iPhone 16~` 。

* 处理器架构：`x86 32|64` 、`arm 32|64` 。

## 模块组成

工具由多个模块组成，不同的模块提供了不同的功能：

* `Kernel`
	
	内核，负责内部功能的实现。
	
	必需模块，分发为动态库。

* `Shell CLI`
	
	外壳，提供命令行界面。
	
	可选模块，分发为可执行程序。
	
	> 在 `Android` 与 `iPhone` 系统中使用 `Shell CLI` 需要 ROOT 权限，若你的设备未获取 ROOT ，请使用 `Shell GUI` 。

* `Shell GUI`
	
	外壳，提供图形界面。
	
	可选模块，分发为应用安装包。

* `Script`
	
	脚本，控制工具的运行流程。
	
	必需模块，发为脚本包。

* `Forwarder For Windows`
	
	转发器，使用户可以通过 Windows Explorer 右键菜单将文件转发至工具。
	
	可选模块，分发为应用安装包。
	
	> 该模块仅适用于 `Windows` 系统。

* `Forwarder For Macintosh`
	
	转发器，使用户可以通过 Macintosh Finder 右键菜单将文件转发至工具。
	
	可选模块，分发为应用安装包。
	
	> 该模块仅适用于 `Macintosh` 系统。

* `Forwarder For Android`
	
	转发器，使用户可以通过 Android 文件分享功能将文件转发至工具。
	
	可选模块，分发为应用安装包。
	
	> 该模块仅适用于 `Android` 系统。

* `Helper`
	
	助手，提供额外的高级功能。
	
	可选模块，分发为应用安装包。
	
	> 该模块仅适用于 `Windows` 系统。

## 安装步骤

你可以克隆本项目并自行编译，或是直接下载本项目的捆绑包分发。

1. 下载并解压捆绑包。
	
	查看 [Release](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Latest) ，捆绑包分发的命名为 `<version>.<system>.<architecture>.bundle.zip` ，其中 `system` 代表适配的操作系统，`architecture` 代表适配的 CPU 架构。
	
	> Release 页中只保存了最新的捆绑包，如果需要历史版本，可以查看我的 [个人 OneDrive](https://1drv.ms/f/s!AkIzoME-1oU-fB6En185husw59Q?e=EKJ1e9) 。
	
	目前，提供了预编译分发的平台如下：
	
	* `Windows` - `x86_64`
	
	* `Android` - `arm_64`
	
	* `Macintosh` - `x86_64` `arm_64`
	
	* `iPhone` - `arm_64`
	
	* `Linux` - `x86_64`
	
	其中，仅 `Windows` 与 `Android` 平台的预编译分发会始终跟随最新版本，其他平台的预编译分发一般很少更新，如果需要最新版本，或者你的平台不在上述列表中，请尝试自行编译。

2. 选择主文件夹。
	
	将解压得到的文件夹移动到适合的位置，它作为工具的主文件夹，绝对路径记录为 `<home>` 。

3. 为主文件夹下的 `kernel` 与 `shell_cli` 文件签名。
	
	> 该步骤只需 `iPhone` 用户操作，且需要在 `Macintosh` 设备上操作。
	
	在系统终端中运行命令：`> codesign -s <certificate-name> kernel` & `> codesign -s <certificate-name> shell_cli` 。

4. 为主文件夹下的 `shell_cli` 文件赋予可执行权限。
	
	> 如果不需要 `Shell CLI` 模块，可以跳过该步骤。\
	> 该步骤只需 `Linux` 、`Macintosh` 、`Android` 、`iPhone` 用户操作。
	
	在系统终端中运行命令：`> chmod +x shell_cli` 。

5. 为系统安装 C++ 共享库。
	
	> 如果不需要 `Shell CLI` 模块，可以跳过该步骤。\
	> 该步骤只需 `Android` 用户操作。
	
	将主文件夹内的 `libc++_shared.so` 文件复制至系统库文件夹 `/system/lib64` 中。

6. 安装主文件夹内 `Shell GUI` 、`Forward` 、`Helper` 模块的应用安装包。
	
	应用安装包文件以 `msix` 、`dmg` 、`apk` 、`ipa` 等作为扩展名。
	
	> @ `Windows` \
	> 安装前需要先信任 MSIX 中的签名证书。\
	> 右键查看 `.msix` 的属性，切换到 ⌈ 数字签名 ⌋ 页，选择列表中第一项，再点击 ⌈ 详细信息 ⌋ ，在弹出的窗口中依次选择 ⌈ 查看证书 ⌋ - ⌈ 安装证书 ⌋ - ⌈ 本地计算机 ⌋ - ⌈ 将所有证书都放入下列存储 ⌋ - ⌈ 受信任人 ⌋ ，完成证书的安装。
	> 
	> @ `Macintosh` \
	> 挂载 `dmg` 后，将其中的 `*.app` 文件夹移动至系统应用文件夹中。
	> 
	> @ `iPhone` \
	> 需要通过 AltStore 或其他工具对 ipa 进行自签名与安装。

7. 配置 `Shell GUI` 设置项。
	
	> 如果不需要 `Shell GUI` 模块，可以跳过该步骤。
	
	打开新安装的 `Shell GUI` 应用，点击右下角的 `Setting` 按钮进入设置页面，编辑以下设置项：
	
	* `Kernel` = `<home>/kernel` 。
	
	* `Script` = `<home>/script/main.js` 。
	
	* `Argument` = `<home>` 。
	
	* `Fallback Directory` = `<home>/workspace` 。
		
		> 该项仅适用于 `Android` 与 `iPhone` 。
	
	* `Storage Permission` 点击并为应用授予存储空间读写权限 。
		
		> 该项仅适用于 `Android` 。
	
	> 上述设置中的 `<home>` 需要替换为主文件夹的绝对路径。

8. 配置 `Forwarder For Windows` 设置项。
	
	> 如果不需要 `Forwarder For Windows` 模块，可以跳过该步骤。
	
	打开新安装的 `Forwarder For Windows` 应用，会弹出一个文件夹窗口，其中有名为 `forward.cmd` 的文件，该脚本负责接收文件路径参数并启动工具，以文本形式打开并编辑该文件。
	
	以下示例将参数转发至 `Shell CLI` ：
	
	```cmd
	@echo off
	set home=C:\TwinStar.ToolKit
	start "" "%home%\shell_cli.exe" ^
		"%home%\kernel" ^
		"%home%\script\main.js" ^
		"%home%" ^
		%*
	```
	
	以下示例将参数转发至 `Shell GUI` ：
	
	```cmd
	@echo off
	start "" "C:\Program Files\WindowsApps\TwinStar.ToolKit.ShellGUI_32.0.0.0_x64__7qfdsg797hj0p\shell_gui.exe" ^
		-additional_argument %*
	```
	
	以下示例将参数转发至 `Helper` - `Modding Worker`：
	
	```cmd
	@echo off
	start "" "C:\Program Files\WindowsApps\TwinStar.ToolKit.Helper_26.0.0.0_x64__7qfdsg797hj0p\Helper.exe" ^
		-WindowSize        496 968 ^
		-WindowAlwaysOnTop true ^
		-ModuleType        ModdingWorker ^
		-ModuleOption ^
			-ImmediateLaunch    true ^
			-AdditionalArgument %*
	```
	
	以下示例将参数转发至 `Helper` - `Resource Forwarder`：
	
	```cmd
	@echo off
	start "" "C:\Program Files\WindowsApps\TwinStar.ToolKit.Helper_26.0.0.0_x64__7qfdsg797hj0p\Helper.exe" ^
		-WindowSize        496 968 ^
		-WindowAlwaysOnTop true ^
		-ModuleType        ResourceForwarder ^
		-ModuleOption ^
			-AutomaticClose true ^
			-Input          %*
	```

9. 配置 `Forwarder For Macintosh` 设置项。
	
	> 如果不需要 `Forwarder For Macintosh` 模块，可以跳过该步骤。
	
	进入 ⌈ 系统设置 ⌋ - ⌈ 隐私与安全性 ⌋ - ⌈ 扩展 ⌋ - ⌈ 添加的扩展 ⌋ - ⌈ TwinStar ToolKit - Forwarder ⌋ ，勾选其中的 ⌈ “访达”扩展 ⌋ ，确保应用能够生效。
	
	打开新安装的 `Forwarder For Macintosh` 应用，会弹出一个文件夹窗口，其中有名为 `forward.sh` 的文件，该脚本负责接收文件路径参数并启动工具，以文本形式打开并编辑该文件。
	
	> 注意：脚本的执行处于应用的沙盒环境中。
	
	以下示例将参数转发至 `Shell GUI` ：
	
	```sh
	#!/bin/bash
	"/Applications/TwinStar ToolKit - Shell GUI.app/Contents/MacOS/TwinStar ToolKit - Shell GUI" \
		-additional_argument "$@"
	```

10. 配置 `Helper` 设置项。
	
	> 如果不需要 `Helper` 模块，可以跳过该步骤。
	
	打开新安装的 `Helper` 应用，点击主页中各个模块按钮右上方的齿轮图标可以打开模块设置对话框，编辑以下设置项：
	
	* `Modding Worker` - `Kernel` = `<home>/kernel` 。
	
	* `Modding Worker` - `Script` = `<home>/script/main.js` 。
	
	* `Modding Worker` - `Argument` = `<home>` 。
	
	* `Resource Forwarder` - `Option Configuration` = `<home>/helper/OptionConfiguration.json` 。
	
	* `Command Sender` - `Method Configuration` = `<home>/helper/MethodConfiguration.json` 。
	
	> 上述设置中的 `<home>` 需要替换为主文件夹的绝对路径。

11. 设置交互语言。
	
	以文本形式打开并编辑主文件夹内的 `script/Entry/Entry.json` 文件，找到 `"language": "English"` 部分，修改它以切换工具的交互语言。
	
	* `English` 英文（默认）
	
	* `Chinese` 中文
	
	* `Vietnamese` 越文

12. 至此，已经完成了所有安装步骤，可以通过终端命令行或直接打开应用的方式启动工具。
	
	> @ `Windows` \
	> 可以直接双击主文件夹中的启动脚本 `launch*.cmd` 启动工具，或将文件对象拖拽至其上并释放。\
	> 如果安装了 `Forwarder` 模块，可以在任意文件或文件夹的右键菜单项中看到 `⌈ TwinStar ToolKit - Forwarder ⌋` ，并通过它将文件对象快速转发至工具。\
	> 如果安装了 `Helper` 模块，可以通过其中的 `Resource Forwarder` 进行更快捷的转发，也可以通过 `Command Sender` 可视化地选择需要的功能并编辑参数。
	
	> @ `Macintosh` \
	> 如果安装了 `Forwarder` 模块，可以在任意文件或文件夹的右键菜单项中看到 `⌈ TwinStar ToolKit - Forwarder ⌋` ，并通过它将文件对象快速转发至工具。
	
	> @ `Android` \
	> 如果安装了 `Forwarder` 模块，可以在系统或第三方的文件管理器的文件分享列表中看到 `⌈ TwinStar ToolKit - Forwarder ⌋` ，并通过它将文件对象快速转发至工具。\
	> 注意：由于 Android 系统的限制，转发器无法直接获取所转发文件的绝对路径，而是将所转发文件的 Content URI 传递至 Shell GUI 作为其启动的命令参数，Shell GUI 会尝试解析 Content URI 。具体参见 [Android Content URI 处理方式](./question.md#Android-Content-URI-处理方式) 。

## 外部程序

工具的某些功能需要调用外部程序，需要用户自行下载与安装。

* [adb](https://developer.android.com/studio/releases/platform-tools)
	
	用于 **远程安卓辅助** 。
	
	安装并配置 `PATH` 环境变量，以确保工具能通过 `PATH` 环境变量检索到 `adb` 可执行程序。
	
	> 该程序仅支持 `Windows` 、`Linux` 、`Macintosh` 系统。

* [vgmstream-cli](https://vgmstream.org/)
	
	用于 **WEM 音频解码** 。
	
	安装并配置 `PATH` 环境变量，以确保工具能通过 `PATH` 环境变量检索到 `vgmstream-cli` 可执行程序。
	
	> 该程序仅支持 `Windows` 、`Linux` 、`Macintosh` 系统。

* [WwiseConsole](https://www.audiokinetic.com/en/download) `=2019.2`
	
	用于 **WEM 音频编码** 。
	
	安装并配置 `PATH` 环境变量，以确保工具能通过 `PATH` 环境变量检索到 `WwiseConsole.exe` 或 `WwiseConsole.sh` 可执行程序。
	
	> `WwiseConsole` 可执行程序的路径参见 [官方文档](https://www.audiokinetic.com/zh/library/edge/?source=SDK&id=bankscommandline.html) 。
	
	> 该程序仅支持 `Windows` 、`Macintosh` 系统。

* [QuickTime](https://support.apple.com/kb/DL837) `>7.6`
	
	用于 **WEM 音频编码** 。
	
	如果需要编码 `AAC` 格式的 `WEM` ，请确保 `QuickTime` 已正确安装至系统中，该程序会被 `WwiseConsole` 调用。

* [Il2CppDumper](https://github.com/Perfare/Il2CppDumper) `=6.7.40,x86`
	
	用于 **开罗游戏程序修改** 。
	
	安装并配置 `PATH` 环境变量，以确保工具能通过 `PATH` 环境变量检索到 `Il2CppDumper-x86` 可执行程序。
	
	> 该程序仅支持 `Windows` 系统。
