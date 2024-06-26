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

## 已知问题

* `Kernel`

	* 由于所依赖的第三方库 `md5` 的不足，应用在计算大文件（4G及以上）时会无限循环。

* `Assistant`

	* 应用内的一些编辑控件只在触发失焦事件时才会对数据与 UI 进行更新，这是预期行为；但是，在 `Android` 与 `Iphone` 上，如果在编辑控件时直接与需要消费编辑控件所控制的数据的控件进行交互，该控件将无法接收到最新编辑的数据值，必须先退出 IME 以使控件失焦。

	* 在 `Android` 上，AboutPanel 中的第一行文本可能被截断了一些顶部区域，在与界面中的 Chip 进行一次交互后会恢复正常。尚不知该 BUG 的成因，故无法修复。

	* 在 `Android` 上，默认遵循的系统配色值未适配 Flutter 3.22 的变更，因此会出现部分颜色混淆的 BUG 。需要等待第三方依赖包的修复。

	* 在 `Windows` 上，应用的图标四角存在黑边。这是用于打包应用的第三方依赖 `msix` 的问题。

	* 在 `Windows` 上，应用通知只有在刚弹出的时候才能相应点击事件，系统通知中心的应用通知无法响应点击事件。这是第三方依赖 `local_notifier` 的 BUG 。

	* 在 `Windows` 上，如果 AppData 目录中不存在应用自身的数据目录，应用会在自身的沙盒 AppData 目录中新建自身的数据目录并存储数据；预期行为应当是始终读写非沙盒 AppData 目录。如果要修复这个 BUG ，需要在 MSIX appxmanifest 中添加 `<rescap:Capability Name="unvirtualizedResources" />` 及 `<desktop6:FileSystemWriteVirtualization>disabled</desktop6:FileSystemWriteVirtualization>` 配置项。用于打包应用的第三方依赖 `msix` 目前只能构建容器化的安装包，需要手动修改所生成的 MSIX 并重打包。

* `AssistantPlus`

	* 由于写死了部分对话框的尺寸，在低尺寸窗口的情况下无法显示完整的对话框。
