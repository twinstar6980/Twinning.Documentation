# Installation

- [Platform supportability](#Platform-supportability)

- [Module types](#Module-Types)

- [Installation steps](#Installation-Steps)

- [External programs](#External-Programs)

## Platform supportability

* Operating systems: `Windows 10+` 、`Linux ~` 、`Macintosh 11~` 、`Android 9+` 、`Iphone 14~`.

* Processor architectures: `x86_64`, `arm_64`.

## Module types

The tool consists of several modules, different modules provide different functions:

* `Kernel`
	
	Kernel, responsible for the implementation of internal functions.
	
	Required modules, distributed as an dynamic library.

* `Shell`
	
	Shell, provides the basic user interface.
	
	Optional modules, distributed as executable programs.
	
	> Using `Shell` on `Android` and `Iphone` requires ROOT privileges, if your device is not ROOTed, use `Assistant`.

* `Script`
	
	Script, controls the workflow of the tool.
	
	Required module, distributed as a script package.

* `Assistant`
	
	Assistant, implements additional advanced helper functions.
	
	Optional module, distributed as an application installation package.

* `Assistant Plus`
	
	Assistant+, specialized version for Windows platform.
	
	Optional module, distributed as an application installation package.
	
	> This module is only avaliable for `Windows` system.

## Installation steps

You can clone this project and compile it, or just download the bundle package of this project for distribution.

1. Download and unzip the bundle package.
	
	View the [Release](https://github.com/twinstar6980/Twinning.Documentation/releases/tag/Latest) page, the bundle distribution is named `<version>.<system>.<architecture>.bundle.zip`, where `system` represents the adapted operating system and `architecture` represents the adapted CPU architecture.
	
	> Only the latest bundle is saved in the Release page. If you need historical versions, you can found them on my [Personal OneDrive](https://1drv.ms/f/s!Au1005zrHzItgQSJKEhvbBskx5mz?e=VcmdOo).
	
	Currently, the platforms that provide precompiled distribution are as follows:
	
	* `Windows` - `x86_64`
	
	* `Macintosh` - `x86_64`
	
	* `Linux` - `x86_64`
	
	* `Android` - `arm_64`
	
	* `Iphone` - `arm_64`

2. Select the home directory.
	
	Move the unzipped directory to a suitable location. It will serve as the home directory of the toolkit. recorded the absolute path as `<home>`.

3. Sign the `kernel` and `shell` files in the home directory.
	
	> If you don't need the `Shell` module, you can skip this step. \
	> This step only required for `Iphone` users.
	
	Run these command in the system terminal: `> ldid -S ./kernel` & `> ldid -S ./shell` .

4. Grant executable permissions to the `shell` file in the home directory.
	
	> If you don't need the `Shell` module, you can skip this step. \
	> This step only required for `Linux`, `Macintosh`, `Android`, `Iphone` users.
	
	Run these command in the system terminal: `> chmod +x ./shell` .

5. Install the C++ shared library for your system.
	
	> If you don't need the `Shell` module, you can skip this step. \
	> This step only required for `Android` user.
	
	Copy the `libc++_shared.so` file in the home directory to the system library directory `/system/lib64` .

6. Install the application installation packages of the `Assistant` and `Assistant Plus` modules in the home directory.
	
	Application installation package files have extensions such as `msix`, `dmg`, `apk`, `ipa`, etc.
	
	> @ `Windows` \
	> You need to trust the signing certificate in MSIX before installation. \
	> Right-click the properties of `.msix`, switch to the ⌈Digital Signatures ⌋ page, select the first item in the list, and then click ⌈Details ⌋ , and in the pop-up window, select ⌈View Certificate⌋ - ⌈Install Certificate⌋ - ⌈Local machine⌋ - ⌈Place all certificates in the following storage⌋ - ⌈Trusted persons⌋, to complete the installation of the certificates.
	> 
	> @ `Iphone` \
	> Need to self-sign and install `ipa` through AltStore or other tools.

7. Configure the `Assistant` settings.
	
	> If you don't need the `Assistant` module, you can skip this step.
	
	Open the `Assistant` application, click the gear icon to the right of each module list item on the page to open the module settings dialog box and edit the following settings:
	
	* `Storage Permission` Click and grant storage read and write permissions to the app.
		
		> This is only available on `Android`.
	
	* `Fallback Directory` = `<home>/workspace` .
		
		> This is only available on `Android`.
	
	* `Modding Worker` - `Kernel` = `<home>/kernel`.
	
	* `Modding Worker` - `Script` = `<home>/script/main.js`.
	
	* `Modding Worker` - `Argument` = `<home>`.
	
	* `Command Sender` - `Method Configuration` = `<home>/assistant/method_configuration.json` .
	
	* `Resource Shipper` - `Option Configuration` = `<home>/assistant/option_configuration.json` 。
	
	> `<home>` in the above settings needs to be replaced with the absolute path of the home directory.

8. Enable the `Assistant` forwarder extension.
	
	> If you don't need the `Assistant` module, you can skip this step. \
	> This step only required for `Windows`, `Macintosh`, `Android`, `Iphone` user.
	
	`Assistant` provides an optional forwarder extension that provides a quick option to forward files and directories to applications in the system file manager.
	
	* `Windows`: The forwarder extension is disabled by default. If you need to enable it, please create a blank `forwarder` file in the `%APPDATA%/TwinStar.Twinning.Assistant` directory.
	
	* `Macintosh`: The forwarder extension is disabled by default. If you need to enable it, open `Assistant` and close it, then open ⌈ System Settings ⌋ - ⌈ Privacy & Security ⌋ - ⌈ Extensions ⌋ - ⌈ Added Extensions ⌋ - ⌈ Twinning Assistant ⌋ and check ⌈ Finder Extension ⌋ .
	
	* `Android`: The forwarder extension is always enabled.
	
	* `Iphone`: The forwarder extension is disabled by default. If you need to enable it, open ⌈ Files ⌋ , select any file, click ⌈ Share ⌋ - ⌈ Edit Actions... ⌋ - ⌈ Twinning Assistant ⌋ , and check the switch button on the right.

9. Configure the `Assistant Plus` settings.
	
	> If you don't need the `Assistant Plus` module, you can skip this step.
	
	Open the `Assistant Plus` application, click the gear icon to the right of each module list item on the page to open the module settings dialog box and edit the following settings:
	
	* `Modding Worker` - `Kernel` = `<home>/kernel`.
	
	* `Modding Worker` - `Script` = `<home>/script/main.js`.
	
	* `Modding Worker` - `Argument` = `<home>`.
	
	* `Command Sender` - `Method Configuration` = `<home>/assistant_plus/MethodConfiguration.json`.
	
	* `Resource Shipper` - `Option Configuration` = `<home>/assistant_plus/OptionConfiguration.json`.
	
	> `<home>` in the above settings needs to be replaced with the absolute path of the home directory.

10. Set the script's interactive language.
	
	Open and edit `script/Entry/Entry.json` file in the home directory as text, find the `"language": "English"` section, and modify it to switch the interactive language of the tool.
	
	* `English` (default)
	
	* `Chinese`
	
	* `Vietnamese`

11. At this point, all the installation steps have been completed and you can use the tool through the terminal or GUI application.

## External programs

Some methods of the tool require calling external programs, which need to be downloaded and installed by users.

* [adb](https://developer.android.com/studio/releases/platform-tools)
	
	Used for **Remote Android Helper**.
	
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
