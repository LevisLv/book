<link rel="stylesheet" href="../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../static/css/console.css"/>

# 变量作用域与解构赋值
---

在JavaScript中，用<font color="red"><code>var</code></font>申明的变量实际上是有作用域的。

如果一个变量在函数体内部申明，则该变量的作用域为整个函数体，在函数体外不可引用该变量：

```javascript
'use strict';

function foo() {
    var x = 1;
    x = x + 1;
}

x = x + 2; // ReferenceError! 无法在函数体外引用变量x
```

如果两个不同的函数各自申明了同一个变量，那么该变量只在各自的函数体内起作用。换句话说，不同函数内部的同名变量互相独立，互不影响：

```javascript
'use strict';

function foo() {
    var x = 1;
    x = x + 1;
}

function bar() {
    var x = 'A';
    x = x + 'B';
}
```

由于JavaScript的函数可以嵌套，此时，内部函数可以访问外部函数定义的变量，反过来则不行：

```javascript
'use strict';

function foo() {
    var x = 1;
    function bar() {
        var y = x + 1; // bar可以访问foo的变量x!
    }
    var z = y + 1; // ReferenceError! foo不可以访问bar的变量y!
}
```

如果内部函数和外部函数的变量名重名怎么办？来测试一下：

```javascript
'use strict';
function foo() {
    var x = 1;
    function bar() {
        var x = 'A';
        console.log('x in bar() = ' + x); // 'A'
    }
    console.log('x in foo() = ' + x); // 1
    bar();
}

foo();
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#var');
    try {
        'use strict';
        function foo() {
            var x = 1;
            function bar() {
                var x = 'A';
                console.log('x in bar() = ' + x); // 'A'
            }
            console.log('x in foo() = ' + x); // 1
            bar();
        }
        foo();
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>x in foo() = 1<br>x in bar() = A</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="var" hidden></p>

这说明JavaScript的函数在查找变量时从自身函数定义开始，从“内”向“外”查找。如果内部函数定义了与外部函数重名的变量，则内部函数的变量将“屏蔽”外部函数的变量。

### 变量提升
JavaScript的函数定义有个特点，它会先扫描整个函数体的语句，把所有申明的变量“提升”到函数顶部：

```javascript
'use strict';

function foo() {
    var x = 'Hello, ' + y;
    console.log(x);
    var y = 'Bob';
}

foo();
```

虽然是strict模式，但语句<font color="red"><code>var x = 'Hello, ' + y;</code></font>并不报错，原因是变量<font color="red"><code>y</code></font>在稍后申明了。但是<font color="red"><code>console.log</code></font>显示<font color="red"><code>Hello, undefined</code></font>，说明变量<font color="red"><code>y</code></font>的值为<font color="red"><code>undefined</code></font>。这正是因为JavaScript引擎自动提升了变量<font color="red"><code>y</code></font>的声明，但不会提升变量<font color="red"><code>y</code></font>的赋值。

对于上述<font color="red"><code>foo()</code></font>函数，JavaScript引擎看到的代码相当于：

```javascript
function foo() {
    var y; // 提升变量y的申明，此时y为undefined
    var x = 'Hello, ' + y;
    console.log(x);
    y = 'Bob';
}
```

由于JavaScript的这一怪异的“特性”，我们在函数内部定义变量时，请严格遵守“在函数内部首先申明所有变量”这一规则。最常见的做法是用一个<font color="red"><code>var</code></font>申明函数内部用到的所有变量：

```javascript
function foo() {
    var
        x = 1, // x初始化为1
        y = x + 1, // y初始化为2
        z, i; // z和i为undefined
    // 其他语句:
    for (i=0; i<100; i++) {
        ...
    }
}
```

### 全局作用域
不在任何函数内定义的变量就具有全局作用域。实际上，JavaScript默认有一个全局对象<font color="red"><code>window</code></font>，全局作用域的变量实际上被绑定到<font color="red"><code>window</code></font>的一个属性：

```javascript
'use strict';

var course = 'Learn JavaScript';
alert(course); // 'Learn JavaScript'
alert(window.course); // 'Learn JavaScript'
```

因此，直接访问全局变量<font color="red"><code>course</code></font>和访问<font color="red"><code>window.course</code></font>是完全一样的。

你可能猜到了，由于函数定义有两种方式，以变量方式<font color="red"><code>var foo = function () {}</code></font>定义的函数实际上也是一个全局变量，因此，顶层函数的定义也被视为一个全局变量，并绑定到<font color="red"><code>window</code></font>对象：

```javascript
'use strict';

function foo() {
    alert('foo');
}

foo(); // 直接调用foo()
window.foo(); // 通过window.foo()调用
```

进一步大胆地猜测，我们每次直接调用的<font color="red"><code>alert()</code></font>函数其实也是<font color="red"><code>window</code></font>的一个变量：

```javascript
'use strict';

