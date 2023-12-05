# 使用方法

- [使用 `Shell CLI`](#使用-Shell-CLI)

- [使用 `Shell GUI`](#使用-Shell-GUI)

- [使用 `Helper`](#使用-Helper)

- [转发文件](#转发文件)

- [附加参数](#附加参数)

- [用户输入](#用户输入)

- [配置文件](#配置文件)

- [使用 `Helper` 的高级功能](#使用-Helper-的高级功能)

## 使用 `Shell CLI`

通过 `Shell CLI` 可以在系统终端中以命令行的形式使用工具。

用户需要在终端中启动 `Shell CLI` ，命令行参数的格式如下：

`<kernel> <script> <argument>...`

* `<kernel>`
	
	第一参数为内核文件路径。

* `<script>`
	
	第二参数是传给内核处理逻辑的脚本文件路径。也可以 `$` 为首字符标识，指示字符串的余下内容作为 JS 脚本。

* `<argument>...`
	
	剩余参数作为传给内核处理逻辑的参数。

若执行过程无错误发生，则 main 函数返回值为 0 ，否则为 1 。

## 使用 `Shell GUI`

`Shell GUI` 提供了类似终端的 UI ，而不依赖系统终端的行为。

点击应用控制台界面下方的 `Launch` 按钮即可启动会话，长按该按钮可以编辑每次启动的附加参数。

同样支持在终端中启动 `Shell GUI` ，命令行参数的格式如下：

`-additional_argument <additional_argument>...`

* `<additional_argument>...`
	
	传给内核处理逻辑的附加参数。

若传入命令行参数，则应用将在启动后自动启动控制台。

> @ `Android` `iPhone` \
> 无法直接传入命令行参数。
> 
> @ `Android` \
> 可以通过 `Intent` 传入命令行参数：`action = "com.twinstar.toolkit.shell_gui.action.LAUNCH", extra = { "command": Array<String> }` 。

## 使用 `Helper`

`Helper` 的 `Modding Worker` 模块提供了类似 `Shell GUI` 的 UI ，但样式更加现代化，契合 `Windows 11` 系统主题。

打开 `Modding Worker` 页面后，点击界面下方的 `Launch` 按钮即可启动会话，点击界面右上方的别针按钮可以编辑每次启动的附加参数。

> 主文件夹内的 `launch_helper.cmd` 脚本可以快速启动 `Modding Worker` ，可以将多个文件拖拽至脚本上并释放，以追加附加参数。

## 转发文件

工具被设计为主要处理外存空间中的文件对象，术语 ⌈ **转发** ⌋ 是指以文件对象的路径作为附加参数以启动工具。可以通过启动脚本、转发器提供的系统功能扩展等方式进行转发。

将需要处理的文件对象转发给工具，工具会根据该文件对象的类型列出可用功能，用户输入需要执行的功能序号即可。

> 注意：如果向工具转发了一个文件夹，用户将在可选功能列表中看到诸多以 `[*]` 作为前缀的功能，它们是批处理功能，会依次处理文件夹中的每一个子文件与子文件夹。大部分单一输入且单一输出的功能都具备批处理版本。
> 
> 常见的误区是用户在需要对某个文件夹执行常规功能时，错误选用了批处理功能。例如，如果需要将一个 `.rsb.bundle` 文件夹打包为 `rsb` 文件，那么需要选择常规版本的 `PopCap Resource-Stream-Bundle Pack` 而非带 `[*]` 前缀的批处理版本。

## 附加参数

用户可以在启动工具时传入附加参数，若未提供附加参数，则会在运行时要求用户输入。附加参数如下格式：

```
[
	<input>
	[ -disable_filter ]
	[ -method <method-id> ]
	[ -argument <argument-json> ]
]...
```

* `<input>`
	
	指定了命令的输入数据，即一个文件或文件夹的路径，作为功能的输入参数。
	
	如果输入值为 `?` ，则将在运行时请求用户输入该参数。

* `[ -disable_filter ]`
	
	用于禁用 ⌈ 候选功能过滤 ⌋ 。
	
	默认情况下，如果未指定 `-method` ，工具将根据输入对象的类型（主要通过扩展名）筛选出可用功能供用户选择；
	
	若禁用 ⌈ 候选功能过滤 ⌋ ，则将列出所有功能供用户选择。

* `[ -method <method-id> ]`
	
	指定需执行的功能，后跟该功能的 ID ，若未指定功能，将在运行时等待用户选择功能。

* `[ -argument <argument-json> ]`
	
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

* `Floater` 数
	
	一个十进制数，可包含小数点。

* `Size` 尺寸
	
	一个无符号十进制数，后跟随一个二进制单位（ b = 2^0 , k = 2^10 , m = 2^20 , g = 2^30 ），一般用于表示存储容量。
	
	> 例如 `4k` 表示 4096 字节的存储容量。

* `String` 字符串
	
	一行文本。

	> 如果输入的文本为空，则视作输入了空值；\
	> 如果输入的文本以 `::` 起始，则工具将截取其后的文本作为真实输入，通过这种方式可以输入空字符串而非空值。

* `Path` 路径
	
	可用于本地文件系统的路径，分为输入路径与输出路径，前者指向磁盘上一个已存在的文件或文件夹的路径，后者指向磁盘上一个不存在的路径。
	
	> 如果输入的文本为空，则视作输入了空值；\
	> 如果输入的文本以 `::` 起始，则工具将截取其后的文本作为真实输入，通过这种方式可以输入空字符串而非空值。
	> 
	> 输入 `:p` 将唤起文件选择窗口（仅限 `GUI` 或 `Windwos CLI` 、`Linux CLI` 、`Macintosh CLI` ）；\
	> 输入 `:g` 将通过所给路径生成可用输出路径（附加后缀）；\
	> 输入 `:m` 将移动已有文件；\
	> 输入 `:d` 将删除已有文件；\
	> 输入 `:o` 将覆写已有文件。
	> 
	> 如果输入路径由一对单引号或双引号包围，工具将自动去除引号；如果输入了一段相对路径，则该路径是相对于工具的工作文件夹 `<home>/workspace` 计算的。

* `Enumeration` 枚举
	
	多个可选项中的一项。

## 配置文件

在工具的脚本文件夹 `<home>/script` 中，扩展名为 `.json` 的文件是工具的配置文件，其中的配置项会影响工具的某些行为，如 JSON 的输出格式，用户可以自行修改配置文件。

`<home>/script/Entry/Entry.json` 是工具的主配置文件，它的配置项如下。

* `<configuration>`

	* `language` : `string` = `English`
		
		交互语言。可以为 `English` 或 `Chinese` 或 `Vietnamese` 。
	
	* `disable_cli_virtual_terminal_sequence` : `boolean` = `false`
		
		禁用命令行虚拟终端序列。仅当使用命令行外壳时有效。
	
	* `byte_stream_use_big_endian` : `boolean` = `false`
		
		内部字节流操作时使用大端序。这个选项对一些大端序文件的处理是必要的。默认禁用，因为大多数时候用户处理的都是小端序文件。
	
	* `common_buffer_size` : `string` = `64.0m`
		
		公用缓存区大小。用于编码 JSON 字符串、编码 PNG 图像等，如果编码时所需的内存用量超过这一大小，工具将处理失败。
	
	* `json_format` : `{ ... }`
		
		JSON 输出格式。
		
		* `disable_trailing_comma` : `boolean` = `false`
			
			禁用尾随逗号。
		
		* `disable_array_wrap_line` : `boolean` = `false`
			
			禁用数组元素间的换行。
	
	* `thread_limit` : `bigint` = `0`
		
		线程池上限数。目前无实际作用。
	
	* `command_notification_time_limit` : `null | bigint` = `15000`
		
		命令通知时限。在某项命令执行完成后，如果有效执行时间超过该值（毫秒），将推送系统通知以提醒用户。设为 null 将禁用通知。

> 脚本提供的每组功能都有与之对应的配置文件，参见 [功能列表](./method.md) 章节。

## 使用 `Helper` 的高级功能

* `Resource Forwarder`
	
	该模块提供了更好的文件转发 UI ，能够通过输入文件的类型、扩展名来筛选可用的选项，并传递给 `Modding Worker` 处理，更加高效易用。
	
	> 选项由模块设置中的 `Option Configuration` 文件定义。
	
	> 主文件夹内的 `launch_helper_forwarder.cmd` 脚本可以快速开启 `Resource Forwarder` ，可以将多个文件拖拽至脚本上并释放，以追加附加参数。

* `Command Sender`
	
	该模块可用可视化地选择需要使用的功能并填入参数。
	
	> 功能由模块设置中的 `Method Configuration` 文件定义。
	
	> 主文件夹内的 `launch_helper_forwarder.cmd` 脚本可以快速开启 `Resource Forwarder` ，可以将多个文件拖拽至脚本上并释放，以追加附加参数。

* `Animation Viewer`
	
	该模块用于播放 PopCap Animation (PAM) 动画文件。
	
	1. 点击本模块页面 ⌈ Stage ⌋ 栏右上方的 ⌈ Animation File ⌋ 文本框的右侧按钮，在弹出的窗口中选择 `*.pam.json` 文件；或是从 ⌈ Explorer ⌋ 中拖拽 `*.pam.json` 文件至应用窗口中并释放。
		
		> `*.pam.json` 由工具解码 `*.pam` 文件而得。
	
	2. 如果动画所需的分解图位于动画文件同级文件夹内，则可以正常渲染动画，否则需要点击 ⌈ Image Directory ⌋ 文本框的右侧按钮，选择分解图所在文件夹。
	
	3. 通过其他 UI 控件可以选择想要播放的子动画、调整播放区间与帧率、设置元件过滤项、等。
