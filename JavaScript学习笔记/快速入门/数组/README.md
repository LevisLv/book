<link rel="stylesheet" href="../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../static/css/console.css"/>

# 数组
---

JavaScript的<font color="red"><code>Array</code></font>可以包含任意数据类型，并通过索引来访问每个元素。

要取得<font color="red"><code>Array</code></font>的长度，直接访问<font color="red"><code>length</code></font>属性：

```javascript
var arr = [1, 2, 3.14, 'Hello', null, true];
arr.length; // 6
```

<font color="red"><code>请注意</code></font>，直接给<font color="red"><code>Array</code></font>的<font color="red"><code>length</code></font>赋一个新的值会导致<font color="red"><code>Array</code></font>大小的变化：

```javascript
var arr = [1, 2, 3];
arr.length; // 3
arr.length = 6;
arr; // arr变为[1, 2, 3, undefined, undefined, undefined]
arr.length = 2;
arr; // arr变为[1, 2]
```

<font color="red"><code>Array</code></font>可以通过索引把对应的元素修改为新的值，因此，对<font color="red"><code>Array</code></font>的索引进行赋值会直接修改这个<font color="red"><code>Array</code></font>：

```javascript
var arr = ['A', 'B', 'C'];
arr[1] = 99;
arr; // arr现在变为['A', 99, 'C']
```
<font color="red"><code>请注意</code></font>，如果通过索引赋值时，索引超过了范围，同样会引起<font color="red"><code>Array</code></font>大小的变化：

```javascript
var arr = [1, 2, 3];
arr[5] = 'x';
arr; // arr变为[1, 2, 3, undefined, undefined, 'x']
```

大多数其他编程语言不允许直接改变数组的大小，越界访问索引会报错。然而，JavaScript的<font color="red"><code>Array</code></font>却不会有任何错误。在编写代码时，不建议直接修改<font color="red"><code>Array</code></font>的大小，访问索引时要确保索引不会越界。

### indexOf
与String类似，<font color="red"><code>Array</code></font>也可以通过<font color="red"><code>indexOf()</code></font>来搜索一个指定的元素的位置：

```javascript
var arr = [10, 20, '30', 'xyz'];
arr.indexOf(10); // 元素10的索引为0
arr.indexOf(20); // 元素20的索引为1
arr.indexOf(30); // 元素30没有找到，返回-1
arr.indexOf('30'); // 元素'30'的索引为2
```

注意了，数字<font color="red"><code>30</code></font>和字符串<font color="red"><code>'30'</code></font>是不同的元素。

### slice
<font color="red"><code>slice()</code></font>就是对应String的<font color="red"><code>substring()</code></font>版本，它截取<font color="red"><code>Array</code></font>的部分元素，然后返回一个新的<font color="red"><code>Array</code></font>：

```javascript
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
arr.slice(0, 3); // 从索引0开始，到索引3结束，但不包括索引3: ['A', 'B', 'C']
arr.slice(3); // 从索引3开始到结束: ['D', 'E', 'F', 'G']
```

注意到<font color="red"><code>slice()</code></font>的起止参数包括开始索引，不包括结束索引。

如果不给<font color="red"><code>slice()</code></font>传递任何参数，它就会从头到尾截取所有元素。利用这一点，我们可以很容易地复制一个<font color="red"><code>Array</code></font>：

```javascript
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
var aCopy = arr.slice();
aCopy; // ['A', 'B', 'C', 'D', 'E', 'F', 'G']
aCopy === arr; // false
```

### push和pop
<font color="red"><code>push()</code></font>向<font color="red"><code>Array</code></font>的末尾添加若干元素，<font color="red"><code>pop()</code></font>则把<font color="red"><code>Array</code></font>的最后一个元素删除掉：

```javascript
var arr = [1, 2];
arr.push('A', 'B'); // 返回Array新的长度: 4
arr; // [1, 2, 'A', 'B']
arr.pop(); // pop()返回'B'
arr; // [1, 2, 'A']
arr.pop(); arr.pop(); arr.pop(); // 连续pop 3次
arr; // []
arr.pop(); // 空数组继续pop不会报错，而是返回undefined
arr; // []
```

### unshift和shift
如果要往<font color="red"><code>Array</code></font>的头部添加若干元素，使用<font color="red"><code>unshift()</code></font>方法，<font color="red"><code>shift()</code></font>方法则把<font color="red"><code>Array</code></font>的第一个元素删掉：

