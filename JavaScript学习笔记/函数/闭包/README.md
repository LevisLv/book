<link rel="stylesheet" href="../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../static/css/console.css"/>

# 闭包
---

### 函数作为返回值
高阶函数除了可以接受函数作为参数外，还可以把函数作为结果值返回。

我们来实现一个对<font color="red"><code>Array</code></font>的求和。通常情况下，求和的函数是这样定义的：

```javascript
function sum(arr) {
    return arr.reduce(function (x, y) {
        return x + y;
    });
}

sum([1, 2, 3, 4, 5]); // 15
```

但是，如果不需要立刻求和，而是在后面的代码中，根据需要再计算怎么办？可以不返回求和的结果，而是返回求和的函数！

```javascript
function lazy_sum(arr) {
    var sum = function () {
        return arr.reduce(function (x, y) {
            return x + y;
        });
    }
    return sum;
}
```

当我们调用<font color="red"><code>lazy_sum()</code></font>时，返回的并不是求和结果，而是求和函数：

```javascript
var f = lazy_sum([1, 2, 3, 4, 5]); // function sum()
```

调用函数<font color="red"><code>f</code></font>时，才真正计算求和的结果：

```javascript
f(); // 15
```

在这个例子中，我们在函数<font color="red"><code>lazy_sum</code></font>中又定义了函数<font color="red"><code>sum</code></font>，并且，内部函数<font color="red"><code>sum</code></font>可以引用外部函数<font color="red"><code>lazy_sum</code></font>的参数和局部变量，当<font color="red"><code>lazy_sum</code></font>返回函数<font color="red"><code>sum</code></font>时，相关参数和变量都保存在返回的函数中，这种称为“闭包（Closure）”的程序结构拥有极大的威力。

请再注意一点，当我们调用<font color="red"><code>lazy_sum()</code></font>时，每次调用都会返回一个新的函数，即使传入相同的参数：

```javascript
var f1 = lazy_sum([1, 2, 3, 4, 5]);
var f2 = lazy_sum([1, 2, 3, 4, 5]);
f1 === f2; // false
```

<font color="red"><code>f1()</code></font>和<font color="red"><code>f2()</code></font>的调用结果互不影响。

### 闭包
注意到返回的函数在其定义内部引用了局部变量<font color="red"><code>arr</code></font>，所以，当一个函数返回了一个函数后，其内部的局部变量还被新函数引用，所以，闭包用起来简单，实现起来可不容易。

另一个需要注意的问题是，返回的函数并没有立刻执行，而是直到调用了<font color="red"><code>f()</code></font>才执行。我们来看一个例子：

```javascript
function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push(function () {
            return i * i;
        });
    }
    return arr;
}

var results = count();
var f1 = results[0];
var f2 = results[1];
var f3 = results[2];
```

在上面的例子中，每次循环，都创建了一个新的函数，然后，把创建的3个函数都添加到一个<font color="red"><code>Array</code></font>中返回了。

你可能认为调用<font color="red"><code>f1()</code></font>，<font color="red"><code>f2()</code></font>和<font color="red"><code>f3()</code></font>结果应该是<font color="red"><code>1</code></font>，<font color="red"><code>4</code></font>，<font color="red"><code>9</code></font>，但实际结果是：

```javascript
f1(); // 16
f2(); // 16
f3(); // 16
```

全部都是<font color="red"><code>16</code></font>！原因就在于返回的函数引用了变量<font color="red"><code>i</code></font>，但它并非立刻执行。等到3个函数都返回时，它们所引用的变量<font color="red"><code>i</code></font>已经变成了<font color="red"><code>4</code></font>，因此最终结果为<font color="red"><code>16</code></font>。

返回闭包时牢记的一点就是：返回函数不要引用任何循环变量，或者后续会发生变化的变量。

如果一定要引用循环变量怎么办？方法是再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变：

```javascript
function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push((function (n) {
            return function () {
                return n * n;
            }
        })(i));
    }
    return arr;
}

var results = count();
var f1 = results[0];
var f2 = results[1];
var f3 = results[2];

f1(); // 1
f2(); // 4
f3(); // 9
```

注意这里用了一个“创建一个匿名函数并立刻执行”的语法：

```javascript
(function (x) {
    return x * x;
})(3); // 9
```

理论上讲，创建一个匿名函数并立刻执行可以这么写：

```javascript
function (x) { return x * x } (3);
```

但是由于JavaScript语法解析的问题，会报SyntaxError错误，因此需要用括号把整个函数定义括起来：

```javascript
(function (x) { return x * x }) (3);
```

通常，一个立即执行的匿名函数可以把函数体拆开，一般这么写：

```javascript
(function (x) {
    return x * x;
})(3);
```

说了这么多，难道闭包就是为了返回一个函数然后延迟执行吗？

当然不是！闭包有非常强大的功能。举个栗子：

