<link rel="stylesheet" href="../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../static/css/console.css"/>

# Map和Set
---

JavaScript的默认对象表示方式<font color="red"><code>{}</code></font>可以视为其他语言中的<font color="red"><code>Map</code></font>或Dictionary的数据结构，即一组键值对。

但是JavaScript的对象有个小问题，就是键必须是字符串。但实际上Number或者其他数据类型作为键也是非常合理的。

为了解决这个问题，最新的ES6规范引入了新的数据类型<font color="red"><code>Map</code></font>。要测试你的浏览器是否支持ES6规范，请执行以下代码，如果浏览器报ReferenceError错误，那么你需要换一个支持ES6的浏览器：

```javascript
'use strict';
var m = new Map();
var s = new Set();
console.log('你的浏览器支持Map和Set！');
// 直接运行测试
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#mapAndSet');
    try {
        'use strict';
        var m = new Map();
        var s = new Set();
        console.log('你的浏览器支持Map和Set！');
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>'你的浏览器支持Map和Set！'</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="mapAndSet" hidden></p>

### Map
<font color="red"><code>Map</code></font>是一组键值对的结构，具有极快的查找速度。

举个例子，假设要根据同学的名字查找对应的成绩，如果用<font color="red"><code>Array</code></font>实现，需要两个<font color="red"><code>Array</code></font>：

```javascript
var names = ['Michael', 'Bob', 'Tracy'];
var scores = [95, 75, 85];
```

给定一个名字，要查找对应的成绩，就先要在names中找到对应的位置，再从scores取出对应的成绩，<font color="red"><code>Array</code></font>越长，耗时越长。

如果用<font color="red"><code>Map</code></font>实现，只需要一个“名字”-“成绩”的对照表，直接根据名字查找成绩，无论这个表有多大，查找速度都不会变慢。用JavaScript写一个<font color="red"><code>Map</code></font>如下：

```javascript
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Michael'); // 95
```

初始化<font color="red"><code>Map</code></font>需要一个二维数组，或者直接初始化一个空<font color="red"><code>Map</code></font>。<font color="red"><code>Map</code></font>具有以下方法：

```javascript
var m = new Map(); // 空Map
m.set('Adam', 67); // 添加新的key-value
m.set('Bob', 59);
m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
m.get('Adam'); // undefined
```

由于一个key只能对应一个value，所以，多次对一个key放入value，后面的值会把前面的值冲掉：

```javascript
var m = new Map();
m.set('Adam', 67);
m.set('Adam', 88);
m.get('Adam'); // 88
```

### Set
<font color="red"><code>Set</code></font>和<font color="red"><code>Map</code></font>类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在<font color="red"><code>Set</code></font>中，没有重复的key。

要创建一个<font color="red"><code>Set</code></font>，需要提供一个<font color="red"><code>Array</code></font>作为输入，或者直接创建一个空<font color="red"><code>Set</code></font>：

```javascript
var s1 = new Set(); // 空Set
var s2 = new Set([1, 2, 3]); // 含1, 2, 3
```

重复元素在<font color="red"><code>Set</code></font>中自动被过滤：

```javascript
var s = new Set([1, 2, 3, 3, '3']);
s; // Set {1, 2, 3, "3"}
```

注意数字<font color="red"><code>3</code></font>和字符串<font color="red"><code>'3'</code></font>是不同的元素。

通过<font color="red"><code>addlete(key)</code></font>方法可以添加元素到<font color="red"><code>Set</code></font>中，可以重复添加，但不会有效果：

```javascript
s.add(4);
s; // Set {1, 2, 3, 4}
s.add(4);
s; // 仍然是 Set {1, 2, 3, 4}
```

通过<font color="red"><code>delete(key)</code></font>方法可以删除元素：

```javascript
var s = new Set([1, 2, 3]);
s; // Set {1, 2, 3}
s.delete(3);
s; // Set {1, 2}
```

### 小结
<font color="red"><code>Map</code></font>和<font color="red"><code>Set</code></font>是ES6标准新增的数据类型，请根据浏览器的支持情况决定是否要使用。
