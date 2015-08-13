# chrome扩展开发 — 词汇

### manifest.json
manifest文件，manifest清单
> Chrome扩展都包含一个Manifest文件——manifest.json，这个文件可以告诉Chrome关于这个扩展的相关信息，它是整个扩展的入口，也是Chrome扩展必不可少的部分。

### options_page
选项页面
>Chrome通过Manifest文件的options_page属性为开发者提供了这样的接口，可以为扩展指定一个选项页面。当用户在扩展图标上点击右键，选择菜单中的“选项”后，就会打开这个页面1。

### runtime.sendMessage
发送消息
> 在扩展的多个页面，或者不同扩展的多个页面间发送数据的借口

### runtime.onMessage
监听消息
> 监听通过sendMessage发送过来的消息

### sendResponse
发送回应 - 方法
> 在onMessage.addListener的回调中用于对监听到的消息做出回应

### chrome.storage
Chrome存储API
> Chrome为扩展应用提供了存储API，以便将扩展中需要保存的数据写入本地磁盘。与localStorage相比，有可以数据同步、content_scripts可以直接读取、可以存储对象类型能改进。

### storage.StorageArea
存储区域
> chrome存储API提供了两个存储区域sync和local。StorageArea必须指定为其中一个

### StorageArea > sync
同步存储区域
> 若指定StorageArea为sync，这个区域中的数据在联网状态下能够通过Google账户进行同步

### storage.onChanged
监听storage数据改变
> Chrome同时还为存储API提供了一个onChanged事件，当存储区的数据发生改变时，这个事件会被激发。

### broswerAction
扩展按钮
> Browser Actions将扩展图标置于Chrome浏览器工具栏中，地址栏的右侧。

### broswerAction > default_icon
扩展按钮的默认图标
> 一般情况下，Chrome会选择使用19像素的图片显示在工具栏中，但如果用户正在使用视网膜屏幕的计算机，则会选择38像素的图片显示。两种尺寸的图片并不是必须都指定的，如果只指定一种尺寸的图片，在另外一种环境下，Chrome会试图拉伸图片去适应，这样可能会导致图标看上去很难看。

### popup
弹出页面
> Popup页面是当用户点击扩展图标时，展示在图标下面的页面。

### broswerAction > badge
徽章
> 在broswerAction的icon上，可以一直展示，内容不能超过4字节，可以通过方法设置背景色和内容。

### contextMenus
右键菜单
> 当用户在网页中点击鼠标右键后，会唤出一个菜单，在上面有复制、粘贴和翻译等选项，为用户提供快捷便利的功能。Chrome也将这里开放给了开发者，也就是说我们可以把自己所编写的扩展功能放到右键菜单中。

### notification
桌面提醒
> Chrome提供了桌面提醒功能，这个功能可以为用户提供更加丰富的信息。

### Omnibox
chrome地址栏 - 多功能输入框
> Chrome和其他浏览器相比一个最大的区别就是地址栏——其实不仅仅是地址栏，而是一个多功能的输入框，Google将其称为omnibox（中文为“多功能框”）。我们熟悉的一个功能就是用户可以直接在omnibox搜索关键字，Chrome也将omnibox开放给开发者，这使得omnibox更加强大。

### pageAction
地址栏内部右侧action
> Page Actions与Browser Actions非常类似，除了Page Actions没有badge外，其他Browser Actions所有的方法Page Actions都有。另外的区别就是，Page Actions并不像Browser Actions那样一直显示图标，而是可以在特定标签特定情况下显示或隐藏，所以它还具有独有的show和hide方法。

### bookmarks
书签
> 操作书签的API集合, 有`create`, `move`, `update`, `remove`, `removeTree`, `getTree`, `getChildren`, `getSubTree`, `get`, `getRecent`, `search`等方法。

### cookies
cookies的API
> 操作cookie的API集合，有`get`, `getAll`, `set`, `remove`, `getAllCookiesStores`等方法。

### history
历史API
> 操作访问历史的API，有`search`, `getVisits`, `addUrl`, `deleteUrl`, `deleteRange`, `deleteAll`等方法。

### management
chrome扩展和应用的管理接口
> 除了通过chrome://extensions/管理Chrome扩展和应用外，也可以通过Chrome的management接口管理。management接口可以获取用户已安装的扩展和应用信息，同时还可以卸载和禁用它们。通过management接口可以编写出智能管理扩展和应用的程序。
> 有`getAll`, `get`, `getPermissionWarningsById`, `getPermissionWarningsByManifest`, `setEnabled`, `uninstall`, `uninstallSelf`, `showConfirmDialog`等方法。

















