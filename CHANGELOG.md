# 更新日志

- [24-05-22](#24-05-22)

- [24-05-23](#24-05-23)

- [24-05-25](#24-05-25)

- [24-05-27](#24-05-27)

- [24-05-28](#24-05-28)

- [24-05-29](#24-05-29)

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

## 24-05-30

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
