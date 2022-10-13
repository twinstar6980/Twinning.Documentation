# 安装

- [平台支持](#%E5%B9%B3%E5%8F%B0%E6%94%AF%E6%8C%81)

- [模块组成](#%E6%A8%A1%E5%9D%97%E7%BB%84%E6%88%90)

- [必要安装](#%E5%BF%85%E8%A6%81%E5%AE%89%E8%A3%85)

- [外部程序](#%E5%A4%96%E9%83%A8%E7%A8%8B%E5%BA%8F)

- [Windows 资源管理器拓展](#windows-%E8%B5%84%E6%BA%90%E7%AE%A1%E7%90%86%E5%99%A8%E6%8B%93%E5%B1%95)

- [多语言支持](#%E5%A4%9A%E8%AF%AD%E8%A8%80%E6%94%AF%E6%8C%81)

- [对宿主终端的要求](#%E5%AF%B9%E5%AE%BF%E4%B8%BB%E7%BB%88%E7%AB%AF%E7%9A%84%E8%A6%81%E6%B1%82)

## 平台支持

* 操作系统
	
	* Windows | Linux | MacOS
	
	* Android | iOS

* 处理器架构
	
	* x86_32 | x86_64
	
	* arm_32 | arm_64

> 对于 Windows | Linux | MacOS ，只提供 x86_64 处理器架构的分发；\
> 对于 Android | iOS ，只提供 arm_64 处理器架构的分发；\
> 如有面向其他平台的需要，请自行克隆并编译本项目。
> 
> @ Windows \
> 仅支持 Windows 7 及之后的版本。
> 
> @ Android \
> 仅支持 Android 9 及之后的版本。
> 
> @ Android | iOS \
> 由于目前仅提供基于终端的交互程序，因此需要 **ROOT | 越狱** 环境。

## 模块组成

* Core
	
	核心功能的实现，以动态库形式分发。

* Shell
	
	面向用户的外壳程序，提供基于终端的命令行界面。

* Script
	
	与 Core 及 Shell 配合使用的脚本。

* Windows Explorer Extension
	
	适用于 Windows 的资源管理器拓展，将 ToolKit 集成至系统资源管理器的右键菜单中。

## 必要安装

**Core | Shell | Script** 三者是运行所需的 **必要模块** ，按照以下步骤进行安装。

1. 创建 **主目录**

	请在存储空间中创建一个空目录，它用于存放工具运行所需的所有文件，称为主目录。

	> 主目录的位置可以随意，但要确保用户有对该目录及其内容的 **读写权限** 。

	如果你想卸载本工具，只需删除主目录，工具不会有任何数据残留。

	> @ Windows \
	> 建议将主目录设为 **C:/Program Files/TwinKleS/ToolKit** 。
	> 
	> @ Linux | MacOS | Android | iOS \
	> 建议将主目录设为 **/opt/TwinKleS/ToolKit** 。
	> 
	> @ Android \
	> **sdcard 存储** 是 FUSE 文件系统，无法进行所需的文件权限修改步骤，请勿将主目录放置其中。
	
2. 安装 **Core 模块**

	查看本项目的 [Core 分发](https://github.com/TwinKleS-C/TwinKleS.ToolKit/releases/tag/Core) ，下载所需文件。

	> 由于 Core 模块是由 C++ 编写的程序，因此针对不同的平台有不同的分发。分发文件的命名规则为 **\<system>.\<arch>.\<compiler>@\<version>.core** ，请下载对应于自己设备平台的分发文件。\
	> 例如：若你的操作系统为 Windows 、处理器架构为 x86_64 ，那么就下载 windows.x86_64 开头的文件。

	下载完毕后，将该文件移动到主目录内，并重命名为 **core** 。

	> @ Android \
	> 由于 **Android 7** 引入的 **链接器命名空间** 机制，程序无法加载系统库目录与私有库目录（对于单纯的命令行程序而言，后者是不存在的）之外的动态库。因此，须将 core 文件重命名为 **TwinKleS.ToolKit.core** ，并移动到 **系统库目录** 中（对于 64 位系统，其路径为 /system/lib64 ）。
	
2. 安装 **Shell 模块**

	查看本项目的 [Shell 分发](https://github.com/TwinKleS-C/TwinKleS.ToolKit/releases/tag/Shell) ，下载所需文件。

	> 与 Core 模块的下载基本一致，区别是分发的文件名以 .shell 结尾而非 .core 结尾，不作赘述。
	
	下载完毕后，将该文件移动到主目录内，并重命名为 **shell** 。

	> 可执行程序的运行需要文件具有可执行权限，Windows 系统上一般不需额外操作，其他系统的用户请使用 **chmod +x shell** 命令为 shell 文件添加可执行权限。
	> 
	> @ Windows \
	> 请在 CMD 终端中执行 **mklink shell.exe shell** 创建一个 shell.exe 软链接文件；也可以直接复制 shell 文件并将副本重命名为 shell.exe 。

3. 安装 **Script 模块**

	查看本项目的 [Script 分发](https://github.com/TwinKleS-C/TwinKleS.ToolKit/releases/tag/Script) ，下载所需文件。

	> Script 模块只有一个公共分发，即 **common@\<version>.script.zip** 。

	下载完毕后，将该文件移动到主目录内，创建 **script** 目录，将压缩包的所有内容解压至其中。

4. 创建 **工作目录**

	请在主目录内创建名为 **workspace** 的空目录，它用于存放工具运行时可能产生的一些缓存文件，称为工作目录。
	
5. 安装 **启动脚本**

	查看本项目的 [Misc 分发](https://github.com/TwinKleS-C/TwinKleS.ToolKit/releases/tag/Misc) ，下载所需文件。

	* Windows 用户下载 **launch.bat** 。

	* Linux | MacOS | iOS 用户下载 **launch.sh** 。

	* Android 用户下载 **launch_android.sh** 。

	下载完毕后，将该文件移动到主目录内，保留原有文件名。

6. 检查 **主目录结构**

	如果操作无误，主目录内的组织结构将如下所示：
	
	```
	<home>
	  - core
	  - shell
	  + script
	      - main.js
	      - ...
	  + workspace
	  - launch.[sh|bat]
	```

7. 安装 **libc++_shared.so**

	> **此步骤只需要 Android 用户执行。**

	出于安全性与减小体积的考虑，程序使用的 C++ 运行时 为 c++_shared 。因此，用户还需要将对应处理器架构的 **libc++_shared.so** 文件复制到系统库目录中。
	
	> 该文件可从 Android NDK 工具链中提取，也可在 [Misc 分发](https://github.com/TwinKleS-C/TwinKleS.ToolKit/releases/tag/Misc) 中找到。

8. 签名 **程序文件**

	> **此步骤只需要 iOS 用户执行。**

	在 iOS 上运行可执行文件需要通过签名验证，Release 页所分发的程序文件（ **core | shell** ）未经过签名，用户需要自行对其进行签名，才能在 iOS 中正常运行。
	
	> 可以在 **MacOS** 上使用 **codesign** 工具进行签名。

9. 测试 **启动脚本**

	最后一步是测试启动脚本能否成功运行。

	Windows 用户，请 **双击 launch.bat** 以启动该脚本。

	其他系统的用户，请在终端中执行 **sh launch\[_android].sh** 。

	如果操作无误，你可以在终端中看到工具的运行界面。
	
	> 请不要直接启动 shell 程序，因为它需要用户传入一些必需参数，而启动脚本会使用自动添加启动参数。
	> 
	> @ Windows \
	> 若提示 **缺失动态库（DLL）** ，请安装 [Visual C++ 运行时组件](https://visualstudio.microsoft.com/zh-hans/downloads/#microsoft-visual-c-redistributable-for-visual-studio-2022) 。

## 外部程序

工具的某些功能需要调用外部程序，用户需自行下载并安装这些程序，并放置于主目录下的 **extern** 目录内。

* [ffmpeg](https://ffmpeg.org/download.html)

	用于 **WEM 音频解码** 。

	下载完成后，将文件 ffmpeg(.exe) 重命名为 ffmpeg 并放置于 **<home>/extern/ffmpeg** 目录下。

* [ww2ogg](https://github.com/hcs64/ww2ogg/releases/tag/0.24)

	用于 **WEM 音频解码** 。

	下载完成后，将文件 ww2ogg(.exe) 重命名为 ww2ogg 并放置于 **<home>/extern/ww2ogg** 目录下。

	再将文件 packed_codebooks_aoTuV_603.bin 放置于该目录下。

	> 该项目的作者只分发了适用于 Windows 的可执行程序，其他系统的用户须自行下载源码构建。

## Windows 资源管理器拓展

对于 Windows 系统，还可向资源管理器的右键菜单植入功能选项，只需几步鼠标点击即可完成大部分文件处理操作，这需要用户额外安装一个拓展程序。

> 该拓展要求工具的主目录为 **C:/Program Files/TwinKleS/ToolKit** ，否则将无法启动工具窗口。

1. 查看本项目的 [Windows Explorer Extension 分发](https://github.com/TwinKleS-C/TwinKleS.ToolKit/releases/tag/WindowsExplorerExtension) ，下载其中的两个文件：

	* package_\<version>_x64.cer

	* package_\<version>_x64.msixbundle

2. 双击证书文件 **.cer** ，在弹出的窗口中依次选择 ⌈ 安装证书 ⌋ - ⌈ 本地计算机 ⌋ - ⌈ 将所有证书都放入下列存储 ⌋ - ⌈ 受信任的根证书颁发机构 ⌋ ，完成证书的安装。

3. 双击安装包文件 **.msixbundle** ，点击安装即可。

4. 你可以在任意文件或目录的右键菜单内看到 ⌈ TwinKleS ⌋ 选项，它为大多数功能提供了快捷入口。如果没有看到该选项，请尝试重启资源管理器 explorer.exe ，或重启计算机。

5. 卸载拓展程序后，这些选项将不再出现。

> 右键菜单拓展在新、旧式右键菜单上的表现有所区别：\
> 在新式右键菜单（即 Windows 11 式）中，所有功能选项将合并为一个二级菜单；\
> 在旧式右键菜单（即 Windows 10 式）中，各个功能选项都单独显示为一项一级菜单，这将导致右键菜单的面积大大增加；\
> 因此，Windows 10 用户请衡量利弊以决定是否安装。
> 
> *如此设计是因为新式右键菜单不允许单个右键拓展提供二级菜单，需为拓展程序绑定多个右键拓展以达到二级菜单效果，而旧式右键菜单中单个右键拓展只能提供至多16项选项。*

## 多语言支持

工具提供多语言支持，目前支持中文与英文。

默认情况下，工具将使用中文进行交互，如果需要切换为其他语言，应修改配置文件，步骤如下：

1. 进入工具的主目录，找到 **script/Entry/Entry.json** 文件，使用文本编辑器打开。

2. 文本中的第二行为 **"language": "Chinese"** ，表示目前所使用的交互语言为中文，若需切换为英文，请将 **Chinese** 改为 **English** 。

> 多语言文本通过 **script/Language/Language.json** 定义，如果需要修正文本错误，或增加对其他语言的支持，请修改该文件（方便的话，也请提交PR为本项目做贡献）。

## 对宿主终端的要求

Shell 模块提供了基于终端的命令行界面，但需要宿主终端支持以下特性：

1. UTF-8 输入/输出：必需，若不支持，将导致程序无法正常进行输入输出。

2. 虚拟终端控制序列：可选，若不支持，将导致程序无法对不同类型的文本修饰以不同的颜色。

3. 完备的字体：可选，若不支持，一些字符（如汉字、emoji ）将无法正常显示。

有些操作系统的默认终端不提供（或默认关闭）这些支持，需要用户安装第三方终端并在其中运行本程序，可以参照以下列表：

* Windows 7 ~ 8

	自带 cmd 不提供支持，推荐使用 [cmder](https://cmder.app/) 。

* Windows 10

	自带 cmd 默认关闭支持，推荐使用 [Windows Terminal](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701?hl=en-us&gl=us) 。

* Windows 11

	自带 cmd 默认关闭支持，自带 Windows Terminal 提供支持，请将其设置为系统的默认终端。

* MacOS Monterey+

	自带终端提供支持。

* Linux - Ubuntu 20.04+

	自带终端提供支持。

* Android

	推荐使用 [Termux](https://termux.dev/en/) 。

	> 当然，你也可以通过 ADB 来使用桌面系统的终端。

> @ Windows \
> 双击以运行启动脚本时，将在系统默认终端中运行程序，若想更改为在指定第三方终端中运行，请修改 **launch.bat** ，在第六行最开始添加对应终端的启动命令，如 **wt** ，但这会导致一次 cmd 窗口闪烁，除非你使用 Windows 11 并将 Windows Terminal 设为系统默认终端。
