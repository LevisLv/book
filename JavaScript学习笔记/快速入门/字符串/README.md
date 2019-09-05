<link rel="stylesheet" href="../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../static/css/console.css"/>

# 字符串
---

JavaScript的字符串就是用<font color="red"><code>''</code></font>或<font color="red"><code>""</code></font>括起来的字符表示。

如果'本身也是一个字符，那就可以用<font color="red"><code>""</code></font>括起来，比如<font color="red"><code>"I'm OK"</code></font>包含的字符是<font color="red"><code>I</code></font>，<font color="red"><code>'</code></font>，<font color="red"><code>m</code></font>，空格，<font color="red"><code>O</code></font>，<font color="red"><code>K</code></font>这6个字符。

如果字符串内部既包含'又包含"怎么办？可以用转义字符\来标识，比如：

```javascript
'I\'m \"OK\"!';
```

表示的字符串内容是：<font color="red"><code>I'm "OK"!</code></font>

转义字符\可以转义很多字符，比如<font color="red"><code>\n</code></font>表示换行，<font color="red"><code>\t</code></font>表示制表符，字符<font color="red"><code>\</code></font>本身也要转义，所以<font color="red"><code>\\</code></font>表示的字符就是<font color="red"><code>\</code></font>。

ASCII字符可以以\x##形式的十六进制表示，例如：

```javascript
'\x41'; // 完全等同于 'A'
```

还可以用<font color="red"><code>\u####</code></font>表示一个Unicode字符：

```javascript
'\u4e2d\u6587'; // 完全等同于 '中文'
```

### 多行字符串
由于多行字符串用<font color="red"><code>\n</code></font>写起来比较费事，所以最新的ES6标准新增了一种多行字符串的表示方法，用反引号 <font color="red"><code>* ... *</code></font> 表示：

```
`这是一个
多行
字符串`;
```

<font color="red"><code>注意</code></font>：反引号在键盘的ESC下方，数字键1的左边：

```
┌─────┐ ┌─────┬─────┬─────┬─────┐
│ ESC │ │ F1  │ F2  │ F3  │ F4  │
└─────┘ └─────┴─────┴─────┴─────┘
┌─────┬─────┬─────┬─────┬─────┐
│  ~  │  !  │  @  │  #  │  $  │
│  `  │  1  │  2  │  3  │  4  │
├─────┴──┬──┴──┬──┴──┬──┴──┬──┘
│  tab   │  Q  │  W  │  E  │
├────────┴──┬──┴──┬──┴──┬──┘
│ caps lock │  A  │  S  │
└───────────┴─────┴─────┘
```

练习：测试你的浏览器是否支持ES6标准，如果不支持，请把多行字符串用<font color="red"><code>\n</code></font>重新表示出来：

```
// 如果浏览器不支持ES6，将报SyntaxError错误:
console.log(`多行
字符串
测试`);
```

<button class="run" onclick="(() => {
    const element = document.getElementById('multiLineString');
    try {
        // 如果浏览器不支持ES6，将报SyntaxError错误:
        console.log(`多行\n字符串\n测试`);
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>多行<br>字符串<br>测试</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="multiLineString" hidden></p>

### 模板字符串
要把多个字符串连接起来，可以用<font color="red"><code>+</code></font>号连接：

```javascript
var name = '小明';
var age = 20;
var message = '你好, ' + name + ', 你今年' + age + '岁了!';
alert(message);
```

如果有很多变量需要连接，用<font color="red"><code>+</code></font>号就比较麻烦。ES6新增了一种模板字符串，表示方法和上面的多行字符串一样，但是它会自动替换字符串中的变量：

```javascript
var name = '小明';
var age = 20;
var message = `你好, ${name}, 你今年${age}岁了!`;
alert(message);
```

练习：测试你的浏览器是否支持ES6模板字符串，如果不支持，请把模板字符串改为<font color="red"><code>+</code></font>连接的普通字符串：

```javascript
'use strict';

// 如果浏览器支持模板字符串，将会替换字符串内部的变量:
var name = '小明';
var age = 20;
console.log(`你好, ${name}, 你今年${age}岁了!`);
```

<button class="run" onclick="(() => {
    const element = document.getElementById('templateString');
    try {
        'use strict';
        // 如果浏览器支持模板字符串，将会替换字符串内部的变量:
        var name = '小明';
        var age = 20;
        console.log(`你好, ${name}, 你今年${age}岁了!`);
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>` + `你好, ${name}, 你今年${age}岁了!` + `</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="templateString" hidden></p>

### 操作字符串
字符串常见的操作如下：

```javascript
var s = 'Hello, world!';
s.length; // 13
```

要获取字符串某个指定位置的字符，使用类似Array的下标操作，索引号从0开始：

```javascript
var s = 'Hello, world!';

s[0]; // 'H'
s[6]; // ' '
s[7]; // 'w'
s[12]; // '!'
s[13]; // undefined 超出范围的索引不会报错，但一律返回undefined
```

<font color="red"><code>需要特别注意的是</code></font>，字符串是不可变的，如果对字符串的某个索引赋值，不会有任何错误，但是，也没有任何效果：

```javascript
var s = 'Test';
s[0] = 'X';
alert(s); // s仍然为'Test'
```

JavaScript为字符串提供了一些常用方法，注意，调用这些方法本身不会改变原有字符串的内容，而是返回一个新字符串：

### toUpperCase
<font color="red"><code>toUpperCase()</code></font>把一个字符串全部变为大写：

```javascript
var s = 'Hello';
s.toUpperCase(); // 返回'HELLO'
```

### toLowerCase
<font color="red"><code>toLowerCase()</code></font>把一个字符串全部变为小写：

```javascript
var s = 'Hello';
var lower = s.toLowerCase(); // 返回'hello'并赋值给变量lower
lower; // 'hello'
```

### indexOf
<font color="red"><code>indexOf()</code></font>会搜索指定字符串出现的位置：

```javascript
var s = 'hello, world';
s.indexOf('world'); // 返回7
s.indexOf('World'); // 没有找到指定的子串，返回-1
```

### substring
<font color="red"><code>substring()</code></font>返回指定索引区间的子串：

```javascript
var s = 'hello, world'
s.substring(0, 5); // 从索引0开始到5（不包括5），返回'hello'
s.substring(7); // 从索引7开始到结束，返回'world'
```
