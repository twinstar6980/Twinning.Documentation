# Installation

- [Platform supportability](#Platform-supportability)

- [Module types](#Module-Types)

- [Installation steps](#Installation-Steps)

- [External programs](#External-Programs)

## Platform supportability

* Operating systems: `Windows 7+`, `Linux ~ `, `Macintosh 12~`, `Android 9+`, `iPhone 16~`.

* Processor architectures: `x86 32|64`, `arm 32|64`.

## Module types

The tool consists of several modules, different modules provide different functions:

* `Kernel`
	
	Kernel, responsible for the implementation of internal functions.
	
	Required modules, distributed as an dynamic library.

* `Shell CLI`
	
	Shell, provides the command line interface.
	
	Optional modules, distributed as executable programs.
	
	> Using `Shell CLI` on `Android` and `iPhone` requires ROOT privileges, if your device is not ROOTed, use `Shell GUI`.

* `Shell GUI`
	
	Shell, provides the graphical interface.
	
	Optional module, distributed as an application installation package.

* `Script`
	
	Script, controls the workflow of the tool.
	
	Required module, distributed as a script package.

* `Forwarder For Windows`
	
	Forwarder, which enables users to forward files to tools via the Windows Explorer right-click menu.
	
	Optional module, distributed as an application installation package.
	
	> This module is only avaliable for `Windows` systems.

* `Forwarder For Macintosh`
	
	Forwarder, which enables users to forward files to tools via the Macintosh Finder right-click menu.
	
	Optional module, distributed as an application installation package.
	
	> This module is only avaliable for `Macintosh` system.

* `Forwarder For Android`
	
	Forwarder, which enables users to forward files to tools via the Android file sharing.
	
	Optional module, distributed as an application installation package.
	
	> This module is only avaliable for `Android` system.

* `Helper`
	
	Helper, which provides additional advanced functions.
	
	Optional module, distributed as an application installation package.
	
	> This module is only avaliable for `Windows` system.

## Installation steps

You can clone this project and compile it, or just download the bundle package of this project for distribution.

> Only bundle are distributed in Release page. If you only need individual modules, you can download them in my [Personal OneDrive](https://1drv.ms/f/s!AkIzoME-1oU-fB6En185husw59Q?e=EKJ1e9), which also provides downloads of historical versions.

1. Download and unzip the bundle package.
	
	View the [Release](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Latest) page, the bundle distribution is named `<version>.<system>.<architecture>.bundle.zip`, where `system` represents the adapted operating system and `architecture` represents the adapted CPU architecture.
	
	> Only the latest bundle is saved in the Release page. If you need historical versions, you can found them on my [Personal OneDrive](https://1drv.ms/f/s!AkIzoME-1oU-fB6En185husw59Q?e=EKJ1e9).
	
	Currently, the platforms that provide precompiled distribution are as follows:
	
	* `Windows` - `x86_64`
	
	* `Android` - `arm_64`
	
	* `Macintosh` - `x86_64` `arm_64`
	
	* `iPhone` - `arm_64`
	
	* `Linux` - `x86_64`
	
	Among them, only the precompiled distributions for `Windows` and `Android` platforms will always follow the latest version. Precompiled distributions for other platforms are generally rarely updated. If you need the latest version, or your platform is not in the above list, please try to compile it by yourself.

2. Select the home directory.
	
	Move the unzipped directory to a suitable location. It will serve as the home directory of the toolkit. recorded the absolute path as `<home>`.

3. Sign the `kernel` and `shell_cli` files in the home directory.
	
	> This step only required for `iPhone` users, and need to operate on `Macintosh` device.
	
	Run these command in the system terminal: `> codesign -s <certificate-name> kernel` & `> codesign -s <certificate-name> shell_cli` .

4. Grant executable permissions to the `shell_cli` file in the home directory.
	
	> If you don't need the `Shell CLI` module, you can skip this step. \
	> This step only required for `Linux`, `Macintosh`, `Android`, `iPhone` users.
	
	Run these command in the system terminal: `> chmod +x shell_cli` .

5. Install the C++ shared library for your system.
	
	> If you don't need the `Shell CLI` module, you can skip this step. \
	> This step only required for `Android` user.
	
	Copy the `libc++_shared.so` file in the home directory to the system library directory `/system/lib64` .

6. Install the application installation packages of the `Shell GUI`, `Forward`, and `Helper` modules in the home directory.
	
	Application installation package files have extensions such as `msix`, `dmg`, `apk`, `ipa`, etc.
	
	> @ `Windows` \
	> You need to trust the signing certificate in MSIX before installation. \
	> Right-click the properties of `.msix`, switch to the ⌈Digital Signatures ⌋ page, select the first item in the list, and then click ⌈Details ⌋ , and in the pop-up window, select ⌈View Certificate⌋ - ⌈Install Certificate⌋ - ⌈Local machine⌋ - ⌈Place all certificates in the following storage⌋ - ⌈Trusted persons⌋, to complete the installation of the certificates.
	> 
	> @ `Macintosh` \
	> After mounting `dmg`, move the `*.app` directory to the system application directory.
	> 
	> @ `iPhone` \
	> Need to self-sign and install ipa through AltStore or other tools.

7. Configure the `Shell GUI` settings.
	
	> If you do not need the `Shell GUI` module, you can skip this step.
	
	Open the newly installed `Shell GUI` application, click the `Setting` button in the lower right corner to enter the settings page, and edit the following setting items:
	
	* `Kernel` = `<home>/kernel`.
	
	* `Script` = `<home>/script/main.js`.
	
	* `Argument` = `<home>` .
	
	* `Fallback Directory` = `<home>/workspace`.
		
		> This item is only available on `Android` and `iPhone`.
	
	* `Storage Permission` Click and grant storage space read and write permissions to the application.
		
		> This is only available on `Android`.
	
	> `<home>` in the above settings needs to be replaced with the absolute path of the home directory.
8. Configure the `Forwarder For Windows` setting.
	
	> If you do not need the `Forwarder For Windows` module, you can skip this step.
	
	Open the newly installed `Forwarder For Windows` application, and a directory window will pop up with a file named `forward.cmd`. This script is responsible for receiving the file path parameter and launching the tool. Open and edit the file as text.
	
	The following example forward arguments to `Shell CLI` :
	
	```cmd
	@echo off
	set home=C:\TwinStar.ToolKit
	start "" "%home%\shell_cli.exe" ^
		"%home%\kernel" ^
		"%home%\script\main.js" ^
		"%home%" ^
		%*
	```
	
	The following example forward arguments to `Shell GUI` :
	
	```cmd
	@echo off
	start "" "C:\Program Files\WindowsApps\TwinStar.ToolKit.ShellGUI_28.0.0.0_x64__7qfdsg797hj0p\shell_gui.exe" ^
		-additional_argument %*
	```
	
	The following example forward arguments to `Helper` - `Modding Worker` :
	
	```cmd
	@echo off
	start "" "C:\Program Files\WindowsApps\TwinStar.ToolKit.Helper_20.0.0.0_x64__7qfdsg797hj0p\Helper.exe" ^
		-WindowSize        496 968 ^
		-WindowAlwaysOnTop true ^
		-ModuleType        ModdingWorker ^
		-ModuleOption ^
			-ImmediateLaunch    true ^
			-AdditionalArgument %*
	```
	
	The following example forward arguments to `Helper` - `Resource Forwarder` :
	
	```cmd
	@echo off
	start "" "C:\Program Files\WindowsApps\TwinStar.ToolKit.Helper_20.0.0.0_x64__7qfdsg797hj0p\Helper.exe" ^
		-WindowSize        496 968 ^
		-WindowAlwaysOnTop true ^
		-ModuleType        ResourceForwarder ^
		-ModuleOption ^
			-AutomaticClose true ^
			-Input          %*
	```

9. Configure the `Forwarder For Macintosh` setting.
	
	> If you do not need the `Forwarder For Macintosh` module, you can skip this step.
	
	Go to ⌈ System Settings ⌋ - ⌈ Privacy and Security ⌋ - ⌈ Extensions ⌋ - ⌈ Added Extensions ⌋ - ⌈ TwinStar ToolKit - Forwarder ⌋ and check the ⌈ "Finder" extension ⌋ to ensure that the application can take effect.
	
	Open the newly installed `Forwarder For Macintosh` application, and a directory window will pop up with a file named `forward.sh`. This script is responsible for receiving the file path parameter and launching the tool. Open and edit the file as text.
	
	> Note: The script is executed in the application's sandbox environment.
	
	The following example forward arguments to `Shell GUI` :
	
	```sh
	#!/bin/bash
	"/Applications/TwinStar ToolKit - Shell GUI.app/Contents/MacOS/TwinStar ToolKit - Shell GUI" \
		-additional_argument "$@"
	```
10. Configure the `Helper` settings.
	
	> If you don't need the `Helper` module, you can skip this step.
	
	Open the newly installed `Helper` application, click the gear icon on the upper right side of each module button on the page to open the module settings dialog and edit the following settings:
	
	* `Modding Worker` - `Kernel` = `<home>/kernel`.
	
	* `Modding Worker` - `Script` = `<home>/script/main.js`.
	
	* `Modding Worker` - `Argument` = `<home>`.
	
	* `Resource Forwarder` - `Option Configuration` = `<home>/helper/OptionConfiguration.json`.
	
	* `Command Sender` - `Method Configuration` = `<home>/helper/MethodConfiguration.json`.
	
	> `<home>` in the above settings needs to be replaced with the absolute path of the home directory.

11. Set the interactive language.
	
	Open and edit `script/Entry/Entry.json` file in the home directory as text, find the `"language": "English"` section, and modify it to switch the interactive language of the tool.
	
	* `English` (default)
	
	* `Chinese`
	
	* `Vietnamese`

12. At this point, all installation steps have been completed and the tool can be launched from the terminal command line or by opening the application directly.
	
	> @ `Windows` \
	> You can launch the tool directly by double-clicking on the launch script `launch*.cmd` in the home directory, or by dragging and dropping a file object onto it and releasing it.\
	> If the `Forwarder` module is installed, you can see `⌈ TwinStar ToolKit - Forwarder ⌋` in the right-click menu item of any file or directory, and use it to quickly forward file objects to the tool.\
	> If the `Helper` module is installed, you can use the `Resource Forwarder` in it for faster forwarding, or you can use the `Command Sender` to visually select the desired method and edit the arguments.
	
	> @ `Macintosh` \
	> If the `Forwarder` module is installed, you can see `⌈ TwinStar ToolKit - Forwarder ⌋` in the right-click menu item of any file or directory, and use it to quickly forward file objects to the tool.
	
	> @ `Android` \
	> If the `Forwarder` module is installed, you can see `⌈ TwinStar ToolKit - Forwarder ⌋` can in the file sharing list of the system or third-party file manager, and through which file objects can be quickly forwarded to the tool.\
	> Due to the limitations of the Android system, the forwarder cannot directly obtain the absolute path of the forwarded file, but passes the Content URI of the forwarded file to the Shell GUI as a command argument for its startup, and the Shell GUI will try to parse the Content URI. See [Android Content URI processing strategy](./question.md#Android-Content-URI-processing-strategy) for details.

## External programs

Some methods of the tool require calling external programs, which need to be downloaded and installed by users.

* [adb](https://developer.android.com/studio/releases/platform-tools)
	
	For **Remote Android Helper**.
	
	Install and configure the `PATH` environment variable to ensure that the tool can retrieve the `adb` executable through the `PATH` environment variable.
	
	> This program only supports `Windows` and `Linux` and `Macintosh` systems.

* [vgmstream-cli](https://vgmstream.org/)
	
	Used for **WEM audio decode**.
	
	Install and configure the `PATH` environment variable to ensure that the tool can retrieve the `vgmstream-cli` executable through the `PATH` environment variable.
	
	> This program only supports `Windows` and `Linux` and `Macintosh` systems.

* [WwiseConsole](https://www.audiokinetic.com/en/download) `=2019.2`
	
	Used for **WEM audio encode**.
	
	Install and configure the `PATH` environment variable to ensure that the tool can retrieve the `WwiseConsole.exe` or `WwiseConsole.sh` executable through the `PATH` environment variable.
	
	> For the path of the `WwiseConsole` executable program, see [Official Documentation](https://www.audiokinetic.com/zh/library/edge/?source=SDK&id=bankscommandline.html).
	
	> This program only supports `Windows` and `Macintosh` systems.

* [QuickTime](https://support.apple.com/kb/DL837) `>7.6`
	
	Used for **WEM audio encode**.
	
	If you need to encode `WEM` in `AAC` format, please make sure that `QuickTime` is correctly installed on the system. This program will be called by `WwiseConsole`.

* [Il2CppDumper](https://github.com/Perfare/Il2CppDumper) `=6.7.40,x86`
	
	Used for **Kairosoft game program modify** 。
	
	Install and configure the `PATH` environment variable to ensure that the tool can retrieve the `Il2CppDumper-x86` executable through the `PATH` environment variable.
	
	> This program only supports `Windows` systems.
