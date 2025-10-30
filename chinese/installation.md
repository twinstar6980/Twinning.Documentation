# 安装步骤

- [平台支持](#平台支持)

- [模块组成](#模块组成)

- [安装步骤](#安装步骤)

- [外部程序](#外部程序)

## 平台支持

工具支持以下平台：

* 操作系统：`Windows 10+` 、`Linux ~` 、`Macintosh 11+` 、`Android 11+` 、`Iphone 15+` 。

* 处理器架构：`AMD64` 、`ARM64` 。

## 模块组成

工具由多个模块组成，不同的模块提供了不同的功能：

* `Kernel`
	
	内核，负责内部功能的实现。
	
	必需模块，分发为动态库。

* `Shell`
	
	外壳，提供基本的用户界面。
	
	可选模块，分发为可执行程序。
	
	> 在 `Android` 与 `Iphone` 系统中使用 `Shell` 需要 ROOT 权限，若你的设备未获取 ROOT ，请使用 `Assistant` 。

* `Script`
	
	脚本，控制工具的运行流程。
	
	必需模块，发为脚本包。

* `Assistant`
	
	助理，实现了额外的高级辅助功能。
	
	可选模块，分发为应用安装包。

* `Assistant Plus`
	
	助理+，面向 Windows 平台的特化版本。
	
	可选模块，分发为应用安装包。
	
	> 该模块仅适用于 `Windows` 系统。

## 安装步骤

你可以克隆本项目并自行编译，或是直接下载本项目的捆绑包分发。

1. 下载并解压捆绑包。
	
	查看 [Release](https://github.com/twinstar6980/Twinning.Documentation/releases/tag/Latest) ，捆绑包分发的命名为 `<version>.<system>.<architecture>.bundle.zip` ，其中 `system` 代表适配的操作系统，`architecture` 代表适配的 CPU 架构。
	
	> GitHub Release 中仅保留最后一次分发，历史分发可以在我的 [OneDrive](https://1drv.ms/f/c/2d321feb9cd374ed/Qu1005zrHzIggC2EAAAAAAAAiShIb2wbJMeZsw) 中找到。
	
	目前，提供了预编译分发的平台如下：
	
	* `Windows` - `AMD64`
	
	* `Linux` - `AMD64`
	
	* `Macintosh` - `ARM64`
	
	* `Android` - `ARM64`
	
	* `Iphone` - `ARM64`

2. 选择主目录。
	
	将解压得到的目录移动到适合的位置，它作为工具的主目录，绝对路径记录为 `<home>` 。

3. 为主目录下的 `shell` 文件赋予可执行权限。
	
	> 如果不需要 `Shell` 模块，可以跳过该步骤。\
	> 该步骤只需 `Linux` 、`Macintosh` 、`Android` 、`Iphone` 用户操作。
	
	在系统终端中运行命令：`> chmod +x ./shell` 。

4. 安装主目录内 `Assistant` 、`Assistant Plus` 模块的应用安装包。
	
	应用安装包文件以 `msix` 、`dmg` 、`apk` 、`ipa` 等作为扩展名。
	
	> @ `Windows` \
	> 安装前需要先信任 `msix` 中的签名证书。\
	> 右键查看 `msix` 的属性，切换到 ⌈ 数字签名 ⌋ 页，选择列表中第一项，再点击 ⌈ 详细信息 ⌋ ，在弹出的窗口中依次选择 ⌈ 查看证书 ⌋ - ⌈ 安装证书 ⌋ - ⌈ 本地计算机 ⌋ - ⌈ 将所有证书都放入下列存储 ⌋ - ⌈ 受信任人 ⌋ ，完成证书的安装。
	> 
	> @ `Iphone` \
	> 需要通过 AltStore 或其他工具对 `ipa` 进行自签名与安装。

5. 配置 `Assistant` 设置项。
	
	> 如果不需要 `Assistant` 模块，可以跳过该步骤。
	
	打开 `Assistant` 应用，点击页面中各个模块列表项右侧的齿轮图标可以打开模块设置对话框，编辑以下设置项：
	
	* `Storage Permission` 点击以授予应用存储空间读写权限。
		
		> 该项仅适用于 `Android` 。
	
	* `Storage Fallback Directory` = `<home>/workspace` 。
		
		> 该项仅适用于 `Android` 。
	
	* `Forwarder Extension` 点击以启用转发器扩展（可选）。
		
		> 对于 `Windows` ，应用默认禁用转发器扩展，如果需要更改，请点击对话框中的开关。
		
		> 对于 `Macintosh` ，系统默认禁用转发器扩展，如果需要更改，请点击对话框中的开关，跳转到系统设置面板后，切换窗口中本应用所对应的开关。
		
		> 对于 `Linux` ，转发器扩展不支持。
		
		> 对于 `Android` ，系统将始终启用转发器扩展，应用无法自行更改启用状态。
		
		> 对于 `Iphone` ，系统默认启用转发器扩展，应用无法自行更改启用状态，如果需要更改，请打开 ⌈ 文件 ⌋ ，选择任意文件后点击 ⌈ 共享 ⌋ - ⌈ 编辑操作... ⌋ - ⌈ Twinning Assistant ⌋ ，勾选右侧的开关按钮。
	
	* `Forwarder Default Target` 转发器扩展的默认转发目标模块。
	
	* `Forwarder Immediate Jump` 转发器扩展是否立即转发至默认目标模块，而非等待用户选择。
	
	* `Modding Worker` - `Kernel` = `<home>/kernel` 。
	
	* `Modding Worker` - `Script` = `<home>/script/main.js` 。
	
	* `Modding Worker` - `Argument` = `<home>` 。
	
	* `Command Sender` - `Method Configuration` = `<home>/assistant/method_configuration.json` 。
	
	* `Resource Shipper` - `Option Configuration` = `<home>/assistant/option_configuration.json` 。
	
	> 上述设置中的 `<home>` 需要替换为主目录的绝对路径。

6. 配置 `Assistant Plus` 设置项。
	
	> 如果不需要 `Assistant Plus` 模块，可以跳过该步骤。
	
	打开 `Assistant Plus` 应用，点击页面中各个模块列表项右侧的齿轮图标可以打开模块设置对话框，编辑以下设置项：
	
	* `Forwarder Extension` 点击以启用转发器扩展（可选）。
	
	* `Forwarder Default Target` 转发器扩展的默认转发目标模块。
	
	* `Forwarder Immediate Jump` 转发器扩展是否立即转发至默认目标模块，而非等待用户选择。
	
	* `Modding Worker` - `Kernel` = `<home>/kernel` 。
	
	* `Modding Worker` - `Script` = `<home>/script/main.js` 。
	
	* `Modding Worker` - `Argument` = `<home>` 。
	
	* `Command Sender` - `Method Configuration` = `<home>/assistant_plus/method_configuration.json` 。
	
	* `Resource Shipper` - `Option Configuration` = `<home>/assistant_plus/option_configuration.json` 。
	
	> 上述设置中的 `<home>` 需要替换为主目录的绝对路径。

7. 设置脚本交互语言。
	
	以文本形式打开并编辑主目录内的 `<home>/script/configuration/setting.json` 文件，找到 `"language": "english"` 部分，修改它以切换工具的交互语言。
	
	* `english` 英文（默认）
	
	* `chinese` 中文
	
	* `vietnamese` 越文

8. 至此，已经完成了所有安装步骤，你可以通过终端或 GUI 应用使用工具。

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

* [dotnet](https://dotnet.microsoft.com/en-us/download/dotnet/7.0) `=7.0.20`
	
	用于 **开罗游戏程序修改** 。
	
	安装并配置 `PATH` 环境变量，以确保工具能通过 `PATH` 环境变量检索到 `dotnet` 可执行程序。
	
	> 该程序仅支持 `Windows` 、`Linux` 、`Macintosh` 系统。

* [Il2CppDumper.dll](https://github.com/Perfare/Il2CppDumper) `=6.7.46,net7`
	
	用于 **开罗游戏程序修改** 。
	
	安装并配置 `PATH` 环境变量，以确保工具能通过 `PATH` 环境变量检索到 `Il2CppDumper.dll` 动态库文件。
	
	> 该程序仅支持 `Windows` 、`Linux` 、`Macintosh` 系统。