在面向对象的程序设计语言里，比如Java和C++，要在对象内部封装一个私有变量，可以用<font color="red"><code>private</code></font>修饰一个成员变量。

在没有<font color="red"><code>class</code></font>机制，只有函数的语言里，借助闭包，同样可以封装一个私有变量。我们用JavaScript创建一个计数器：

```javascript
'use strict';

function create_counter(initial) {
    var x = initial || 0;
    return {
        inc: function () {
            x += 1;
            return x;
        }
    }
}
```

它用起来像这样：

```javascript
var c1 = create_counter();
c1.inc(); // 1
c1.inc(); // 2
c1.inc(); // 3

var c2 = create_counter(10);
c2.inc(); // 11
c2.inc(); // 12
c2.inc(); // 13
```

在返回的对象中，实现了一个闭包，该闭包携带了局部变量<font color="red"><code>x</code></font>，并且，从外部代码根本无法访问到变量<font color="red"><code>x</code></font>。换句话说，闭包就是携带状态的函数，并且它的状态可以完全对外隐藏起来。

闭包还可以把多参数的函数变成单参数的函数。例如，要计算 <font color="red">x<sup>y</sup></font> 可以用<font color="red"><code>Math.pow(x, y)</code></font>函数，不过考虑到经常计算 <font color="red">x<sup>2</sup></font> 或 <font color="red">x<sup>3</sup></font> ，我们可以利用闭包创建新的函数<font color="red"><code>pow2</code></font>和<font color="red"><code>pow3</code></font>：

```javascript
'use strict';

function make_pow(n) {
    return function (x) {
        return Math.pow(x, n);
    }
}
// 创建两个新函数:
var pow2 = make_pow(2);
var pow3 = make_pow(3);

console.log(pow2(5)); // 25
console.log(pow3(7)); // 343
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#make_pow');
    try {
        'use strict';
        function make_pow(n) {
            return function (x) {
                return Math.pow(x, n);
            }
        }
        // 创建两个新函数:
        var pow2 = make_pow(2);
        var pow3 = make_pow(3);
        console.log(pow2(5)); // 25
        console.log(pow3(7)); // 343
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>25<br>343</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="make_pow" hidden></p>

### 脑洞大开
很久很久以前，有个叫阿隆佐·邱奇的帅哥，发现只需要用函数，就可以用计算机实现运算，而不需要<font color="red"><code>0</code></font>、<font color="red"><code>1</code></font>、<font color="red"><code>2</code></font>、<font color="red"><code>3</code></font>这些数字和<font color="red"><code>+</code></font>、<font color="red"><code>-</code></font>、<font color="red"><code>*</code></font>、<font color="red"><code>/</code></font>这些符号。

JavaScript支持函数，所以可以用JavaScript用函数来写这些计算。来试试：

```javascript
'use strict';

// 定义数字0:
var zero = function (f) {
    return function (x) {
        return x;
    }
};

// 定义数字1:
var one = function (f) {
    return function (x) {
        return f(x);
    }
};

// 定义加法:
function add(n, m) {
    return function (f) {
        return function (x) {
            return m(f)(n(f)(x));
        }
    }
}
// 计算数字2 = 1 + 1:
var two = add(one, one);

// 计算数字3 = 1 + 2:
var three = add(one, two);

// 计算数字5 = 2 + 3:
var five = add(two, three);

// 你说它是3就是3，你说它是5就是5，你怎么证明？

// 呵呵，看这里:

// 给3传一个函数,会打印3次:
(three(function () {
    console.log('print 3 times');
}))();

// 给5传一个函数,会打印5次:
(five(function () {
    console.log('print 5 times');
}))();

// 继续接着玩一会...
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#function');
    try {
        'use strict';
        // 定义数字0:
        var zero = function (f) {
            return function (x) {
                return x;
            }
        };
        // 定义数字1:
        var one = function (f) {
            return function (x) {
                return f(x);
            }
        };
        // 定义加法:
        function add(n, m) {
            return function (f) {
                return function (x) {
                    return m(f)(n(f)(x));
                }
            }
        }
        // 计算数字2 = 1 + 1:
        var two = add(one, one);
        // 计算数字3 = 1 + 2:
        var three = add(one, two);
        // 计算数字5 = 2 + 3:
        var five = add(two, three);
        // 你说它是3就是3，你说它是5就是5，你怎么证明？
        // 呵呵，看这里:
        // 给3传一个函数,会打印3次:
        (three(function () {
            console.log('print 3 times');
        }))();
        // 给5传一个函数,会打印5次:
        (five(function () {
            console.log('print 5 times');
        }))();
        // 继续接着玩一会...
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>print 3 times<br>print 3 times<br>print 3 times<br>print 5 times<br>print 5 times<br>print 5 times<br>print 5 times<br>print 5 times</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="function" hidden></p>
