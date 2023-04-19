# FAQ

- [Windows file path length limit](#Windows-file-path-length-limit)

- [JSON file format](#JSON-file-format)

- [Shell requirements for the terminal](#Shell-requirements-for-the-terminal)

- [Strict file handling mechanism](#Strict-file-handling-mechanism)

- [Version compatibility and old version downloads](#Version-compatibility-and-old-version-downloads)

- [Processing big-endian files](#Processing-big-endian-files)

- [Processing OBB files](#Processing-OBB-files)

- [Processing SMF files](#Processing-SMF-files)

## Windows file path length limit

In Windows, the system's file paths are limited to 260 characters in length, which prevents long path files from existing on the system and prevents tools from processing any long path files.

Starting with Windows 10+, long path support is available, but this is turned off by default, see [Microsoft official documentation](https://learn.microsoft.com/windows/win32/fileio/maximum-file-path-limitation?tabs=registry) to enable it. When long path support is enabled, the tool will be able to handle long path files properly.

> In some application scenarios (e.g. unpacking RSB files with long path file resources), the tool may output files with long paths and the tool will terminate early due to file read/write failure. Therefore, it is recommended that you always enable long path support for your system.

## JSON file format

The JSON read and write rules for the tool are self-implementing and does not follow the JSON standard, with the following differences:

1. Comments

	The JSON standard does not allow comments, but the tools do allow comments, including line comments `// ... ` and block comments `/* ... */`. 2.

2. Trailing commas

	The comma after the last element of arrays and objects is called a trailing comma. The JSON standard does not allow trailing commas, but they are a permitted and even recommended syntax in JS and many programming languages. Considering the convenience that trailing commas provide for JSON reading and writing, tools support trailing commas and add them by default when outputting JSON.

	In some editors, trailing commas are treated as a formatting error by default, and users can disable trailing comma output by modifying the `json_format.disable_trailing_comma` item to `true` in the `- <home>/script/Entry/Entry.json` configuration.

	If you want to launch the JSON file to the game, it is recommended that you should `disable_trailing_comma` because the JSON Parsing system of the game follow the JSON Standard.

	> In `VS Code`, the following configuration can be added to `settings.json` to make the editor allow JSON trailing commas:
	> 
	> ```json
	> "json.schemas": [
	> 	{
	> 		"fileMatch": [
	> 			"*.json",
	> 		],
	> 		"schema": {
	> 			"allowTrailingCommas": true,
	> 		},
	> 	},
	> ],
	> ```
	> 
	> `allowTrailingCommas` is an extension of `VS Code` and does not conform to the `JSON-Schema` standard.

3. Escape characters

	The tool's escape character support does not follow the JSON standard, and the escape rules are as follows:

	- `\\\ \' \"` escapes to the original character.

	- `\a \b \f \n \r \t \v` is escaped to the corresponding control character.

	- `\0` Escape to the null character.

	- `\oNNN` Unicode character represented by a 3-bit octal number, allowing values above 0xFF.

	- `\xNNN` 2-bit Unicode character in hexadecimal numbers.

	- \\uNNNN` 4-bit hexadecimal Unicode character.

	- `\UNNNNNNNN` 8-bit hexadecimal Unicode character.

4. Number formatting

	The number formatting support of the tool does not follow the JSON standard, and the formatting rules are as follows:

	- Integers: match the regular expression `^[+-]? [\d]+$` .

	- Floating-point numbers: match the regular expression `^[+-]? [\d]+[.] [\d]+$` .

	- Floating-point numbers in scientific notation: match the regular expression `^[+-]? [\d]+[.] [\d]+[e][+-][\d]+$` .

	In other words, the differences from the JSON standard are as follows:

	- Positive numbers are allowed to have a positive sign, and unsigned numbers are considered positive.

	- Floating-point numbers must start with a number, must have a decimal point, and must be followed by a number after the decimal point.

	- Floating-point numbers expressed in scientific notation must start with a number, must have a decimal point, and must be followed by a number; only lowercase `e` is allowed, not uppercase `E`, and must be followed by a plus or minus sign.

	In addition, the tool makes a strict distinction between integers and floating point numbers, with integers being decoded as `bigint` in JS and floating point numbers being decoded as `number` in JS, and users using the wrong value type will result in potential bugs or even runtime failures.

## Shell requirements for the terminal

`Shell` provides a terminal-based command line interface, but it requires the host terminal to support the following features:

1. UTF-8 input/output: required, if not supported, the program will not be able to do input/output properly (Or poorly displayed).

2. Virtual Terminal Control Sequence: Optional, if not supported, it will prevent the program from modifying different types of text with different colors.

	> By default, the tool will use control sequences to optimize the output effect, but if running in a terminal that does not support control sequences, the control sequences will be output directly as strings, which will affect the user's reading.

	> Users can disable the use of control sequences by modifying the `cli_disable_virtual_terminal_sequence` entry to `true` in the `- <home>/script/Entry/Entry.json` configuration.

3. Full fonts: optional, if not supported, some characters (e.g. Chinese characters, emoji) will not be displayed properly. However, you can fix this by changing the language to `English`. It was indicated in `installation.md`.

Some operating systems do not provide (or turn off by default) these support in the default terminal, users can install a third-party terminal and run this program in it, you can refer to the following list:

- Windows 7 ~ 8

	Windows 7 to 8 does not provide support for cmd, I recommend using [cmder](https://cmder.app/).

- Windows 10

	Windows 10 comes with cmd support turned off by default, I recommend using [Windows Terminal](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701).

- Windows 11

	Windows 11 comes with cmd default shutdown support, and comes with Windows Terminal to provide support, please set it as the default terminal of your system.

- Macintosh 12.0.1+

	Macintosh 12.0.1+ comes with Terminal support.

- Linux - Ubuntu 20.04+

	Terminal support is included.

- Android

	I recommend using [Termux](https://termux.dev/en/).

	> Of course, you can also use the terminal of your desktop system via ADB.

> @ `Windows` \
> When you double-click to run the launch script, it will run the program in the system default terminal. To change it to run in a specified third-party terminal, change `launch.cmd`, but switching terminals will cause the cmd window to blink once, unless you use Windows 11 and set Windows Terminal as the system default terminal.

## Strict file handling mechanism

The loose handling of data (fault tolerance) can lead to potential bugs and unpredictable behavior, so the tool uses a strict file handling mechanism, any non-standard file data will cause the tool to fail, even if they can be read properly by some game programs (due to fault tolerance). The strict processing mechanism is the expected behavior of the tool and will always be so.

A typical example is RTON files that do not have `RTON` as the starting quad byte of the file, and although such files can be read normally by PvZ-2 games, the tool cannot decode them because this is considered by the tool to be a file data structure exception.

To handle non-standard files, the user needs to either fix the exception structure segments or modify and recompile the tool's `Core` module. Strictness judgments on the data are typically asserted via the `assert_test` snippet of the `Core` module, and most strictness judgments can be lifted by removing these assertion statements.

In addition, the tool also implements additional `PopCap Reflection-Object-Notation Lenient Decode` and `PopCap Resource-Stream-Bundle Repair` features at the script level, which can be used to handle non-standard RTON files and RSB files.

## Version compatibility and old version downloads

For purposes of bug fixing, semantic correctness, etc., updates to the tool may introduce destructive changes that cause the project files generated by the old and new versions of the tool to be incompatible with each other, so that users will not be able to use the project files generated by the old version in the new version, and vice versa.

When a corrupt change is introduced in an update, users need to use the old version of the tool to encode the project files into binary format (e.g. `*.pam.json` encoded as `*.pam`, `*.rsb.bundle` packaged as `*.rsb`), then update to the new version and decode these binary formats to get the project files for the new version of the tool.

Destructive changes are marked in the version update log, and users can join the [Discord server](https://discord.com/invite/v7qvttSX8K) of this tool to view the version update log.

Users should always use the latest version and only use older versions when project file version migration is required. Distributions in GitHub Release are the latest version only, and all historical distributions can be found in my [Personal OneDrive](https://1drv.ms/f/s!AkIzoME-1oU-fB6En185husw59Q?e=EKJ1e9).

## Processing big-endian files

By default, the tool always treats the processed files as little-endian. However, users may also need to process big-endian files, such as unpacking RSB files extracted from a big-endian device, Xbox.

Before forwarding the big-endian file to the tool for processing, the user needs to modify the `byte_stream_use_big_endian` entry in the `- <home>/script/Entry/Entry.json` configuration to `true` to tell the tool to treat the incoming file as big-endian.

After that, the big-endian files are forwarded to the tool, and if there are no errors in the files themselves, the program will be able to process the big-endian files normally.

> If you need to process little-endian files later, you should restore the `byte_stream_use_big_endian` item to `false`.

## Processing OBB files

PvZ-2 for the Android platform stores RSB packet files as `OBB` extension packet files, which is a generic extension for Android application extension packets and is not discernible; OBB files can store data in any format, not just RSB packet format.

For this reason, the tool does not consider the `.obb` file to be an RSB file, and if the `.obb` file is forwarded directly to the tool, the user will not see the functions related to RSB from the options. If the user is sure that the `.obb` file is stored in RSB packet format, the file extension needs to be changed from `.obb` to `.rsb` in order for the tool to recognize RSB functions.

In addition, it is possible to create soft links to `.obb` files with `.rsb` as the link extension, which avoids the need to rename `.rsb` to `.obb` after repackaging.

> The regular expression for filtering RSB files in `- <home>/script/Entry/method/popcap.resource_stream_bundle.ts` can be modified to allow the tool to recognize `*.obb` files as RSB files.

## Processing SMF files

`SMF` itself is a media file extension, and the tool does not support handling real SMF files.

`SMF` files have a feature in Android development: when an Android project is packaged as an APK, the packager does not compress the SMF files in the project assets (based on the extension).

PopCap takes advantage of this feature by having SMF files in the Android-side APK installers of some PopCap games, but they are not really SMF files, but could be any type of file (such as RSB file).

Users can extract SMF files directly from the APK, but to forward them to the tool for processing, they should first remove the `.smf` extension from these files.

> For example, there are `*.rsb.smf` files in PvZ-2 Chinese Android version, they are `*.rsb` files themselves, you need to remove the `.smf` extension before forwarding them to the tool for processing.

> Note: These \*.rsb.smf files are ZLIB compressed once, and need to be `PopCap ZLib Uncompress` by the tool first. The PvZ-2 Chinese project team is very stupid, as their RSB does not use ZLIB to compress the resource data, but compresses the RSB as a whole in the APK, and then decompresses it to the application's external storage directory. This caused a serious waste of storage space.
