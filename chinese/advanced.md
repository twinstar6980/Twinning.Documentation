# 高级应用

- [理解运行流程](#理解运行流程)

- [第三方集成](#第三方集成)

- [自定义脚本](#自定义脚本)

	- [主脚本](#主脚本)

	- [核心接口](#核心接口)

	- [内存操作](#内存操作)

	- [文件系统](#文件系统)

	- [JSON 读写](#JSON-读写)

	- [特殊文件编码解码](#特殊文件编码解码)

	- [核心接口封装](#核心接口封装)

## 理解运行流程

工具采用前后端分离架构，分为三个层级：

* `Core` 层：后端，负责数据处理，不会进行任何用户交互；`Core` 被分发为面向各平台的本机动态库。
	
	> `Core` 模块实现了这一层级。

* `Shell` 层：前端，负责用户交互，不会进行任何数据处理；`Shell` 被分发为面向各平台的本机应用程序，会加载核心动态库，并以用户提供的脚本作为参数调用核心动态库的接口函数。
	
	> `Shell` 、`Shell GUI` 模块分别实现了这一层级。

* `Script` 层：前后端的桥梁，通过调用核心与外壳提供的接口函数进行数据处理与用户交互；`Script` 被分发为不依赖平台的 JavaScript 脚本。
	
	> `Script` 模块实现了这一层级。

通过前后端分离的方式，工具能够方便地进行跨平台适配与多客户端集成。

工具的运行流程如下：

1. 用户启动 `Shell` ，并传递工具运行所需参数。

2. `Shell` 启动后，从用户提供的参数中获取 `Core` 的文件路径、工具需要执行的 `Script` ，以及传递给 `Script` 的参数。

3. `Shell` 加载 `Core` ，调用其接口函数，并将 `Script` 及参数传递给它。

4. `Core` 执行 `Script` ，并将执行结果返回至 `Shell` 。

5. `Shell` 得到返回值，输出给用户，终止程序。
	
	>  `Script` 可以调用 `Shell` 提供给 `Core` 的回调函数，从而在执行期间与用户交互。

## 第三方集成

> **以下内容需要用户掌握一定的编程技能。**

用户可以通过 `C` 、`C++` 、`C#` 、`Kotlin` 、`Python` 在内的各类编程语言所提供的 `Foreign Function Interface` 机制将工具后端（`Core`）集成至自己的项目中，主要步骤如下：

1. 在自定义项目（即自实现的 `Shell` 层）中加载 `Core` 的本机动态库，获取 `Core` 的接口函数。

2. 调用 `Core` 的接口函数，接口函数中的 `callback` 参数是自定义项目所需要提供的外壳回调实现。

具体参照本工具内的几个外壳实现：

* `Shell` with [`C++`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/core)

* `Shell GUI` with [`Dart`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/ShellGUI/lib/core)

* `Helper` with [`C#`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Helper/Core)

下面以 [`Shell`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/core) 模块为例介绍应如何集成 `Core` ：

1. [`interface.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/core/interface.hpp) ：声明 `Core` 的接口。
	
	> `Core` 模块中已经提供了适用于 `C++` 的接口声明 [`interface.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/blob/master/Core/core/interface/interface.hpp) 。

2. [`symbol.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/core/symbol.hpp) ：定义 `Core` 接口的导出符号，用于获取 `Core` 库内的接口的地址。

3. [`library.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/core/library.hpp) ：定义 `Core` 库的抽象类，它封装了对 `Core` 的库加载与符号调用。
	
	* [`static_library.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/core/static_library.hpp) ：实现了对静态库的封装。
	
	* [`dynamic_library.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/core/dynamic_library.hpp) ：实现了对动态库的封装。

4. [`converter.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/core/converter.hpp) ：创建一个辅助类进行所需的 C 式结构体的构造与析构。
	
	>  `Core` 接口需要用户传递 C 式的结构体作为参数值与返回值。

5. [`invoker.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/core/invoker.hpp) ：创建一个辅助类，对 `Core` 接口的调用进行封装，使之能够简单地被调用而不必考虑类型转换在内的调用细节。

6. [`host.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/blob/master/Shell/shell/host/cli_host.hpp) ：定义 `Shell` 端的抽象类，它封装了 `Shell` 端的生命周期，以及需要提供给 `Core` 的回调函数。
	
	> [`cli_host.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/blob/master/Shell/shell/host/cli_host.hpp) ：实现了对 CLI 式 `Shell` 端的封装。

7. [`launcher.hpp`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Shell/shell/host/launcher.hpp) ：创建一个辅助类，用于简单地启动工具。

## 自定义脚本

> **以下内容需要用户掌握 TypeScript 编程技能。**

工具的 `Script` 层以 `JavaScript` 作为脚本语言。JS 引擎为第三方开源项目 `quickjs` 。

用户可以通过自定义脚本让工具执行出自己想要的结果。

用户可以自行重写主脚本的运行逻辑，但更推荐在已有的 [Script 模块](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Script) 基础上扩展开发，它已为用户提供了丰富的功能与良好的交互机制。

下面介绍如何自定义脚本。

### 主脚本

`Core` 只会执行一段脚本字符串或脚本文件（以路径指定），这段脚本称为主脚本。可以在主脚本中加载与执行其他的脚本。

`Core` 要求主脚本计算出的值为一个函数，即脚本层的 ⌈ 主函数 ⌋ ，其拥有如下类型：

```ts
type JS_MainFunction = (
	data: {
		argument: Array<string>;    // 输入。用户传递给工具的脚本参数。
		result: string | undefined; // 输出。脚本的执行结果，必须在程序结束前设置为非 `undefined` 。
		error: any | undefined;     // 输出。脚本运行时发生的错误，如果不为 `undefined` ，工具将视为脚本执行错误并抛出异常。
	},
) => void;
```

主函数必须是同步函数，但其中可以调用异步函数，`Core` 会等待所有异步调用执行完毕。

> 需要注意的是，如果异步执行中抛出的错误未通过 Promise.catch 处理，则这个错误将被静默地忽略。

### 核心接口

工具的 `Core` 负责执行用户提供的脚本，其中定义的核心接口为脚本层提供了多样的功能，例如文件读写、数据操作等基本功能与 BNK 、PAM 编解码等高级功能。

核心接口具有严格的类型限制，因此，在开发层面，应使用 `TypeScript` 作为开发语言，并编译为 `JavaScript` 供工具使用。`Script` 模块中的 [`Core.d.ts`](https://github.com/twinkles-twinstar/TwinStar.ToolKit/blob/master/Script/Core.d.ts) 声明了 `Core` 所定义的接口。

核心接口分为核心类型与核心函数，核心类型是对 `Core` 中的 `C++` 类型的封装，例如接口中的 `Core.Boolean` 类是对 `Core` 中 `C++ Boolean` 类的封装。用户需要借助核心类型才能与核心函数交互。

一般来说，核心类型的类定义中提供以下函数与方法：

* `static default(): T;` ：默认构造函数，构造一个带有默认值的核心对象。

* `static copy(it: T): T;` ：复制构造函数，通过深复制另一个核心对象来构造新的核心对象。

* `static value(it: typeof T.Value): T;` ：值构造函数，从 JS 值构造新的核心对象。

* `get value(): typeof T.Value;` ：值访问器，访问核心对象所对应的 JS 值。

* `set value(it: typeof T.Value);` ：值设定器，设定核心对象所对应的 JS 值。

以下是一些例子：

```ts
/ 构造一个值为 true 的核心布尔对象
let b1 = Core.Boolean.value(true); 
// 通过复制 b1 构造核心布尔对象，b2 值为 true
let b2 = Core.Boolean.copy(b1);
// false ，b1 与 b2 是内存中的不同对象，即使其中的值相等
b1 == b2;
// true ，通过值访问器可以获取核心对象中对应的 JS 值
b1.value === true;
// true ，此时二者存储的值相等
b1.value === b2.value;
// 通过值设定器将 b1 的值设为 false
b1.value = !b2.value;
// true ，此时二者存储的值不等
b1.value !== b2.value;
```

```ts
// 通过 JS 字符串构造核心字符串对象
let s1 = Core.String.value('this is a string value.');
// true
s1.value === 'this is a string value.'
```

```ts
// 测试C盘根目录下是否存在 sample.txt 文件
// 设定路径对象
let target_path = Core.Path.value(`C:/sample.txt`);
// 调用核心函数，传递路径对象，得到 Core.Boolean 对象
let state = Core.FileSystem.exist_file(target_path);
// 取出 Core.Boolean 对象的值
let state_value = state.value;
if (state_value) {
	// 如果存在 C:/sample.txt 文件，则该值为 true
}
```

### 内存操作

核心接口提供了对内存的直接操作功能。

* `Core.ByteArray` ：字节序列容器，持有一段内存空间，当该对象被析构时，其持有的内存也会被释放。这个类型负责申请并持有内存。

* `Core.ByteListView` ：字节序列视图，仅保存内存的地址与尺寸信息，不持有所有权。这个类型用于在不进行深复制的情况下访问内存空间。

* `Core.ByteStreamView` ：字节流视图，相当于字节序列视图附加一个位置信息，位置信息用于表示程序对内存的读取或写入的当前位置，不持有所有权。这个类型被核心函数所广泛使用。

* `Core.CharacterListView` ：字符序列视图，内存层面与 `Core.ByteListView` 等同。

* `Core.CharacterStreamView` ：字符流视图，内存层面与 `Core.ByteStreamView` 等同。

以下是通过上述接口进行内存读写的例子：

```ts
// 申请 16 字节内存空间
let data = Core.ByteArray.allocate(Core.Size.value(16n));
// 获取 data 对应的内存空间的视图，此时 view 与 data 指向同一块内存
let view = data.view();
// 获取 view 对应内存空间的尺寸，此时为 16n 即 16 字节
let size = view.size().value;
// 获取 data 对应的 JS 值，这会得到一个 ArrayBuffer 对象，该对象是对 data 对应内存的深复制，而不指向 data 对应的内存空间。
let data_buffer = data.value;
// 获取 data 对应的 JS 值，这会得到一个 ArrayBuffer 对象，该对象不复制 view 指向的内存空间，它与 view 及 data 指向了同一内存空间。
let view_buffer = data.value;
// 获取 view 中 [4, 12] 区间的内存空间视图。
let view_sub = view.sub(Core.Size.value(4n), Core.Size.value(8n));
// 以 view_sub 为视图源构造流视图
let stream = Core.ByteStreamView.watch(view_sub);
// 将流定位设为 2n ，此时流位于 view_sub 内存空间中的第 3 字节，也是 view 内存空间中的第 11 字节。
stream.set_position(Core.Size.value(2n));
// 获取流定位，此时为 2n
stream.position();
```

```ts
// Core.CharacterListView 无法从 Core.ByteArray 得到，需要从 Core.ByteListView 转换，char_view 将与 view 指向同一内存空间
let char_view = Core.Miscellaneous.cast_ByteListView_to_CharacterListView(view);
// Core.String 对象存储 UTF-8 字符串。
let string = Core.String.value('UTF-8 😄 汉字');
// 还可以从 Core.String 对象获取 Core.CharacterListView ，char_view_of_string 将指向 string 字符串数据的内存空间
let char_view_of_string = Core.Miscellaneous.cast_String_to_CharacterListView(string);
// 如果重设 string 的内容，将导致字符串数据内存的重新分配，此时，char_view_of_string 指向的内存空间不应再被访问
string.value = 'xxx';
// 可以将 Core.String 的内存转交给 Core.ByteArray 对象，Core.String 的内容将变为空，Core.ByteArray 将持有原本归属 Core.String 的内存空间。
let data = Core.Miscellaneous.cast_moveable_String_to_ByteArray(string);
// 同样可以将 Core.ByteArray 的内存转交给 Core.String 但应确保 Core.ByteArray 所持有的内存存储的是 UTF-8 字符串数据
let string_in_memory = Core.Miscellaneous.cast_moveable_String_to_ByteArray(string);
```

### 文件系统

核心接口提供了对本地文件系统的访问功能。

```ts
// 读取 file.txt 中的文本为 JS 字符串，需确保 file.txt 中存储的是 UTF-8 文本
// 设定文件路径
let path = Core.Path.value('file.txt');
// 读取文件数据
let data = Core.FileSystem.read_file(path);
// 通过移动内存的方式将 Core.ByteArray 转换为 Core.String
let string = Core.Miscellaneous.cast_moveable_ByteArray_to_String(data);
// 得到 Core.String 对应的 JS 字符串
let string_value = string.value;
```

```ts
// 遍历某一目录下的子文件
// 设定需要遍历的目录路径
let target = Core.Path.value('C:/dir1');
// 设定遍历深度，如果为null，则遍历所有层级
let depth = Core.SizeOptional.value(2n);
// 获取目录中的所有子文件路径
let path_list = Core.FileSystem.list_file(target, depth);
for (let e of path_list.value) {
	// 遍历子文件路径
}
```

### JSON 读写

核心接口提供了对 JSON 的支持，但与标准 JSON 不同，可以参见 [常见问题](./question.md#JSON-文件格式) 中的说明。

```ts
// 调用 CoreX 中封装的 JSON 字符串解析功能将 JS 字符串转换为 Core.JSON.Value 对象
let j1 = CoreX.JSON.read_s<{
	int: bigint;
	num: number;
}>(`{ "int": 0, "num": 1.2 }`);
let j2 = Core.JSON.Value.value({
	int: 0,
	num: 1.2,
});
j1.value.int === j2.value.int; // false ，因为 j1 中的 int 属性值类型为 bigint ，而 j2 为 number
```

### 特殊文件编码解码

核心接口提供了对多种特殊文件的编码、解码支持，例如 PTX、PAM 、BNK 、RSB 。

核心接口一般是基于内存的，如果需要对文件进行解码，需要先读取文件内容到内存，再调用处理函数进行解码，下面以解码BNK文件为例：

```ts
// 对 Core.Tool.Wwise.SoundBank.Decode 接口的封装
export function decode_fs(
	data_file: string, // BNK文件路径
	manifest_file: string, // 输出的清单文件路径，即BNK中的数据信息
	embedded_media_directory: string, // 输出的BNK内嵌WEM路径
	version: typeof Core.Tool.Wwise.SoundBank.Version.Value, // BNK文件版本
): void {
	// 文件版本
	let version_c = Core.Tool.Wwise.SoundBank.Version.value(version);
	// 从外存读取数据到内存
	let data = FileSystem.read_file(data_file);
	// 构造流视图对象，作为解码函数的输入
	let stream = Core.ByteStreamView.watch(data.view());
	// 构造清单对象，作为解码函数的输出
	let manifest = Core.Tool.Wwise.SoundBank.Manifest.SoundBank.default();
	// 调用解码函数，完成之后，stream.position() 将存储已读取的数据总数，这可以用于判别BNK文件的实际尺寸
	Core.Tool.Wwise.SoundBank.Decode.process_sound_bank(stream, manifest, Core.PathOptional.value(embedded_media_directory), version_c);
	// 将解码得到的清单数据存储保存为文件
	CoreX.JSON.write_fs(manifest_file, manifest.get_json(version_c));
	return;
}
// 解码BNK文件
decode_fs(
	'C:/sample.bnk',
	'C:/sample.bnk.bundle/manifest.json',
	'C:/sample.bnk.bundle/embedded_media_directory',
	{ number: 140n },
);
```

### 核心接口封装

核心接口的调用较为繁琐，`Script` 中已经对核心接口进行了封装，使用户能够方便地使用核心接口，包括以下内容：

* `utility.CoreX` ：对大部分核心接口进行封装，包括文件系统功能、JSON与XML的字符串或文件式的序列化功能、特殊文件的文件式编码解码功能、等。

* `utility.ThreadManager` ：对线程功能进行封装。

* `utility.ProcessHelper` ：对进程功能进行封装，可以通过它执行其他可执行程序。

* `utility.Shell` ：对外壳回调与外壳专有功能的封装。

* `utility.Console` ：对控制台交互的封装，为 Shell 与 ShellGUI 提供一致的用户交互。

* `...`
