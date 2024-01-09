# Usage

- [Using `Shell CLI`](#Using-Shell-CLI)

- [Using `Shell GUI`](#Using-Shell-GUI)

- [Using `Helper`](#Using-Helper)

- [Forwarding file](#Forwarding-file)

- [Additional argument](#Additional-argument)

- [User input](#User-input)

- [Configuration file](#Configuration-file)

- [Using `Helper`'s advanced features](#Using-Helper's-advanced-features)

## Using `Shell CLI`

`Shell CLI` allows you to use tools as a command line in the system terminal.

The user needs to launch `Shell CLI` in the terminal via command line arguments in the following format:

`<kernel> <script> <argument>...`

* `<kernel>`
	
	The first argument is the path to the kernel file.

* `<script>`
	
	The second argument is the script file path passed to the kernel processing logic. It can also be marked with `$` as the first character, indicating that the rest of the string is used as a JS script.

* `<argument>...`
	
	The remaining arguments are passed as arguments to the kernel processing logic.

The main function returns 0 if no errors occur during execution, otherwise 1.

## Using `Shell GUI`

The `Shell GUI` provides a terminal-like UI, and not depend on the behavior of the system terminal.

Click the `Launch` button at the bottom of the application console interface to launch the session. Press and hold the button to edit additional parameters for each launch.

Launching the `Shell GUI` in the terminal is also supported, with command line arguments in the following format:

`-additional_argument <additional_argument>...`

* `<additional_argument>...`
	
	Additional arguments to the kernel processing logic.

If the command line arguments are passed in, the application will automatically launch console after stared.

> @ `Android` `iPhone` \
> cannot pass in command line arguments directly.
> 
> @ `Android` \
> can pass in command line arguments through `Intent`: `action = "com.twinstar.toolkit.shell_gui.action.LAUNCH", extra = { "command": Array<String> }`.

## Using `Helper`

The `Modding Worker` module of `Helper` provides a UI similar to the `Shell GUI`, but the style is more modernize for the `Windows 11` system theme.

After opening the `Modding Worker` page, click the `Launch` button at the bottom of the interface to launch the session. Click the pin button at the top right of the interface to edit additional arguments for each launch.

> The `launch_helper.cmd` script in the home directory can quickly start the `Modding Worker`. Multiple files can be dragged onto the script and released to append additional arguments.

## Forwarding file

The tool is designed to primarily handle file objects in external storage space. The term ⌈ **forwarding** ⌋ refers to launching the tool with the path to the file object as an additional argument. Forwarding can be done through startup scripts, system extension provided by the forwarder module, etc.

The file object to be processed is forwarded to the tool, and when the tool is launched, the available methods are listed according to the type of that file object, and the user can enter the serial number of the method to be executed.

> Note: If a directory is forwarded to the tool, the user will see many methods prefixed with `[*]` in the optional method list. They are batch processing methods that will process each sub-file and sub-directory in the directory in turn. Most single-input single-output methods have batch versions.
> 
> A common mistakes is that users mistakenly select the batch processing methods when they need to perform regular methods on a directory. For example, if you need to pack a `.rsb.bundle` directory into an `rsb` file, you need to select the regular version of `PopCap Resource-Stream-Bundle Pack` rather than the batch version with the `[*]` prefix.

## Additional argument

The user can pass in additional arguments when starting the tool, or if no additional arguments are provided, the user will be asked to enter them at runtime。 The additional arguments are in the following format：

```
[
	<input>
	[ -disable_filter ]
	[ -method <method-id> ]
	[ -argument <argument-json> ]
]...
```

* `<input>`
	
	Specifies the input data of the command, usually the path to a file or directory, as the input argument of the method.
	
	If the input value is `?` , the user will be asked to input the argument at runtime.

* `[ -disable_filter ]`
	
	Used to disable method filtering.
	
	By default, if `-method` is not specified, the tool will filter the available methods for user selection based on the type of input object (mainly by extension);
	
	If candidate method filtering is disabled, all methods will be listed for user selection. This should not be enabled because toolkit provides too many methods and it is always recommended to have filter on.

* `[ -method <method-id>]`
	
	Specify the method to be executed, followed by the ID of the method. If no method is specified, it will wait for the user to select the method at runtime.

* `[ -argument <argument-json> ]`
	
	Specifies the argument to be passed to the method, followed by a JSON string, and must be parsable as an Object.

The IDs of the methods and their arguments are defined in [method](. /method.md) section.

> Example - Using the tool to decode the `test.pam` file on the desktop:
>
> `> . \launch.cmd "C:\Users\TwinStar\Desktop\test.pam" "-method" "popcap.animation.decode"`
>
> This command takes the path to the `test.pam` file as input argument and specifies the method to be executed as `PopCap Animation Decode`. After evaluate finish, you can see a new file named `test.pam.json` on the desktop, which is the decoded PAM data.

## User input

When the tool is running, it will output some message to the user or request the user to input some arguments. The input value type and format are as follows:

* `Pause`
	
	Pause the program to wait for a user response.

* `Boolean`
	
	A single character `y` or `n` , indicating ⌈ yes ⌋ and ⌈ no ⌋ .

* `Integer`
	
	A decimal integer that may not contain a decimal point.
	
* `Floater`
	
	A decimal number that may contain a decimal point.

* `Size`
	
	An unsigned decimal number followed by a binary unit (b = 2^0 , k = 2^10 , m = 2^20 , g = 2^30), typically used to indicate storage capacity.
	
	> For example, `4k` represents 4096 bytes of storage capacity.

* `String`
	
	A line of text.
	
	> If the entered text is empty, it is deemed to have entered a null value;\
	> If the input text starts with `::` , the tool will intercept the text after it as the real input. In this way, an empty string can be entered instead of a null value.

* `Path`
	
	The paths that can be used in the local file system, divided into input paths and output paths, the former points to the path of an existing file or directory on the disk, and the latter points to a path that does not exist on the disk.
	
	> If the entered text is empty, it is deemed to have entered a null value;\
	> If the input text starts with `::` , the tool will intercept the text after it as the real input. In this way, an empty string can be entered instead of a null value.
	> 
	> Enter `:p` will open the system file pick dialog (`GUI` or `Windwos CLI` 、`Linux CLI` 、`Macintosh CLI` only);\
	> Enter `:g` will generate avaliable path (append suffix);\
	> Enter `:m` will move original file;\
	> Enter `:d` will delete original file;\
	> Enter `:o` will overwrite original file.
	>
	> If the input path is surrounded by a pair of quotes, the quotation marks are automatically removed; If a relative path is entered, the path is calculated relative to the tool's working directory `<home>/workspace` .

* `Enumeration`
	
	One of several optional options.

## Configuration file

In the tool's script directory `<home>/script`, the file with the extension `.json` is the configuration file of the tool. The configuration items in it will affect certain behaviors of the tool, such as the output format of JSON. Users can modify the configuration file.

`<home>/script/Entry/Entry.json` is the main configuration file of the tool, and its configuration items are as follows.

* `<configuration>`
	
	* `language` : `string` = `English`
		
		Interaction language. Can be `English` or `Chinese` or `Vietnamese`.
	
	* `console_cli_disable_virtual_terminal_sequence` : `boolean` = `false`
		
		Disable command-line virtual terminal sequences. Valid only when using a command-line shell.
	
	* `executor_typical_method_disable_name_filter` : `boolean` = `false`
		
		Disables the name filtering behavior of the command executor. By default, the tool will match the extension of the file path of the command input, unmatched methods will not be displayed to the user. When disabled, all functions will be displayed.
	
	* `byte_stream_use_big_endian` : `boolean` = `false`
		
		Use big end-order for internal byte stream operations. This option is necessary for some big-endian file processing. It is disabled by default, because most of the time the user is dealing with little-endian files.
	
	* `common_buffer_size` : `string` = `64.0m`
		
		The size of the common buffer. Used for encoding JSON strings, encoding PNG images, etc. If the amount of memory required for encoding exceeds this size, the tool will fail to process it.
	
	* `json_format` : `{ ... }`
		
		JSON output format.
		
		* `disable_array_trailing_comma` : `boolean` = `false`
			
			Disable array's trailing comma.
		
		* `disable_array_line_breaking` : `boolean` = `false`
			
			Disable array's line breaking.
		
		* `disable_object_trailing_comma` : `boolean` = `false`
			
			Disable object's trailing comma.
		
		* `disable_object_line_breaking` : `boolean` = `false`
			
			Disable object's line breaking.
	
	* `external_program` : `{ ... }`
		
		Specifies the path to external programs that may be called, if null, the `PATH` environment variable will be retrieved at runtime.
		
		* `sh` : `null | string` = `null`
			
			Used for `/utility/AndroidHelper.ts` 。
		
		* `adb` : `null | string` = `null`
			
			Used for `/utility/AndroidHelper.ts` 。
		
		* `vgmstream` : `null | string` = `null`
			
			Used for `/Support/Wwise/Media/Decode.ts` 。
		
		* `wwise` : `null | string` = `null`
			
			Used for `/Support/Wwise/Media/Encode.ts` 。
		
		* `il2cpp_dumper` : `null | string` = `null`
			
			Used for `/Support/Kairosoft/Game/ModifyProgram.ts` 。
	
	* `thread_limit` : `bigint` = `0`
		
		The maximum number of thread pools. Currently has no practical effect.
	
	* `command_notification_time_limit` : `null | bigint` = `15000`
		
		Command notification time limit. If the vaild execution duration exceeds this value (in milliseconds) after a command has completed, a system notification will be pushed to alert the user to set up the configuration. Setting to null will disable notifications.

> Each group of functions provided by the script has a corresponding configuration file, see the [Method](./method.md) section.

## Using `Helper`'s advanced features

* `Resource Forwarder`
	
	This module provides a better file forwarding UI, which can filter the available options by inputting the file type and extension, and passes them to the `Modding Worker` for processing, making it more efficient and easier to use.
	
	> Options are defined by the `Option Configuration` file in the module setting.
	
	> The `launch_helper_forwarder.cmd` script in the home directory can quickly open the `Resource Forwarder`. Multiple files can be dragged onto the script and released to add additional arguments.

* `Command Sender`
	
	This module can visually select the methods you want to use and fill in the arguments.
	
	> Methods are defined by the `Method Configuration` file in the module settings.

* `Animation Viewer`
	
	This module can view PopCap Animation (PAM) file.
	
	1. Click the ⌈ Animation File ⌋ button on the right side of the text box in the upper right corner of the ⌈ Stage ⌋ column on this module page, and select the `*.pam.json` file in the pop-up window; or drag `*.pam.json` file from ⌈ Explorer ⌋ to the application window then drop.
		
		> `*.pam.json` is obtained by decoding `*.pam` files by toolkit.
	
	2. If the exploded view required for the animation is located in the same directory as the animation file, the animation can be rendered normally. Otherwise, you need to click the button on the right side of the ⌈ Image Directory ⌋ text box to select the directory where the exploded view is located.
	
	3. Through other UI controls, you can select the sub-animation you want to play, adjust the playback interval and frame rate, set component filter items, etc.

* `Package Builder`
	
	This module can visually manage the `PvZ-2 Package Project` project.
