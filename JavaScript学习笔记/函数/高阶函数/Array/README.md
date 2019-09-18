<link rel="stylesheet" href="../../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../../static/css/console.css"/>

# Array
---

对于数组，除了<font color="red"><code>map()</code></font>、<font color="red"><code>reduce()</code></font>、<font color="red"><code>filter()</code></font>、<font color="red"><code>sort()</code></font>这些方法可以传入一个函数外，<font color="red"><code>Array</code></font>对象还提供了很多非常实用的高阶函数。

### every
<font color="red"><code>every()</code></font>方法可以判断数组的所有元素是否满足测试条件。

例如，给定一个包含若干字符串的数组，判断所有字符串是否满足指定的测试条件：

```javascript
'use strict';
var arr = ['Apple', 'pear', 'orange'];
console.log(arr.every(function (s) {
    return s.length > 0;
})); // true, 因为每个元素都满足s.length>0

console.log(arr.every(function (s) {
    return s.toLowerCase() === s;
})); // false, 因为不是每个元素都全部是小写
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#every');
    try {
        'use strict';
        var arr = ['Apple', 'pear', 'orange'];
        console.log(arr.every(function (s) {
            return s.length > 0;
        })); // true, 因为每个元素都满足s.length>0
        console.log(arr.every(function (s) {
            return s.toLowerCase() === s;
        })); // false, 因为不是每个元素都全部是小写
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>true<br>false</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="every" hidden></p>

### find
<font color="red"><code>find()</code></font>方法用于查找符合条件的第一个元素，如果找到了，返回这个元素，否则，返回<font color="red"><code>undefined</code></font>：

```javascript
'use strict';
var arr = ['Apple', 'pear', 'orange'];
console.log(arr.find(function (s) {
    return s.toLowerCase() === s;
})); // 'pear', 因为pear全部是小写

console.log(arr.find(function (s) {
    return s.toUpperCase() === s;
})); // undefined, 因为没有全部是大写的元素
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#find');
    try {
        'use strict';
        var arr = ['Apple', 'pear', 'orange'];
        console.log(arr.find(function (s) {
            return s.toLowerCase() === s;
        })); // 'pear', 因为pear全部是小写
        console.log(arr.find(function (s) {
            return s.toUpperCase() === s;
        })); // undefined, 因为没有全部是大写的元素
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>pear<br>undefined</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="find" hidden></p>

### findIndex
<font color="red"><code>findIndex()</code></font>和<font color="red"><code>find()</code></font>类似，也是查找符合条件的第一个元素，不同之处在于<font color="red"><code>findIndex()</code></font>会返回这个元素的索引，如果没有找到，返回-1：

```javascript
'use strict';
var arr = ['Apple', 'pear', 'orange'];
console.log(arr.findIndex(function (s) {
    return s.toLowerCase() === s;
})); // 1, 因为'pear'的索引是1

console.log(arr.findIndex(function (s) {
    return s.toUpperCase() === s;
})); // -1
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#findIndex');
    try {
        'use strict';
        var arr = ['Apple', 'pear', 'orange'];
        console.log(arr.findIndex(function (s) {
            return s.toLowerCase() === s;
        })); // 1, 因为'pear'的索引是1
        console.log(arr.findIndex(function (s) {
            return s.toUpperCase() === s;
        })); // -1
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>1<br>-1</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="findIndex" hidden></p>

### forEach
<font color="red"><code>forEach()</code></font>和<font color="red"><code>map()</code></font>类似，它也把每个元素依次作用于传入的函数，但不会返回新的数组。<font color="red"><code>forEach()</code></font>常用于遍历数组，因此，传入的函数不需要返回值：

```javascript
'use strict';
var arr = ['Apple', 'pear', 'orange'];
arr.forEach(console.log); // 依次打印每个元素
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#forEach');
    try {
        'use strict';
        var arr = ['Apple', 'pear', 'orange'];
        arr.forEach(console.log); // 依次打印每个元素
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>Apple<br>pear<br>orange</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="forEach" hidden></p>
