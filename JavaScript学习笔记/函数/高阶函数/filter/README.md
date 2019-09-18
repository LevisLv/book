<link rel="stylesheet" href="../../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../../static/css/console.css"/>

# filter
---

<font color="red"><code>filter</code></font>也是一个常用的操作，它用于把<font color="red"><code>Array</code></font>的某些元素过滤掉，然后返回剩下的元素。

和<font color="red"><code>map()</code></font>类似，<font color="red"><code>Array</code></font>的<font color="red"><code>filter()</code></font>也接收一个函数。和<font color="red"><code>map()</code></font>不同的是，<font color="red"><code>filter()</code></font>把传入的函数依次作用于每个元素，然后根据返回值是<font color="red"><code>true</code></font>还是<font color="red"><code>false</code></font>决定保留还是丢弃该元素。

例如，在一个<font color="red"><code>Array</code></font>中，删掉偶数，只保留奇数，可以这么写：

```javascript
var arr = [1, 2, 4, 5, 6, 9, 10, 15];
var r = arr.filter(function (x) {
    return x % 2 !== 0;
});
r; // [1, 5, 9, 15]
```

把一个<font color="red"><code>Array</code></font>中的空字符串删掉，可以这么写：

```javascript
var arr = ['A', '', 'B', null, undefined, 'C', '  '];
var r = arr.filter(function (s) {
    return s && s.trim(); // 注意：IE9以下的版本没有trim()方法
});
r; // ['A', 'B', 'C']
```
可见用<font color="red"><code>filter()</code></font>这个高阶函数，关键在于正确实现一个“筛选”函数。

### 回调函数
<font color="red"><code>filter()</code></font>接收的回调函数，其实可以有多个参数。通常我们仅使用第一个参数，表示<font color="red"><code>Array</code></font>的某个元素。回调函数还可以接收另外两个参数，表示元素的位置和数组本身：

```javascript
var arr = ['A', 'B', 'C'];
var r = arr.filter(function (element, index, self) {
    console.log(element); // 依次打印'A', 'B', 'C'
    console.log(index); // 依次打印0, 1, 2
    console.log(self); // self就是变量arr
    return true;
});
```

利用<font color="red"><code>filter</code></font>，可以巧妙地去除<font color="red"><code>Array</code></font>的重复元素：

```javascript
'use strict';

var
    r,
    arr = ['apple', 'strawberry', 'banana', 'pear', 'apple', 'orange', 'orange', 'strawberry'];
r = arr.filter(function (element, index, self) {
    return self.indexOf(element) === index;
});

console.log(r.toString());
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#filter');
    try {
        'use strict';
        var
            r,
            arr = ['apple', 'strawberry', 'banana', 'pear', 'apple', 'orange', 'orange', 'strawberry'];
        r = arr.filter(function (element, index, self) {
            return self.indexOf(element) === index;
        });
        console.log(r.toString());
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>${r.toString()}</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="filter" hidden></p>

去除重复元素依靠的是<font color="red"><code>indexOf</code></font>总是返回第一个元素的位置，后续的重复元素位置与<font color="red"><code>indexOf</code></font>返回的位置不相等，因此被<font color="red"><code>filter</code></font>滤掉了。

### 练习
请尝试用<font color="red"><code>filter()</code></font>筛选出素数：

```javascript
'use strict';

function get_primes(arr) {
    return [];

}

// 测试:
var
    x,
    r,
    arr = [];
for (x = 1; x < 100; x++) {
    arr.push(x);
}
r = get_primes(arr);
if (r.toString() === [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97].toString()) {
    console.log('测试通过!');
} else {
    console.log('测试失败: ' + r.toString());
}
```

<button class="run" onclick="(() => {
    const answer = `
'use strict';
function get_primes(arr) {
    return arr.filter(((value, index, array) => {
        if (value === 1) {
            return false;
        } else {
            for (let i = 2; i <= Math.sqrt(value); i++) {
                if (value % i === 0) {
                    return false;
                }
            }
            return true;
        }
    }));
}
`;
    alert(answer);
})();">Show Answer</button>
### 
