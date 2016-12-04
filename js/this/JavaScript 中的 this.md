# 精确把握 JavaScript 中的 this

this 是 JavaScript 中的一个关键字，当一个函数被调用时，除了传入函数的显式参数以外，名为 this 的隐式参数也被传入了函数。this 参数指向了一个自动生成的内部对象，这个内部对象被称为**函数上下文**。与其他面向对象的语言不同的是， JavaScript 中的 this 依赖于函数的调用方式。所以，想要明白 this 的指向问题，还必须先研究函数在 JavaScript 中是如何被调用的。

## 调用方式 1、作为函数进行调用

这说法有点奇怪，函数当然是被作为函数进行调用的，但我们说一个函数“作为函数”被调用，只是为了区别于其他的调用方式。先看一个简单的例子：


    function func1() {
        console.log(this === window); // true
    }
    func1();

    var func2 = function () {
        console.log(this === window); // true
    }
    func2();

    function func3() {
        "use strict";
        console.log(this); // undefined
    }
    func3();

上面例子中的第一第二个 this 在非严格模式下指向全局上下文，即 window 对象，第三个在严格模式下则为 undefined。

所以，当函数被作为函数进行调用时，在非严格模式下，函数的上下文是 window 对象，而在严格模式下，函数上下文为 undefined。

## 调用方式 2、作为方法进行调用

当一个函数被赋值给一个对象的一个属性，并使用引用该函数的这个属性进行调用函数时，那么函数就是作为该对象的一个方法进行调用的。看下面的例子：

    var obj = {
        func: function() {
            console.log(this === obj); // true
        }
    };
    obj.func();

上面的例子中，函数 func 的调用对象为 obj，所以函数上下文便是 obj。由此可见，将函数作为对象的一个方法进行调用时，该对象就是函数上下文，并且在函数内部可以用 this 来访问这个对象。

此时，我们再看一下第一种调用方式，即“作为函数”调用。“作为函数”进行调用的函数是定义在 window 上的，调用时也不需要 window 的引用，其实方式 1 中例子中的 `func1()` 就是 `window.func1()`，所以例子中的函数上下文便是 this。

## 调用方式 3、作为构造器进行调用

将函数作为构造器时，函数的声明与其他调用方式的函数声明一致。将函数作为构造器调用的例子如下：

    function Student() {
        this.getContext = function() {
            return this;
        }
    }
    var stu = new Student();
    console.log(stu.getContext() === stu); // true

将函数作为构造器调用时，便会通过这个函数生成一个新对象，这时，this 指向这个新创建的对象。

从上面几种调用方式来看，函数调用方式之间的主要差异是：作为 this 参数传递给执行函数的上下文对象之间的区别。作为方法进行调用，该上下文是方法的拥有者；作为全局函数调用，其上下文永远是 window （也就是说，该函数是 window 的一个方法）；作为构造器进行调用时，其上下文对象则是新创建的对象实例。

下面的一种调用方式可以显式地指定上下文。

## 调用方式 4、使用 apply() 和 call() 方法进行调用

JavaScript 的每个函数都有 apply() 和 call() 函数，可以利用任何一个函数都可以显式指定任何一个对象作为其函数上下文。

通过 apply() 方法来调用函数，我们要给 apply() 传入两个参数：一个作为函数上下文对象，另一个作为函数参数所组成的数组。call() 方法的使用方式类似，唯一不同的是给函数传入的参数是一个参数列表，而不是单个数组。

    function func() {
        var result = 0;
        for(var i = 0; i < arguments.length; i++) {
            result += arguments[i];
        }
        this.result = result;
    }
    var obj1 = {};
    var obj2 = {};
    func.apply(obj1, [1, 2, 3]);
    func.call(obj2, 4, 5, 6);

    console.log(obj1.result === 6); // true
    console.log(obj2.result === 15); // true

在上面的代码中，`func.apply(obj1, [1, 2, 3]);` 将函数的上下文定义为 obj1，并且传入 1、2、3 三个参数，`func.call(obj2, 4, 5, 6);` 将函数的上下文定义为 obj2，并且传入 4、5、6 三个参数。

那 apply 和 call 基本相同，那么我们该用哪一个呢？其实 apply 和 call 的区别仅仅在于调用时传入的参数不同，其他完全一样。所以，在选择时，主要看传入的参数。如果已知参数已经在数组里了则用 apply 即可，或者参数是动态生成的，可以把参数 push 进一个数组，然后再用 apply 调用。当参数数量已知，或者在参数里有很多无关的值则用 call 方法调用。


## 其他补充

### 利用 bind() 改变函数上下文

先看下面例子：

    var obj1 = {
        a: 1
    };
    var obj2 = {
        a: 2,
        func: function() {
            console.log(this.a);
        }.bind(obj1)
    };

    obj2.func(); // 1

ECMAScript 5 引入了 `Function.prototype.bind`，其会创建一个绑定函数，当调用这个绑定函数时，函数上下文将会是 bind() 方法的第一个参数。上面的例子中，将 obj1 设置为函数上下文，所以利用 func 来调用函数时，函数的上下文为 obj1，而不是它的调用者 obj2。

### 利用 Array 的 5 个方法改变函数上下文

5 个方法分别是：

- `Array.prototype.every(callbackfn [, thisArg ])`
- `Array.prototype.some(callbackfn [, thisArg ])`
- `Array.prototype.forEach(callbackfn [, thisArg ])`
- `Array.prototype.map(callbackfn [, thisArg ])`
- `Array.prototype.filter(callbackfn [, thisArg ])`

当调用以上 5 个方法时，传入的参数除了回调函数以外，还可以传入另外一个可选地参数，即函数上下文，代表回调函数中的函数上下文。如果省略该参数，则 callback 被调用时的 this 值，在非严格模式下为全局对象，在严格模式下传入 undefined。看下面的例子：

    var arr = ["segmentfault"];
    var obj = {};
    arr.forEach(function(ele, ind) {
        console.log(this === window); // true
    });
    arr.forEach(function(ele, ind) {
        console.log(this === obj);    // true
    }, obj);

## 测试

在网上找了几个代码小例子，来测试对上面内容的理解，答案在最下面。

### 1、

    if (true) {
        // this
    }

### 2、

    var obj = {
        someData: "a string"
    };

    function myFun() {
        // this
    }

    obj.staticFunction = myFun;
    obj.staticFunction();

### 3、

    var obj = {
        myMethod : function () {
            // this
        }
    };
    var myFun = obj.myMethod;
    myFun();


### 4、

    function myFun() {
        // this
    }
    var obj = {
        someData: "a string"
    };
    myFun.call(obj);

### 答案

- 1、window
- 2、obj
- 3、window
- 4、obj


注：以上代码均在浏览器环境中测试。
