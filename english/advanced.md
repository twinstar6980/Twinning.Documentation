# Advanced

- [Understanding Operational Flow](#Understanding-Operational-Flow)

- [Third Party Integration](#Third-Party-Integration)

- [Custom Scripts](#Custom-Scripts)

	- [Main Script](#Main-Script)

	- [Kernel Interface](#Kernel-Interface)

	- [Memory operations](#Memory-operations)

	- [File System](#File-System)

	- [JSON Read/Write](#JSON-Read/Write)

	- [Special file encoding and decoding](#Special-file-encoding-and-decoding)

	- [Kernel Interface Encapsulation](#Kernel-Interface-Encapsulation)

## Understanding Operational Flow

The tool uses a front-end and back-end separation architecture, divided into three part:

* `Kernel`: the back-end, which is responsible for data processing and does not perform any user interaction; `Kernel` is distributed as a native dynamic library for each platform.
	
	> `Kernel` module implements this layer.

* The `Shell`: the front-end, responsible for user interaction without any data processing; `Shell` is distributed as a native application for each platform, which will load the kernel dynamic library and call the interface functions of the kernel dynamic library with user-supplied scripts as arguments.
	
	> `Shell` and `Assistant` and `Assistant Plus` are modules implement this layer.

* `Script`: the bridge between the front and back end, which performs data processing and user interaction by calling interface functions provided by the kernel and shell; `Script` is distributed as platform-independent JavaScript scripts.
	
	> `Script` module implements this layer.

By separating the front and back ends, the tool enables easy cross-platform adaptation and multi-client integration.

The runtime flow of the tool is as follows:

1. The user starts `Shell` and passes the arguments needed to run the tool.

2. `Shell` starts and gets the file path of `Kernel`, the `Script` that the tool needs to execute, and the arguments passed to `Script` from the arguments provided by the user. The user can perform drag and drop or input path to the tool to provide the tool arguments.

3. `Shell` loads `Kernel`, calls its interface function, and passes `Script` and arguments to it.

4. `Kernel` executes `Script` and returns the result to `Shell`.

5. `Shell` gets the return value, outputs it to the user, and terminates the program.
	
	> `Script` can call the callback functions provided by `Shell` to `Kernel`, thus interacting with the user during execution.

## Third Party Integration

> **The following requires the user to have some programming skills.**

Users can integrate the tool backend (`Kernel`) into their projects through the `Foreign Function Interface` provided by various programming languages including `C`, `C++`, `C#`, `Kotlin`, `Python`, with the following main steps:

1. Loads the native dynamic library of `Kernel` in your project (a implement of `Shell`) to get the interface functions of `Kernel`.

2. Call the interface function of `Kernel`, the `callback` argument in the interface function is the shell callback implementation that the custom project needs to provide.

Refer to several shell implementations in this tool:

* [`Shell`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/bridge) with `C++`

* [`Assistant`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Assistant/lib/bridge) with `Dart`

* [`Assistant Plus`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/AssistantPlus.Windows/Bridge) with `C#`

The following is an example of how to integrate `Kernel` with the [`Shell`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/bridge) modules:

1. [`data.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/bridge/data.hpp): Declare the data type of `Kernel`.
	
	> The declaration of the interface for `C++` is already provided in the `Kernel` module [`interface.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/blob/master/Kernel/kernel/interface/interface.hpp)

2. [`proxy.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/bridge/proxy.hpp) : Creates a helper class for the `C-style` structure parsing and constructing.

3. [`service.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/bridge/service.hpp): defines the service type of the `Kernel`.

4. [`library.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/bridge/library.hpp): defines the abstract class of the `Kernel` library, which encapsulates the library loading and symbolic getting to `Kernel`.

5. [`client.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/blob/master/Shell/shell/bridge/client.hpp): Define a `Shell` client abstract class.
	
	> [`main_console_bridge_client.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/blob/master/Shell/shell/main_console_bridge_client.hpp): implements the CLI-style `Shell` client.

6. [`launcher.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/bridge/launcher.hpp) : Create a helper class to call the `Kernel` interface with `Library` and `Client` instances.

## Custom Scripts

> **The following requires the user to have TypeScript programming skills.**

The `Script` layer of the tool uses `JavaScript` as the scripting language. JS engine is a third-party open source project called `quickjs`.

Users can customize scripts to make the tool execute the results they want.

You can rewrite the logic of the main script yourself, but it is recommended to build on the existing [Script module](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Script), which already provides a rich set of functions with a good interaction mechanism.

The following section describes how to customize the script.

### Main Script

`Kernel` only executes a script string or script file (specified by path), which is called the main script. It is possible to load and execute other scripts within the main script.

`Kernel` requires that the value computed by the main script be a function, i.e., a ⌈ main function ⌋ at the script level, which has the following type:

```ts
type JS_MainFunction = (data: {
	argument: Array<string>;           // Input. The script argument passed to the tool by the user.
	result: Array<string> | undefined; // Output. The result of the script execution, which must be set to `non-undefined` before the end of the program.
	error: any | undefined;            // Output. If the error occurs while the script is running and is not `undefined`, the tool will treat the script execution as an error and throw an exception.
}) => void;
```

The main function must be synchronous, but asynchronous functions can be called within it, and `Kernel` will wait for all asynchronous calls to finish executing.

> Note that if an error thrown during asynchronous execution is not handled by `Promise.catch`, the error will be silently ignored.

### Kernel Interface

The `Kernel` of the tool is responsible for executing the scripts provided by the user. The kernel interface defined therein provides various functions for the scripting layer, such as basic functions like file reading and writing, data manipulation, and advanced functions like BNK, PAM, etc...

The kernel interface has strict type restrictions, so at the development level, `TypeScript` should be used as the development language and compiled to `JavaScript` for use in tools. The [`Kernel.d.ts`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/blob/master/Script/Kernel.d.ts) in the `Script` module declares the interfaces defined by `Kernel`.

Kernel interfaces are divided into kernel types and kernel functions. Kernel types encapsulate the `C++` types in `Kernel`, for example, the `Kernel.Boolean` class in the interface encapsulates the `C++ Boolean` class in `Kernel`. The user needs the kernel type in order to interact with the kernel functions.

In general, the following functions and methods are provided in the class definition of the kernel type:

* `static default(): T;` : default constructor that constructs a kernel object with default values.

* `static copy(it: T): T;` : copy constructor that constructs a new kernel object by making a deep copy of another kernel object.

* `static value(it: typeof T.Value): T;` : value constructor that constructs a new kernel object from a JS value.

* `get value(): typeof T.Value;` : value accessor that accesses the JS value corresponding to the kernel object.

* `set value(it: typeof T.Value);` : value setter, sets the JS value corresponding to the kernel object.

The following are some examples:

```ts
// Constructs a kernel boolean obj with the value `true`
let b1 = Kernel.Boolean.value(true);
// Construct a kernel boolean object by copying b1 with a b2 value of true
let b2 = Kernel.Boolean.copy(b1);
// false, b1 and b2 are different objects in memory, even if their values are equal
b1 == b2.
// true , the value accessor can get the corresponding JS value in the kernel object
b1.value === true;
// true, when the two stored values are equal
b1.value === b2.value;
// set the value of b1 to false via the value setter
b1.value = !b2.value;
// true, where the two stored values are not equal
b1.value ! == b2.value;
```

```ts
// Construct a kernel string object from a JS string
let s1 = Kernel.String.value("this is a string value.");
// true
s1.value === "this is a string value.";
```

```ts
// test if the sample.txt file exists in the root directory of the C drive
// Set the path object
let target_path = Kernel.Path.value(`C:/sample.txt`);
// Call the kernel function, pass the path object, and get the Kernel.
let state = Kernel.FileSystem.exist_file(target_path);
// fetch the value of the Kernel.Boolean object
let state_value = state.value;
if (state_value) {
	// If the C:/sample.txt file exists, then the value is true
}
```

### Memory operations

The kernel interface provides directly memory operations functions.

* `Kernel.ByteArray` : A byte sequence container that holds a segment of memory space that is freed when the object is destructured. This type is responsible for requesting and holding memory.

* `ByteListView` : A byte sequence view that holds only the address and size information of the memory and does not hold ownership. This type is used to access the memory space without deep copy.

* `ByteStreamView`: Byte stream view, equivalent to the byte sequence view with an additional location information, which is used to indicate the current location of the program reading or writing to memory and does not hold ownership. This type is widely used by kernel functions.

* `Kernel.CharacterListView` : Character sequence view, memory level equivalent to `Kernel`.

* `Kernel.CharacterStreamView` : Character stream view, memory level is the same as `Kernel.ByteStreamView`.

The following are examples of memory reads and writes through the above interfaces:

```ts
// requires 16 bytes of memory space
let data = Kernel.ByteArray.allocate(Kernel.Size.value(16n));
// Get the view of the memory space corresponding to data, when view and data point to the same block of memory
let view = data.view();
// get the size of the memory space corresponding to view, which is 16n, i.e. 16 bytes
let size = view.size().value;
// Get the JS value corresponding to data, which will result in an ArrayBuffer object that is a deep copy of the memory corresponding to data, and does not point to the memory space corresponding to data.
let data_buffer = data.value;
// Get the JS value corresponding to data, which will result in an ArrayBuffer object that does not copy the memory space pointed to by view, but points to the same memory space as view and data.
let view_buffer = view.value;
// Get a view of the memory space between [4, 12] in view.
let view_sub = view.sub(Kernel.Size.value(4n), Kernel.Size.value(8n));
// Construct a stream view with view_sub as the view source
let stream = Kernel.ByteStreamView.watch(view_sub);
// Set the stream position to 2n, where the stream is located in byte 3 of view_sub's memory space and byte 11 of the view memory space.
stream.set_position(Kernel.Size.value(2n));
// Get the position of the stream, which is 2n
stream.position();
```

```ts
// The Kernel.ByteListView need to convert from Kernel.ByteArray, char_view will point to the same memory space as view
let char_view = Kernel.Miscellaneous.cast_ByteListView_to_CharacterListView(view);
// Kernel.String object stores UTF-8 strings.
let string = Kernel.String.value("UTF-8 😄 汉字");
// Kernel.CharacterListView from Kernel.String object , char_view_of_string will point to the memory space of string string data
let char_view_of_string = Kernel.Miscellaneous.cast_String_to_CharacterListView(string);
// If the content of string is reset, this will result in a reallocation of memory for the string data, at which point the memory space pointed to by char_view_of_string should no longer be accessed
string.value = "xxx";
// Move the memory from Kernel.String to Kernel.ByteArray, the contents of Kernel.String will become empty and Kernel.ByteArray will hold the memory space originally belonging to Kernel.
let data = Kernel.Miscellaneous.cast_moveable_String_to_ByteArray(string);
// Move the memory from Kernel.ByteArray to Kernel.String, but make sure that the memory data is a UTF-8 string.
let string_in_memory = Kernel.Miscellaneous.cast_moveable_String_to_ByteArray(string);
```

### File System

The kernel interface provides access to the local file system.

```ts
// Read the text in file.txt as a JS string, make sure that file.txt stores UTF-8 text
// Set the path to the file
let path = Kernel.Path.value("file.txt");
// read the file data
let data = Kernel.FileSystem.read_file(path);
// convert Kernel.ByteArray to Kernel.String by moving memory
let string = Kernel.Miscellaneous.cast_moveable_ByteArray_to_String(data);
// Get the JS string corresponding to Kernel.
let string_value = string.value;
```

```ts
// Iterate through the subfiles in a directory
// set the path of the directory to be traversed
let target = Kernel.Path.value("C:/dir1");
// set traversal depth, if null, traverse all levels
let depth = Kernel.SizeOptional.value(2n);
// get all the subfile paths in the directory
let path_list = Kernel.FileSystem.list_file(target, depth);
for (let e of path_list.value) {
	// iterate through the subfile paths
}
```

### JSON Read/Write

The kernel interface provides support for JSON, but differs from standard JSON, as can be seen in [FAQ](./question.md#JSON-file-format) for a description.

```ts
// Call the JSON parsing function wrapped in KernelX to convert a JS string to a Kernel.JSON.Value object
let j1 = KernelX.JSON.read_s<{
	int: bigint;
	num: number;
}>(`{ "int": 0, "num": 1.2 }`).
let j2 = Kernel.JSON.Value.value({
	int: 0,
	num: 1.2,
}).
j1.value.int === j2.value.int; // false, because the value of the int property in j1 is of type bigint, while j2 is number
```

### Special file encoding and decoding

The kernel interface provides support for encoding and decoding many kinds of special files, such as PTX, PAM, BNK, RSB, RSG.

The kernel interface is generally memory-based, so if you need to decode a file, you need to read the file content into memory first, and then call the processing function to decode it, taking the following example of decoding BNK files:

```ts
// Wwise.SoundBank.Decode interface
export function decode_fs(
	data_file: string, // BNK file path
	definition_file: string, // path to the exported definition file, i.e. the data information in BNK
	embedded_media_directory: string, // The path to the BNK embedded WEM for export
	Version: typeof Kernel.Tool.Wwise.SoundBank
): void {
	// Version number
	let version_c = Kernel.Tool.Wwise.SoundBank.Version.value(version);
	// Read data from external memory to memory
	let data = FileSystem.read_file(data_file);
	// Construct the stream view object as input to the decode function
	let stream = Kernel.ByteStreamView.watch(data.view());
	// Construct the definition object as the output of the decode function
	let definition = Kernel.Tool.Wwise.SoundBank.Definition.SoundBank.default();
	// Call the decode function and when it's done, stream.position() will store the total amount of data read, which can be used to discern the actual size of the BNK file
	Kernel.Tool.Wwise.SoundBank.Decode.process_sound_bank(
		stream,
		definition,
		Kernel.PathOptional.value(embedded_media_directory),
		version_c
	)
	// Store and save the decoded definition data as a file
	KernelX.JSON.write_fs(definition_file, definition.get_json(version_c));
	return;
}
// Decode the BNK file
decode_fs(
	"C:/sample.bnk",
	"C:/sample.bnk.bundle/definition.json",
	"C:/sample.bnk.bundle/embedded_media",
	{ number: 140n },
);
```

### Kernel interface encapsulation

Kernel interfaces are cumbersome to call. `Script` has encapsulated the kernel interfaces so that users can use them easily, including the following:

* `KernelX`: encapsulates most of the kernel interfaces, including file system functions, string or file-based serialization of JSON and XML, file-based encoding and decoding of special files, etc.

* `ThreadManager`: encapsulates the threads function.

* `ProcessHelper`: Wraps the process functions, which can be used to execute other executable programs.

* `Shell`: Wraps shell callbacks and shell-specific functions.

* `Console`: Wraps console interaction, providing consistent user interaction between difference `Shell` clients.

* `...`