window.alert('调用window.alert()');
// 把alert保存到另一个变量:
var old_alert = window.alert;
// 给alert赋一个新函数:
window.alert = function () {}
alert('无法用alert()显示了!');

// 恢复alert:
window.alert = old_alert;
alert('又可以用alert()了!');
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#alert');
    try {
        'use strict';
        window.alert('调用window.alert()');
        // 把alert保存到另一个变量:
        var old_alert = window.alert;
        // 给alert赋一个新函数:
        window.alert = function () {}
        alert('无法用alert()显示了!');
        // 恢复alert:
        window.alert = old_alert;
        alert('又可以用alert()了!');
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>(no output)</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="alert" hidden></p>

这说明JavaScript实际上只有一个全局作用域。任何变量（函数也视为变量），如果没有在当前函数作用域中找到，就会继续往上查找，最后如果在全局作用域中也没有找到，则报<font color="red"><code>ReferenceError</code></font>错误。

### 名字空间
全局变量会绑定到<font color="red"><code>window</code></font>上，不同的JavaScript文件如果使用了相同的全局变量，或者定义了相同名字的顶层函数，都会造成命名冲突，并且很难被发现。

减少冲突的一个方法是把自己的所有变量和函数全部绑定到一个全局变量中。例如：

```javascript
// 唯一的全局变量MYAPP:
var MYAPP = {};

// 其他变量:
MYAPP.name = 'myapp';
MYAPP.version = 1.0;

// 其他函数:
MYAPP.foo = function () {
    return 'foo';
};
```

把自己的代码全部放入唯一的名字空间<font color="red"><code>MYAPP</code></font>中，会大大减少全局变量冲突的可能。

许多著名的JavaScript库都是这么干的：jQuery，YUI，underscore等等。

### 局部作用域
由于JavaScript的变量作用域实际上是函数内部，我们在<font color="red"><code>for</code></font>循环等语句块中是无法定义具有局部作用域的变量的：

```javascript
'use strict';

function foo() {
    for (var i=0; i<100; i++) {
        //
    }
    i += 100; // 仍然可以引用变量i
}
```

为了解决块级作用域，ES6引入了新的关键字<font color="red"><code>let</code></font>，用<font color="red"><code>let</code></font>替代<font color="red"><code>var</code></font>可以申明一个块级作用域的变量：

```javascript
'use strict';

function foo() {
    var sum = 0;
    for (let i=0; i<100; i++) {
        sum += i;
    }
    // SyntaxError:
    i += 1;
}
```

### 常量
由于<font color="red"><code>var</code></font>和<font color="red"><code>let</code></font>申明的是变量，如果要申明一个常量，在ES6之前是不行的，我们通常用全部大写的变量来表示“这是一个常量，不要修改它的值”：

```javascript
var PI = 3.14;
```

ES6标准引入了新的关键字<font color="red"><code>const</code></font>来定义常量，<font color="red"><code>const</code></font>与<font color="red"><code>let</code></font>都具有块级作用域：

```javascript
'use strict';

const PI = 3.14;
PI = 3; // 某些浏览器不报错，但是无效果！
PI; // 3.14
```

### 解构赋值
从ES6开始，JavaScript引入了解构赋值，可以同时对一组变量进行赋值。

什么是解构赋值？我们先看看传统的做法，如何把一个数组的元素分别赋值给几个变量：

```javascript
var array = ['hello', 'JavaScript', 'ES6'];
var x = array[0];
var y = array[1];
var z = array[2];
```

现在，在ES6中，可以使用解构赋值，直接对多个变量同时赋值：

```javascript
'use strict';

// 如果浏览器支持解构赋值就不会报错:
var [x, y, z] = ['hello', 'JavaScript', 'ES6'];
// x, y, z分别被赋值为数组对应元素:
console.log('x = ' + x + ', y = ' + y + ', z = ' + z);
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#destructuringAssignment');
    try {
        'use strict';
        // 如果浏览器支持解构赋值就不会报错:
        var [x, y, z] = ['hello', 'JavaScript', 'ES6'];
        // x, y, z分别被赋值为数组对应元素:
        console.log('x = ' + x + ', y = ' + y + ', z = ' + z);
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>x = hello, y = JavaScript, z = ES6</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="destructuringAssignment" hidden></p>

注意，对数组元素进行解构赋值时，多个变量要用<font color="red"><code>[...]</code></font>括起来。

如果数组本身还有嵌套，也可以通过下面的形式进行解构赋值，注意嵌套层次和位置要保持一致：

```javascript
let [x, [y, z]] = ['hello', ['JavaScript', 'ES6']];
x; // 'hello'
y; // 'JavaScript'
z; // 'ES6'
```

解构赋值还可以忽略某些元素：

```javascript
let [, , z] = ['hello', 'JavaScript', 'ES6']; // 忽略前两个元素，只对z赋值第三个元素
z; // 'ES6'
```

如果需要从一个对象中取出若干属性，也可以使用解构赋值，便于快速获取对象的指定属性：

```javascript
'use strict';

var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school'
};
var {name, age, passport} = person;
// name, age, passport分别被赋值为对应属性:
console.log('name = ' + name + ', age = ' + age + ', passport = ' + passport);
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#objectDestructuringAssignment');
    try {
        'use strict';
        var person = {
            name: '小明',
            age: 20,
            gender: 'male',
            passport: 'G-12345678',
            school: 'No.4 middle school'
        };
        var {name, age, passport} = person;
        // name, age, passport分别被赋值为对应属性:
        console.log('name = ' + name + ', age = ' + age + ', passport = ' + passport);
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>name = 小明, age = 20, passport = G-12345678</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="objectDestructuringAssignment" hidden></p>

对一个对象进行解构赋值时，同样可以直接对嵌套的对象属性进行赋值，只要保证对应的层次是一致的：

```javascript
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school',
    address: {
        city: 'Beijing',
        street: 'No.1 Road',
        zipcode: '100001'
    }
};
var {name, address: {city, zip}} = person;
name; // '小明'
city; // 'Beijing'
zip; // undefined, 因为属性名是zipcode而不是zip
// 注意: address不是变量，而是为了让city和zip获得嵌套的address对象的属性:
address; // Uncaught ReferenceError: address is not defined
```

使用解构赋值对对象属性进行赋值时，如果对应的属性不存在，变量将被赋值为<font color="red"><code>undefined</code></font>，这和引用一个不存在的属性获得<font color="red"><code>undefined</code></font>是一致的。如果要使用的变量名和属性名不一致，可以用下面的语法获取：

```javascript
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school'
};

// 把passport属性赋值给变量id:
let {name, passport:id} = person;
name; // '小明'
id; // 'G-12345678'
// 注意: passport不是变量，而是为了让变量id获得passport属性:
passport; // Uncaught ReferenceError: passport is not defined
```

解构赋值还可以使用默认值，这样就避免了不存在的属性返回<font color="red"><code>undefined</code></font>的问题：

```javascript
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678'
};

// 如果person对象没有single属性，默认赋值为true:
var {name, single=true} = person;
name; // '小明'
single; // true
```

有些时候，如果变量已经被声明了，再次赋值的时候，正确的写法也会报语法错误：

```javascript
// 声明变量:
var x, y;
// 解构赋值:
{x, y} = { name: '小明', x: 100, y: 200};
// 语法错误: Uncaught SyntaxError: Unexpected token =
```

这是因为JavaScript引擎把<font color="red"><code>{</code></font>开头的语句当作了块处理，于是<font color="red"><code>=</code></font>不再合法。解决方法是用小括号括起来：

```javascript
({x, y} = { name: '小明', x: 100, y: 200});
```

### 使用场景
解构赋值在很多时候可以大大简化代码。例如，交换两个变量<font color="red"><code>x</code></font>和<font color="red"><code>y</code></font>的值，可以这么写，不再需要临时变量：

```javascript
var x=1, y=2;
[x, y] = [y, x]
```

快速获取当前页面的域名和路径：

```javascript
var {hostname:domain, pathname:path} = location;
```

如果一个函数接收一个对象作为参数，那么，可以使用解构直接把对象的属性绑定到变量中。例如，下面的函数可以快速创建一个<font color="red"><code>Date</code></font>对象：

```javascript
function buildDate({year, month, day, hour=0, minute=0, second=0}) {
    return new Date(year + '-' + month + '-' + day + ' ' + hour + ':' + minute + ':' + second);
}
```

它的方便之处在于传入的对象只需要<font color="red"><code>year</code></font>、<font color="red"><code>month</code></font>和<font color="red"><code>day</code></font>这三个属性：

```javascript
buildDate({ year: 2017, month: 1, day: 1 });
// Sun Jan 01 2017 00:00:00 GMT+0800 (CST)
```

也可以传入<font color="red"><code>hour</code></font>、<font color="red"><code>minute</code></font>和<font color="red"><code>second</code></font>属性：

```javascript
buildDate({ year: 2017, month: 1, day: 1, hour: 20, minute: 15 });
// Sun Jan 01 2017 20:15:00 GMT+0800 (CST)
```

使用解构赋值可以减少代码量，但是，需要在支持ES6解构赋值特性的现代浏览器中才能正常运行。目前支持解构赋值的浏览器包括Chrome，Firefox，Edge等。
