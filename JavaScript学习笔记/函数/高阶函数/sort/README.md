<link rel="stylesheet" href="../../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../../static/css/console.css"/>

# sort
---

### 排序算法
排序也是在程序中经常用到的算法。无论使用冒泡排序还是快速排序，排序的核心是比较两个元素的大小。如果是数字，我们可以直接比较，但如果是字符串或者两个对象呢？直接比较数学上的大小是没有意义的，因此，比较的过程必须通过函数抽象出来。通常规定，对于两个元素<font color="red"><code>x</code></font>和<font color="red"><code>y</code></font>，如果认为<font color="red"><code>x < y</code></font>，则返回<font color="red"><code>-1</code></font>，如果认为<font color="red"><code>x == y</code></font>，则返回<font color="red"><code>0</code></font>，如果认为<font color="red"><code>x > y</code></font>，则返回<font color="red"><code>1</code></font>，这样，排序算法就不用关心具体的比较过程，而是根据比较结果直接排序。

JavaScript的<font color="red"><code>Array</code></font>的<font color="red"><code>sort()</code></font>方法就是用于排序的，但是排序结果可能让你大吃一惊：

```javascript
// 看上去正常的结果:
['Google', 'Apple', 'Microsoft'].sort(); // ['Apple', 'Google', 'Microsoft'];

// apple排在了最后:
['Google', 'apple', 'Microsoft'].sort(); // ['Google', 'Microsoft", 'apple']

// 无法理解的结果:
[10, 20, 1, 2].sort(); // [1, 10, 2, 20]
```

第二个排序把<font color="red"><code>apple</code></font>排在了最后，是因为字符串根据ASCII码进行排序，而小写字母<font color="red"><code>a</code></font>的ASCII码在大写字母之后。

第三个排序结果是什么鬼？简单的数字排序都能错？

这是因为<font color="red"><code>Array</code></font>的<font color="red"><code>sort()</code></font>方法默认把所有元素先转换为String再排序，结果<font color="red"><code>'10'</code></font>排在了<font color="red"><code>'2'</code></font>的前面，因为字符<font color="red"><code>'1'</code></font>比字符<font color="red"><code>'2'</code></font>的ASCII码小。

![](https://www.liaoxuefeng.com/files/attachments/1035534501673632/l)

如果不知道<font color="red"><code>sort()</code></font>方法的默认排序规则，直接对数字排序，绝对栽进坑里！

幸运的是，<font color="red"><code>sort()</code></font>方法也是一个高阶函数，它还可以接收一个比较函数来实现自定义的排序。

要按数字大小排序，我们可以这么写：

```javascript
'use strict';

var arr = [10, 20, 1, 2];
arr.sort(function (x, y) {
    if (x < y) {
        return -1;
    }
    if (x > y) {
        return 1;
    }
    return 0;
});
console.log(arr); // [1, 2, 10, 20]
```

<button class="run" onclick="(() => {
    const element = document.querySelector('p#sort');
    try {
        'use strict';
        var arr = [10, 20, 1, 2];
        arr.sort(function (x, y) {
            if (x < y) {
                return -1;
            }
            if (x > y) {
                return 1;
            }
            return 0;
        });
        console.log(arr); // [1, 2, 10, 20]
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>${arr}</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="sort" hidden></p>

如果要倒序排序，我们可以把大的数放前面：

```javascript
var arr = [10, 20, 1, 2];
arr.sort(function (x, y) {
    if (x < y) {
        return 1;
    }
    if (x > y) {
        return -1;
    }
    return 0;
}); // [20, 10, 2, 1]
```

默认情况下，对字符串排序，是按照ASCII的大小比较的，现在，我们提出排序应该忽略大小写，按照字母序排序。要实现这个算法，不必对现有代码大加改动，只要我们能定义出忽略大小写的比较算法就可以：

```javascript
var arr = ['Google', 'apple', 'Microsoft'];
arr.sort(function (s1, s2) {
    x1 = s1.toUpperCase();
    x2 = s2.toUpperCase();
    if (x1 < x2) {
        return -1;
    }
    if (x1 > x2) {
        return 1;
    }
    return 0;
}); // ['apple', 'Google', 'Microsoft']
```

忽略大小写来比较两个字符串，实际上就是先把字符串都变成大写（或者都变成小写），再比较。

从上述例子可以看出，高阶函数的抽象能力是非常强大的，而且，核心代码可以保持得非常简洁。

最后友情提示，<font color="red"><code>sort()</code></font>方法会直接对<font color="red"><code>Array</code></font>进行修改，它返回的结果仍是当前<font color="red"><code>Array</code></font>：

```javascript
var a1 = ['B', 'A', 'C'];
var a2 = a1.sort();
a1; // ['A', 'B', 'C']
a2; // ['A', 'B', 'C']
a1 === a2; // true, a1和a2是同一对象
```
