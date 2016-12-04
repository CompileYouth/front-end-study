# Javascript 中 console 的用法

在调试 JS 代码时，很多人倾向于使用 `alert()` 或者 `console.log()` 方法来输出信息，正如某些 Java 程序员喜欢在调试代码时使用 `System.out.println()` 输出信息一样。但与 Java 输出不一样的是， console 对象拥有多种方法可以更好的呈现信息，从而给代码调试带来方便。根据常用程度，列出以下几种 console 对象的方法：

- `console.log()`
- `console.debug()`、`console.info()`、`console.warn()` 与 `console.error()`
- `console.table()`
- `console.time()` 与 `console.timeEnd()`
- `console.assert()`
- `console.count()`
- `console.group`、`console.groupEnd()` 与 `console.groupCollapsed()`

以下示例的运行环境是 Chrome 43。

## console.log()

先来谈谈我们最熟悉也最常用的 `console.log()` 方法。

我们最常用的做法是通过它来输出一个变量或者输出一个字符串。比如下面：

	console.log("Hello China!");
	var str = "Hello world!";
	console.log(str);

运行结果如下：

	Hello China!
	Hello world!

但我们也可以这样用 `console.log()` ：

	// 代码段 1
	var str1 = "hello";
	var str2 = "world";
	console.log(str1, str2);

	// 代码段 2
	console.log("%d年%d月%d日", 2015, 09, 22);

控制台会输出：

	hello world
	2015年9月22日

代码片段 1 显示，`console.log()` 的参数可以有多个，输出的结果以一个空格隔开。

代码片段 2 显示，`console.log()` 可以使用 C 语言 `printf()` 风格的占位符，不过其支持的占位符种类较少，只支持字符串（%s）、整数（%d或%i）、浮点数（%f）和对象（%o）。


## console.debug()、 console.info()、 console.warn() 与 console.error()

这四个方法的使用方法跟 `console.log()` 一模一样，差别在于输出的颜色与图标不同。下面是示例：

	console.log("log");
	console.debug("debug");
	console.info("info");
	console.warn("warn");
	console.error("error");

运行结果如下：

![](http://i.imgur.com/vJJoNFM.png)


## console.table()

我们看下面一个变量：

	var people = {
		"person1": {"fname": "san", "lname": "zhang"},
		"person2": {"fname": "si", "lname": "li"},
		"person3": {"fname": "wu", "lname": "wang"}
	};

我们用 `console.log()` 将之在 Chrome 的控制台中输出：

![](http://i.imgur.com/zvnvFOx.png)

再用 `console.table()` 输出：

![](http://i.imgur.com/IivvkKr.png)

所以从上面两种输出我们可以看出，当输出类似于这种两层嵌套的对象时，我们可以选择 `console.table()` 以表格的形式输出。当然，嵌套三层及以上的也会以表格形式输出，但限于表格只能显示二维信息的特点，其会在嵌套三层或以上的地方会显示 "Object" 字符串。

## console.time() 与 console.timeEnd()

在调试时，我们经常需要知道一段代码执行时间，我们可以使用这两行代码来实现。看下面一段代码：

	console.time("for-test");
	var arr = [];
	for(var i = 0; i < 100000; i++) {
		arr.push({"key": i});
	}
	console.timeEnd("for-test");

输出为：

	for-test: 176.152ms

从上面的例子可以看出，我们用 `console.time()` 和 `console.timeEnd()` 包围要测试运行时间的代码，这两个方法的参数保持一致，以便正确识别和匹配代码开始和结束的位置。


## console.assert()

`console.assert()` 类似于单元测试中的断言，当表达式为 false 时，输出错误信息。示例如下：

	var arr = [1, 2, 3];
	console.assert(arr.length === 4);

输出结果如下：

![](http://i.imgur.com/1qwQFNl.png)


## console.count()

调试代码时，我们经常需要知道一段代码被执行了多少次，我们可以使用 `console.count()` 来方便的达到我们的目的。示例如下：

	function func() {
		console.count("label");
	}

	for(var i = 0; i < 3; i++) {
		func();
	}

运行结果为：

	label: 1
	label: 2
	label: 3


## console.group()、 console.groupEnd() 与 console.groupCollapsed()

一般的 `console.log()` 方法的输出没有层级关系，在需要一些显示层级关系的输出中显得苍白无力，使用 `console.group()` 可以达到我们的目的。示例代码如下：

	console.group("1");
	console.log("1-1");
	console.log("1-2");
	console.log("1-3");
	console.groupEnd();
	console.group("2");
	console.log("2-1");
	console.log("2-2");
	console.log("2-3");
	console.groupEnd();

运行结果为：

![](http://i.imgur.com/t1DjKDc.png)

把 "group" 换成 "groupCollapsed"，则默认为折叠运行结果。

由于本文只列出部分方法，查看完整方法请移步阮一峰前辈的[博客](http://javascript.ruanyifeng.com/tool/console.html)。
