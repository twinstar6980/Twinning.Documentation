# Usage

- [Use `Shell`](#Use-Shell)

- [Use `Shell GUI`](#Use-Shell-GUI)

- [Tool Interaction](#Tool-Interaction)

- [Configuration file](#Configuration-file)

- [Additional parameters](#additional-parameters)

- [Forward File Object](#Forward-File-Object)

- [Script Overview](#Script-Overview)

- [Third-party integration](#Third-party-integration)

- [Use `Helper`](#Use-Helper)

## Using `Shell`

The `Shell` provides a command line interface for the user.

The user needs to start `Shell` in the terminal with command line arguments in the following format:

`<core> <script> <argument>...`

- `<core>`

  The first argument is the path to the core file.

- `<script>`

  The second argument is the script to be passed to the core processing logic. This parameter is a string representing the JS script, or a script file path identified by the `@` as the first character of the script file path.

- `<argument>...`

  The remaining arguments are passed as parameters to the core processing logic.

The main function returns 0 if no errors occur during execution, otherwise 1.

For ease of use, a startup script can be created to start the `Shell` and pass some required arguments. Startup scripts for each platform are provided in the distribution and can be installed by following these steps:

1. Check the [Release page](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Miscellaneous) and download the launch file for your device.

2. Move the downloaded file to your home directory and rename it to `launch`, but keep the original extension.

3. Run to test the launch script and if it works correctly, you will see `Shell` being launched successfully in the terminal.

   > You can pass additional parameters to the launch script and all additional parameters will be forwarded to `Shell` automatically.

## Using the `Shell GUI`

The `Shell GUI` provides a GUI for the user.

The user can use `Shell GUI` as if it were a normal application.

When you open the application for the first time, you need to specify the core, scripts, and parameters to be used at runtime in the application settings, and a typical configuration is as follows:

1. Set the value of the `Core` item to `<home>/core`

2. Set the value of the `Script` item to `@<home>/script/main.js`

3. Set the value of the `Argument` item to `<home>`

> The `<home>` in the above settings needs to be replaced with the path to the home directory.
>
> @ `Android` \
> In `Android`, the `FUSE` mechanism degrades the performance of the app for reading and writing files in external storage, especially for small files; therefore, it is recommended to place the home directory in the app's internal storage directory `/data/user/<id>/<package>` or in the external storage directory `/storage/ emulated/<id>/Android/data/<package>`, which will effectively improve the performance of reading and writing files in the home directory, especially the loading speed of scripts.

Launching the `Shell GUI` in the terminal is also supported, with command line arguments in the following format:

`[<ignore> <additional-argument>...] `

- `<ignore>`

  The first argument is used as an identifier for the command start mode, and its value is ignored.

- `<additional-argument>...`

  The remaining arguments are used as additional arguments to be passed to the core processing logic.

If no command-line arguments are passed, the application is started directly.

If the command line argument is passed, the application will exit automatically after the command has been executed and succeeded (this behavior can be disabled within the application settings).

> Specifying core paths and scripts from the command line is not supported and their values must be set correctly in the app settings first.

For Android, the app also supports launching the app with ⌈File sharing feature ⌋, which is equivalent to launching the app with the file path as an additional parameter. You can select the file object in the third-party app and then forward the file object to the app via the ⌈File sharing feature ⌋.

> The URI passed by the third-party application must explicitly point to the file object in the external storage space with the file or media protocol, otherwise the application cannot resolve to get the real file object.

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

The tool is designed to primarily handle file objects in external storage space. The term ⌈ **forwarding** ⌋ refers to launching the tool with the path to the file object as an additional parameter. Forwarding can be done through startup scripts, Android file sharing function, Windows Explorer extensions, etc.

The file object to be processed is forwarded to the tool, and when the tool is launched, the available functions are listed according to the type of that file object, and the user can enter the serial number of the function to be executed.

The tool is preconfigured with many functions such as RTON decoding and RSB unpacking, which are described in [function](./method.md) section.

## Script Overview

> **For most users, the pre-built features are sufficient, but if you want to improve your productivity with custom scripts, go ahead and learn more. **\
> **The following is geared towards users with basic programming skills. **

This tool uses `JavaScript` as the scripting language. the JS engine is a third-party open source project called `quickjs`.

In short, the shell `Shell` does only two things:

1. Load the ⌈Core library⌋ , which defines the JS-oriented `Core interface`.

   The various functions of the tool are provided to the user by scripts that directly or indirectly call the `Core interface`, which provides basic functions such as file reading and writing, data manipulation, and advanced functions such as BNK and PAM codecs.

   > The interface of `Core interface` has strict type restrictions, therefore, at the development level, `TypeScript` should be used as the development language and compiled into JS for use by tools. Its `.d.ts` declaration is included in the scripting module project.

2. Execute main script whose path is set to `- <home>/script/main.js` by the startup script.

   The tool requires the value computed by the main script to be a function, i.e., a script-level ⌈main function ⌋ which has the following type:

  ```ts
  type JS_MainFunction = (
    data: {
      argument: Array<string>;
      result: string | undefined;
      error: any | undefined;
    },
  ) => void;
  ```

   The tool passes the start argument to call the function until it finishes executing and the tool finishes running.

Users can rewrite the logic of the main script themselves, but it is recommended to build on the existing [Script](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Script), which already provides users with a various set of features with a good interaction and implementatation mechanism.

## Third-party integration

> **The following content is intended for users with basic programming skills. ** The following is for users with basic programming skills.

The tool adopts a front- and back-end separation architecture, divided into three layers:

- Core: back-end, responsible for data processing and will not perform any user interaction; the core is distributed as native dynamic library files for each platform.

- Shell: front-end, responsible for user interaction and will not perform any data processing; shell will load the core dynamic library and call the interface functions of the core dynamic library with the script provided by the user as parameters.

- Script: the bridge between the front and back end, which is responsible for data processing and user interaction by calling the interface functions provided by the core and the shell.

The front and back ends of the tool are completely decoupled, and users can integrate the backend (core) of the tool into their own projects through the `Foreign Function Interface` mechanism provided by various programming languages such as `C`, `C++`, `C#`, `Java`, `Kotlin`, `Python`, and the main steps are as follows:

1. Load the tool's backend (i.e., the dynamic library file distributed by the core module) in a custom project to obtain the core's interface functions.

2. Call the interface function of the core, the `callback` parameter in the interface function is the shell callback implementation that the custom project needs to provide.

Refer to several shell implementations in this tool:

- `Shell` with [`C++`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/core)

- `Shell GUI` with [`Dart`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/ShellGUI/lib/core)

- `Helper` with [`C#`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Helper/Core)

## Using `Helper`

`Helper` provides additional advanced features for users.

Currently, only PopCap Animate Viewer is available.

1. Launch the application and click `Animation Viewer` on the main page.

2. Click the right button in the `Animation File` text box at the top right of the new page and select the `*.pam.json` animation file in the popup window.

   > `*.pam.json` is obtained by decoding the `*.pam` file with the tool.

3. If the animation's required decomposition is located in the same level of the animation file, the animation can be rendered normally, otherwise, you need to click the right button of the `Image Directory` text box to select the directory where the decomposition is located.

4. Other UI controls allow you to select the sub-animation you want to play, adjust the playback interval and frame rate, set component filters, etc.
