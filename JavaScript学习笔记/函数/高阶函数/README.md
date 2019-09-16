<link rel="stylesheet" href="../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../static/css/console.css"/>

# 高阶函数
---

高阶函数英文叫Higher-order function。那么什么是高阶函数？

JavaScript的函数其实都指向某个变量。既然变量可以指向函数，函数的参数能接收变量，那么一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数。

一个最简单的高阶函数：

```javascript
function add(x, y, f) {
    return f(x) + f(y);
}
```

当我们调用<font color="red"><code>add(-5, 6, Math.abs)</code></font>时，参数<font color="red"><code>x</code></font>，<font color="red"><code>y</code></font>和f分别接收<font color="red"><code>-5</code></font>，<font color="red"><code>6</code></font>和函数<font color="red"><code>Math.abs</code></font>，根据函数定义，我们可以推导计算过程为：

```javascript
x = -5;
y = 6;
f = Math.abs;
f(x) + f(y) ==> Math.abs(-5) + Math.abs(6) ==> 11;
return 11;
```

用代码验证一下：

```javascript
'use strict';

function add(x, y, f) {
    return f(x) + f(y);
}
var x = add(-5, 6, Math.abs); // 11
console.log(x);
```

<button class="run" onclick="(() => {
    const element = document.getElementById('higherOrderFunction');
    try {
        'use strict';
        function add(x, y, f) {
            return f(x) + f(y);
        }
        var x = add(-5, 6, Math.abs); // 11
        console.log(x);
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>${x}</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="higherOrderFunction" hidden></p>

编写高阶函数，就是让函数的参数能够接收别的函数。
