# Installation

- [Platform](#Platform)

- [Module](#Module)

- [Preparation](#Preparation)

- [Compile or download binary distribution](#Compile-or-download-binary-distribution)

- [Use releases](#Use-releases)

- [Install `Kernel`](#Install-Kernel)

- [Install `Shell`](#Install-Shell)

- [Install `Shell GUI`](#Install-Shell-GUI)

- [Install `Script`](#Install-Script)

- [Install `Forwarder For Windows`](#Install-Forwarder-For-Windows)

- [Install `Forwarder For Macintosh`](#Install-Forwarder-For-Macintosh)

- [Install `Forwarder For Android`](#Install-Forwarder-For-Android)

- [Install `Helper`](#Install-Helper)

- [External Programs](#External-Program)

- [Languages](#Languages)

## Platform

- Operating systems: `Windows 7+`, `Linux ~ `, `Macintosh 12~`, `Android 9+`, `iPhone 16~`.

- Processor architectures: `x86 32|64`, `arm 32|64`.

## Module

The tool consists of several modules, different modules provide different functions:

- `Kernel`

	Kernel, responsible for the implementation of internal functions.

- `Shell`

	Shell, provides the command line interface.

- `Shell GUI`

	Shell, provides the graphical interface.

- `Script`

	Script, controls the workflow of the tool.

- `Forwarder For Windows`

	Forwarder, which enables users to forward files to tools via the Windows Explorer right-click menu.

- `Forwarder For Macintosh`

	Forwarder, which enables users to forward files to tools via the Macintosh Finder right-click menu.

- `Forwarder For Android`

	Forwarder, which enables users to forward files to tools via the Android file sharing.

- `Helper`

	Helper, which provides additional advanced functions.

## Preparation

Before installing each component, you need to do some preparations:

1. Locate **home directory**

	Create an empty directory in the storage space which will hold all the files needed to run the tool, called the home directory.

	`Kernel` , `Shell` , `Script` are portable, so place them in the home directory so that no data remains in other locations.

	`Shell GUI` , `Forwarder For Windows` , `Helper` are applications that need to be installed and need to be uninstalled by the user and cleared of application data.

	> The location of the home directory can be optional, but make sure the user has **read and write allows permission** to the directory and its contents.

## Compile or download binary distributions

You can clone this project and compile it yourself. Pre-compiled binary distribution is also available in [Release](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Latest).

> Only the binary distributions for `windows x86_64` and `android arm_64` are provided in the Release, so please clone and compile the project yourself if you need it for other platforms.

> If you need only individual modules, you can find them on my [Personal Onedrive](https://1drv.ms/f/s!AkIzoME-1oU-fB6En185husw59Q?e=EKJ1e9), which contains all the history of this tool.

## Use Bundle Package

The bundle package is already organized the `Kernel`, `Shell`, `Script` and script for launch `Shell`, so it is not necessary to follow the detailed steps below to install the releases.

1. Check [Release](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Latest) and download the distribution for your device.

2. Uncompress the downloaded zip file to the home directory.

	> @ `Android` \
	> You also need to do the following additional configuration: Install **C++ shared libraries** to the system library directory. \
	> See below for the detailed steps.
	>
	> @ `iPhone` \
	> You will also need to do the following additional configuration: sign `kernel` and `shell`. \
	> See below for details.

3. Execute the `launch.[sh|cmd]` scripts to launch the program, the tool should start and run properly.

	> @ `Windows` \
	> Either double-click `launch.cmd` directly to run it, or drag and drop the file object onto it and release it.

The bundle package also contains installion package for `Shell GUI`, `Forwarder` modules, please refer below to install them manually if you need them.

## Install `Kernel`

Kernel, responsible for the implementation of internal functions.

This is a required installation and is distributed as a dynamic library.

1. Compile or download the distribution for your device.

2. Move the resulting file to the home directory, then rename it to `kernel`.

> You have to choose the right `kernel` for your device and rename to `kernel`, otherwise the tool won't be able to launch correctly.

## Install `Shell`

Shell, provides the command line interface.

This is an optional installation, distributed as an executable.

> @ `Android` `iPhone` \
> Using `Shell` on Android and iPhone requires ROOT or JAILBREAK permission, if your device does not get ROOT, please use `Shell GUI`.

1. Compile or download the distribution for your device.

2. Move the resulting file to the home directory, then rename it to `shell`.

> @ `Windows` \
> Windows does not support calling executables without the `.exe` extension non-programmatically, so you need to run `> mklink shell.exe shell` in the terminal to create a soft link `- shell.exe` to `- shell`; or just rename `- shell` to `- shell.exe`.

3. Give `shell` executable privileges.

	\*\*Users on other systems may have to do `> chmod +x shell` in the terminal to add executable permissions to `- shell`.

For `Android` systems, you also need to perform the following actions:

1. Install **C++ shared library** to the system library directory.

	For security and size reduction reasons, the C++ runtime used is c++_shared. Therefore, you also need to copy the `libc++_shared.so` file of the corresponding processor architecture to the system library directory.

	> The system library directory is `+ /system/lib` on 32-bit systems and `+ /system/lib64` on 64-bit systems.
	> This file can be extracted from the Android NDK toolchain.

For `iPhone` systems, you also need to do the following:

1. sign `kernel` and `shell`.

	Running executables on the iPhone requires signature verification, and the distributed program files (**kernel | shell**) are not signed and need to be signed by the user in order to run on the iPhone.

	> You can use the **codesign** tool on the **Macintosh** to do this.

## Install `Shell GUI`

Shell, provides the graphical interface.

This is an optional installation item, distributed as an application package.

1. Compile or download the distribution for your device.

	> Currently, only `Windows X86-64` and `Android ARM-64` distributions are available. Users of other platforms need to clone this project and build and sign the application package by themselves.

2. Install the application package.

	> @ `Windows` \
	> You need to trust the signing certificate in MSIX before installation. \
	> Right-click the properties of `.msix`, switch to the ⌈Digital Signatures ⌋ page, select the first item in the list, and then click ⌈Details ⌋ , and in the pop-up window, select ⌈View Certificate⌋ - ⌈Install Certificate⌋ - ⌈Local machine⌋ - ⌈Place all certificates in the following storage⌋ - ⌈Trusted persons⌋, to complete the installation of the certificates.

For `Android` systems, you also need to perform the following actions:

1. Open the application, click ⌈ Setting ⌋ at the bottom right to enter the setting page, and click ⌈ Storage Permission ⌋ to request the storage permission.

	If the application does not obtain this permission, it will only be able to access the application's special storage, the device's shared external storage is not accessible.

## Install `Script`

Script, controls the workflow of the tool.

This is a required installation item, issued as a script package.

1. Compile or download the distribution for your device.

2. Move the resulting file to the home directory, then create a `script` directory, and uncompress all the contents of the zip package to it.

## Install `Forwarder For Windows`

Forwarder, which enables users to forward files to tools via the Windows Explorer right-click menu.

> This module is dedicated to Windows systems and does not available for other systems.

This is an optional installation and is distributed as an application package.

1. Compile or download the distribution for your device.

2. Install the application package.

	> You need to trust the signing certificate in MSIX before installation. \
	> Right-click to view the properties of `.msix`, switch to the ⌈Digital Signatures⌋ page, select the first item in the list, then click ⌈Details⌋, and in the pop-up window, select ⌈View Certificate⌋ - ⌈Install Certificate⌋ - ⌈Local machine⌋ - ⌈Place all certificates in the following storage⌋ - ⌈Trusted persons⌋ to complete the installation of the certificate.

3. Start the application, and a Explorer window will pop up after running successfully, it pointing to the application's private data directory, which contains a script file named `forward.cmd`, open and edit this file in text form.

	Every time the user selects the extension menu item of the module, the module will run this script with the path of the selected file as the arguments, and the script is responsible for forwarding the received arguments to the toolkit.

	Here's an example that forward arguments to the `launch.cmd` and `Helper`'s quick forwarder window :

	```cmd
	@echo off
	"C:\TwinStar.ToolKit\launch.cmd" %*
	"C:\Program Files\WindowsApps\TwinStar.ToolKit.Helper_14.0.0.0_x64__7qfdsg797hj0p\Helper.exe" ^
		-WindowSize        480 960 ^
		-WindowAlwaysOnTop true ^
		-ModuleType        CommandForwarderQuick ^
		-ModuleOption ^
			-AutomaticClose  true ^
			-ParallelExecute false ^
			-EnableBatch     false ^
			-EnableFilter    true ^
			-RemainInput     false ^
			-Input           %*
	```

4. You can now see the `⌈ TwinStar ToolKit - Extension ⌋` option inside the context menu of any file or directory, and use these options to quickly forward file objects to the tool.

	If you do not see this option, try restarting Explorer `explorer.exe`, or restarting your computer.

## Install `Forwarder For Macintosh`

Forwarder, which enables users to forward files to tools via the Macintosh Finder right-click menu.

> This module is dedicated to Macintosh systems and is not available for other systems.

This is an optional installation item, distributed as an application package.

1. Compile or download the distribution for your device.

2. Install the application package.

3. Start the application, and a Finder window will pop up after running successfully, it pointing to the application's private data directory, which contains a script file named `forward.sh`, open and edit this file in text form.

	Every time the user selects the extension menu item of the module, the module will run this script with the path of the selected file as the arguments, and the script is responsible for forwarding the received arguments to the toolkit; but pay attention, it should be noted that the execution of the script is in the sandbox environment of the application.

	Here's an example that forward arguments to the `Shell GUI` :

	```sh
	#! /bin/bash
	"/Applications/TwinStar ToolKit - Shell GUI.app/Contents/MacOS/TwinStar ToolKit - Shell GUI" \
		"-additional" \
		"$@"
	```
	
	> `Helper.exe`'s path should search from your device.

4. Go to ⌈ System Settings ⌋ - ⌈ Privacy and Security ⌋ - ⌈ Extensions ⌋ - ⌈ Added Extensions ⌋ - ⌈ TwinStar ToolKit - Forwarder ⌋ and check one of the ⌈ "Finder" extension ⌋.

5. Now you can see ⌈ TwinStar ToolKit - Forwarder ⌋ in the right-click menu item of any file or directory and use it to quickly forward file objects to the tool.
	
	If you do not see this option, try re-checking the extensions settings, or restarting your computer.

## Install `Forwarder For Android`

Forwarder, which enables users to forward files to tools via the Android file sharing.

> This module is dedicated to Android and is not available for other systems.

This is an optional installation and is distributed as an application package.

1. Compile or download the distribution for your device.

2. Install the application package.

3. Now you can see ⌈ TwinStar ToolKit - Forwarder ⌋ within the file sharing target of other third-party file managers and use it to quickly forward file objects to the tool.

> Due to the limitations of the Android system, the forwarder cannot directly obtain the absolute path of the forwarded file, but passes the Content URI of the forwarded file to the Shell GUI as a command argument for its startup, and the Shell GUI will try to parse the Content URI. See [Android Content URI processing strategy](./question.md#Android-Content-URI-processing-strategy) for details.

## Install `Helper`

Helper, which provides additional advanced functions.

> This module is dedicated to Windows systems and is not available for other systems.

This is an optional installation item, distributed as an application package.

1. Compile or download the distribution for your device.

2. Install the application package.

	> You need to trust the signing certificate in MSIX before installation. \
	> Right-click the properties of `.msix`, switch to the ⌈Digital Signatures⌋ page, select the first item in the list, and then click ⌈Details⌋ , and in the pop-up window, select ⌈View Certificate ⌋ - ⌈Install Certificate ⌋ - ⌈Local machine⌋ - ⌈Place all certificates in the following storage⌋ - ⌈ Trusted persons ⌋, to complete the installation of the certificates.

## External-Program

Some functions of the tool need to call external programs, so the users need to download and install these programs by themselves, and place them in the `external` directory under the home directory.

- [ffmpeg](https://ffmpeg.org/download.html)

	Used for **WEM audio decoding**.

	After downloading, rename the file `ffmpeg[.exe]` to `ffmpeg` and place it in the `+ <home>/external/ffmpeg` directory.

- [ww2ogg](https://github.com/hcs64/ww2ogg/releases/tag/0.24)

	For **WEM audio decoding** .

	After downloading, rename the file `ww2ogg[.exe]` to `ww2ogg` and place it in the `+ <home>/external/ww2ogg` directory.

	Then place the file `packed_codebooks_aoTuV_603.bin` in that directory.

	> The author of this project only distributes the executable for Windows, users of other systems must download the source code and build it themselves.

- [adb](https://developer.android.com/studio/command-line/adb)

	For **Remote-Android-Helper** .

	Download [`Android SDK Platform Tools`](https://developer.android.com/studio/releases/platform-tools), unzip it and configure it in the `PATH` environment variable.

## Languages

The tool provides multi-language support, currently supports Chinese and English.

By default, the tool uses Chinese, if you need to switch to another language, you should modify the configuration file as follows:

1. Go to the tool's home directory, find the `- <home>/script/Entry/Entry.json` file and open it with a text editor.

2. The second line in the text is `"language": "Chinese"`, which means the current interaction language is Chinese, if you want to switch to English, please change `Chinese` to `English`.

> The multilingual text is defined in `- <home>/script/Language/<Language-ID>.json`, if you need to fix text errors or add support for other languages, please modify this file; please also submit a pull request to contribute to this project if you can.
