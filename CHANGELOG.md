# 更新日志

- [24-05-22](#24-05-22)

- [24-05-23](#24-05-23)

- [24-05-25](#24-05-25)

- [24-05-27](#24-05-27)

- [24-05-28](#24-05-28)

- [24-05-29](#24-05-29)

- [24-06-06](#24-06-06)

- [24-06-08](#24-06-08)

- [24-06-20](#24-06-20)

- [24-06-23](#24-06-23)

- [24-06-24](#24-06-24)

- [24-06-25](#24-06-25)

- [24-06-26](#24-06-26)

- [24-06-27](#24-06-27)

- [24-07-07](#24-07-07)

- [24-07-08](#24-07-08)

- [24-07-14](#24-07-14)

- [24-07-25](#24-07-25)

- [24-08-14](#24-08-14)

- [24-09-01](#24-09-01)

- [24-09-07](#24-09-07)

- [24-10-24](#24-10-24)

- [24-10-25](#24-10-25)

- [24-11-14](#24-11-14)

- [24-11-29](#24-11-29)

- [已知问题](#已知问题)

## 24-05-22

* `Shell` 38

	* 优化代码。

* `Script` 106

	* 修复在一些情况下解析 Size 字符串失败的 BUG 。

* `Assistant` 36

	* 迁移项目至 Flutter 3.22 。

	* 修改应用缓存目录位置，允许用户在应用内手动清除缓存。

	* 在 About 页面中添加设置文件、共享目录、缓存目录相关功能。

	* 统一 Desktop 与 Mobile 平台上的 materialTapTargetSize 行为为 shrinkWrap 。

	* 修复在 Windows 平台上打开目录选择对话框时可能报错的 BUG 。

	* 为清空近期启动项列表操作添加确认步骤。

	* `Modding Worker` 优化 SubmissionBar UI 。

	* `Modding Woeker` 修复 SubmissionBar 输入浮点数后向 Kernel 返回指数形式字符串的 BUG 。

	* `Modding Worker` 添加输入值历史记录功能。

* `AssistantPlus.Windows` 30

	* 修改应用缓存目录位置，允许用户在应用内手动清除缓存。

	* 在 About 页面中添加共享目录、缓存目录相关功能。

	* 添加标签页记录与复制功能。

	* 为清空近期启动项列表操作添加确认步骤。

	* `Modding Woeker` 优化 MessageCard UI ，不再为 Type=Input 设置独立的 FlowDirection 。

	* `Modding Woeker` 优化 SubmissionBar UI 。

	* `Modding Woeker` 修复 SubmissionBar 输入浮点数后向 Kernel 返回指数形式字符串的 BUG 。

	* `Modding Worker` 添加输入值历史记录功能。

	* `Resource Forwarder` 在每次转发后将记录标签页至近期启动项列表中。

## 24-05-23

* `Assistant` 37

	* 添加 tooltip 以优化 UX 。

	* `Modding Worker` 优化历史输入项的 UI 。

	* `Modding Woeker` 优化 SubmissionBar Type=Enumeration 的可选项列表 UI 。

* `AssistantPlus.Windows` 31

	* 迁移项目至 Visual Studio 2022 17.10 。

	* `Modding Woeker` 为 SubmissionBar Type=Boolean 的按钮添加 tooltip 。

* `Forwarder.Windows`

	* 迁移项目至 Visual Studio 2022 17.10 。

## 24-05-25

* `Shell` 39

	* 在目录选择对话框中将显示 WSL 等非本地磁盘文件。

* `Script` 107

	* 修复无法正常解析 Size 字符串表达式的 BUG 。

* `Assistant` 38

	* 优化代码。

	* 修复 SettingItem 后缀组件布局溢出的 BUG 。

	* 应用设置文件增加含制表符缩进，以便于手动编辑。

	* 选择在 Windows 平台中目录选择对话框选中非文件系统对象时返回空字符串而非空值的 BUG 。

	* 在 WSL 环境下也可以调起文件选择对话框。

	* `Modding Worker` 当历史输入列表为空时，点击历史记录按钮依然将显示空菜单。

	* `Resource Forwarder` 新增模块。

* `AssistantPlus.Windows` 32

	* 简化部分控件的 `"Pick In Window"` 文本为 `"Pick"` 。

	* `Modding Worker` 移除 AlternativeLaunchScript 选项，不再支持通过外部脚本代理转发行为。

	* `Modding Worker` 移除全页范围的文件拖放操作，该功能原本用于将接收的文件路径添加至附加参数列表中。

	* `Modding Worker` 附加参数编辑控件 Flyout Placement 更改为 Full 。

	* `Resource Forwarder` 性能优化。

	* `Resource Forwarder` 优化输入项控件 UI 。

	* `Resource Forwarder` 移除无输入项时的页面提示 UI 。

	* `Resource Forwarder` 移除在执行转发后自动关闭标签页的行为。

	* `Resource Descriptor` 支持全页范围的文件拖放操作，用以接受 `DescriptionFile` 。

	* `Animation Viewer` 修复页面左右两侧无法接收文件拖放操作的 BUG 。

	* `Package Builder` 支持全页范围的文件拖放操作，用以接受 `PackageDirectory` 。

## 24-05-27

* `Assistant` 39

	* 优化代码。

* `Forwarder.Macintosh` 7

	* 优化代码。

* `Forwarder.Android` 9

	* 增加主活动页。

	* 应用现在将资源文件转发至 `Assistant - Resource Forwarder` 模块。

* `Forwarder.Iphone` 4

	* 增加主活动页。

	* 应用现在将资源文件转发至 `Assistant - Resource Forwarder` 模块。

## 24-05-28

> 这个版本更新了项目的名称与图标。

* `Kernel` 65

* `Shell` 40

* `Script` 108

* `Assistant` 40

* `Forwarder.Windows` 39

* `Forwarder.Macintosh` 8

* `Forwarder.Android` 10

* `Forwarder.Iphone` 5

* `AssistantPlus.Windows` 33

## 24-05-29

* `Kernel` 66

	* 修复创建子进程后未关闭其句柄的 BUG 。

	* 重命名部分函数与脚本接口名。

* `Script` 109

* `Assistant` 41

	* About 页面的操作 Chip 改成自适应尺寸，以避免一些情况下出现文本溢出的情况。

* `Forwarder.Windows` 40

	* 优化代码。

	* 修复创建子进程后未关闭其句柄的 BUG 。

* `Forwarder.Macintosh` 9

	* 优化代码。

* `AssistantPlus.Windows` 34

	* 修复创建子进程后未关闭其句柄的 BUG 。

## 24-06-06

> 这个版本优化了项目中的构建系统配置，并将签名相关的敏感配置统一剥离至 `/common/certificate` 中。

* `Assistant` 42

	* 修改了应用启动命令的参数格式，现在要求显式地向应用传递 `launch` 作为首项参数以指示余下参数为启动命令。

	* 移除 `window_position` 与 `window_size` 命令支持。

	* 应用链接增加对 `Windows` 、`Macintosh` 系统的支持。

	* 在受支持的系统上，通过应用链接启动应用时，如果系统中已存在应用实例，该实例将会接收新的应用链接，而不创建新的实例。

	* `Android` 更改应用的启动模式为“单个实例”，就如应用在 `Iphone` 系统上的行为一样。

	* `Macintosh` 修复应用在菜单栏左侧显示的应用名称为 `assistant` 而非 `Twinning Assistant` 的问题，应用的可执行文件更名为 `Runner` 以与 `Iphone` 保持一致。

	* 修改默认的主题色。

	* `Android` 修复应用在推送消息时使用了无效图标资源的 BUG 。

	* `Modding Worker` 修复当通过 `-additional_argument` 参数启动标签页时，左下角的参数计数 Badge 未同步更新的 BUG 。

	* `Resource Forwarder` 当通过命令行指定的输入对象路径为 Windows-Style 时，内部将自动转换为 POSIX-Style 路径。

* `AssistantPlus.Windows` 35

	* 修改了应用启动命令的参数格式，现在要求显式地向应用传递 `Launch` 作为首项参数以指示余下参数为启动命令。

	* 当用户通过文件对象拖放操作而获取路径时，将始终获取到长路径，在此之前对于目录会获取到短路径。

	* 将 `/Bridge/Data.cs` 中的数据类型定义为平台相关类型，使代码能够兼容 32 位环境。

* `Forwarder.Windows` 41

	* 默认的转发行为改为生成并打开 `Assistant` 的应用链接，仍可提供脚本文件以让应用执行脚本。

	* 应用现在将转发 POSIX 风格的文件路径，而非之前的 Windows 风格。

* `Forwarder.Macintosh` 10

	* 转发行为改为生成并打开 `Assistant` 的应用链接，移除脚本文件的支持。

* `Forwarder.Android` 11

	* 修改打开应用链接时的 intent flag 。

	* 修复当所选文件路径中包含特殊字符时无法正确转发的 BUG 。

* `Forwarder.Iphone` 6

	* 移除项目中不需要的 Storyboard 文件。

	* 修复当所选文件路径中包含特殊字符时无法正确转发的 BUG 。

## 24-06-08

* `Shell` 41

	* 重命名部分 Shell Callback 函数。

	* 回退第三方依赖项 `tinyfiledialogs` 的版本至 `3.17.5` ，以修复在 `Windows` 上调用 `push_system_notification` 函数弹出文本框而非系统级通知的 BUG 。

* `Script` 110

* `Assistant` 43

	* 重命名部分 MethodChannel 函数。

	* `Android` 优化 Content URI 处理方案。

	* `Android` 修复选择文件时指定初始目录可能无效的 BUG 。

	* `Android` 选取文件时使用的 action 由 GET_CONTENT 更改为 OPEN_DOCUMENT ，前者在 singleInstance 应用中无法使用部分 UI 功能。

	* `Modding Worker` 重命名部分 Shell Callback 函数。

* `AssistantPlus.Windows` 36

	* 支持应用链接。

	* `Modding Worker` 重命名部分 Shell Callback 函数。

## 24-06-20

> 这个版本更新了项目的图标。

* `Shell` 42

	* `ExecutorProxy` 的默认实现将抛出 `runtime_error` 而非 `string` 。

	* 更改 `pick_storage_item` 外壳回调的参数规则。

* `Script` 111

	* 现在会在脚本开始执行时向用户显示所传递的参数列表。

	* 执行功能时会显示用户传递的参数。

* `Assistant` 44

	* 优化一些 UI 与处理逻辑。

	* 在所有平台上统一使用标准视觉密度，在桌面系统中会导致部分控件尺寸略大于之前版本。

	* 修复在显示主题颜色值时错误地填补了尾随 0 的 BUG 。

	* `Modding Worker` 更改 `pick_storage_item` 外壳回调的参数规则。

	* `Command Sender` 新增模块，用于可视化地编辑适用于 `Modding Worker` 的命令参数。

	* `Resource Forwarder` 更改命令参数中的 `-input` 为 `-resource` 。

	* `Resource Forwarder` 重命名设置项 `Parallel Execute` 为 `Parallel Forward`。

	* `Resource Forwarder` 增加清除所有资源项的选项。

	* `Resource Forwarder` 移除在执行转发后清除所以资源项的设置项。

	* `Resource Forwarder` 优化选项列表控件性能。

	* `Resource Forwarder` 添加输入项后将保留弹出菜单。

	* `Resource Forwarder` 修复切换 EnableFilter 设置项时可能导致选项列表子项状态混淆的 BUG 。

	* `Resource Forwarder` 修复在启用平行转发设置项时转发资源可能报错的 BUG 。

* `AssistantPlus.Windows` 37

	* 优化一些 UI 与处理逻辑。

	* 优化添加固定标签项时的策略，现在将直接添加至列表底部；在之前的版本中，会添加至列表顶部，且当存在相似项时会将其冒泡至顶部而不添加新项。

	* `Modding Worker` 更改 `pick_storage_item` 外壳回调的参数规则。

	* `Command Sender` 移除配置文件中的 `initial` 设置项，现在初始值始终为 `null` ；添加 `preset` 设置项以支持快速选择预置参数。

	* `Command Sender` 重命名设置项 `Parallel Execute` 为 `Parallel Forward`。

	* `Command Sender` 支持 `Command` 参数以指定初始命令。

	* `Command Sender` 移除直接打开 Modding Worker 而不传递任何参数的按钮。

	* `Resource Forwarder` 更改命令参数中的 `-Input` 为 `-Resource` 。

	* `Resource Forwarder` 重命名设置项 `Parallel Execute` 为 `Parallel Forward`。

	* `Resource Forwarder` 增加清除所有资源项的选项。

	* `Resource Forwarder` 移除在执行转发后清除所以资源项的设置项。

	* `Resource Forwarder` 不再在转发后添加近期标签项。

* `Forwarder.Windows` 42

* `Forwarder.Macintosh` 11

* `Forwarder.Android` 12

* `Forwarder.Iphone` 7

## 24-06-23

> 这个版本优化了项目图标在 Macintosh 及 Iphone 平台上的显示效果。

* `Script` 112

	* 显示用户传递的参数时，不再显示尾随的逗号。

* `Assistant` 45

	* 优化设置文件的格式。

* `AssistantPlus.Windows` 38

* `Forwarder.Macintosh` 12

* `Forwarder.Iphone` 8

## 24-06-24

> 这个版本将 `Forwarder` 模块整合进了 `Assistant` 模块中。

* `Assistant` 46

	* `Macintosh` 调整目标系统版本为 11.0 。

	* `Iphone` 调整目标系统版本为 14.0 。

## 24-06-25

> 这个版本将 `AssistantPlus.Windows` 模块重命名为 `AssistantPlus` ，并为各模块增加了构建分发脚本。

## 24-06-26

* `Assistant` 47

	* `Windows` 修复应用弹出系统级通知后点击通知无任何行为的 BUG ，现在会将应用窗口置为前台。

	* `Windows` 转发扩展现在根据是否存在任意类型的 forwarder 文件对象（而不是必须为文件）来判断是否启用上下文扩展。

* `AssistantPlus` 39

	* 调整项目结构。

	* 更改应用图标。

	* 更改应用的共享数据目录位置，原本位于应用的沙盒存储空间中，现在更改为非沙盒的 `%APPDATA%/<App>` 。

	* 应用退出时，将正确地注销关联的系统级通知。

	* 提供对 explorer 上下文扩展的支持。

## 24-06-27

* `Assistant` 48

	* 桌面平台现在支持部分控件的文件拖放操作。

## 24-07-07

* `Script` 113

	* 修复 RSB 资源转换功能在一些情况下报错的 BUG 。

* `Assistant` 49

	* 优化 UI 。

	* 更好地适配 Android 系统导航栏与状态栏。

	* 改进文本输入控件的值更新规则，现在只会在输入控件失焦时进行更新，而不是每次键入文本时都更新一次。

	* 对于关联值为路径类型的文本输入控件，添加调起文件选择窗口的后缀按钮，同时，会在执行值更新时自动对路径值执行规范化。

	* 修复移除标签项时被删除项的按钮水波特效会显示在下一个标签项按钮上的 BUG 。

	* 移除标签页时不再弹出确认对话框。

	* 修复在 `Windows` 上，第一次唤起文件选择对话框时，初始目录未成功设置为 `C:/` 的 BUG 。

	* 这导航抽屉中添加 Commander 功能项，用于输入命令并执行。

	* `Modding Worker` 如果在会话处于运行状态时尝试移除标签页，应用将弹出对话框告知用户无法移除。强制移除将会致使后台 Isolate 一直等待并占用部分资源。

	* `Modding Worker` 修复快速多次点击启动按钮可能会重复启动任务会话的 BUG 。

	* `Modding Worker` 优化 ArgumentBar Type=Size 的行为。

	* `Command Sender` 在串行模式下执行转发时，始终调起新的 `Modding Worker` 标签项，即使命令列表为空。

	* `Command Sender` 为命令项添加收起模式，该模式下仅列出非空参数的纯文本视图。

	* `Command Sender` 移除命令项时，若存在不为空的参数值，将弹出确认窗口。

	* `Command Sender` 修复文件选择窗口与 `Modding Worker` 的文件选择窗口共用路径记录的 BUG 。

	* `Command Sender` 优化 ArgumentBar Type=Size 的行为。

	* `Resource Forwarder` 修复参数列表不保留展开状态的 BUG 。

	* `Resource Forwarder` 修复在启用批处理模式的情况下仍进行名称匹配的 BUG 。

	* `Resource Forwarder` 修复选项列表不保留展开状态的 BUG 。

	* `Animation Viewer` 新增模块，用于查看 PAM 动画。目前还在开发测试阶段，存在 UI 错误、功能缺失的问题。

* `AssistantPlus` 40

	* 修复 AboutPanel 中的一处 typo 。

	* 优化 pam.json 动画文件的读取逻辑。

	* `Command Sender` 在串行模式下执行转发时，始终调起新的 `Modding Worker` 标签项，即使命令列表为空。

	* `Resource Forwarder` 修复在启用批处理模式的情况下仍进行名称匹配的 BUG 。

## 24-07-08

* `Assistant` 50

	* 优化 UI 。

	* 修复中设置页中编辑设置项时 UI 未同步更新的 BUG 。

	* 修复应用在桌面系统中时界面中同一可滚动区域出现完全重复的滚动条的 BUG 。

	* 在文本输入控件外部进行点击，都会使控件失焦并更新数据，即使应用运行在移动平台上。

	* `Android` 修复一些可滚动区域的滚动条存在不需要的额外 padding 的 BUG 。

	* `Windows` 更改转发器扩展的 Verb Id 。

	* `Resource Forwarder` 优化资源面部中选项的文本与图标，并新增手动输入资源路径的选项。

* `Assistant Plus` 41

	* 更改转发器扩展的 Verb Id 。

## 24-07-14

* `Assistant` 51

	* 优化 UI 。

	* 修复文件拖放区域的空白部分无法接收放置事件的 BUG 。

	* 为 Tooltip 在桌面平台的触发添加 1000 毫秒的等待时间。

	* 优化 AboutPanel 页面中显示共享目录选项的逻辑，并支持在 `Iphone` 平台上跳转至 `Files` 以显示目录。

	* `Resource Forwarder` 清除所有资源项时显示确认对话框。

	* `Resource Forwarder` 资源项的显示方式由 Menu 更改为 BottomSheet 。

* `Assistant Plus` 42

	* 优化 UI 。

	* 修复报错对话框内容文本过多时无法滚动以查看更多错误信息的问题。

	* `Resource Forwarder` 清除所有资源项时显示确认对话框。

	* `Resource Forwarder` 新增手动输入资源路径的选项。

## 24-07-25

* `Script` 114

	* 支持为目标平台为 Android Arm-32 的开罗游戏启用 DEBUG 模式选项。

	* 支持为目标平台为 Android Arm-64 的开罗游戏执行自动修改。

* `Assistant` 52

## 24-08-14

* `Kernel` 67

	* 更新工具链及依赖项，移除不必要的依赖项 `ETCPACK` 。

	* 在 `Linux` 上，程序将依赖 `libc++` 而非 `libstdc++` 。

	* 重命名部分函数与脚本接口名。

	* 修复在输出 JSON 文件时，有可能将数字输出为科学计数法的 BUG ，现在始终输出为 fixed 格式。

* `Shell` 43

	* 更新工具链及依赖项。

	* 在 `Linux` 上，程序将依赖 `libc++` 而非 `libstdc++` 。

* `Script` 115

* `Assistant` 53

	* 更新工具链及依赖项。

## 24-09-01

* `Assistant` 54

	* 更新工具链及依赖项。

* `Assistant Plus` 43

	* 更新工具链及依赖项。

## 24-09-07

* `Assistant` 55

	* 更新工具链及依赖项。

	* 修复在 `Windows` 上通过 url 启动应用时始终启动新实例而非当前实例的 BUG 。

* `Assistant Plus` 44

	* 更新工具链及依赖项。

## 24-10-24

* `Kernel` 68

	* 更新工具链及依赖项。

	* 修复几处非法代码。

	* 修复在某些编译环境下可能出现的全局变量初始化失败问题。

* `Shell` 44

	* 更新工具链。

* `Assistant` 56

	* 更新工具链及依赖项。

* `Assistant Plus` 45

	* 更新工具链及依赖项。

## 24-10-25

* `Kernel` 69

	* 在 `Windows` 上，使用的编译器由 `msvc` 替换为 `clang-cl` 。

	* 在 `Windows` 上，使用 `mscharconv`（而非 MSVC-STL）处理数字文本，以保持应用在不同平台上的行为一致性。

* `Shell` 45

	* 在 `Windows` 上，使用的编译器由 `msvc` 替换为 `clang-cl` 。

* `Assistant Plus` 46

	* 更新工具链及依赖项。

	* 支持的最低系统版本更改为 Windows 10 1809 (10.0.17763.0) 。

## 24-11-14

* `Script` 116

	* 更新越南语的本地化文本，由 [Haruma](https://github.com/Haruma-VN) 完成。

## 24-11-29

* `Kernel` 70

	* 更新工具链及依赖项。

* `Assistant` 57

	* 更新工具链及依赖项。

	* 优化部分 UI 细节。

* `Assistant Plus` 47

	* 更新工具链及依赖项。

## 已知问题

* `Kernel`

	* 由于所依赖的第三方库 `md5` 的不足，应用在计算大文件（4G及以上）时会无限循环。

* `Assistant`

	* ListTile 的水波特效会溢出父容器的约束范围，这在一些情境下会对视觉效果造成负面影响。这是 `Flutter` 的 BUG ，参见 [issue #86584](https://github.com/flutter/flutter/issues/86584) 。

	* 文本输入框组件在离开视图区域后可能会被回收，当它再次进入视图时，会重构新的 State ，此时，之前的输入历史记录将丢失，故而无法通过 Undo/Redo 快捷键切换输入值。这是预期行为。

	* 应用覆盖了文本输入控件的默认 onTapOutside 行为，以使控件在移动平台上也会在点击外部时失焦，但这也导致用户无法在显示 IME 的情况下对应用 UI 进行列表滚动等操作。

	* 为 `Iphone` 构建时能够完成打包，但无法通过 flutter 进行实机调试，期间会弹出未正确配置证书的错误。这是在某次更新后才出现的问题。

	* 在 `Android` 上，用户在系统设置中更改系统显示大小时，应用可能不会立即更新显示大小，每次更改显示大小都要到下一次更改显示大小时才会生效。这是 `Flutter` 的 BUG 。

	* 在 `Android` 上，AboutPanel 中的第一行文本可能被截断了一些顶部区域，在与界面中的 Chip 进行交互时会恢复正常，但交互结束后又会显示为被截断，更换主题字体后可能不会出现这种情况。这是 `Flutter` 的 BUG ，参见 [issue #151540](https://github.com/flutter/flutter/issues/151540) 。

	* 在 `Android` 上，默认遵循的系统配色值未适配 Flutter 3.22 的变更，因此会出现部分颜色混淆的 BUG 。这是第三方依赖 `dynamic_color` 的 BUG ，参见 [issue #582](https://github.com/material-foundation/flutter-packages/issues/582) 。

	* 在 `Windows` 上，使用文件选择对话框时可能报错，但可以直接忽略而无其他副作用（大概？）。这是第三方依赖 `file_selector` 的 BUG 。

	* 在 `Windows` 上，应用通知只有在刚弹出的时候才能相应点击事件，系统通知中心的应用通知无法响应点击事件。这是第三方依赖 `local_notifier` 的 BUG 。

	* 在 `Windows` 上，应用的图标四角存在黑边。这是第三方依赖 `msix` 的 BUG ，参见 [issue #239](https://github.com/YehudaKremer/msix/issues/239) 。

	* 在 `Windows` 上，如果 AppData 目录中不存在应用自身的数据目录，应用会在自身的沙盒 AppData 目录中新建自身的数据目录并存储数据；预期行为应当是始终读写非沙盒 AppData 目录。如果要修复这个 BUG ，需要在 MSIX appxmanifest 中添加 `<rescap:Capability Name="unvirtualizedResources" />` 及 `<desktop6:FileSystemWriteVirtualization>disabled</desktop6:FileSystemWriteVirtualization>` 配置项。用于打包应用的第三方依赖 `msix` 目前只能构建容器化的安装包，需要手动修改所生成的 MSIX 并重打包。

	* `Modding Worker` 尽管整个消息列表内的文本都处于同一选择域内，但当用户执行全选时，有可能弹出报错对话框，这是因为无法获取列表内处于非可视区的文本控件的内容。这是预期行为。

* `AssistantPlus`

	* 有时，标题栏顶部的一部分区域（高度为标准标题栏的高度）会错误地变为可拖拽区域，同时区域中的图标等控件无法响应鼠标输入。这似乎是 WindowsAppSDK 的 BUG 。

	* 有时，可滚动的 UI 控件无法滚动到最底部，而是会呈现出一种混乱的滚动效果，且有可能导致程序崩溃。这似乎是 WindowsAppSDK 的 BUG 。

	* 由于写死了部分对话框的尺寸，在低尺寸窗口的情况下无法显示完整的对话框。

	* 由于 `FileSavePicker` 的限制，文件保存对话框无法很好地显示为无扩展名模式。

	* 应用内的 `TreeView` 项目可能不遵循所设定的 `IsExpanded` 值，致使本应展开的项目呈现为折叠状态。这似乎是 WindowsAppSDK 1.6 的 BUG 。

	* `Modding Worker` 在消息列表中，每个文本控件都有各自独立的选择域，无法跨多个文本控件选择文本。
