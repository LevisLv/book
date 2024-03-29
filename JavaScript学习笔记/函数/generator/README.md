<link rel="stylesheet" href="../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../static/css/console.css"/>

# generator
---

generator（生成器）是ES6标准引入的新的数据类型。一个generator看上去像一个函数，但可以返回多次。

ES6定义generator标准的哥们借鉴了Python的generator的概念和语法，如果你对Python的generator很熟悉，那么ES6的generator就是小菜一碟了。如果你对Python还不熟，赶快恶补[Python教程](http://www.liaoxuefeng.com/wiki/1016959663602400/1017318207388128)！。

我们先复习函数的概念。一个函数是一段完整的代码，调用一个函数就是传入参数，然后返回结果：

```javascript
function foo(x) {
    return x + x;
}

var r = foo(1); // 调用foo函数
```

函数在执行过程中，如果没有遇到<font color="red"><code>return</code></font>语句（函数末尾如果没有<font color="red"><code>return</code></font>，就是隐含的<font color="red"><code>return undefined;</code></font>），控制权无法交回被调用的代码。

generator跟函数很像，定义如下：

```javascript
function* foo(x) {
    yield x + 1;
    yield x + 2;
    return x + 3;
}
```

generator和函数不同的是，generator由<font color="red"><code>function*</code></font>定义（注意多出的<font color="red"><code>*</code></font>号），并且，除了<font color="red"><code>return</code></font>语句，还可以用<font color="red"><code>yield</code></font>返回多次。

大多数同学立刻就晕了，generator就是能够返回多次的“函数”？返回多次有啥用？

还是举个栗子吧。

我们以一个著名的斐波那契数列为例，它由<font color="red"><code>0</code></font>，<font color="red"><code>1</code></font>开头：

> 0 1 1 2 3 5 8 13 21 34 ...

要编写一个产生斐波那契数列的函数，可以这么写：

```javascript
function fib(max) {
    var
        t,
        a = 0,
        b = 1,
        arr = [0, 1];
    while (arr.length < max) {
        [a, b] = [b, a + b];
        arr.push(b);
    }
    return arr;
}

// 测试:
fib(5); // [0, 1, 1, 2, 3]
fib(10); // [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

函数只能返回一次，所以必须返回一个<font color="red"><code>Array</code></font>。但是，如果换成generator，就可以一次返回一个数，不断返回多次。用generator改写如下：

```javascript
function* fib(max) {
    var
        t,
        a = 0,
        b = 1,
        n = 0;
    while (n < max) {
        yield a;
        [a, b] = [b, a + b];
        n ++;
    }
    return;
}
```

直接调用试试：

```javascript
fib(5); // fib {[[GeneratorStatus]]: "suspended", [[GeneratorReceiver]]: Window}
```

直接调用一个generator和调用函数不一样，<font color="red"><code>fib(5)</code></font>仅仅是创建了一个generator对象，还没有去执行它。

调用generator对象有两个方法，一是不断地调用generator对象的<font color="red"><code>next()</code></font>方法：

```javascript
var f = fib(5);
f.next(); // {value: 0, done: false}
f.next(); // {value: 1, done: false}
f.next(); // {value: 1, done: false}
f.next(); // {value: 2, done: false}
f.next(); // {value: 3, done: false}
f.next(); // {value: undefined, done: true}
```

<font color="red"><code>next()</code></font>方法会执行generator的代码，然后，每次遇到<font color="red"><code>yield x</code></font>;就返回一个对象<font color="red"><code>{value: x, done: true/false}</code></font>，然后“暂停”。返回的<font color="red"><code>value</code></font>就是<font color="red"><code>yield</code></font>的返回值，<font color="red"><code>done</code></font>表示这个generator是否已经执行结束了。如果<font color="red"><code>done</code></font>为<font color="red"><code>true</code></font>，则<font color="red"><code>value</code></font>就是<font color="red"><code>return</code></font>的返回值。

当执行到done为<font color="red"><code>true</code></font>时，这个generator对象就已经全部执行完毕，不要再继续调用<font color="red"><code>next()</code></font>了。

第二个方法是直接用<font color="red"><code>for ... of</code></font>循环迭代generator对象，这种方式不需要我们自己判断<font color="red"><code>done</code></font>：

```javascript
'use strict'

function* fib(max) {
    var
        t,
        a = 0,
        b = 1,
        n = 0;
    while (n < max) {
        yield a;
        [a, b] = [b, a + b];
        n ++;
    }
    return;
}
for (var x of fib(10)) {
    console.log(x); // 依次输出0, 1, 1, 2, 3, ...
}
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#generator');
    try {
        'use strict'
        function* fib(max) {
            var
                t,
                a = 0,
                b = 1,
                n = 0;
            while (n < max) {
                yield a;
                [a, b] = [b, a + b];
                n ++;
            }
            return;
        }
        for (var x of fib(10)) {
            console.log(x); // 依次输出0, 1, 1, 2, 3, ...
        }
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>0<br>1<br>1<br>2<br>3<br>5<br>8<br>13<br>21<br>34</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="generator" hidden></p>

generator和普通函数相比，有什么用？

因为generator可以在执行过程中多次返回，所以它看上去就像一个可以记住执行状态的函数，利用这一点，写一个generator就可以实现需要用面向对象才能实现的功能。例如，用一个对象来保存状态，得这么写：

```javascript
var fib = {
    a: 0,
    b: 1,
    n: 0,
    max: 5,
    next: function () {
        var
            r = this.a,
            t = this.a + this.b;
        this.a = this.b;
        this.b = t;
        if (this.n < this.max) {
            this.n ++;
            return r;
        } else {
            return undefined;
        }
    }
};
```

用对象的属性来保存状态，相当繁琐。

generator还有另一个巨大的好处，就是把异步回调代码变成“同步”代码。这个好处要等到后面学了AJAX以后才能体会到。

没有generator之前的黑暗时代，用AJAX时需要这么写代码：

```javascript
ajax('http://url-1', data1, function (err, result) {
    if (err) {
        return handle(err);
    }
    ajax('http://url-2', data2, function (err, result) {
        if (err) {
            return handle(err);
        }
        ajax('http://url-3', data3, function (err, result) {
            if (err) {
                return handle(err);
            }
            return success(result);
        });
    });
});
```

回调越多，代码越难看。

有了generator的美好时代，用AJAX时可以这么写：

```javascript
try {
    r1 = yield ajax('http://url-1', data1);
    r2 = yield ajax('http://url-2', data2);
    r3 = yield ajax('http://url-3', data3);
    success(r3);
}
catch (err) {
    handle(err);
}
```

看上去是同步的代码，实际执行是异步的。

### 练习
要生成一个自增的ID，可以编写一个<font color="red"><code>next_id()</code></font>函数：

```javascript
var current_id = 0;

function next_id() {
    current_id ++;
    return current_id;
}
```

由于函数无法保存状态，故需要一个全局变量<font color="red"><code>current_id</code></font>来保存数字。

不用闭包，试用generator改写：

```javascript
'use strict';
function* next_id() {
    ???

}

// 测试:
var
    x,
    pass = true,
    g = next_id();
for (x = 1; x < 100; x ++) {
    if (g.next().value !== x) {
        pass = false;
        console.log('测试失败!');
        break;
    }
}
if (pass) {
    console.log('测试通过!');
}
```

<button class="run" onclick="(() => {
    const answer = `
'use strict';
function* next_id() {
    var current_id = 1;
    while (current_id < Number.MAX_VALUE) {
        yield current_id++;
    }
}
`;
    alert(answer);
})();">Show Answer</button>
### 