```javascript
var arr = [1, 2];
arr.unshift('A', 'B'); // 返回Array新的长度: 4
arr; // ['A', 'B', 1, 2]
arr.shift(); // 'A'
arr; // ['B', 1, 2]
arr.shift(); arr.shift(); arr.shift(); // 连续shift 3次
arr; // []
arr.shift(); // 空数组继续shift不会报错，而是返回undefined
arr; // []
```

### sort
<font color="red"><code>sort()</code></font>可以对当前<font color="red"><code>Array</code></font>进行排序，它会直接修改当前<font color="red"><code>Array</code></font>的元素位置，直接调用时，按照默认顺序排序：

```javascript
var arr = ['B', 'C', 'A'];
arr.sort();
arr; // ['A', 'B', 'C']
```

能否按照我们自己指定的顺序排序呢？完全可以，我们将在后面的函数中讲到。

### reverse
<font color="red"><code>reverse()</code></font>把整个<font color="red"><code>Array</code></font>的元素给掉个个，也就是反转：

```javascript
var arr = ['one', 'two', 'three'];
arr.reverse(); 
arr; // ['three', 'two', 'one']
```

### splice
<font color="red"><code>splice()</code></font>方法是修改<font color="red"><code>Array</code></font>的“万能方法”，它可以从指定的索引开始删除若干元素，然后再从该位置添加若干元素：

```javascript
var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
// 从索引2开始删除3个元素,然后再添加两个元素:
arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
// 只删除,不添加:
arr.splice(2, 2); // ['Google', 'Facebook']
arr; // ['Microsoft', 'Apple', 'Oracle']
// 只添加,不删除:
arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
```

### concat
<font color="red"><code>concat()</code></font>方法把当前的<font color="red"><code>Array</code></font>和另一个<font color="red"><code>Array</code></font>连接起来，并返回一个新的<font color="red"><code>Array</code></font>：

```javascript
var arr = ['A', 'B', 'C'];
var added = arr.concat([1, 2, 3]);
added; // ['A', 'B', 'C', 1, 2, 3]
arr; // ['A', 'B', 'C']
```

<font color="red"><code>请注意</code></font>，<font color="red"><code>concat()</code></font>方法并没有修改当前<font color="red"><code>Array</code></font>，而是返回了一个新的<font color="red"><code>Array</code></font>。

实际上，<font color="red"><code>concat()</code></font>方法可以接收任意个元素和<font color="red"><code>Array</code></font>，并且自动把<font color="red"><code>Array</code></font>拆开，然后全部添加到新的<font color="red"><code>Array</code></font>里：

```javascript
var arr = ['A', 'B', 'C'];
arr.concat(1, 2, [3, 4]); // ['A', 'B', 'C', 1, 2, 3, 4]
```

### join
<font color="red"><code>join()</code></font>方法是一个非常实用的方法，它把当前<font color="red"><code>Array</code></font>的每个元素都用指定的字符串连接起来，然后返回连接后的字符串：

```javascript
var arr = ['A', 'B', 'C', 1, 2, 3];
arr.join('-'); // 'A-B-C-1-2-3'
```

如果<font color="red"><code>Array</code></font>的元素不是字符串，将自动转换为字符串后再连接。

### 多维数组
如果数组的某个元素又是一个<font color="red"><code>Array</code></font>，则可以形成多维数组，例如：

```javascript
var arr = [[1, 2, 3], [400, 500, 600], '-'];
```

上述<font color="red"><code>Array</code></font>包含3个元素，其中头两个元素本身也是<font color="red"><code>Array</code></font>。

练习：如何通过索引取到<font color="red"><code>500</code></font>这个值：

```javascript
'use strict';
var arr = [[1, 2, 3], [400, 500, 600], '-'];
var x = ??;
console.log(x); // x应该为500
```

<button class="run" onclick="(() => {
    const answer = `
'use strict';
var arr = [[1, 2, 3], [400, 500, 600], '-'];
var x = arr[1][1];
console.log(x); // x应该为500
`;
    alert(answer);
})();">Show Answer</button>
### 

### 小结
<font color="red"><code>Array</code></font>提供了一种顺序存储一组元素的功能，并可以按索引来读写。

练习：在新生欢迎会上，你已经拿到了新同学的名单，请排序后显示：<font color="red"><code>欢迎XXX，XXX，XXX和XXX同学！</code></font>：

```javascript
'use strict';
var arr = ['小明', '小红', '大军', '阿黄'];
console.log('???');
```

<button class="run" onclick="(() => {
    const answer = `
'use strict';
var arr = ['小明', '小红', '大军', '阿黄'];
console.log(\`欢迎\${arr.sort().slice(0, arr.length - 1).join('，')}和\${arr.slice(arr.length - 1)}同学\`);`;
    alert(answer);
})();">Show Answer</button>
### 
