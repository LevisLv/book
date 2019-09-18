<link rel="stylesheet" href="../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../static/css/console.css"/>

# 箭头函数
---

ES6标准新增了一种新的函数：Arrow Function（箭头函数）。

为什么叫Arrow Function？因为它的定义用的就是一个箭头：

```javascript
x => x * x
```

上面的箭头函数相当于：

```javascript
function (x) {
    return x * x;
}
```

在继续学习箭头函数之前，请测试你的浏览器是否支持ES6的Arrow Function：

```javascript
'use strict';
var fn = x => x * x;

console.log('你的浏览器支持ES6的Arrow Function!');
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#arrowFunction');
    try {
        'use strict';
        var fn = x => x * x;
        console.log('你的浏览器支持ES6的Arrow Function!');
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>你的浏览器支持ES6的Arrow Function!</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="arrowFunction" hidden></p>

箭头函数相当于匿名函数，并且简化了函数定义。箭头函数有两种格式，一种像上面的，只包含一个表达式，连<font color="red"><code>{ ... }</code></font>和<font color="red"><code>return</code></font>都省略掉了。还有一种可以包含多条语句，这时候就不能省略<font color="red"><code>{ ... }</code></font>和<font color="red"><code>return</code></font>：

```javascript
x => {
    if (x > 0) {
        return x * x;
    }
    else {
        return - x * x;
    }
}
```

如果参数不是一个，就需要用括号<font color="red"><code>()</code></font>括起来：

```javascript
// 两个参数:
(x, y) => x * x + y * y

// 无参数:
() => 3.14

// 可变参数:
(x, y, ...rest) => {
    var i, sum = x + y;
    for (i=0; i<rest.length; i++) {
        sum += rest[i];
    }
    return sum;
}
```

如果要返回一个对象，就要注意，如果是单表达式，这么写的话会报错：

```javascript
// SyntaxError:
x => { foo: x }
```

因为和函数体的<font color="red"><code>{ ... }</code></font>有语法冲突，所以要改为：

```javascript
// ok:
x => ({ foo: x })
```

### this
箭头函数看上去是匿名函数的一种简写，但实际上，箭头函数和匿名函数有个明显的区别：箭头函数内部的<font color="red"><code>this</code></font>是词法作用域，由上下文确定。

回顾前面的例子，由于JavaScript函数对<font color="red"><code>this</code></font>绑定的错误处理，下面的例子无法得到预期结果：

```javascript
var obj = {
    birth: 1990,
    getAge: function () {
        var b = this.birth; // 1990
        var fn = function () {
            return new Date().getFullYear() - this.birth; // this指向window或undefined
        };
        return fn();
    }
};
```

现在，箭头函数完全修复了<font color="red"><code>this</code></font>的指向，<font color="red"><code>this</code></font>总是指向词法作用域，也就是外层调用者<font color="red"><code>obj</code></font>：

```javascript
var obj = {
    birth: 1990,
    getAge: function () {
        var b = this.birth; // 1990
        var fn = () => new Date().getFullYear() - this.birth; // this指向obj对象
        return fn();
    }
};
obj.getAge(); // 25
```

如果使用箭头函数，以前的那种hack写法：

```javascript
var that = this;
```

就不再需要了。

由于<font color="red"><code>this</code></font>在箭头函数中已经按照词法作用域绑定了，所以，用<font color="red"><code>call()</code></font>或者<font color="red"><code>apply()</code></font>调用箭头函数时，无法对<font color="red"><code>this</code></font>进行绑定，即传入的第一个参数被忽略：

```javascript
var obj = {
    birth: 1990,
    getAge: function (year) {
        var b = this.birth; // 1990
        var fn = (y) => y - this.birth; // this.birth仍是1990
        return fn.call({birth:2000}, year);
    }
};
obj.getAge(2015); // 25
```

### 练习
请使用箭头函数简化排序时传入的函数：

```javascript
'use strict'
var arr = [10, 20, 1, 2];
arr.sort((x, y) => {
    ???
});
console.log(arr); // [1, 2, 10, 20]
```

<button class="run" onclick="(() => {
    const answer = `
'use strict'
var arr = [10, 20, 1, 2];
arr.sort((x, y) => {
    return x - y;
});
console.log(arr); // [1, 2, 10, 20]
`;
    alert(answer);
})();">Show Answer</button>
### 
