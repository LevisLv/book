<link rel="stylesheet" href="../../../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../../../static/css/console.css"/>

# map/reduce
---

如果你读过Google的那篇大名鼎鼎的论文“[MapReduce: Simplified Data Processing on Large Clusters](http://research.google.com/archive/mapreduce.html)”，你就能大概明白map/reduce的概念。

### map
举例说明，比如我们有一个函数f(x)=x2，要把这个函数作用在一个数组<font color="red"><code>[1, 2, 3, 4, 5, 6, 7, 8, 9]</code></font>上，就可以用<font color="red"><code>map</code></font>实现如下：

![](https://www.liaoxuefeng.com/files/attachments/925425803658112/0)

由于<font color="red"><code>map()</code></font>方法定义在JavaScript的<font color="red"><code>Array</code></font>中，我们调用<font color="red"><code>Array</code></font>的<font color="red"><code>map()</code></font>方法，传入我们自己的函数，就得到了一个新的<font color="red"><code>Array</code></font>作为结果：

```javascript
'use strict';

function pow(x) {
    return x * x;
}
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var results = arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81]
console.log(results);
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#map');
    try {
        'use strict';
        function pow(x) {
            return x * x;
        }
        var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
        var results = arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81]
        console.log(results);
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>${results}</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="map" hidden></p>

注意：<font color="red"><code>map()</code></font>传入的参数是<font color="red"><code>pow</code></font>，即函数对象本身。

你可能会想，不需要<font color="red"><code>map()</code></font>，写一个循环，也可以计算出结果：

```javascript
var f = function (x) {
    return x * x;
};

var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var result = [];
for (var i=0; i<arr.length; i++) {
    result.push(f(arr[i]));
}
```

的确可以，但是，从上面的循环代码，我们无法一眼看明白“把f(x)作用在Array的每一个元素并把结果生成一个新的Array”。

所以，<font color="red"><code>map()</code></font>作为高阶函数，事实上它把运算规则抽象了，因此，我们不但可以计算简单的f(x)=x2，还可以计算任意复杂的函数，比如，把<font color="red"><code>Array</code></font>的所有数字转为字符串：

```javascript
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
arr.map(String); // ['1', '2', '3', '4', '5', '6', '7', '8', '9']
```

只需要一行代码。

### reduce
再看<font color="red"><code>reduce</code></font>的用法。<font color="red"><code>Array</code></font>的<font color="red"><code>reduce()</code></font>把一个函数作用在这个<font color="red"><code>Array</code></font>的<font color="red"><code>[x1, x2, x3...]</code></font>上，这个函数必须接收两个参数，<font color="red"><code>reduce()</code></font>把结果继续和序列的下一个元素做累积计算，其效果就是：

```javascript
[x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4)
```

比方说对一个<font color="red"><code>Array</code></font>求和，就可以用<font color="red"><code>reduce</code></font>实现：

```javascript
var arr = [1, 3, 5, 7, 9];
arr.reduce(function (x, y) {
    return x + y;
}); // 25
```

练习：利用<font color="red"><code>reduce()</code></font>求积：

```javascript
'use strict';

function product(arr) {
    return 0;

}

// 测试:
if (product([1, 2, 3, 4]) === 24 && product([0, 1, 2]) === 0 && product([99, 88, 77, 66]) === 44274384) {
    console.log('测试通过!');
}
else {
    console.log('测试失败!');
}
```

<button class="run" onclick="(() => {
    const answer = `
'use strict';
function product(arr) {
    return arr.reduce((x, y) => {
        return x * y;
    });
}
`;
    alert(answer);
})();">Show Answer</button>
### 

要把<font color="red"><code>[1, 3, 5, 7, 9]</code></font>变换成整数<font color="red"><code>13579</code></font>，<font color="red"><code>reduce()</code></font>也能派上用场：

```javascript
var arr = [1, 3, 5, 7, 9];
arr.reduce(function (x, y) {
    return x * 10 + y;
}); // 13579
```

如果我们继续改进这个例子，想办法把一个字符串<font color="red"><code>13579</code></font>先变成<font color="red"><code>Array</code></font>——<font color="red"><code>[1, 3, 5, 7, 9]</code></font>，再利用<font color="red"><code>reduce()</code></font>就可以写出一个把字符串转换为Number的函数。

练习：不要使用JavaScript内置的<font color="red"><code>parseInt()</code></font>函数，利用<font color="red"><code>map</code></font>和<font color="red"><code>reduce</code></font>操作实现一个<font color="red"><code>string2int()</code></font>函数：

```javascript
'use strict';

function string2int(s) {
    return 0;

}

// 测试:
if (string2int('0') === 0 && string2int('12345') === 12345 && string2int('12300') === 12300) {
    if (string2int.toString().indexOf('parseInt') !== -1) {
        console.log('请勿使用parseInt()!');
    } else if (string2int.toString().indexOf('Number') !== -1) {
        console.log('请勿使用Number()!');
    } else {
        console.log('测试通过!');
    }
}
else {
    console.log('测试失败!');
}
```

<button class="run" onclick="(() => {
    const answer = `
'use strict';
function string2int(s) {
    return s.length > 1
        ? s.split('').reduce((x, y) => {
            return x * 10 + y * 1;
        })
        : s * 1;
}
`;
    alert(answer);
})();">Show Answer</button>
### 

### 练习
请把用户输入的不规范的英文名字，变为首字母大写，其他小写的规范名字。输入：<font color="red"><code>['adam', 'LISA', 'barT']</code></font>，输出：<font color="red"><code>['Adam', 'Lisa', 'Bart']</code></font>。

```javascript
'use strict';

function normalize(arr) {
    return [];

}

// 测试:
if (normalize(['adam', 'LISA', 'barT']).toString() === ['Adam', 'Lisa', 'Bart'].toString()) {
    console.log('测试通过!');
}
else {
    console.log('测试失败!');
}
```

<button class="run" onclick="(() => {
    const answer = `
'use strict';
function normalize(arr) {
    return arr.map(x => {
        return x.split('').reduce((y, z) => {
            return \`\${y.charAt(0).toUpperCase()}\${y.substring(1)}\${z.toLowerCase()}\`;
        });
    });
}
`;
    alert(answer);
})();">Show Answer</button>
### 

小明希望利用<font color="red"><code>map()</code></font>把字符串变成整数，他写的代码很简洁：

```javascript
'use strict';

var arr = ['1', '2', '3'];
var r;
r = arr.map(parseInt);

console.log(r);
```

<button class="run" onclick="(() => {
    const element = document.getElementById('parseInt');
    try {
        'use strict';
        var arr = ['1', '2', '3'];
        var r;
        r = arr.map(parseInt);
        console.log(r);
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>${r}</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="parseInt" hidden></p>

结果竟然是<font color="red"><code>1, NaN, NaN</code></font>，小明百思不得其解，请帮他找到原因并修正代码。

提示：参考[Array.prototype.map()的文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)。

<button class="analyze" onclick="(() => {
    this.setAttribute('hidden', 'hidden');
    document.querySelector('dev#analyze').removeAttribute('hidden');
})();">原因分析</button>
### 

<dev id="analyze" hidden="hidden">
由于<code style="color: red">map()</code>接收的回调函数可以有3个参数：<code style="color: red">callback(currentValue, index, array)</code>，通常我们仅需要第一个参数，而忽略了传入的后面两个参数。不幸的是，<code style="color: red">parseInt(string, radix)</code>没有忽略第二个参数，导致实际执行的函数分别是：
<ul>
    <li>parseInt('1', 0); // 1, 按十进制转换</li>
    <li>parseInt('2', 1); // NaN, 没有一进制</li>
    <li>parseInt('3', 2); // NaN, 按二进制转换不允许出现3</li>
</ul>
可以改为<font color="red"><code>r = arr.map(Number);</code></font>，因为<font color="red"><code>Number(value)</code></font>函数仅接收一个参数。
</dev>
