<link rel="stylesheet" href="../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../static/css/console.css"/>

# Iterable
---

遍历<font color="red"><code>Array</code></font>可以采用下标循环，遍历<font color="red"><code>Map</code></font>和<font color="red"><code>Set</code></font>就无法使用下标。为了统一集合类型，ES6标准引入了新的<font color="red"><code>iterable</code></font>类型，<font color="red"><code>Array</code></font>、<font color="red"><code>Map</code></font>和<font color="red"><code>Set</code></font>都属于<font color="red"><code>iterable</code></font>类型。

具有<font color="red"><code>iterable</code></font>类型的集合可以通过新的<font color="red"><code>for ... of</code></font>循环来遍历。

<font color="red"><code>for ... of</code></font>循环是ES6引入的新的语法，请测试你的浏览器是否支持：

```javascript
'use strict';
var a = [1, 2, 3];
for (var x of a) {
}
console.log('你的浏览器支持for ... of');
// 请直接运行测试
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#of');
    try {
        'use strict';
        var a = [1, 2, 3];
        for (var x of a) {
        }
        console.log('你的浏览器支持for ... of');
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>'你的浏览器支持for ... of'</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="of" hidden></p>

用<font color="red"><code>for ... of</code></font>循环遍历集合，用法如下：

```javascript
var a = ['A', 'B', 'C'];
var s = new Set(['A', 'B', 'C']);
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
for (var x of a) { // 遍历Array
    console.log(x);
}
for (var x of s) { // 遍历Set
    console.log(x);
}
for (var x of m) { // 遍历Map
    console.log(x[0] + '=' + x[1]);
}
```

你可能会有疑问，<font color="red"><code>for ... of</code></font>循环和<font color="red"><code>for ... in</code></font>循环有何区别？

<font color="red"><code>for ... in</code></font>循环由于历史遗留问题，它遍历的实际上是对象的属性名称。一个<font color="red"><code>Array</code></font>数组实际上也是一个对象，它的每个元素的索引被视为一个属性。

当我们手动给<font color="red"><code>Array</code></font>对象添加了额外的属性后，<font color="red"><code>for ... in</code></font>循环将带来意想不到的意外效果：

```javascript
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x in a) {
    console.log(x); // '0', '1', '2', 'name'
}
```

<font color="red"><code>for ... in</code></font>循环将把<font color="red"><code>name</code></font>包括在内，但<font color="red"><code>Array</code></font>的<font color="red"><code>length</code></font>属性却不包括在内。

<font color="red"><code>for ... of</code></font>循环则完全修复了这些问题，它只循环集合本身的元素：

```javascript
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x of a) {
    console.log(x); // 'A', 'B', 'C'
}
```

这就是为什么要引入新的<font color="red"><code>for ... of</code></font>循环。

然而，更好的方式是直接使用<font color="red"><code>iterable</code></font>内置的<font color="red"><code>forEach</code></font>方法，它接收一个函数，每次迭代就自动回调该函数。以<font color="red"><code>Array</code></font>为例：

```javascript
'use strict';
var a = ['A', 'B', 'C'];
a.forEach(function (element, index, array) {
    // element: 指向当前元素的值
    // index: 指向当前索引
    // array: 指向Array对象本身
    console.log(element + ', index = ' + index);
});
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#forEach');
    try {
        'use strict';
        var a = ['A', 'B', 'C'];
        a.forEach(function (element, index, array) {
            // element: 指向当前元素的值
            // index: 指向当前索引
            // array: 指向Array对象本身
            console.log(element + ', index = ' + index);
        });
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>A, index = 0<br>B, index = 1<br>C, index = 2</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="forEach" hidden></p>

<font color="red"><code>注意</code></font>，<font color="red"><code>forEach()</code></font>方法是ES5.1标准引入的，你需要测试浏览器是否支持。

<font color="red"><code>Set</code></font>与<font color="red"><code>Array</code></font>类似，但<font color="red"><code>Set</code></font>没有索引，因此回调函数的前两个参数都是元素本身：

```javascript
var s = new Set(['A', 'B', 'C']);
s.forEach(function (element, sameElement, set) {
    console.log(element);
});
```

<font color="red"><code>Map</code></font>的回调函数参数依次为<font color="red"><code>value</code></font>、<font color="red"><code>key</code></font>和<font color="red"><code>map</code></font>本身：

```javascript
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
m.forEach(function (value, key, map) {
    console.log(value);
});
```

如果对某些参数不感兴趣，由于JavaScript的函数调用不要求参数必须一致，因此可以忽略它们。例如，只需要获得<font color="red"><code>Array</code></font>的<font color="red"><code>element</code></font>：

```javascript
var a = ['A', 'B', 'C'];
a.forEach(function (element) {
    console.log(element);
});
```
