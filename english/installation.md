# Installation

- [Platform](#Platform)

- [Module](#Module)

- [Preparation](#Preparation)

- [Script-Usage](#Script-Usage)

- [Install `Core`](#Install-Core)

- [Install `Shell`](#Install-Shell)

- [Install `Shell GUI`](#Install-Shell-GUI)

- [Install `Script`](#Install-Script)

- [Install `Windows Explorer Extension`](#Install-Windows-Explorer-Extension)

- [Install `Helper`](#Install-Helper)

- [External Programs](#External-Program)

- [Languages](#Languages)

## Platform

- Operating systems: `Windows 7+`, `Linux ? `, `Macintosh 13.1~`, `Android 9~`, `iPhone 16.2~`.

- Processor architectures: `x86 32|64`, `arm 32|64`.

> For `Windows`, `Linux`, `Macintosh`, only `x86_64` is provided for distribution; for `Android`, `iPhone`, only `arm_64` is provided for distribution; if you have needs for other platforms, you need to clone and compile this project by yourself.

## Module

The tool consists of several modules, different modules provide different functions:

- `Core`

  Core, responsible for the implementation of internal functions.

- `Shell`

  Shell, which provides the command line interface.

- `Shell GUI`

  Shell, providing the graphical interface.

- `Script`

  Script that controls the flow of the tool.

- `Windows Explorer Extension`

  Windows Explorer extension that integrates the tool into the Windows Explorer right-click menu.

- `Helper`

  Helper, which provides additional advanced functionality.

## Preparation

Before installing each component, you need to do some preparations:

1. Locate **home directory**

   Create an empty directory in the storage space which will hold all the files needed to run the tool, called the home directory.

   `Core` 、`Shell` 、`Script` are portable, so place them in the home directory so that no data remains in other locations.

   `Shell GUI` 、`Windows Explorer Extension` 、`Helper` are applications that need to be installed and need to be uninstalled by the user and cleared of application data.

   > The location of the home directory can be optional, but make sure the user has **read and write allows permission** to the directory and its contents.

## Script-Usage

The installation script can be used to quickly install or update the CLI version of the tool.

1. Download the installation Python script [install.py](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/download/Miscellaneous) 。

2. Move the downloaded file to the home directory.

3. Launch the script with `python "./install.py"` 。

   The script requires the `Python` environment to run, so make sure your computer has [`Python`](https://www.python.org/downloads/) 。

4. In the script execution window, select the appropriate operating system and processor architecture for your device, and the script will find and download the required files from the project's GitHub Release and install them in the home directory.

The install script will automatically install the `Core`, `Shell`, and `Script` modules, but if you need to use other modules, please see below to install them manually.

## Install `Core`

Core, which is responsible for the implementation of internal functions.

This is a required installation and is distributed as a dynamic library.

1. Check the [Release page](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Core) ，and download the distribution for your device.

2. Move the downloaded file to the home directory and rename it to `core` 。

> You have to choose the right `core` for your device and rename to `core`, otherwise the tool won't be able to launch correctly.

## Install `Shell`

Shell, which provides a command line interface.

This is an optional installation, distributed as an executable.

> @ `Android` `iPhone` \
> Using `Shell` on Android and iPhone requires ROOT or JAILBREAK permission, if your device does not get ROOT, please use `Shell GUI`.

1. Check [Release Page](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Shell) ，and download the distribution for your device.

2. Move the downloaded file to your home directory and rename it to `shell`.

> @ `Windows` \
> Windows does not support calling executables without the `.exe` extension non-programmatically, so you need to run `> mklink shell.exe shell` in the terminal to create a soft link `- shell.exe` to `- shell`; or just rename `- shell` to `- shell.exe`. 3.

3. Give `shell` executable privileges.

   \*\*Users on other systems may have to do `> chmod +x shell` in the terminal to add executable permissions to `- shell`.

   > @ `Android` \
   > On systems with the FUSE scheme, `+ /storage/emulated/<id>` is actually a mapping of `+ /data/media/<id>`, but FUSE restrictions prevent users from modifying and executing files via `+ /storage/emulated/<id>`, which must be done via the real path ` + /data/media/<id>`.

For `Android` systems, you also need to perform the following actions:

1. Install **C++ shared library** to the system library directory.

   For security and size reduction reasons, the C++ runtime used is c++\_shared. Therefore, you also need to copy the `libc++_shared.so` file of the corresponding processor architecture to the system library directory.

   > The system library directory is `+ /system/lib` on 32-bit systems and `+ /system/lib64` on 64-bit systems.
   > This file can be extracted from the Android NDK toolchain or found in the [Release page](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Miscellaneous).

For `iPhone` systems, you also need to do the following:

1. sign `core` and `shell`.

   Running executables on the iPhone requires signature verification, and the distributed program files (**core | shell**) are not signed and need to be signed by the user in order to run properly on the iPhone.

   > You can use the **codesign** tool on the **Macintosh** to do this.

## Install `Shell GUI`

Shell, which provides a graphical interface.

This is an optional installation item, distributed as an application package.

1. Check the [Release Page](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/ShellGUI) to download the distribution for your device.

   > Currently, only `Windows X86-64` and `Android ARM-64` distributions are available. Users of other platforms need to clone this project and build and sign the application package by themselves.

2. Install the application package.

   > @ `Windows` \
   > You need to trust the signing certificate in MSIX before installation. \
   > Right-click the properties of `.msix`, switch to the ⌈Digital Signatures ⌋ page, select the first item in the list, and then click ⌈Details ⌋ , and in the pop-up window, select ⌈View Certificate⌋ - ⌈Install Certificate⌋ - ⌈Local machine⌋ - ⌈Place all certificates in the following storage⌋ - ⌈Trusted persons⌋, to complete the installation of the certificates.

For `Android` systems, you also need to perform the following actions:

1. Grant full external storage read/write permission to the application in the system settings.

   If the app does not obtain this permission, a ⌈Permission denied ⌋ error will appear when launching the console.

   > In different versions and vendor-customized systems, this permission is typically expressed as ⌈Read and write storage ⌋, ⌈Manage all files ⌋, etc. \
   > In native systems, the authorization page can be found in the application settings; \
   > For some vendor-customized systems, authorization may be required elsewhere, for example, MIUI requires granting the app ⌈All file access rights⌋ in ⌈Protect privacy⌋.

## Install `Script`

Scripts that control the flow of the tool's operation.

This is a required installation item, issued as a script package.

1. Check the [Release page](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Script) and download the distribution for your device.

2. Move the downloaded files to your home directory, create a `script` directory, and uncompress all the contents of the zip package to it.

## Install Windows Explorer Extension

Windows Explorer Extension, which integrates tools into the Windows Explorer right-click menu.

> This module is dedicated to Windows systems and does not available for other systems.

This is an optional installation and is distributed as an application package.

Check the [Release page](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/WindowsExplorerExtension) to download the distribution for your device.

There are two sub-versions of the extension, which need to be selected according to your needs:

- `<n>_0` provides only the normal file forwarding portal; it appears as a single menu item in both the new and old-style right-click menus.

- `<n>_1` additionally provides a quick forwarding entry for each function, but has a greater impact on the speed of the right-click menu call-out; it is displayed as a single secondary menu in the new-style right-click menu, and as multiple primary menus in the old-style right-click menu.

2. Install the application package.

   > You need to trust the signing certificate in MSIX before installation. \
   > Right-click to view the properties of `.msix`, switch to the ⌈Digital Signatures⌋ page, select the first item in the list, then click ⌈Details⌋, and in the pop-up window, select ⌈View Certificate⌋ - ⌈Install Certificate⌋ - ⌈Local machine⌋ - ⌈Place all certificates in the following storage⌋ - ⌈Trusted persons⌋ to complete the installation of the certificate.

3. Open the `create_setting.reg` file as text, edit the configuration items in it, and right-click and select Registry Editor to open the file to merge the registry when you are done.

   The extension will read the extension configuration data from the system registry, all configurations are located in `HKEY_CURRENT_USER\Software\TwinStar\ToolKit\WindowsExplorerExtension` and the following configuration items are listed:

   - `launch_script`

     String, path to the launch script, default is empty string.

     The extension will execute the specified launch script and pass the path to the selected file to it. Therefore, a launch script must be created in which to launch the shell and pass the parameters.

     > Launch file for `Shell` are provided and can be found in the [Release page](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Miscellaneous).

   - `launch_limit`

     Double word, limit on the number of file objects required, defaults to `0`.

     If `0`, there is no limit, then all file objects are forwarded to the tool together and need to be processed sequentially; otherwise, the limit is no more than the set number, then each file object is forwarded to the tool separately, and the user can process them separately in different windows.

   - `language`

     A string that extends the language used, default is `Chinese`.

     Can be `Chinese` or `English`.

   - `hidden_group_when_unavailable`

     A double-word for whether to hide a group when none of its function items are available for the selected file object, default is `0`.

     If the value is `0`, the group is shown, otherwise it is hidden.

   - `hidden_item_when_unavailable`

     Double-word whether to hide a feature item when it is not available for the selected file object, defaults to `0`.

     The value is `0`, otherwise it is hidden.

   - `visible_<group>`

     Double-word to control whether to show each function group, default is `0`.

     If the value is `0`, it is hidden, otherwise it is shown.

     You can hide the less frequently used function groups to reduce the space occupied by the right-click menu and reduce the impact on the speed of the right-click menu. 4.

4. You can now see the `⌈TwinStar ToolKit - Extension⌋` option inside the context menu of any file or directory, and use these options to quickly forward file objects to the tool.

   If you do not see this option, try restarting Explorer `explorer.exe`, or restarting your computer.

   > Please note that due to Windows limitations, you can only select a maximum of 16 file objects at a time, beyond that number you will not be able to successfully forward file objects.

5. After uninstalling the extension, you can use the `remove_setting.reg` file to clear the extension configuration data left in the system registry.

## Install `Helper`

Helper, which provides additional advanced features.

> This module is dedicated to Windows systems and is not available for other systems.

This is an optional installation item, distributed as an application package.

Check the [Release page](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Helper) to download the distribution for your device.

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