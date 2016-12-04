# JavaScript 事件解读

## 1. 事件基本概念

**事件**是指在文档或者浏览器中发生的一些特定交互瞬间，比如打开某一个网页，浏览器加载完成后会触发 `load` 事件，当鼠标悬浮于某一个元素上时会触发 `hover` 事件，当鼠标点击某一个元素时会触发 `click` 事件等等。

**事件处理**就是当事件被触发后，浏览器响应这个事件的行为，而这个行为所对应的代码即为**事件处理程序**。

## 2. 事件操作：监听与移除监听

### 2.1 监听事件

浏览器会根据一些事件作出相对应的事件处理，事件处理的前提是需要监听事件，监听事件的方法主要有以下三种：

#### 2.1.1 HTML 内联属性

即在 HTML 元素里直接填写与事件相关的属性，属性值为事件处理程序。示例如下：

    <button onclick="console.log('You clicked me!');"></button>

`onclick` 对应着 `click` 事件，所以当按钮被点击后，便会执行事件处理程序，即控制台输出 `You clicked me!`。

不过我们需要指出的是，这种方式将 HTML 代码与 JavaScript 代码耦合在一起，不利于代码的维护，所以应该尽量避免使用这样的方式。

#### 2.1.2 DOM 属性绑定

通过直接设置某个 DOM 节点的属性来指定事件和事件处理程序，上代码：

    const btn = document.getElementById("btn");
    btn.onclick = function(e) {
        console.log("You clicked me!");
    };

上面示例中，首先获得 btn 这个对象，通过给这个对象添加 `onclick` 属性的方式来监听 `click` 事件，这个属性值对应的就是事件处理程序。这段程序也被称作 DOM 0 级事件处理程序。

#### 2.1.3 事件监听函数

标准的事件监听函数如下：

    const btn = document.getElementById("btn");
    btn.addEventListener("click", () => {
        console.log("You clicked me!");
    }, false);

上面的示例表示先获得表示节点的 btn 对象，然后在这个对象上面添加了一个事件监听器，当监听到 `click` 事件发生时，则调用回调函数，即在控制台输出 `You clicked me!`。`addEventListener` 函数包含了三个参数 false，第三个参数的含义在后面的事件触发三个阶段之后再讲解。这段程序也被称作 DOM 2 级事件处理程序。IE9+、FireFox、Safari、Chrome 和 Opera 都是支持 DOM 2 级事件处理程序的，对于 IE8 及以下版本，则用 `attacEvent()` 函数绑定事件。

所以我们可以写一段具有兼容性的代码：

    function addEventHandler(obj, eventName, handler) {
        if (document.addEventListener) {
            obj.addEventListener(eventName, handler, false);
        }
        else if (document.attachEvent) {
            obj.attachEvent("on" + eventName, handler);
        }    
        else {
            obj["on" + eventName] = handler;
        }
    }

### 2.2 移除事件监听

在为某个元素绑定了一个事件后，如果想接触绑定，则需要用到 `removeEventListener` 方法。看如下例子：

    const handler = function() {
        // handler logic
    }
    const btn = document.getElementById("btn");

    btn.addEventListener("click", handler);
    btn.removeEventListener("click", handler);

需要注意的是，绑定事件的回调函数不能是匿名函数，必须是一个已经被声明的函数，因为解除事件绑定时需要传递这个回调函数的引用。

同样，IE8 及以下版本也不支持上面的方法，而是用 `detachEvent` 代替。

    const handler = function() {
        // handler logic
    }
    const btn = document.getElementById("btn");

    btn.attachEvent("onclick", handler);
    btn.detachEvent("onclick", handler);

同样，可以写一段具有兼容性的删除事件函数：

    function removeEventHandler(obj, eventName, handler) {
        if (document.removeEventListener) {
            obj.removeEventListener(eventName, handler, false);
        }
        else if (document.detachEvent) {
            obj,detachEvent("on" + eventName, handler);
        }
        else {
            obj["on" + eventName] = null;
        }
    }


## 3. 事件触发过程

事件流描述了页面接收事件的顺序。现代浏览器（指 IE6-IE8 除外的浏览器，包括 IE9+、FireFox、Safari、Chrome 和 Opera 等）事件流包含三个过程，分别是捕获阶段、目标阶段和冒泡阶段，下图形象地说明这个过程：

