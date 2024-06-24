# Usage

- [Using `Shell`](#Using-Shell)

- [Using `Assistant`](#Using-Assistant)

- [Forwarding file](#Forwarding-file)

- [Additional argument](#Additional-argument)

- [User input](#User-input)

- [Configuration file](#Configuration-file)

## Using `Shell`

`Shell` allows you to use tools as a command line in the system terminal.

Command parameters need to be passed when starting the application, the format is as follows:

`<kernel> <script> <argument>...`

* `kernel` : *`string`*
	
	The kernel file path.

* `script` : *`string`*
	
	The script file path. It can also be marked with `$` as the first character, indicating that the rest of the string is used as a JS script.

* `argument` : *`string...`*
	
	The execution argument.

The main function returns `0` if no errors occur during execution, otherwise `1`.

## Using `Assistant`

`Assistant` implements a variety of sub-functional modules based on `Kernel`, `Script` and other modules.

`Assistant Plus` is a specialized version of `Assistant` on the `Windows` platform, and the design style is more consistent with the `Windows 11` system.

Command can be passed via command line argument, which is available on `Windows`, `Linux`, and `Macintosh` system:

* ***`launch`***
	
	Set the first argument to this value to instruct the application to read and apply the remaining arguments as command.
	
	Otherwise, the application silently ignores all arguments ​​and does not execute any command.

* **`-insert_tab`**
	
	Insert tab page.
	
	* `title` : *`string`*
		
		Page title.
	
	* `type` : *`string`*
		
		Module type.
	
	* `option` : *`string...`*
		
		Module option.

> When launching an application via command line argument, a new application instance will always be created.

Command can also be passed via application link, which is available on `Windows`, `Macintosh`, `Android`, and `Iphone` system:

* ***`twinstar.twinning.assistant:/launch?`***
	
	* `command` : *`string...`*
		
		command argument. can be specified multiple times, and all query values are treated as string arrays.

> When launching an application through an application link, a new application instance will be created only if one does not exist. If an application instance already exists in the system, it will be switched to the foreground and the new command argument will be applied.

> The command format of `Assistant Plus` is similar to `Assistant`, but the command name uses camel case *CamelCase* instead of snake style *snack_case*.

The application also provides file's forwarder extension support, which is available in `Windows`, `Macintosh`, `Android`, and `Iphone` systems:

* `Windows`

	If the forwarder extension is enabled, you can see the `⌈ Twinning Assistant ⌋` option in the file context menu of `Explorer`.

* `Macintosh`

	If the forwarder extension is enabled, you can see the `⌈ Twinning Assistant ⌋` option in the file context menu of `Finder`.

* `Android`

	If the forwarder extension is enabled, you can see the `⌈ Twinning Assistant ⌋` option in the file sharing list of the system or third-party file manager.

	> Note: Due to the limitations of the Android system, the application cannot directly obtain the absolute path of the forwarded file. For details, see [Android Content URI processing strategy](./question.md#Android-Content-URI-processing-strategy) .

* `Iphone`

	If the forwarder extension is enabled, you can see the `⌈ Twinning Assistant ⌋` option in the file sharing list of the system or third-party file manager.

The following functional modules are provided by application:

* `Modding Worker`
	
	This module simulates the `Shell` console UI.
	
	> `Kernel` 、`Script` 、`Argument` parameters need to be defined in the module settings.

* `Command Sender`
	
	This module is used with `Modding Worker`, it can visually edit the methods and arguments you wants to use.
	
	> Methods are defined by the `Method Configuration` file in the module settings.

* `Resource Forwarder`
	
	This module is used with `Modding Worker`, it can easily forward files and select the methods and arguments you wants to use.
	
	> Options are defined by the `Option Configuration` file in the module setting.

* `Reflection Descriptor` *`<Plus-Only>`*
	
	This module can view RTON object structure definition files.

* `Animation Viewer` *`<Plus-Only>`*
	
	This module can view PopCap Animation (PAM) file.
	
	1. Click the ⌈ Animation File ⌋ button on the right side of the text box in the upper right corner of the ⌈ Stage ⌋ column on this module page, and select the `*.pam.json` file in the pop-up window; or drag `*.pam.json` file from ⌈ Explorer ⌋ to the application window then drop.
		
		> `*.pam.json` is obtained by decoding `*.pam` files by toolkit.
	
	2. If the exploded view required for the animation is located in the same directory as the animation file, the animation can be rendered normally. Otherwise, you need to click the button on the right side of the ⌈ Image Directory ⌋ text box to select the directory where the exploded view is located.
	
	3. Through other UI controls, you can select the sub-animation you want to play, adjust the playback interval and frame rate, set component filter items, etc.

* `Package Builder` *`<Plus-Only>`*
	
	This module can visually manage the `PvZ-2 Package Project` project.

## Forwarding file

The tool is designed to primarily handle file objects in external storage space. The term ⌈ **forwarding** ⌋ refers to launching the tool with the path to the file object as an additional argument. You can forward the file by passing arguments through the command line.

The file object to be processed is forwarded to the tool, and when the tool is launched, the available methods are listed according to the type of that file object, and the user can enter the serial number of the method to be executed.

> Note: If a directory is forwarded to the tool, the user will see many methods prefixed with `[*]` in the optional method list. They are batch processing methods that will process each sub-file and sub-directory in the directory in turn. Most single-input single-output methods have batch versions.
> 
> A common mistakes is that users mistakenly select the batch processing methods when they need to perform regular methods on a directory. For example, if you need to pack a `.rsb.bundle` directory into an `rsb` file, you need to select the regular version of `PopCap Resource-Stream-Bundle Pack` rather than the batch version with the `[*]` prefix.

## Additional argument

The user can pass in additional arguments when starting the tool, or if no additional arguments are provided, the user will be asked to enter them at runtime。 The additional arguments are in the following format：

* `item`
	
	Single command item, can be specified multiple times.
	
	* `input` : *`string`*
		
		Specifies the input data of the command, usually the path to a file or directory, as the input argument of the method.
		
		If the input value is `?` , the user will be asked to input the argument at runtime.
	
	* **`-disable_filter`** : *`boolean`*
		
		Used to disable method filtering.
		
		By default, if `-method` is not specified, the tool will filter the available methods for user selection based on the type of input object (mainly by extension);
		
		If candidate method filtering is disabled, all methods will be listed for user selection. This should not be enabled because toolkit provides too many methods and it is always recommended to have filter on.
	
	* **`-method`** : *`string`*
		
		Specify the method to be executed, followed by the ID of the method. If no method is specified, it will wait for the user to select the method at runtime.
	
	* **`-argument`** : *`string`*
		
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
	> Enter `:p` will open the system file pick dialog;\
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
	
	* `disable_basic_virtual_terminal_sequence` : `boolean` = `false`
		
		Disable command-line virtual terminal sequences. Valid only when using `Shell`.
	
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
