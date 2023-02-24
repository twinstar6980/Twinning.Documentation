# 常见问题

- [Windows 文件路径长度限制](#Windows-文件路径长度限制)

- [JSON 文件格式](#JSON-文件格式)

- [Shell 对宿主终端的要求](#Shell-对宿主终端的要求)

## Windows 文件路径长度限制

在 Windows 中，系统的文件路径的长度限制在了 260 字符内，这使得系统中不会存在长路径文件，工具也无法处理任何长路径文件。

自 Windows 10+ 开始，系统提供了长路径支持，但这在默认情况下是关闭的，请参阅 [微软官方文档](https://learn.microsoft.com/windows/win32/fileio/maximum-file-path-limitation?tabs=registry) 以启用。启用长路径支持后，工具将能够正常处理长路径文件。

> 在一些应用场景下（例如解包存在长路径文件资源的 RSB 文件），工具输出的文件可能具有长路径，此时工具将因文件读写失败而提前终止。因此，推荐用户始终为系统启用长路径支持。

## JSON 文件格式

工具的 JSON 读写规则是自实现的，并且不完全遵循 JSON 标准，差异如下：

1. 注释
	
	JSON 标准不允许注释，但工具允许注释，包括行注释 `// ...` 与块注释 `/* ... */` 。

2. 尾随逗号
	
	数组与对象的最末元素后的逗号称为尾随逗号，JSON 标准不允许尾随逗号的存在，但在 JS 及许多编程语言中，尾随逗号是被允许乃至被推荐使用的语法。考虑到尾随逗号为 JSON 读写提供的便利，工具支持尾随逗号，并且在输出 JSON 时默认添加尾随逗号。
	
	在一些编辑器中，尾随逗号会被默认视为格式错误，用户可以通过修改 `- <home>/script/Entry/Entry.json` 配置中的 `json_format.disable_trailing_comma` 项为 `true` 以禁止尾随逗号的输出。
	
	> 在 `VS Code` 中，可以将以下配置添加进 `settings.json` 中，以使编辑器允许 JSON 尾随逗号：
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
	> 其中，`allowTrailingCommas` 是 `VS Code` 的扩展，并不符合 `JSON-Schema` 标准。

3. 转义字符
	
	工具的转义字符支持不遵从 JSON 标准，转义规则如下：
	
	* `\\ \' \"` 转义为原字符。
	
	* `\a \b \f \n \r \t \v` 转义为对应的控制字符。
	
	* `\0` 转义为空字符。
	
	* `\onnn` 3 位八进制数表示的 Unicode 字符，允许值超过 0xFF 。
	
	* `\xnn` 2 位十六进制数表示的 Unicode 字符。
	
	* `\unnnn` 4 位十六进制数表示的 Unicode 字符。
	
	* `\Unnnnnnnn` 8 位十六进制数表示的 Unicode 字符。

4. 数字格式
	
	工具的数字格式支持不遵从 JSON 标准，格式规则如下：
	
	* 整数：匹配正则 `^[+-]?[\d]+$` 。
	
	* 浮点数：匹配正则 `^[+-]?[\d]+[.][\d]+$` 。
	
	* 科学计数法表示的浮点数：匹配正则 `^[+-]?[\d]+[.][\d]+[e][+-][\d]+$` 。
	
	换言之，与 JSON 标准的差异如下：
	
	* 允许正数具有正号，无符号数视为正数。
	
	* 浮点数必须以数字起始，必须具有小数点，小数点后必须跟随数字。
	
	* 科学计数法表示的浮点数必须以数字起始，必须具有小数点，小数点后必须跟随数字；只能使用小写 `e` 而不允许大写 `E` ，其后必须跟随正号或负号。

## Shell 对宿主终端的要求

`Shell` 提供了基于终端的命令行界面，但需要宿主终端支持以下特性：

1. UTF-8 输入/输出：必需，若不支持，将导致程序无法正常进行输入输出。

2. 虚拟终端控制序列：可选，若不支持，将导致程序无法对不同类型的文本修饰以不同的颜色。
	
	> 默认情况下，工具会使用控制序列来优化输出效果，但如果运行在不支持控制序列的终端中，控制序列将直接输出为字符串，影响用户的阅读。
	> 
	> 用户可以通过修改 `- <home>/script/Entry/Entry.json` 配置中的 `cli_disable_virtual_terminal_sequences` 项为 `true` 以禁用控制序列的使用。

3. 完备的字体：可选，若不支持，一些字符（如汉字、emoji ）将无法正常显示。

有些操作系统的默认终端不提供（或默认关闭）这些支持，用户可以安装第三方终端并在其中运行本程序，可以参照以下列表：

* Windows 7 ~ 8
	
	自带 cmd 不提供支持，推荐使用 [cmder](https://cmder.app/) 。

* Windows 10
	
	自带 cmd 默认关闭支持，推荐使用 [Windows Terminal](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701) 。

* Windows 11
	
	自带 cmd 默认关闭支持，自带 Windows Terminal 提供支持，请将其设置为系统的默认终端。

* Macintosh 12.0.1+
	
	自带终端提供支持。

* Linux - Ubuntu 20.04+
	
	自带终端提供支持。

* Android
	
	推荐使用 [Termux](https://termux.dev/en/) 。
	
	> 当然，你也可以通过 ADB 来使用桌面系统的终端。

> @ `Windows` \
> 双击以运行启动脚本时，将在系统默认终端中运行程序，若想更改为在指定第三方终端中运行，请修改 `launch.cmd` ，但切换终端将导致一次 cmd 窗口闪烁，除非你使用 Windows 11 并将 Windows Terminal 设为系统默认终端。
