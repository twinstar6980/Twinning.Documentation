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

* ***`application`***
	
	Set the first argument to this value to instruct the application to read and apply the remaining arguments as command.
	
	Otherwise, the application silently ignores all arguments ​​and does not execute any command.

* **`-launch`**
	
	Launch new tab page.
	
	* `title` : *`string`*
		
		Page title.
	
	* `type` : *`string`*
		
		Module type.
	
	* `option` : *`string...`*
		
		Module option.

* **`-forward`**
	
	Forward resources to a module.
	
	* `item` : *`string...`*
		
		Resource item.

> When launching an application via command line argument, a new application instance will always be created.

Command can also be passed via application link, which is available on `Windows`, `Macintosh`, `Android`, and `Iphone` system:

* ***`twinstar.twinning.assistant:/application?`***
	
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
	
	> `Kernel`, `Script`, `Argument` parameters need to be defined in the module settings.

* `Command Sender`
	
	This module is used with `Modding Worker`, it can visually edit the methods and arguments you wants to use.
	
	> Methods are defined by the `Method Configuration` file in the module settings.

* `Resource Shipper`
	
	This module is used with `Modding Worker`, it can easily forward files and select the methods and arguments you wants to use.
	
	> Options are defined by the `Option Configuration` file in the module setting.

* `Animation Viewer`
	
	This module can view PopCap Animation (PAM) file.
	
	> Place the `*.pam.json` file obtained by decoding the `*.pam` file and the texture decomposition image `*.png` referenced in the animation in the same directory, and import the `*.pam.json` file in the page to view the animation.

* `Reflection Descriptor` *`<Plus-Only>`*
	
	This module can view RTON object structure definition files.

* `Package Builder` *`<Plus-Only>`*
	
	This module can visually manage the `PvZ-2 Package Project` project.

## Forwarding file

The tool is designed to primarily handle file objects in external storage space. The term ⌈ **forwarding** ⌋ refers to launching the tool with the path to the file object as an additional argument. You can forward the file by passing arguments through the command line.

The file object to be processed is forwarded to the tool, and when the tool is launched, the available methods are listed according to the type of that file object, and the user can enter the serial number of the method to be executed.

> Note: If a directory is forwarded to the tool, the user will see many methods prefixed with `[*]` in the optional method list. They are batch processing methods that will process each sub-file and sub-directory in the directory in turn. Most single-input single-output methods have batch versions.
> 
> A common mistakes is that users mistakenly select the batch processing methods when they need to perform regular methods on a directory. For example, if you need to pack a `.rsb.bundle` directory into an `rsb` file, you need to select the regular version of `PopCap Resource-Stream-Bundle Pack` rather than the batch version with the `[*]` prefix.

## Additional argument

The user can pass in additional arguments when starting the tool, or if no additional arguments are provided, the user will be asked to enter them at runtime. The additional arguments are in the following format：

* `item`
	
	Command items can be specified multiple times, and the tool will process them one by one.
	
	* `input` : *`string`*
		
		Specifies the input data of the command, usually the path to a file or directory, as the input argument of the method.
		
		If the input value is `?none`, then the input value is `null`, and features with no input arguments will only appear in the candidates if the input is `null` .
	
	* **`-method`** : *`string`*
		
		Specify the method to be executed, followed by the ID of the method.
		
		If the input value is `?filtered`, the available features will be filtered based on the input value and listed for the user to consider.
		
		If the input value is `?unfiltered`, all features will be listed for the user to consider.
	
	* **`-argument`** : *`string`*
		
		Specifies the argument to be passed to the method, followed by a JSON string, and must be parsable as an `Object`.

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
	> If the input text starts with `??` , the tool will intercept the text after it as the real input. In this way, an empty string can be entered instead of a null value.

* `Path`
	
	The paths that can be used in the local file system, divided into input paths and output paths, the former points to the path of an existing file or directory on the disk, and the latter points to a path that does not exist on the disk.
	
	> If the entered text is empty, it is deemed to have entered a null value;\
	> If the input text starts with `??` , the tool will intercept the text after it as the real input. In this way, an empty string can be entered instead of a null value.
	> 
	> Enter `?g/generate` will generate avaliable path (append suffix);\
	> Enter `?m/move` will move original file;\
	> Enter `?d/delete` will delete original file;\
	> Enter `?o/overwrite` will overwrite original file.
	> 
	> If the input path is surrounded by a pair of quotes, the quotation marks are automatically removed;\
	> If the input path starts with `~` , `~` will be interpreted as the tool's home directory `<home>` ;\
	> If a relative path is entered, the path is calculated relative to the tool's working directory `<home>/workspace` .

