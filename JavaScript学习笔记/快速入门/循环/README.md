<link rel="stylesheet" href="../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../static/css/console.css"/>

# 循环
---

### 循环
要计算1+2+3，我们可以直接写表达式：

```javascript
1 + 2 + 3; // 6
```

要计算1+2+3+...+10，勉强也能写出来。

但是，要计算1+2+3+...+10000，直接写表达式就不可能了。

为了让计算机能计算成千上万次的重复运算，我们就需要循环语句。

JavaScript的循环有两种，一种是<font color="red"><code>for</code></font>循环，通过初始条件、结束条件和递增条件来循环执行语句块：

```javascript
var x = 0;
var i;
for (i=1; i<=10000; i++) {
    x = x + i;
}
x; // 50005000
```

让我们来分析一下<font color="red"><code>for</code></font>循环的控制条件：
* i=1 这是初始条件，将变量i置为1；
* i<=10000 这是判断条件，满足时就继续循环，不满足就退出循环；
* i++ 这是每次循环后的递增条件，由于每次循环后变量i都会加1，因此它终将在若干次循环后不满足判断条件<font color="red"><code>i<=10000</code></font>而退出循环。

### 练习
利用<font color="red"><code>for</code></font>循环计算<font color="red"><code>1 * 2 * 3 * ... * 10</code></font>的结果：

```javascript
'use strict';
var x = ?;
var i;
for ...
if (x === 3628800) {
    console.log('1 x 2 x 3 x ... x 10 = ' + x);
}
else {
    console.log('计算错误');
}
```

<button class="run" onclick="(() => {
    const answer = `
'use strict';
var x = x;
var 10;
for (i = 1; i <= 10; i++) {
    x *=i;
}
if (x === 3628800) {
    console.log('1 x 2 x 3 x ... x 10 = ' + x);
}
else {
    console.log('计算错误');
}
`;
    alert(answer);
})();">Show Answer</button>
### 

<font color="red"><code>for</code></font>循环最常用的地方是利用索引来遍历数组：

```javascript
var arr = ['Apple', 'Google', 'Microsoft'];
var i, x;
for (i=0; i<arr.length; i++) {
    x = arr[i];
    console.log(x);
}
```

<font color="red"><code>for</code></font>循环的3个条件都是可以省略的，如果没有退出循环的判断条件，就必须使用<font color="red"><code>break</code></font>语句退出循环，否则就是死循环：

```javascript
var x = 0;
for (;;) { // 将无限循环下去
    if (x > 100) {
        break; // 通过if判断来退出循环
    }
    x ++;
}
```

### for ... in
<font color="red"><code>for</code></font>循环的一个变体是<font color="red"><code>for ... in</code></font>循环，它可以把一个对象的所有属性依次循环出来：

```javascript
var o = {
    name: 'Jack',
    age: 20,
    city: 'Beijing'
};
for (var key in o) {
    console.log(key); // 'name', 'age', 'city'
}
```

要过滤掉对象继承的属性，用<font color="red"><code>hasOwnProperty()</code></font>来实现：

```javascript
var o = {
    name: 'Jack',
    age: 20,
    city: 'Beijing'
};
for (var key in o) {
    if (o.hasOwnProperty(key)) {
        console.log(key); // 'name', 'age', 'city'
    }
}
```

由于<font color="red"><code>Array</code></font>也是对象，而它的每个元素的索引被视为对象的属性，因此，<font color="red"><code>for ... in</code></font>循环可以直接循环出<font color="red"><code>Array</code></font>的索引：

```javascript
var a = ['A', 'B', 'C'];
for (var i in a) {
    console.log(i); // '0', '1', '2'
    console.log(a[i]); // 'A', 'B', 'C'
}
```

<font color="red"><code>请注意</code></font>，<font color="red"><code>for ... in</code></font>对<font color="red"><code>Array</code></font>的循环得到的是<font color="red"><code>String</code></font>而不是<font color="red"><code>Number</code></font>。

### while
<font color="red"><code>for</code></font>循环在已知循环的初始和结束条件时非常有用。而上述忽略了条件的<font color="red"><code>for</code></font>循环容易让人看不清循环的逻辑，此时用<font color="red"><code>while</code></font>循环更佳。

<font color="red"><code>while</code></font>循环只有一个判断条件，条件满足，就不断循环，条件不满足时则退出循环。比如我们要计算100以内所有奇数之和，可以用<font color="red"><code>while</code></font>循环实现：

```javascript
var x = 0;
var n = 99;
while (n > 0) {
    x = x + n;
    n = n - 2;
}
x; // 2500
```

在循环内部变量<font color="red"><code>n</code></font>不断自减，直到变为<font color="red"><code>-1</code></font>时，不再满足<font color="red"><code>while</code></font>条件，循环退出。

### do ... while
最后一种循环是<font color="red"><code>do { ... } while()</code></font>循环，它和<font color="red"><code>while</code></font>循环的唯一区别在于，不是在每次循环开始的时候判断条件，而是在每次循环完成的时候判断条件：

```javascript
var n = 0;
do {
    n = n + 1;
} while (n < 100);
n; // 100
```

用<font color="red"><code>do { ... } while()</code></font>循环要小心，循环体会至少执行1次，而<font color="red"><code>for</code></font>和<font color="red"><code>while</code></font>循环则可能一次都不执行。

### 练习
请利用循环遍历数组中的每个名字，并显示<font color="red"><code>Hello, xxx!</code></font>：

```javascript
'use strict';
var arr = ['Bart', 'Lisa', 'Adam'];
for ...
```

请尝试<font color="red"><code>for</code></font>循环和<font color="red"><code>while</code></font>循环，并以正序、倒序两种方式遍历。

<button class="run" onclick="(() => {
    const answer = `
'use strict';
var arr = ['Bart', 'Lisa', 'Adam'];
// for循环正序
for (var i in arr.sort()) {
    console.log(\`Hello, \${arr[i]}!\`);
}
// for循环倒序
for (var j in arr.sort().reverse()) {
    console.log(\`Hello, \${arr[j]}!\`);
}
// while循环正序
var i = 0;
while (i < arr.sort().length) {
    console.log(\`Hello, \${arr[i]}!\`);
    i++;
}
// while循环倒序
var j = 0;
while (j < arr.sort().reverse().length) {
    console.log(\`Hello, \${arr[j]}!\`);
    j++;
}
`;
    alert(answer);
})();">Show Answer</button>
### 

### 小结
循环是让计算机做重复任务的有效的方法，有些时候，如果代码写得有问题，会让程序陷入“死循环”，也就是永远循环下去。JavaScript的死循环会让浏览器无法正常显示或执行当前页面的逻辑，有的浏览器会直接挂掉，有的浏览器会在一段时间后提示你强行终止JavaScript的执行，因此，要特别注意死循环的问题。

在编写循环代码时，务必小心编写初始条件和判断条件，尤其是边界值。特别注意<font color="red"><code>i < 100</code></font>和<font color="red"><code>i <= 100</code></font>是不同的判断逻辑。