![](http://i.imgur.com/jnnolvB.jpg)

下面就详细地讲解这三个过程。

### 3.1 捕获阶段

当我们对 DOM 元素进行操作时，比如鼠标点击、悬浮等，就会有一个事件传输到这个 DOM 元素，这个事件从 Window 开始，依次经过 docuemnt、html、body，再不断经过子节点直到到达目标元素，从 Window 到达目标元素父节点的过程称为**捕获阶段**，注意此时还未到达目标节点。

### 3.2 目标阶段

捕获阶段结束时，事件到达了目标节点的父节点，最终到达目标节点，并在目标节点上触发了这个事件，这就是**目标阶段**。

需要注意的是，事件触发的目标节点为最底层的节点。比如下面的例子：

    <div>
        <p>你猜，目标在这里还是<span>那里</span>。</p>
    </div>

当我们点击“那里”的时候，目标节点是`<span></span>`，点击“这里”的时候，目标节点是`<p></p>`，而当我们点击`<p></p>`区域之外，`<div></div>`区域之内时，目标节点就是`<div></div>`。

### 3.3 冒泡阶段

当事件到达目标节点之后，就会沿着原路返回，这个过程有点类似水泡从水底浮出水面的过程，所以称这个过程为**冒泡阶段**。

针对这个过程，wilsonpage 做了一个 [DEMO](http://jsbin.com/exezex/4/edit?css,js,output)，可以非常直观地查看这个过程。


现在再看 `addEventListener(eventName, handler, useCapture)` 函数。第三个参数是 useCapture，代表是否在捕获阶段进行事件处理， 如果是 false， 则在冒泡阶段进行事件处理，如果是 true，在捕获阶段进行事件处理，默认是 false。这么设计的主要原因是当年微软和 netscape 之间的浏览器战争打得火热，netscape 主张捕获方式，微软主张冒泡方式，W3C 采用了折中的方式，即先捕获再冒泡。


## 4、事件委托

上面我们讲了事件的冒泡机制，我们可以利用这一特性来提高页面性能，事件委托便事件冒泡是最典型的应用之一。

何谓“委托”？在现实中，当我们不想做某件事时，便“委托”给他人，让他人代为完成。JavaScript 中，事件的委托表示给元素的父级或者祖级，甚至页面，由他们来绑定事件，然后利用事件冒泡的基本原理，通过事件目标对象进行检测，然后执行相关操作。看下面例子：

    // HTML
    <ul id="list">
        <li>Item 1</li>
        <li>Item 2</li>
        <li>Item 3</li>
        <li>Item 4</li>
        <li>Item 5</li>
    </ul>

    // JavaScript
    var list = document.getElementById("list");
    list.addEventListener("click", function(e) {
        console.log(e.target);
    });

上面的例子中，5 个列表项的点击事件均委托给了父元素 `<ul id="list"></ul>`。

先看看事件委托的可行性。有人会问，当事件不是加在某个元素上的，如何在这个元素上触发事件呢？我们就是利用事件冒泡的机制，事件流到达目标元素后会向上冒泡，此时父元素接收到事件流便会执行事件执行程序。有人又会问，被委托的父元素下面如果有很多子元素，怎么知道事件流来自于哪个子元素呢？这个我们可以从事件对象中的 `target` 属性获得。事件对象下面会详细讲解。

我们再来看看为什么需要事件委托。

- 减少事件绑定。上面的例子中，也可以分别给每个列表项绑定事件，但利用事件委托的方式不仅省去了一一绑定的麻烦，也提升了网页的性能，因为每绑定一个事件便会增加内存使用。
- 可以动态监听绑定。上面的例子中，我们对 5 个列表项进行了事件监听，当删除一个列表项时不需要单独删除这个列表项所绑定的事件，而增加一个列表项时也不需要单独为新增项绑定事件。

看了上面的例子和解释，我们可以看出事件委托的核心就是监听一个 DOM 中更高层、更不具体的元素，等到事件冒泡到这个不具体元素时，通过 event 对象的 target 属性来获取触发事件的具体元素。

## 5、阻止事件冒泡

事件委托是事件冒泡的一个应用，但有时候我们并不希望事件冒泡。比如下面的例子：

    const ele = document.getElementById("ele");
    ele.addEventListener("click", function() {
        console.log("ele-click");
    }, false);

    document.addEventListener("click", function() {
        console.log("document-click");
    }, false);

我们本意是当点击 ele 元素区域时显示 "ele-click"，点击其他区域时显示 "document-click"。但是我们发现点击 ele 元素区域时会依次显示 "ele-click" "document-click"。那是因为绑定在 ele 上的事件冒泡到了 document 上。想要解决这个问题，只需要加一行代码：

    const ele = document.getElementById("ele");
    ele.addEventListener("click", function(e) {
        console.log("ele-click");
        e.stopPropagation(); // 阻止事件冒泡
    }, false);

    document.addEventListener("click", function(e) {
        console.log("document-click");
    }, false);

我们还能用 `e.cancelBubble = true` 来替代 `e.stopPropagation()`。网上的说法是 `cancelBubble` 仅仅适用于 IE，而 `stopPropagation` 适用于其他浏览器。但根据我实验的结果，现代浏览器（IE9 及 以上、Chrome、FF 等）均同时支持这两种写法。为了保险起见，我们可以采用以下代码：

    function preventBubble(e) {
        if (!e) {
            const e = window.event;
        }
        e.cancelBubble = true;
        if (e.stopPropagation) {
            e.stopPropagation();
        }
    }

## 6、event 对象

Event 对象代表事件的状态，比如事件在其中发生的元素、键盘按键的状态、鼠标的位置、鼠标按钮的状态。当一个事件被触发的时候，就会创建一个事件对象。

我们用下面的代码打印出事件对象：

    <div id="list">
        <li>Item 1</li>
        <li>Item 2</li>
    </div>
    <script>
        var list = document.getElementById("list");
        list.addEventListener("click", function(e) {
            console.log(e);
        });
    </script>

chrome 49 的运行结果如下：

![](http://i.imgur.com/WNGZpcC.png)

下面介绍一些比较常用的属性和方法。

### target、 srcElement、 currentTarget 和 relatedTarget、fromElement、 toElement

- target 与 srcElement 完全相同；
- target 指触发事件的元素， currentTarget 指事件所绑定的元素；
- relatedTarget： 与事件的目标节点相关的节点。对于 mouseover 事件来说，该属性是鼠标指针移到目标节点上时所离开的那个节点。对于 mouseout 事件来说，该属性是离开目标时，鼠标指针进入的节点。
对于其他类型的事件来说，这个属性没有用；
- fromElement 和 toElement 仅仅对于 mouseover 和 mouseout 事件有效。

以上面的例子说明，当点击 `<li>Item 1</li>` 时，target 就是 `<li>Item 1</li>` 元素，而 currentTarget 是 `<div id="list"></div>`。

### clientX/Y、 screenX/Y、 pageX/Y、 offsetX/Y

上图：

![](http://i.imgur.com/RYJisO4.png)

- offsetX/Y： 点击位置相对于所处元素左上角的位置；
- clientX/Y： 点击位置相对于浏览器内容区域左上角的位置；
- screenX/Y： 点击位置相对于屏幕左上角的位置；
- pageX/Y： 点击位置相对整张页面左上角的位置；
- pageX/Y 与 clientX/Y 一般情况下会相同，只有出现滚动条时才不一样。


### altKey、 ctrlKey、 shiftKey

- altKey： 返回当事件被触发时，"ALT" 是否被按下；
- ctrlKey： 返回当事件被触发时，"CTRL" 键是否被按下；
- shiftKey： 返回当事件被触发时，"SHIFT" 键是否被按下；

### 其他属性

- type： 返回当前 Event 对象表示的事件的名称
- bubbles： 返回布尔值，指示事件是否是起泡事件类型；
- cancelable： 返回布尔值，指示事件是否可拥可取消的默认动作；
- eventPhase： 返回事件传播的当前阶段，有三个值： Event.CAPTURING_PHASE、 Event.AT_TARGET、 Event.BUBBLING_PHASE，对应的值为 1、2、3，分别表示捕获阶段、正常事件派发和起泡阶段；
- path： 冒泡阶段经过的节点；

### 方法

- preventDefault()： 通知浏览器不要执行与事件关联的默认动作；
- stopPropagation()： 阻止冒泡；

参考及拓展阅读：

1. [HTML DOM Event 对象](http://www.w3school.com.cn/jsref/dom_obj_event.asp)
2. [最详细的JavaScript和事件解读](http://www.codeceo.com/article/javascript-event-anay.html)
3. [JavaScript Events](http://www.quirksmode.org/js/contents.html#events)
4. [[解惑]JavaScript事件机制](http://www.cnblogs.com/hustskyking/p/problem-javascript-event.html)
