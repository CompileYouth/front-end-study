Chrome DevTools（Chrome 开发者工具） 是内嵌在 Chrome 浏览器里的一组用于网页制作和调试的工具。你可以在 Chrome 浏览器中按 F12 / Alt + Cmd + I 来打开浏览器内嵌的开发者工具，关闭也是同样的按键。当然，官网还推荐一款叫做 [Chrome 金丝雀版本（Chrome Canary）](https://www.google.com/intl/en/chrome/browser/canary.html)的 Chrome 浏览器，从这里你可以获得最新版本的 DevTools。为什么 Google 称之为金丝雀呢，因为金丝雀早期在矿井中被用来预警，而该版本的 Chrome 一定程度上也能起到该作用。不用担心 Chrome Canary 会覆盖原本的 Chrome，从 Logo 就可以看出这是两个软件。

![](./res/chrome.png)

## 访问 DevTools

可以通过以下这些方式打开 Chrome DevTools：

- 选择右上角Chrome 菜单，然后选择更多工具 -> 开发者工具
- 右键，选择检查/审查元素

当然，比较推荐利用快捷键来打开：

- Ctrl + Shift + I, F12 / Cmd + Opt + I，打开 DevTools
- Ctrl + Shift + J / Cmd + Opt + J，打开 DevTools，并且定位到控制台面板

上面两种方式不仅可以打开 DevTools，还可以关闭 DevTools。当然，还有一种方式可以打开 DevTools。

- Ctrl + Shift + C / Cmd + Opt + C，打开 DevTools，并且开启审查元素模式（相当于点击了 DevTools 左上角的图标： ![](./res/inspect.png)）

说到快捷键，这里再跟大家介绍几个非常有用的：

- F5, Ctrl + R / Cmd + R，刷新页面
- Ctrl + F5, Ctrl + Shift + R / Cmd + Shift + R，刷新页面并忽略缓存
- Ctrl + '+' / Cmd + Shift + '+'，放大 DevTools
- Ctrl + '-' / Cmd + Shift + '-'，缩小 DevTools
- Ctrl + 0 / Cmd + 0，DevTools 恢复大小

当然，DevTools 里不仅仅这些有用的快捷键，下面在介绍到具体的场景时再介绍。
