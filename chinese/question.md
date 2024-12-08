# 常见问题

- [版本兼容性与旧版本下载](#版本兼容性与旧版本下载)

- [存储空间访问权限](#存储空间访问权限)

- [Windows 文件路径长度限制](#Windows-文件路径长度限制)

- [Android FUSE 性能影响](#Android-FUSE-性能影响)

- [Android Content URI 处理方式](#Android-Content-URI-处理方式)

- [Shell 对宿主终端的要求](#Shell-对宿主终端的要求)

- [JSON 文件格式](#JSON-文件格式)

- [严格的文件处理机制](#严格的文件处理机制)

- [处理大端序文件](#处理大端序文件)

- [处理 OBB 文件](#处理-OBB-文件)

- [处理 SMF 文件](#处理-SMF-文件)

## 版本兼容性与旧版本下载

出于修复 BUG 、加强语义正确性等目的，工具的更新可能引入破坏性更改，导致工具的新、旧版本生成的工程文件互不兼容，用户将无法在新版中使用旧版生成的工程文件，反之亦然。

当某次更新引入了破坏性更改后，用户需要先使用旧版工具将工程文件编码为二进制格式（如 `*.pam.json` 编码为 `*.pam` 、`*.rsb.bundle` 打包为 `*.rsb` ），再更新到新版，并对这些二进制格式进行解码，得到适用于新版工具的工程文件。

破坏性更改会在 [更新日志](../CHANGELOG.md) 中标注。用户应当始终使用最新版本，并只在需要进行工程文件版本迁移时使用旧版本。GitHub Release 中的分发仅为最新版本，所有历史版本分发可以在我的 [个人网盘](https://1drv.ms/f/s!AkIzoME-1oU-fB6En185husw59Q?e=EKJ1e9) 中找到。

## 存储空间访问权限

工具需要拥有对所所涉及的存储空间的访问权限。

如果使用的外壳模块为 `Shell` ，则访问权限取决于启动程序的用户所具备的权限。

如果使用的外壳模块为 `Assistant` ，则访问权限根据运行系统的不同而不同：

* `Windows` `Linux` `Macintosh`
	
	取决于启动程序的用户所具备的权限。

* `Android`
	
	* 应用的专属内部存储空间：`/data/user/<user>/<package>` 。
	
	* 应用的专属外部存储空间：`/storage/emulated/<user>/Android/data/<package>` 、`/storage/emulated/<user>/Android/obb/<package>` 。
	
	* 系统的共享外部存储空间：`/storage/emulated/<user>` ，但不包括 `Android/data` 、`Android/obb` 。
		
		> 仅当应用取得了外部存储空间的完全访问权限时才可访问该目录。
		> 
		> 该目录实际上是 `/data/media/<user>` 的映射。通过 `/storage/emulated/<user>` 路径访问文件时，会受 FUSE 影响而导致访问性能的降低，并且访问权限被硬编码为 RW ，文件无法被作为程序执行；可以通过 `/data/media/<user>` 路径访问文件以绕过 FUSE ，但需要 ROOT 权限。

* `Iphone`
	
	* 应用的专属沙盒存储空间：`/var/mobile/Containers/Data/Application/<application>` 。
	
	* 其他应用的共享存储空间：`/var/mobile/Containers/Data/Application/<application>/Documents` 。
	
	> 以上存储空间内的文件无法被作为程序执行。

## Windows 文件路径长度限制

在 Windows 中，系统的文件路径的长度限制在了 260 字符内，这使得系统中不会存在长路径文件，工具也无法处理任何长路径文件。

自 Windows 10+ 开始，系统提供了长路径支持，但这在默认情况下是关闭的，请参阅 [微软官方文档](https://learn.microsoft.com/windows/win32/fileio/maximum-file-path-limitation?tabs=registry) 以启用。启用长路径支持后，工具将能够正常处理长路径文件。

> 在一些应用场景下（例如解包存在长路径文件资源的 RSB 文件），工具输出的文件可能具有长路径，此时工具将因文件读写失败而提前终止。因此，推荐用户始终为系统启用长路径支持。

## Android FUSE 性能影响

[Android FUSE](https://source.android.com/docs/core/storage/scoped) 会严重影响应用对系统的外部存储空间的访问性能，这在处理大量文件时尤为突出。

为了提高工具的执行效率，可以考虑将工具的主目录移动至非外部存储空间中。

> 外部存储空间 `/storage/emulated/<user>` 内的 `Android/data` 与 `Android/obb` 不受 FUSE 的影响。

## Android Content URI 处理方式

由于 Android 的系统限制，用户所转发或选择的文件以 Content URI（而非绝对路径）的形式传递给应用，`Assistant` 接收到 Content URI 后，会对其依次进行以下转换：

1. 尝试解析 URI ，得到其对应的绝对路径。

2. 若无法解析出绝对路径，应用会弹出确认对话框：若选择“复制”，应用会将 URI 对应的文件复制到用户定义的回退目录中，并返回副本文件的绝对路径；若选择“忽略”，应用会返回 null ，视为接收了无效路径。

下表列出了受支持的 Content URI 格式，应用能够从以下几类 Content URI 中解析出绝对路径：

| 应用名称         | URI Provider                                | URI Path              |
|:----------------:|:-------------------------------------------:|:---------------------:|
| AOSP DocumentsUI | com.android.externalstorage.documents       | /document/primary:... |
| AOSP DocumentsUI | com.android.externalstorage.documents       | /tree/primary:...     |
| Material Files   | me.zhanghai.android.files.file_provider     | /file://...           |
| Root Explorer    | com.speedsoftware.rootexplorer.fileprovider | /root/...             |
| Solid Explorer   | pl.solidexplorer2.files                     | /...                  |
| MT Manager       | bin.mt.plus.fp                              | /...                  |
| NMM              | in.mfile.files                              | /...                  |

> 建议使用上表中提及的第三方文件管理器应用，以避免不必要的文件复制开销。

## Shell 对宿主终端的要求

`Shell` 提供了基于终端的命令行界面，但需要宿主终端支持以下特性：

1. UTF-8 输入/输出：必需，若不支持，将导致程序无法正常进行输入输出。

2. 虚拟终端控制序列：可选，若不支持，将导致程序无法对不同类型的文本修饰以不同的颜色。
	
	> 默认情况下，工具会使用控制序列来优化输出效果，但如果运行在不支持控制序列的终端中，控制序列将直接输出为字符串，影响用户的阅读。
	> 
	> 用户可以通过修改 `<home>/script/Entry/Entry.json` 配置中的 `disable_basic_virtual_terminal_sequence` 项为 `true` 以禁用控制序列的使用。

3. 完备的字体：可选，若不支持，一些字符（如汉字、emoji ）将无法正常显示。

有些操作系统未提供终端程序，或默认终端程序不提供（或默认关闭）这些支持，用户可以安装第三方终端并在其中运行本程序，可以参照以下列表：

* Windows - [Windows Terminal](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701)

* Android - [Termux](https://termux.dev/en/)

* iPhone - [Filza](https://www.tigisoftware.com/default/?page_id=78)

> @ `Windows` \
> 双击以运行启动脚本时，将在系统默认终端中运行程序，若想更改为在指定第三方终端中运行，请修改 `launch.cmd` ，但切换终端将导致一次 cmd 窗口闪烁，除非你使用 Windows 11 并将 Windows Terminal 设为系统默认终端。

## JSON 文件格式

工具的 JSON 读写规则是自实现的，并且不完全遵循 JSON 标准，差异如下：

1. 注释
	
	JSON 标准不允许注释，但工具允许注释，包括行注释 `// ...` 与块注释 `/* ... */` 。

2. 尾随逗号
	
	数组与对象的最末元素后的逗号称为尾随逗号，JSON 标准不允许尾随逗号的存在，但在 JS 及许多编程语言中，尾随逗号是被允许乃至被推荐使用的语法。考虑到尾随逗号为 JSON 读写提供的便利，工具支持尾随逗号，并且在输出 JSON 时默认添加尾随逗号。
	
	在一些编辑器中，尾随逗号会被默认视为格式错误，用户可以通过修改 `<home>/script/Entry/Entry.json` 配置中的 `json_format.disable_array_trailing_comma` 与 `json_format.disable_object_trailing_comma` 项为 `true` 以禁止尾随逗号的输出。
	
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
	
	* `\oNNN` 3 位八进制数表示的 Unicode 字符。
	
	* `\xNN` 2 位十六进制数表示的 Unicode 字符。
	
	* `\uNNNN` 4 位十六进制数表示的 Unicode 字符。
	
	* `\UNNNNNNNN` 8 位十六进制数表示的 Unicode 字符。

4. 数字格式
	
	工具的数字格式支持不遵从 JSON 标准，格式规则如下：
	
	* 整数：匹配正则 `^[+-]?[\d]+$` 。
	
	* 浮点数：匹配正则 `^[+-]?[\d]+[.][\d]+$` 。
	
	* 科学计数法表示的浮点数：匹配正则 `^[+-]?[\d]+[.][\d]+[e][+-][\d]+$` 。
	
	换言之，与 JSON 标准的差异如下：
	
	* 允许正数具有正号，无符号数视为正数。
	
	* 浮点数必须以数字起始，必须具有小数点，小数点后必须跟随数字。
	
	* 科学计数法表示的浮点数必须以数字起始，必须具有小数点，小数点后必须跟随数字；只能使用小写 `e` 而不允许大写 `E` ，其后必须跟随正号或负号。
	
	此外，工具还严格区分整数与浮点数，整数被解码为 JS 中的 `bigint` ，而浮点数被解码为 JS 中的 `number` ，用户使用错误的值类型将导致潜在 BUG 甚至运行失败。

## 严格的文件处理机制

对数据的宽松处理（容错）会导致一些潜在 BUG 与难以预料的行为，因此工具采用严格的文件处理机制，任何非标准的文件数据都将导致工具处理失败，即使它们能被某些游戏程序正常读取（由于容错机制的存在）。严格的处理机制是工具的预期行为，并将始终如此。

典型的例子是 RTON 文件不以 `RTON` 作为文件起始四字节，虽然 PvZ-2 游戏能正常读取此类文件，但工具无法解码它们，因为这被工具认为是文件数据结构异常。

若要处理非标准的文件，用户需要修复其中的异常结构段，或者对工具的 `Kernel` 模块进行修改与重编译，对数据的严格性判断一般是通过 `Kernel` 模块的 `assert_test` 代码段进行值断言，可以通过删除这些断言语句以解除大多数的严格性判断。

此外，工具也在脚本层实现了额外的 `PopCap Reflection-Object-Notation 宽松解码` 与 `PopCap Resource-Stream-Bundle 宽松解包` 功能，它们可以用于处理非标准的 RTON 文件与 RSB 文件。

## 处理大端序文件

工具默认将处理的文件始终视为小端序。但是，用户也可能需要对大端序文件进行处理，例如对从大端序设备 Xbox 中提取的 RSB 文件进行解包。

在将大端序文件转发给工具进行处理前，用户需要修改 `<home>/script/Entry/Entry.json` 配置中的 `byte_stream_use_big_endian` 项为 `true` ，告知工具将传入的文件视为大端序。

之后，将大端序文件转发给工具，如果文件本身没有错误，程序将能正常处理这些大端序文件。

> 如果后续需要处理小端序文件，应当恢复 `byte_stream_use_big_endian` 项为 `false` 。

## 处理 OBB 文件

安卓平台的 PvZ-2 以 `OBB` 扩展包文件的形式保存 RSB 数据包文件。OBB 是安卓应用程序扩展数据包的通用扩展名，并不具备可辨别性，OBB 文件可以存储任何格式的数据，而不只是 RSB 数据包格式。

基于此，工具并不认为 `.obb` 文件是 RSB 文件，如果直接将 `.obb` 文件转发至工具，用户将无法从备选项中看到 RSB 相关功能。如果用户确定 `.obb` 文件中存储的是 RSB 数据包格式，需要将文件扩展名从 `.obb` 改为 `.rsb` 才可让工具提供 RSB 相关功能选项。

此外，也可创建指向 `.obb` 文件的软链接，并以 `.rsb` 作为链接的扩展名，这可以避免重打包后用户还需将 `.rsb` 重命名为 `.obb` 的麻烦。

> 可以通过修改 `<home>/script/Entry/method/popcap.resource_stream_bundle.ts` 中对 RSB 文件进行筛选的正则表达式，以允许工具识别 `*.obb` 文件为 RSB 文件。

## 处理 SMF 文件

`SMF` 本身是一种媒体文件的扩展名，工具并不支持处理真正的 SMF 文件。

`SMF` 文件在安卓开发中具有一个特性：当安卓项目打包为 APK 时，打包程序不会对项目资产中的 SMF 文件进行压缩（根据扩展名）。

PopCap 利用了这一特性，在一些 PopCap 游戏的安卓端 APK 安装包中存在 SMF 文件，但它们并不是真正的 SMF 文件，而可能是任何类型的文件。

用户可以直接从 APK 中提取 SMF 文件，但若要转发给工具进行处理，则应先删除这些文件的 `.smf` 扩展名。

> 例如 PvZ-2 中文安卓版中存在 `*.rsb.smf` 文件，它们本身是 `*.rsb` 文件，需要删除 `.smf` 扩展名再转发给工具处理。
> 
> 注意：这些 *.rsb.smf 文件经过了一次 ZLib 压缩，需要先用工具对其进行 `PopCap ZLib 解压` ；此外，PvZ-2 中文安卓版不支持在 RSG 中存储 ZLib 压缩数据段。
