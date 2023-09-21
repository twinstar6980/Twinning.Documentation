# Usage

- [Use `Shell`](#Use-Shell)

- [Use `Shell GUI`](#Use-Shell-GUI)

- [Interaction](#Interaction)

- [Configuration file](#Configuration-file)

- [Additional arguments](#Additional-arguments)

- [Forwarding file object](#Forwarding-file-object)

- [Advanced usage](#Advanced-usage)

- [Using `Helper`](#Using-Helper)

## Using `Shell`

The `Shell` provides a command line interface for the user.

The user needs to start `Shell` in the terminal with command line arguments in the following format:

`<kernel> <script> <argument>...`

- `<kernel>`

	The first argument is the path to the kernel file.

- `<script>`

	The second argument is the script file path passed to the kernel processing logic. It can also be marked with `$` as the first character, indicating that the rest of the string is used as a JS script.

- `<argument>...`

	The remaining arguments are passed as arguments to the kernel processing logic.

The main function returns 0 if no errors occur during execution, otherwise 1.

For ease of use, a launch script can be created to start the `Shell` and pass some required arguments. The launch script is also pre-set in the bundle package.

## Using the `Shell GUI`

The `Shell GUI` provides a GUI for the user.

The user can use `Shell GUI` as if it were a normal application.

When you open the application for the first time, you need to specify the kernel, scripts, and arguments to be used at runtime in the application settings, and a typical configuration is as follows:

1. `Kernel` = `<home>/kernel` .

2. `Script` = `<home>/script/main.js` .

3. `Argument` = `<home>` .

4. `Fallback Directory For Invisible File` = `<home>/workspace` .

	> This item is only available for Android and iPhone.

> The `<home>` in the above settings needs to be replaced with the path to the home directory.

Launching the `Shell GUI` in the terminal is also supported, with command line arguments in the following format:

`<kernel> <script> <argument>...`

- `<kernel>`

	The first argument is the path to the kernel file.

- `<script>`

	The second argument is the script file path passed to the kernel processing logic. It can also be marked with `$` as the first character, indicating that the rest of the string is used as a JS script.

- `<argument>...`

	The remaining arguments are passed as arguments to the kernel processing logic.
`-additional <argument>...`

If the first command line argument is `-additional`, the application will be started with the default command set in the application settings, and `<argument>...` will be additional arguments.

If the command line arguments are passed in, the application will automatically exit after the command is executed successfully (this behavior can be disabled in the application settings).

If no command line arguments are passed in, the application will be started directly, and the user can manually execute the default command (the default command can be set in the application settings).

> @ `Android` `iPhone` \
> cannot pass in command line arguments directly.
> 
> @ `Android` \
> can pass in command line arguments through `Intent`: `action = "com.twinstar.toolkit.shell_gui.action.LAUNCH", extra = { "command": Array<String> }`.

## Interaction

A tool will output a message to the user or ask for some arguments when it runs; `Shell` and `Shell GUI` provide essentially the same interaction logic.

### Output

Different types of notifications have different colors, with a solid circle icon of the corresponding color as the flag icon:

- `⚪` Regular (dark theme)

- `⚫` regular (light theme)

- `🔵` Information

- `🟡` Warning

- `🔴` Error

- `🟢` Success

- `🟣` input

### Input

Of the six notification types, `🟣` indicates that the tool is requesting input arguments from the user, and the tool will stop doing anything else and wait for the user to enter until the user has finished typing and then type `Enter to continue...`

When input is requested, a leading character is also displayed as a prompt for the type of argument. The input value type and format are as follows:

- `U` Pause

	Pause the program to wait for a user response.

- `C` Confirmation value

	A single character `y` or `n` , indicating ⌈ yes ⌋ and ⌈ no ⌋ .

- `N` number

	A decimal number.

- `I` Integer

	A decimal integer that may not contain a decimal point.

- ` Z` Size

	An unsigned decimal number followed by a binary unit (b = 2^0 , k = 2^10 , m = 2^20 , g = 2^30), typically used to indicate storage capacity.

	> For example, `4k` represents 4096 bytes of storage capacity.

- `O` option

	Select an optional option by integer ordinal number.

- `S` String

	A line of text.

- `P` Input path

	Points to the path of an existing file or directory on disk.

	> You can also enter `:p` to open the system file pick dialog (`Windwos`, `Linux`, `Macintosh` only).
	>
	> If the input path is surrounded by a pair of quotes, the quotation marks are automatically removed; If a relative path is entered, the path is calculated relative to the tool's working directory `+ <home>/workspace`.

- `P` Output path

	Points to a path on disk that does not exist. Toolkit typically automatically generate a default output path, but request a new output path when the default output path is already existed.

	> You can also enter `:o` to overwrite existing files;
	> or enter `:d` to delete existing files;
	> or type `:t` to move existing files to the tool's recycle directory `+ <home>/trash`.
	>
	> If the input path is surrounded by a pair of quotes, the quotation marks are automatically removed; If a relative path is entered, the path is calculated relative to the tool's working directory `+ <home>/workspace`.

## Configuration file

In the script directory `+ <home>/script`, some script files have accompanying configuration files with the same name as the corresponding script file, but with the extension `.json`. Configuration attributes affect some of the tool's behavior, such as the output format of JSON.

> For example, a script `- Entry/Entry.js` in the scripts directory has a configuration file with the configuration file `- Entry/Entry.json`.

Users can modify the configuration file themselves, and the specification of each configuration file can be found in [method](./method.md) section.

## Additional arguments

The user can pass in additional arguments when starting the tool, or if no additional arguments are provided, the user will be asked to enter them at runtime。 The additional arguments are in the following format：

```
[
	<input>
	[ -disable_filter ]
	[ -method <method-id> ]
	[ -argument <argument-json> ]
]...
```

- `<input>`

	Specifies the input data of the command, usually the path to a file or directory, as the input argument of the method.

- `[ -disable_filter ]`

	Used to disable method filtering.

	By default, if `-method` is not specified, the tool will filter the available methods for user selection based on the type of input object (mainly by extension);

	If candidate method filtering is disabled, all methods will be listed for user selection. This should not be enabled because toolkit provides too many methods and it is always recommended to have filter on.

- `[ -method <method-id>]`

	Specify the method to be executed, followed by the ID of the method. If no method is specified, it will wait for the user to select the method at runtime.

- `[ -argument <argument-json> ]`

	Specifies the argument to be passed to the method, followed by a JSON string, and must be parsable as an Object.

The IDs of the methods and their arguments are defined in [method](. /method.md) section.

> Example - Using the tool to decode the `test.pam` file on the desktop:
>
> `> . \launch.cmd "C:\Users\TwinStar\Desktop\test.pam" "-method" "popcap.animation.decode"`
>
> This command takes the path to the `test.pam` file as input argument and specifies the method to be executed as `PopCap Animation Decode`. After evaluate finish, you can see a new file named `test.pam.json` on the desktop, which is the decoded PAM data.

## Forwarding file object

The tool is designed to primarily handle file objects in external storage space. The term ⌈ **forwarding** ⌋ refers to launching the tool with the path to the file object as an additional argument. Forwarding can be done through startup scripts, system extension provided by the forwarder module, etc.

The file object to be processed is forwarded to the tool, and when the tool is launched, the available methods are listed according to the type of that file object, and the user can enter the serial number of the method to be executed.

The tool is preconfigured with many methods such as RTON decoding and RSB unpacking, which are described in [method](./method.md) section.

## Advanced usage

If you have some programming skills, you can also integrate the tools into your own projects or use them with custom scripts, see the [Advanced](./advanced.md) section.

## Using `Helper`

`Helper` provides additional advanced functions for users.

Currently, the following modules are provided:

* `Command Forwarder`

	This module is used to forward method argument to the toolkit. It can visually select the methods that need to be applied and edit method argument. It can be regarded as the GUI of the tool.

* `Animation Viewer`

	This module is used to view PopCap Animation (PAM).

	1. Click the ⌈ Animation File ⌋ button on the right side of the text box in the upper right corner of the ⌈ Stage ⌋ column on this module page, and select the `*.pam.json` file in the pop-up window; or drag `*.pam.json` file from ⌈ Explorer ⌋ to the application window then drop.

		> `*.pam.json` is obtained by decoding `*.pam` files by toolkit.

	2. If the exploded view required for the animation is located in the same directory as the animation file, the animation can be rendered normally. Otherwise, you need to click the button on the right side of the ⌈ Image Directory ⌋ text box to select the directory where the exploded view is located.

	3. Through other UI controls, you can select the sub-animation you want to play, adjust the playback interval and frame rate, set component filter items, etc.

