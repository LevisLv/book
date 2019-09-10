<link rel="stylesheet" href="../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../static/css/console.css"/>

# 函数定义和调用
---

### 定义函数
在JavaScript中，定义函数的方式如下：

```javascript
function abs(x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```

上述<font color="red"><code>abs()</code></font>函数的定义如下：
* <font color="red"><code>function</code></font>指出这是一个函数定义；
* <font color="red"><code>abs</code></font>是函数的名称；
* <font color="red"><code>(x)</code></font>括号内列出函数的参数，多个参数以,分隔；
* <font color="red"><code>{ ... }</code></font>之间的代码是函数体，可以包含若干语句，甚至可以没有任何语句。

请注意，函数体内部的语句在执行时，一旦执行到<font color="red"><code>return</code></font>时，函数就执行完毕，并将结果返回。因此，函数内部通过条件判断和循环可以实现非常复杂的逻辑。

如果没有<font color="red"><code>return</code></font>语句，函数执行完毕后也会返回结果，只是结果为<font color="red"><code>undefined</code></font>。

由于JavaScript的函数也是一个对象，上述定义的<font color="red"><code>abs()</code></font>函数实际上是一个函数对象，而函数名<font color="red"><code>abs</code></font>可以视为指向该函数的变量。

因此，第二种定义函数的方式如下：

```javascript
var abs = function (x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
};
```

在这种方式下，<font color="red"><code>function (x) { ... }</code></font>是一个匿名函数，它没有函数名。但是，这个匿名函数赋值给了变量<font color="red"><code>abs</code></font>，所以，通过变量<font color="red"><code>abs</code></font>就可以调用该函数。

上述两种定义<font color="red"><code>完全等价</code></font>，注意第二种方式按照完整语法需要在函数体末尾加一个<font color="red"><code>;</code></font>，表示赋值语句结束。

### 调用函数
调用函数时，按顺序传入参数即可：

```javascript
abs(10); // 返回10
abs(-9); // 返回9
```

由于JavaScript允许传入任意个参数而不影响调用，因此传入的参数比定义的参数多也没有问题，虽然函数内部并不需要这些参数：

```javascript
abs(10, 'blablabla'); // 返回10
abs(-9, 'haha', 'hehe', null); // 返回9
```

传入的参数比定义的少也没有问题：

```javascript
abs(); // 返回NaN
```

此时<font color="red"><code>abs(x)</code></font>函数的参数<font color="red"><code>x</code></font>将收到<font color="red"><code>undefined</code></font>，计算结果为<font color="red"><code>NaN</code></font>。

要避免收到<font color="red"><code>undefined</code></font>，可以对参数进行检查：

```javascript
function abs(x) {
    if (typeof x !== 'number') {
        throw 'Not a number';
    }
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```

### arguments
JavaScript还有一个免费赠送的关键字<font color="red"><code>arguments</code></font>，它只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数。<font color="red"><code>arguments</code></font>类似<font color="red"><code>Array</code></font>但它不是一个<font color="red"><code>Array</code></font>：

```javascript
'use strict'
function foo(x) {
    console.log('x = ' + x); // 10
    for (var i=0; i<arguments.length; i++) {
        console.log('arg ' + i + ' = ' + arguments[i]); // 10, 20, 30
    }
}
foo(10, 20, 30);
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#arguments');
    try {
        'use strict'
        function foo(x) {
            console.log('x = ' + x); // 10
            for (var i=0; i<arguments.length; i++) {
                console.log('arg ' + i + ' = ' + arguments[i]); // 10, 20, 30
            }
        }
        foo(10, 20, 30);
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>x = 10<br>arg 0 = 10<br>arg 1 = 20<br>arg 2 = 30</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="arguments" hidden></p>

利用<font color="red"><code>arguments</code></font>，你可以获得调用者传入的所有参数。也就是说，即使函数不定义任何参数，还是可以拿到参数的值：

```javascript
function abs() {
    if (arguments.length === 0) {
        return 0;
    }
    var x = arguments[0];
    return x >= 0 ? x : -x;
}

abs(); // 0
abs(10); // 10
abs(-9); // 9
```

实际上<font color="red"><code>arguments</code></font>最常用于判断传入参数的个数。你可能会看到这样的写法：

```javascript
// foo(a[, b], c)
// 接收2~3个参数，b是可选参数，如果只传2个参数，b默认为null：
function foo(a, b, c) {
    if (arguments.length === 2) {
        // 实际拿到的参数是a和b，c为undefined
        c = b; // 把b赋给c
        b = null; // b变为默认值
    }
    // ...
}
```
要把中间的参数<font color="red"><code>b</code></font>变为“可选”参数，就只能通过<font color="red"><code>arguments</code></font>判断，然后重新调整参数并赋值。

### rest参数
由于JavaScript函数允许接收任意个参数，于是我们就不得不用<font color="red"><code>arguments</code></font>来获取所有参数：

```javascript
function foo(a, b) {
    var i, rest = [];
    if (arguments.length > 2) {
        for (i = 2; i<arguments.length; i++) {
            rest.push(arguments[i]);
        }
    }
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}
```

为了获取除了已定义参数<font color="red"><code>a</code></font>、<font color="red"><code>b</code></font>之外的参数，我们不得不用<font color="red"><code>arguments</code></font>，并且循环要从索引<font color="red"><code>2</code></font>开始以便排除前两个参数，这种写法很别扭，只是为了获得额外的<font color="red"><code>rest</code></font>参数，有没有更好的方法？

ES6标准引入了<font color="red"><code>rest</code></font>参数，上面的函数可以改写为：

```javascript
function foo(a, b, ...rest) {
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}