* `Enumeration`
	
	One of several optional options.

## Configuration file

The tool's script configuration directory `<home>/script/configuration` stores configuration items required for script execution. Users can modify the configuration files to change script behavior.

The `<home>/script/configuration/language` directory stores the multi-language text used by the script. You can modify or create new language text files to change the script's interactive text.

`<home>/script/configuration/setting.json` is the script settings file, and its configuration items are as follows.

* `<configuration>`
	
	* `byte_stream_use_big_endian` : `boolean` = `false`
		
		Use big end-order for internal byte stream operations. This option is necessary for some big-endian file processing. It is disabled by default, because most of the time the user is dealing with little-endian files.
	
	* `common_buffer_size` : `string` = `64.0m`
		
		The size of the common buffer. Used for encoding JSON strings, encoding PNG images, etc. If the amount of memory required for encoding exceeds this size, the tool will fail to process it.
	
	* `json_format_disable_array_trailing_comma` : `boolean` = `false`
		
		When outputting JSON, disable array's trailing comma.
	
	* `json_format_disable_array_line_breaking` : `boolean` = `false`
		
		When outputting JSON, disable array's line breaking.
	
	* `json_format_disable_object_trailing_comma` : `boolean` = `false`
		
		When outputting JSON, disable object's trailing comma.
	
	* `json_format_disable_object_line_breaking` : `boolean` = `false`
		
		When outputting JSON, disable object's line breaking.
	
	* `thread_limit` : `bigint` = `0`
		
		The maximum number of thread pools. Currently has no practical effect.
	
	* `external_program_path` : `Record<string, null | string>` = `{ ... }`
		
		Specifies the path to external programs that may be called, if null, the `PATH` environment variable will be retrieved at runtime.
		
		* `sh` used for `/utility/AndroidHelper.ts` 。
		
		* `adb` used for `/utility/AndroidHelper.ts` 。
		
		* `WwiseConsole.exe` used for `/Support/Wwise/Media/Encode.ts` 。
		
		* `WwiseConsole.sh` used for `/Support/Wwise/Media/Encode.ts` 。
		
		* `vgmstream-cli` used for `/Support/Wwise/Media/Decode.ts` 。
		
		* `dotnet` used for `/Support/Kairosoft/Game/ModifyProgram.ts` 。
		
		* `Il2CppDumper.dll` used for `/Support/Kairosoft/Game/ModifyProgram.ts` 。
	
	* `language` : `string` = `english`
		
		Interaction language. Can be `english` or `chinese` or `vietnamese`.
	
	* `console_basic_disable_virtual_terminal_sequence` : `boolean` = `false`
		
		Disable command-line virtual terminal sequences. Valid only when using `Shell`.
	
	* `executor_typical_method_disable_name_filter` : `boolean` = `false`
		
		Disables the command processor's name filtering behavior. By default, the tool matches the file path extension of the command input, and unmatched methods will not be displayed to the user. Disabling this will display all methods. This also affects the name filtering behavior of the batch method. If disabled, the batch method will no longer skip files with unmatched names.
	
	* `executor_typical_method_configuration` : `boolean` = `false`
		
		The name filtering rules and argument's default values ​​of each method, can be modified to suit your usage habits.
	
	* `executor_pvz2_resource_convert_ptx_format_map_list` : `...` = `...`
		
		Configuration data required for specific methods. Used for `popcap.resource_stream_bundle.resource_convert` 。
	
	* `executor_pvz2_package_project_conversion_setting` : `...` = `...`
		
		Configuration data required for specific methods. Used for `pvz2.package_project.parse` 。
	
	* `command_notification_time_limit` : `null | bigint` = `15000`
		
		Command notification time limit. If the vaild execution duration exceeds this value (in milliseconds) after a command has completed, a system notification will be pushed to alert the user to set up the configuration. Setting to null will disable notifications.

> Each group of functions provided by the script has a corresponding configuration file, see the [Method](./method.md) section.
