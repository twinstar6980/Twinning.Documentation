# 使用方法

- [使用 `Shell`](#使用-Shell)

- [使用 `Assistant`](#使用-Assistant)

- [转发文件](#转发文件)

- [附加参数](#附加参数)

- [用户输入](#用户输入)

- [配置文件](#配置文件)

## 使用 `Shell`

通过 `Shell` 可以在系统终端中以命令行的形式使用工具。

需要在启动应用时传递命令参数，格式如下：

* `kernel` : *`string`*
	
	内核文件路径。

* `script` : *`string`*
	
	脚本文件路径。也可以 `$` 为首字符标识，指示字符串的余下内容作为 JS 脚本。

* `argument` : *`string...`*
	
	执行参数。

> 若执行过程无错误发生，则 main 函数返回值为 `0` ，否则为 `1` 。

## 使用 `Assistant`

`Assistant` 基于 `Kernel` 、`Script` 等模块实现了多种子功能模块。

`Assistant Plus` 是 `Assistant` 在 `Windows` 平台的特化版本，界面样式与 `Windows 11` 系统更加契合。

可以在启动应用时传递命令参数，格式如下：

* **`-window_position`**
	
	窗口位置，不指定则默认居中。
	
	* `x` : *`integer`*
		
		起始水平坐标。
	
	* `y` : *`integer`*
		
		起始垂直坐标。

* **`-window_size`**
	
	窗口尺寸，不指定则遵循初始尺寸。
	
	* `width` : *`integer`*
		
		宽度。
	
	* `height` : *`integer`*
		
		高度。

* **`-initial_tab`**
	
	初始标签页，不指定则显示启动页。
	
	* `title` : *`string`*
		
		页标题。
	
	* `type` : *`string`*
		
		模块类别。
	
	* `option` : *`string...`*
		
		模块参数。

> `Assistant Plus` 的命令格式与 `Assistant` 类似，但指令名使用驼峰风格 *CamelCase* 而非蛇形风格 *snack_case* 。

在 `Android` 与 `Iphone` 系统中，可以通过应用链接传递启动参数：

* ***`twinstar.toolkit.assistant:/run?`***
	
	* `command` : *`string...`*
		
		命令参数。可以多次指定，所有查询值被视作字符串数组。

> 若在应用已启动的状态下打开链接，应用会切换到前台，但不会接收新的命令参数。

提供以下功能模块：

* `Modding Worker`
	
	该模块模拟了 `Shell` 的控制台 UI 。
	
	> `Kernel` 、`Script` 、`Argument` 参数需要在模块设置中定义。
	
	> 主目录内的 `launch.assistant_plus.modding_worker.cmd` 脚本可以快速开启该模块，可以将多个文件拖拽至脚本上并释放，以追加附加参数。

* `Resource Forwarder` *`<Plus-Only>`*
	
	该模块提供了更好的文件转发 UI ，能够通过输入文件的类型、扩展名来筛选可用的选项，并传递给 `Modding Worker` 处理，更加高效易用。
	
	> 选项由模块设置中的 `Option Configuration` 文件定义。
	
	> 主目录内的 `launch.assistant_plus.resource_forwarder.cmd` 脚本可以快速开启该模块，可以将多个文件拖拽至脚本上并释放，以追加附加参数。

* `Command Sender` *`<Plus-Only>`*
	
	该模块能够可视化地选择需要使用的功能并填入参数。
	
	> 功能由模块设置中的 `Method Configuration` 文件定义。

* `Animation Viewer` *`<Plus-Only>`*
	
	该模块能够播放 PopCap Animation (PAM) 动画文件。
	
	1. 点击本模块页面 ⌈ Stage ⌋ 栏右上方的 ⌈ Animation File ⌋ 文本框的右侧按钮，在弹出的窗口中选择 `*.pam.json` 文件；或是从 ⌈ Explorer ⌋ 中拖拽 `*.pam.json` 文件至应用窗口中并释放。
		
		> `*.pam.json` 由工具解码 `*.pam` 文件而得。
	
	2. 如果动画所需的分解图位于动画文件同级目录内，则可以正常渲染动画，否则需要点击 ⌈ Image Directory ⌋ 文本框的右侧按钮，选择分解图所在目录。
	
	3. 通过其他 UI 控件可以选择想要播放的子动画、调整播放区间与帧率、设置元件过滤项、等。

* `Package Builder` *`<Plus-Only>`*
	
	该模块能够可视化地管理 `PvZ-2 Package Project` 项目。

## 转发文件

工具被设计为主要处理外存空间中的文件对象，术语 ⌈ **转发** ⌋ 是指以文件对象的路径作为附加参数以启动工具。可以通过启动脚本、转发器提供的系统功能扩展等方式进行转发。

将需要处理的文件对象转发给工具，工具会根据该文件对象的类型列出可用功能，用户输入需要执行的功能序号即可。

> 注意：如果向工具转发了一个目录，用户将在可选功能列表中看到诸多以 `[*]` 作为前缀的功能，它们是批处理功能，会依次处理目录中的每一个子文件与子目录。大部分单一输入且单一输出的功能都具备批处理版本。
> 
> 常见的误区是用户在需要对某个目录执行常规功能时，错误选用了批处理功能。例如，如果需要将一个 `.rsb.bundle` 目录打包为 `rsb` 文件，那么需要选择常规版本的 `PopCap Resource-Stream-Bundle Pack` 而非带 `[*]` 前缀的批处理版本。

## 附加参数

用户可以在启动工具时传入附加参数，若未提供附加参数，则会在运行时要求用户输入。附加参数格式如下：

* `item`
	
	单个命令项，可以指定多个。
	
	* `input` : *`string`*
		
		指定了命令的输入数据，即一个文件或目录的路径，作为功能的输入参数。
		
		如果输入值为 `?` ，则将在运行时请求用户输入该参数。
	
	* **`-disable_filter`** : *`boolean`*
		
		用于禁用 ⌈ 候选功能过滤 ⌋ 。
		
		默认情况下，如果未指定 `-method` ，工具将根据输入对象的类型（主要通过扩展名）筛选出可用功能供用户选择；
		
		若禁用 ⌈ 候选功能过滤 ⌋ ，则将列出所有功能供用户选择。
	
	* **`-method`** : *`string`*
		
		指定需执行的功能，后跟该功能的 ID ，若未指定功能，将在运行时等待用户选择功能。
	
	* **`-argument`** : *`string`*
		
		指定需要传给功能的参数，后跟一个 JSON 字符串，且必须可解析为一个 Object 。

各功能的 ID 与其参数定义参见 [功能](./method.md) 章节。

> 例 - 使用工具解码桌面上的 `test.pam` 文件：
> 
> `> .\launch.cmd "C:\Users\TwinStar\Desktop\test.pam" "-method" "popcap.animation.decode"`
> 
> 该命令以 `test.pam` 文件的路径作为输入参数，并指定需执行的功能为 `PopCap Animation 解码` 。执行完毕后，可在桌面看到一个名为 `test.pam.json` 的新文件，即为解码后的 PAM 数据。

## 用户输入

工具运行时会向用户输出消息，或请求用户输入一些参数，输入值类型及格式如下：

* `Pause` 暂停
	
	暂停程序等待用户响应。

* `Boolean` 布尔值
	
	单个字符 `y` 或 `n` ，表示 ⌈ 是 ⌋ 与 ⌈ 否 ⌋ 。

* `Integer` 整数
	
	一个十进制整数，不可包含小数点。

* `Floater` 浮点数
	
	一个十进制数，可包含小数点。

* `Size` 尺寸
	
	一个无符号十进制数，后跟随一个二进制单位（ b = 2^0 , k = 2^10 , m = 2^20 , g = 2^30 ），一般用于表示存储容量。
	
	> 例如 `4k` 表示 4096 字节的存储容量。

* `String` 字符串
	
	一行文本。
	
	> 如果输入的文本为空，则视作输入了空值；\
	> 如果输入的文本以 `::` 起始，则工具将截取其后的文本作为真实输入，通过这种方式可以输入空字符串而非空值。

* `Path` 路径
	
	可用于本地文件系统的路径，分为输入路径与输出路径，前者指向磁盘上一个已存在的文件或目录的路径，后者指向磁盘上一个不存在的路径。
	
	> 如果输入的文本为空，则视作输入了空值；\
	> 如果输入的文本以 `::` 起始，则工具将截取其后的文本作为真实输入，通过这种方式可以输入空字符串而非空值。
	> 
	> 输入 `:p` 将唤起文件选择窗口；\
	> 输入 `:g` 将通过所给路径生成可用输出路径（附加后缀）；\
	> 输入 `:m` 将移动已有文件；\
	> 输入 `:d` 将删除已有文件；\
	> 输入 `:o` 将覆写已有文件。
	> 
	> 如果输入路径由一对单引号或双引号包围，工具将自动去除引号；如果输入了一段相对路径，则该路径是相对于工具的工作目录 `<home>/workspace` 计算的。

* `Enumeration` 枚举
	
	多个可选项中的一项。

## 配置文件

在工具的脚本目录 `<home>/script` 中，扩展名为 `.json` 的文件是工具的配置文件，其中的配置项会影响工具的某些行为，如 JSON 的输出格式，用户可以自行修改配置文件。

`<home>/script/Entry/Entry.json` 是工具的主配置文件，它的配置项如下。

* `<configuration>`

	* `language` : `string` = `English`
		
		交互语言。可以为 `English` 或 `Chinese` 或 `Vietnamese` 。
	
	* `disable_basic_virtual_terminal_sequence` : `boolean` = `false`
		
		禁用命令行虚拟终端序列。仅当使用 `Shell` 时有效。
	
	* `executor_typical_method_disable_name_filter` : `boolean` = `false`
		
		禁用命令处理器的名称过滤行为。默认情况下，工具会对命令输入的文件路径进行扩展名匹配，不匹配的功能将不对用户显示；禁用后将显示所有功能。
	
	* `byte_stream_use_big_endian` : `boolean` = `false`
		
		内部字节流操作时使用大端序。这个选项对一些大端序文件的处理是必要的。默认禁用，因为大多数时候用户处理的都是小端序文件。
	
	* `common_buffer_size` : `string` = `64.0m`
		
		公用缓存区大小。用于编码 JSON 字符串、编码 PNG 图像等，如果编码时所需的内存用量超过这一大小，工具将处理失败。
	
	* `json_format` : `{ ... }`
		
		JSON 输出格式。
		
		* `disable_array_trailing_comma` : `boolean` = `false`
			
			禁用数组尾随逗号。
		
		* `disable_array_line_breaking` : `boolean` = `false`
			
			禁用数组换行。
		
		* `disable_object_trailing_comma` : `boolean` = `false`
			
			禁用对象尾随逗号。
		
		* `disable_object_line_breaking` : `boolean` = `false`
			
			禁用对象换行。
	
	* `external_program` : `{ ... }`
		
		指定可能会调用的外部程序的路径，若为 null ，则将在运行时检索 `PATH` 环境变量。
		
		* `sh` : `null | string` = `null`
			
			用于 `/utility/AndroidHelper.ts` 。
		
		* `adb` : `null | string` = `null`
			
			用于 `/utility/AndroidHelper.ts` 。
		
		* `vgmstream` : `null | string` = `null`
			
			用于 `/Support/Wwise/Media/Decode.ts` 。
		
		* `wwise` : `null | string` = `null`
			
			用于 `/Support/Wwise/Media/Encode.ts` 。
		
		* `il2cpp_dumper` : `null | string` = `null`
			
			用于 `/Support/Kairosoft/Game/ModifyProgram.ts` 。
	
	* `thread_limit` : `bigint` = `0`
		
		线程池上限数。目前无实际作用。
	
	* `command_notification_time_limit` : `null | bigint` = `15000`
		
		命令通知时限。在某项命令执行完成后，如果有效执行时间超过该值（毫秒），将推送系统通知以提醒用户。设为 null 将禁用通知。

> 脚本提供的每组功能都有与之对应的配置文件，参见 [功能列表](./method.md) 章节。