foo(1, 2, 3, 4, 5);
// 结果:
// a = 1
// b = 2
// Array [ 3, 4, 5 ]

foo(1);
// 结果:
// a = 1
// b = undefined
// Array []
```

<font color="red"><code>rest</code></font>参数只能写在最后，前面用<font color="red"><code>...</code></font>标识，从运行结果可知，传入的参数先绑定a、b，多余的参数以数组形式交给变量<font color="red"><code>rest</code></font>，所以，不再需要<font color="red"><code>arguments</code></font>我们就获取了全部参数。

如果传入的参数连正常定义的参数都没填满，也不要紧，<font color="red"><code>rest</code></font>参数会接收一个空数组（注意不是<font color="red"><code>undefined</code></font>）。

因为<font color="red"><code>rest</code></font>参数是ES6新标准，所以你需要测试一下浏览器是否支持。请用<font color="red"><code>rest</code></font>参数编写一个<font color="red"><code>sum()</code></font>函数，接收任意个参数并返回它们的和：

```javascript
'use strict';

function sum(...rest) {
   ???
}
```

```javascript
// 测试:
var i, args = [];
for (i=1; i<=100; i++) {
    args.push(i);
}
if (sum() !== 0) {
    console.log('测试失败: sum() = ' + sum());
} else if (sum(1) !== 1) {
    console.log('测试失败: sum(1) = ' + sum(1));
} else if (sum(2, 3) !== 5) {
    console.log('测试失败: sum(2, 3) = ' + sum(2, 3));
} else if (sum.apply(null, args) !== 5050) {
    console.log('测试失败: sum(1, 2, 3, ..., 100) = ' + sum.apply(null, args));
} else {
    console.log('测试通过!');
}
```

<button class="run" onclick="(() => {
    const answer = `
'use strict';
function sum(...rest) {
    let result = 0;
    if (Array.isArray(rest)) {
        rest.forEach((item) => {
            result += item;
        });
    }
    return result;
}
`;
    alert(answer);
})();">Show Answer</button>
### 

### 小心你的return语句
前面我们讲到了JavaScript引擎有一个在行末自动添加分号的机制，这可能让你栽到<font color="red"><code>return</code></font>语句的一个大坑：

```javascript
function foo() {
    return { name: 'foo' };
}

foo(); // { name: 'foo' }
```

如果把return语句拆成两行：

```javascript
function foo() {
    return
        { name: 'foo' };
}

foo(); // undefined
```

<font color="red"><code>要小心了</code></font>，由于JavaScript引擎在行末自动添加分号的机制，上面的代码实际上变成了：

```javascript
function foo() {
    return; // 自动添加了分号，相当于return undefined;
        { name: 'foo' }; // 这行语句已经没法执行到了
}
```

所以正确的多行写法是：

```javascript
function foo() {
    return { // 这里不会自动加分号，因为{表示语句尚未结束
        name: 'foo'
    };
}
```

### 练习
定义一个计算圆面积的函数<font color="red"><code>area_of_circle()</code></font>，它有两个参数：

* r: 表示圆的半径；
* pi: 表示π的值，如果不传，则默认3.14

```javascript
'use strict';

function area_of_circle(r, pi) {
    return 0;
}
```

```javascript
// 测试:
if (area_of_circle(2) === 12.56 && area_of_circle(2, 3.1416) === 12.5664) {
    console.log('测试通过');
} else {
    console.log('测试失败');
}
```

<button class="run" onclick="(() => {
    const answer = `
'use strict';
function area_of_circle(r, pi) {
    pi = pi ? pi : 3.14;
    return pi * r * r;
}
`;
    alert(answer);
})();">Show Answer</button>
### 

小明是一个JavaScript新手，他写了一个<font color="red"><code>max()</code></font>函数，返回两个数中较大的那个：

```javascript
'use strict';

function max(a, b) {
    if (a > b) {
        return
                a;
    } else {
        return
                b;
    }
}

console.log(max(15, 20));
```

<button class="run" onclick="(() => {
    const element = document.getElementById('max');
    try {
        'use strict';
        function max(a, b) {
            if (a > b) {
                return
                        a;
            } else {
                return
                        b;
            }
        }
        console.log(max(15, 20));
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>${max(15, 20)}</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button><button class="run" onclick="(() => {
    const answer = `
'use strict';
function max(a, b) {
    if (a > b) {
        return a;
    } else {
        return b;
    }
}
`;
    alert(answer);
})();">Fix</button>
<p id="max" hidden></p>

但是小明抱怨他的浏览器出问题了，无论传入什么数，<font color="red"><code>max()</code></font>函数总是返回<font color="red"><code>undefined</code></font>。请帮他指出问题并修复。
