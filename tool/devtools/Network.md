Chrome DevTools 的 Network 面板主要用于度量当前网页的网络性能状况，其记录了当前网页每个网络操作的信息，包括时间数据、HTTP 请求、返回数据等等。

先看 Network 面板长啥样~下面是我 Github 首页的 Network 面板。

![](./res/network-1.png)

Network 面板包含很多小面板，有些是默认隐藏的。其中最主要的一个面板就是控制面板(Control Pane)，即上图中的 "1" 部分，控制面板上有很多的功能按钮。"2" 部分是请求表格(Requests Table)，表格内容按照请求的顺序排序，点击一个请求的 "Name" 列可以查看这个请求的具体信息；在表格的头部，右键可以看到还有很多隐藏的列，比如 Method、Protocol、Domain 等等。"3" 部分是概览(Summary)，主要显示这个网页有多少请求，请求的数据有多大，一共花费了多少时间。

上面是 Network 3 个面板的大概了解，下面就对面板 "1"、"2" 进行重点介绍。

## Control 面板

### 开始/停止记录

![](./res/network-3.png) 表示正常记录网络的请求状况，点击，当图标变成 ![](./res/network-4.png) 时，代表停止记录请求状况。

### 清除记录

点击 ![](./res/network-5.png) 可以清除请求表格内的内容。

### 截取加载的瞬间

点击 ![](./res/network-6.png) ，当图标变成 ![](./res/network-7.png) 时，代表开启截屏模式，即截取网页在加载过程中的瞬间，这些瞬间显示了网页在什么时间加载出了哪些部分。以我的 Github 首页为例，打开截屏功能后，出现下面的结果：

![](./res/network-8.png)

在控制面板下面出现了一块新的面板，用来显示网页加载过程中的截屏。当鼠标悬浮在一张截图上时，下面的请求表格面板上出现了这张截图加载时的时间；点击一张截图时，请求表格面板也会显示相应时间内加载的资源及其信息；双击一张截图，可以查看这张截图的大图，可通过左右键进行切换。

### 过滤

点击 ![](./res/network-10.png) 可以显示/隐藏过滤面板，如下：

![](./res/network-9.png)

可以把过滤面板看成由三个部分组成的面板，从左到右分别是：过滤文本域、隐藏 Data URLs 和请求资源类型选择。

隐藏 Data URLs 比较好理解，对 Data URLs 不理解的可以看：[Data URLs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs)。在这里主要就是为了隐藏这些用 Data URLs 来展示的资源。

在 "Hide data URLs" 后面有一组文件类型，选择一个可以在下面的请求表格中只显示相应类型的文件请求，当然你还可以在选择的同时按着 ctrl / cmd，那么就可以多选。

下面来说过滤文本域，就是过滤面板最左边的输入框，为什么放到最后讲呢，因为这里面的文章太多了，超出了我们的想象。不卖关子了，下面具体聊聊吧。

首先，最简单的用法就是我们输入一个字符串，然后可以过滤出那些资源名称中包含相应字符串的资源。这是我们一般使用的，除此以外，文本域还支持一些关键词，举个栗子：

![](./res/network-11.png)

上面的例子就用到了 "larger-than" 关键词，后面跟的参数是文件大小，可以刷选出大于 1k 的文件。

下面就看看这些关键词有哪些：

- domain：刷选来自特定域的请求
- has-response-header：刷选 HTTP 返回值包含特定头部信息的请求
- is：可以用 `is:running` 查看 WebSocket 资源
- larger-than：筛选文件大小超过特定数字的请求，默认单位是 byte
- method：刷选特定 HTTP 请求方法的请求
- mime-type：刷选特定 MIME 类型的请求
- mixed-content：有 `mixed-content:all` 和 `mixed-content:displayed` 两种
- scheme：刷选特定 scheme 的请求
- set-cookie-domain：刷选特定的 HTTP 返回头部的 set-cookie 属性的请求
- set-cookie-name：也是对返回的 HTTP 头部中某个属性进行刷选的关键词
- set-cookie-value：同上
- status-code：对请求的状态值进行刷选

说到这，其实也就差不多了，但还有几点需要注意一下：1) 冒号后面不能有空格；2) 大小写敏感。
