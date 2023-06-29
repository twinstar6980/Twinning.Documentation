# Usage

- [Use `Shell`](#Use-Shell)

- [Use `Shell GUI`](#Use-Shell-GUI)

- [Tool Interaction](#Tool-Interaction)

- [Configuration file](#Configuration-file)

- [Additional parameters](#additional-parameters)

- [Forward File Object](#Forward-File-Object)

- [Advanced Usage](#Advanced-Usage)

- [Use `Helper`](#Use-Helper)

## Using `Shell`

The `Shell` provides a command line interface for the user.

The user needs to start `Shell` in the terminal with command line arguments in the following format:

`<core> <script> <argument>...`

- `<core>`

	The first argument is the path to the core file.

- `<script>`

	The second argument is the script file path passed to the core processing logic. It can also be marked with `$` as the first character, indicating that the rest of the string is used as a JS script.

- `<argument>...`

	The remaining arguments are passed as arguments to the core processing logic.

The main function returns 0 if no errors occur during execution, otherwise 1.

For ease of use, a launch script can be created to start the `Shell` and pass some required arguments. The launch script is also pre-set in the bundle package.

## Using the `Shell GUI`

The `Shell GUI` provides a GUI for the user.

The user can use `Shell GUI` as if it were a normal application.

When you open the application for the first time, you need to specify the core, scripts, and parameters to be used at runtime in the application settings, and a typical configuration is as follows:

1. `Core` = `<home>/core` .

2. `Script` = `<home>/script/main.js` .

3. `Argument` = `<home>` .

4. `Fallback Directory For Invisible File` = `<home>/workspace` .

	> This item is only available for Android and iPhone.

> The `<home>` in the above settings needs to be replaced with the path to the home directory.

Launching the `Shell GUI` in the terminal is also supported, with command line arguments in the following format:

`<core> <script> <argument>...`

- `<core>`

	The first argument is the path to the core file.

- `<script>`

	The second argument is the script file path passed to the core processing logic. It can also be marked with `$` as the first character, indicating that the rest of the string is used as a JS script.

- `<argument>...`

	The remaining arguments are passed as arguments to the core processing logic.
`-additional <argument>...`

If the first command line parameter is `-additional`, the application will be started with the default command set in the application settings, and `<argument>...` will be additional arguments.

If the command line parameters are passed in, the application will automatically exit after the command is executed successfully (this behavior can be disabled in the application settings).

If no command line parameters are passed in, the application will be started directly, and the user can manually execute the default command (the default command can be set in the application settings).

> @ `Android` `iPhone` \
> cannot pass in command line parameters directly.
> 
> @ `Android` \
> can pass in command line parameters through `Intent`: `action = "com.twinstar.toolkit.shell_gui.action.LAUNCH", extra = { "command": Array<String> }`.

## Tool interaction

A tool will output a message to the user or ask for some parameters when it runs; `Shell` and `Shell GUI` provide essentially the same interaction logic.

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

Of the six notification types, `🟣` indicates that the tool is requesting input parameters from the user, and the tool will stop doing anything else and wait for the user to enter until the user has finished typing and then type `Enter to continue...`

When input is requested, a leading character is also displayed as a prompt for the type of parameter. The input value type and format are as follows:

- `U` Pause

	Pause the program to wait for a user response.

- `C` Confirmation value

	A single character ⌈ y ⌋ or ⌈ n ⌋ , indicating ⌈ yes ⌋ and ⌈ no ⌋ .

- `N` number

	A decimal number.

- `I` Integer

	A decimal integer that may not contain a decimal point.

- ` Z` Size

	An unsigned decimal number followed by a binary unit (b = 2^0 , k = 2^10 , m = 2^20 , g = 2^30), typically used to indicate storage capacity.

	> For example, ⌈4k⌋ represents 4096 bytes of storage capacity.

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

## Additional Parameters

The user can pass in additional parameters when starting the tool, or if no additional parameters are provided, the user will be asked to enter them at runtime。 The additional parameters are in the following format：

```
[
	<input>
	[ -disable_filter ]
	[ -method <method-id> ]
	[ -argument <argument-json> ]
]...
```

- `<input>`

	Specifies the input data of the command, usually the path to a file or directory, as the input parameter of the function.

- `[ -disable_filter ]`

	Used to disable function filtering.

	By default, if `-method` is not specified, the tool will filter the available functions for user selection based on the type of input object (mainly by extension);

	If candidate function filtering is disabled, all functions will be listed for user selection. This should not be enabled because toolkit provides too many functions and it is always recommended to have filter on.

- `[ -method <method-id>]`

	Specify the function to be executed, followed by the ID of the function. If no function is specified, it will wait for the user to select the function at runtime.

- `[ -argument <argument-json> ]`

	Specifies the argument to be passed to the function, followed by a JSON string, and must be parsable as an Object.

The IDs of the functions and their arguments are defined in [function](. /method.md) section.

> Example - Using the tool to decode the `test.pam` file on the desktop:
>
> `> . \launch.cmd "C:\Users\TwinStar\Desktop\test.pam" "-method" "popcap.animation.decode"`
>
> This command takes the path to the `test.pam` file as input parameter and specifies the function to be executed as `PopCap Animation Decode`. After evaluate finish, you can see a new file named `test.pam.json` on the desktop, which is the decoded PAM data.

## Forwarding File Objects

The tool is designed to primarily handle file objects in external storage space. The term ⌈ **forwarding** ⌋ refers to launching the tool with the path to the file object as an additional parameter. Forwarding can be done through startup scripts, system extension provided by the forwarder module, etc.

The file object to be processed is forwarded to the tool, and when the tool is launched, the available functions are listed according to the type of that file object, and the user can enter the serial number of the function to be executed.

The tool is preconfigured with many functions such as RTON decoding and RSB unpacking, which are described in [function](./method.md) section.

## Advanced Usage

If you have some programming skills, you can also integrate the tools into your own projects or use them with custom scripts, see the [Advanced](./advanced.md) section.

## Using `Helper`

`Helper` provides additional advanced features for users.

Currently, only PopCap Animate Viewer is available.

1. Launch the application and click `Animation Viewer` on the main page.

2. Click the right button in the `Animation File` text box at the top right of the new page and select the `*.pam.json` animation file in the popup window.

	> `*.pam.json` is obtained by decoding the `*.pam` file with the tool.

3. If the animation's required decomposition is located in the same level of the animation file, the animation can be rendered normally, otherwise, you need to click the right button of the `Image Directory` text box to select the directory where the decomposition is located.

4. Other UI controls allow you to select the sub-animation you want to play, adjust the playback interval and frame rate, set component filters, etc.
