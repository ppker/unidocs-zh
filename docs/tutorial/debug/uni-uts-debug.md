# uts Android调试

uts在Android上的调试，包括3部分：
1. uni-app和uni-app x的uts插件的uts代码。需HBuilderX 4.0+ 
1. uni-app和uni-app x的uts插件的混编kt代码。需HBuilderX 4.61+ 
3. uni-app x的uvue页面。需HBuilderX 4.61+

uts、uvue、kt，这三种文件，本质上都是在调试运行时的kt文件。

HBuilderX中可以对这3种文件打断点，联编、跨语言、跨文件跳转断点。

## 开启调试

如下Gif动画所示，运行uni-app uts项目到Android，运行成功后，HBuilder控制台点击`红色虫子`图标，下拉菜单选择【uts调试】，即可开启uts调试功能。

<img src="https://qiniu-web-assets.dcloud.net.cn/unidoc/zh/uts-android.gif" style="zoom: 72%;" />

> 注意：如果需要触发应用初始化中的断点，比如App.uvue的onLaunch中，需要点击`红色虫子`图标右边的`重启应用`按钮，重启之后应用初始化中的断点才会生效

> 注意：目前部分变量的显示可能还是以kotlin的方式显示，因为uts编译结果是kotlin

> 如果需要调试kt代码的话，需要安装插件kotlin-language。(在打开kt文件时hx会自动提示需要安装该插件)

> 注意：断点时App可能会出现Application Not Responding(应用无响应)的弹框(部分机型的表现是app会重启)，这是因为调试默认是以Attach的方式连接，Android系统不允许UI线程被阻塞太长时间，点击下一步或者断点结束时该弹框会自动消失。(开启断点之后点击点击`红色虫子`图标右边的`重启应用`按钮会以调试模式启动, 此时断点时不会出现`应用无响应`的弹框)

## 注意事项

1、默认打开调试时app并不是以调试模式启动的，这可能会造成一些问题，比如Application Not Responding,也就是应用程序无响应。表现为: 出现弹框显示程序无响应。部分手机可能会出现卡顿或者各种奇怪问题，这是因为有的手机厂商并不希望去调试没有开启调试模式的app。

> 解决办法:  在debug开启之后需要点击`红色虫子`图标右边的`重启应用`按钮，这时候app会以调试模式启动。

> Hx默认是以附加的方式来调试应用，这样的好处是用户如果默认不是以调试模式启动的app，也可以在测试途中来调试应用，而不需要重启应用再走一遍测试流程。

> 开启调试模式的好处是不会出现anr，同时手机厂商一般不会对开启调试模式的app做一些限制。

2、有时候监视区域或者变量区域可能无法显示变量(比如this.xxx.xxx 这时候可能无法查看xxx的内容)，目前显示的内容很多都是kotlin的展示方式，但用户肯定是希望以uts的方式展示和查看。(优化中)

3、变量区域没有显示全局变量和上一级作用域的变量信息 (优化中)


## 添加/删除断点@add-breakpoint

打开要调试的uts文件，在代码行号上，鼠标右击或双击添加断点。

<img src="https://qiniu-web-assets.dcloud.net.cn/unidoc/zh/uts-add-breakpoint.png" style="zoom: 50%;" />

## 调试视图@debug-View

开启调试后，即可在HBuilderX左侧视图，看到调试视图，具体如下：

<img src="https://qiniu-web-assets.dcloud.net.cn/unidoc/zh/uts-debug-view.jpg" style="zoom: 60%;" />

下图中包含了uvue、uts、kotlin的调试步骤

<img src="https://web-ext-storage.dcloud.net.cn/hx/debug/android-debug.gif" style="zoom: 60%;" />

调试视图分为5部分:

- [调试工具栏](#debugactions)
- 变量窗口 (`复制值`、`复制表达式`、`添加到监视`)
- 监视窗口（包含`添加`/`编辑`/`删除`表达式，以及`复制值`）
- 调用堆栈窗口
- 断点窗口（包含`删除`/`启用`/`禁用`断点）

## 调试操作@DebugActions

- 继续 `F8`
- 下一步 `F10`
- 进入 `F11`
- 返回 `Shift+F11`

<img src="https://qiniu-web-assets.dcloud.net.cn/unidoc/zh/uts-debug-action.jpg" style="zoom: 50%;" />

## 数据检查和查看变量@data

### 添加到监视@add-to-watch

在【变量窗口】，选中变量，右键菜单，即可将变量添加到监视窗口。

<img src="https://qiniu-web-assets.dcloud.net.cn/unidoc/zh/uts-add_to_monitor.png" style="zoom: 50%;" />

### 悬停显示@hover

断点调试过程中，将鼠标悬停在要查看的变量上，即可打开悬停窗口。

<img src="https://qiniu-web-assets.dcloud.net.cn/unidoc/zh/uts-hovering_window.jpg" style="zoom: 60%;" />

