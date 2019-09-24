<link rel="stylesheet" href="../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../static/css/console.css"/>

# RegExp
---

字符串是编程时涉及到的最多的一种数据结构，对字符串进行操作的需求几乎无处不在。比如判断一个字符串是否是合法的Email地址，虽然可以编程提取<font color="red"><code> @</code></font>前后的子串，再分别判断是否是单词和域名，但这样做不但麻烦，而且代码难以复用。

正则表达式是一种用来匹配字符串的强有力的武器。它的设计思想是用一种描述性的语言来给字符串定义一个规则，凡是符合规则的字符串，我们就认为它“匹配”了，否则，该字符串就是不合法的。

所以我们判断一个字符串是否是合法的Email的方法是：
1. 创建一个匹配Email的正则表达式；
2. 用该正则表达式去匹配用户的输入来判断是否合法。

因为正则表达式也是用字符串表示的，所以，我们要首先了解如何用字符来描述字符。

在正则表达式中，如果直接给出字符，就是精确匹配。用<font color="red"><code>\d</code></font>可以匹配一个数字，<font color="red"><code>\w</code></font>可以匹配一个字母或数字，所以：
* <font color="red"><code>'00\d'</code></font>可以匹配<font color="red"><code>'007'</code></font>，但无法匹配<font color="red"><code>'00A'</code></font>；
* <font color="red"><code>'\d\d\d'</code></font>可以匹配<font color="red"><code>'010'</code></font>；
* <font color="red"><code>'\w\w'</code></font>可以匹配<font color="red"><code>'js'</code></font>；

<font color="red"><code>.</code></font>可以匹配任意字符，所以：
* <font color="red"><code>'js.'</code></font>可以匹配<font color="red"><code>'jsp'</code></font>、<font color="red"><code>'jss'</code></font>、<font color="red"><code>'js!'</code></font>等等。

要匹配变长的字符，在正则表达式中，用<font color="red"><code>*</code></font>表示任意个字符（包括0个），用<font color="red"><code>+</code></font>表示至少一个字符，用<font color="red"><code>?</code></font>表示0个或1个字符，用<font color="red"><code>{n}</code></font>表示n个字符，用<font color="red"><code>{n,m}</code></font>表示n-m个字符：

来看一个复杂的例子：<font color="red"><code>\d{3}\s+\d{3,8}</code></font>。

我们来从左到右解读一下：
1. <font color="red"><code>\d{3}</code></font>表示匹配3个数字，例如<font color="red"><code>'010'</code></font>；
2. <font color="red"><code>\s</code></font>可以匹配一个空格（也包括Tab等空白符），所以<font color="red"><code>\s+</code></font>表示至少有一个空格，例如匹配<font color="red"><code>' '</code></font>，<font color="red"><code>'\t\t'</code></font>等；
3. <font color="red"><code>\d{3,8}</code></font>表示3-8个数字，例如<font color="red"><code>'1234567'</code></font>。

综合起来，上面的正则表达式可以匹配以任意个空格隔开的带区号的电话号码。

如果要匹配<font color="red"><code>'010-12345'</code></font>这样的号码呢？由于<font color="red"><code>'-'</code></font>是特殊字符，在正则表达式中，要用<font color="red"><code>'\'</code></font>转义，所以，上面的正则是<font color="red"><code>\d{3}\-\d{3,8}</code></font>。

但是，仍然无法匹配<font color="red"><code>'010 - 12345'</code></font>，因为带有空格。所以我们需要更复杂的匹配方式。

### 进阶
要做更精确地匹配，可以用<font color="red"><code>[]</code></font>表示范围，比如：
* <font color="red"><code>[0-9a-zA-Z\_]</code></font>可以匹配一个数字、字母或者下划线；
* <font color="red"><code>[0-9a-zA-Z\_]+</code></font>可以匹配至少由一个数字、字母或者下划线组成的字符串，比如<font color="red"><code>'a100'</code></font>，<font color="red"><code>'0_Z'</code></font>，<font color="red"><code>'js2015'</code></font>等等；
* <font color="red"><code>[a-zA-Z\_\$][0-9a-zA-Z\_\$]*</code></font>可以匹配由字母或下划线、$开头，后接任意个由一个数字、字母或者下划线、$组成的字符串，也就是JavaScript允许的变量名；
* <font color="red"><code>[a-zA-Z\_\$][0-9a-zA-Z\_\$]{0, 19}</code></font>更精确地限制了变量的长度是1-20个字符（前面1个字符+后面最多19个字符）。

<font color="red"><code>A|B</code></font>可以匹配A或B，所以<font color="red"><code>(J|j)ava(S|s)cript</code></font>可以匹配<font color="red"><code>'JavaScript'</code></font>、<font color="red"><code>'Javascript'</code></font>、<font color="red"><code>'javaScript'</code></font>或者<font color="red"><code>'javascript'</code></font>。

<font color="red"><code>^</code></font>表示行的开头，<font color="red"><code>^\d</code></font>表示必须以数字开头。

<font color="red"><code>$</code></font>表示行的结束，<font color="red"><code>\d$</code></font>表示必须以数字结束。

你可能注意到了，<font color="red"><code>js</code></font>也可以匹配<font color="red"><code>'jsp'</code></font>，但是加上<font color="red"><code>^js$</code></font>就变成了整行匹配，就只能匹配<font color="red"><code>'js'</code></font>了。

### RegExp
有了准备知识，我们就可以在JavaScript中使用正则表达式了。

JavaScript有两种方式创建一个正则表达式：

第一种方式是直接通过<font color="red"><code>/正则表达式/</code></font>写出来，第二种方式是通过<font color="red"><code>new RegExp('正则表达式')</code></font>创建一个RegExp对象。

两种写法是一样的：

```javascript
var re1 = /ABC\-001/;
var re2 = new RegExp('ABC\\-001');

re1; // /ABC\-001/
re2; // /ABC\-001/
```

注意，如果使用第二种写法，因为字符串的转义问题，字符串的两个<font color="red"><code>\\</code></font>实际上是一个<font color="red"><code>\</code></font>。

先看看如何判断正则表达式是否匹配：

```javascript
var re = /^\d{3}\-\d{3,8}$/;
re.test('010-12345'); // true
re.test('010-1234x'); // false
re.test('010 12345'); // false
```

RegExp对象的<font color="red"><code>test()</code></font>方法用于测试给定的字符串是否符合条件。

### 切分字符串
用正则表达式切分字符串比用固定的字符更灵活，请看正常的切分代码：

```javascript
'a b   c'.split(' '); // ['a', 'b', '', '', 'c']
```

嗯，无法识别连续的空格，用正则表达式试试：

```javascript
'a b   c'.split(/\s+/); // ['a', 'b', 'c']
```
无论多少个空格都可以正常分割。加入<font color="red"><code>,</code></font>试试：

```javascript
'a,b, c  d'.split(/[\s\,]+/); // ['a', 'b', 'c', 'd']
```

再加入<font color="red"><code>;</code></font>试试：

```javascript
'a,b;; c  d'.split(/[\s\,\;]+/); // ['a', 'b', 'c', 'd']
```

如果用户输入了一组标签，下次记得用正则表达式来把不规范的输入转化成正确的数组。

### 分组
除了简单地判断是否匹配之外，正则表达式还有提取子串的强大功能。用<font color="red"><code>()</code></font>表示的就是要提取的分组（Group）。比如：

<font color="red"><code>^(\d{3})-(\d{3,8})$</code></font>分别定义了两个组，可以直接从匹配的字符串中提取出区号和本地号码：

```javascript
var re = /^(\d{3})-(\d{3,8})$/;
re.exec('010-12345'); // ['010-12345', '010', '12345']
re.exec('010 12345'); // null
```

如果正则表达式中定义了组，就可以在<font color="red"><code>RegExp</code></font>对象上用<font color="red"><code>exec()</code></font>方法提取出子串来。

<font color="red"><code>exec()</code></font>方法在匹配成功后，会返回一个<font color="red"><code>Array</code></font>，第一个元素是正则表达式匹配到的整个字符串，后面的字符串表示匹配成功的子串。

<font color="red"><code>exec()</code></font>方法在匹配失败时返回<font color="red"><code>null</code></font>。

提取子串非常有用。来看一个更凶残的例子：

```javascript
var re = /^(0[0-9]|1[0-9]|2[0-3]|[0-9])\:(0[0-9]|1[0-9]|2[0-9]|3[0-9]|4[0-9]|5[0-9]|[0-9])\:(0[0-9]|1[0-9]|2[0-9]|3[0-9]|4[0-9]|5[0-9]|[0-9])$/;
re.exec('19:05:30'); // ['19:05:30', '19', '05', '30']
```

这个正则表达式可以直接识别合法的时间。但是有些时候，用正则表达式也无法做到完全验证，比如识别日期：

```javascript
var re = /^(0[1-9]|1[0-2]|[0-9])-(0[1-9]|1[0-9]|2[0-9]|3[0-1]|[0-9])$/;
```

对于<font color="red"><code>'2-30'</code></font>，<font color="red"><code>'4-31'</code></font>这样的非法日期，用正则还是识别不了，或者说写出来非常困难，这时就需要程序配合识别了。

### 贪婪匹配
需要特别指出的是，正则匹配默认是贪婪匹配，也就是匹配尽可能多的字符。举例如下，匹配出数字后面的<font color="red"><code>0</code></font>：

```javascript
var re = /^(\d+)(0*)$/;
re.exec('102300'); // ['102300', '102300', '']
```

由于<font color="red"><code>\d+</code></font>采用贪婪匹配，直接把后面的<font color="red"><code>0</code></font>全部匹配了，结果<font color="red"><code>0*</code></font>只能匹配空字符串了。

必须让<font color="red"><code>\d+</code></font>采用非贪婪匹配（也就是尽可能少匹配），才能把后面的<font color="red"><code>0</code></font>匹配出来，加个<font color="red"><code>?</code></font>就可以让<font color="red"><code>\d+</code></font>采用非贪婪匹配：

```javascript
var re = /^(\d+?)(0*)$/;
re.exec('102300'); // ['102300', '1023', '00']
```

### 全局搜索
JavaScript的正则表达式还有几个特殊的标志，最常用的是<font color="red"><code>g</code></font>，表示全局匹配：

```javascript
var r1 = /test/g;
// 等价于:
var r2 = new RegExp('test', 'g');
```

全局匹配可以多次执行<font color="red"><code>exec()</code></font>方法来搜索一个匹配的字符串。当我们指定<font color="red"><code>g</code></font>标志后，每次运行<font color="red"><code>exec()</code></font>，正则表达式本身会更新<font color="red"><code>lastIndex</code></font>属性，表示上次匹配到的最后索引：

```javascript
var s = 'JavaScript, VBScript, JScript and ECMAScript';
var re=/[a-zA-Z]+Script/g;

// 使用全局匹配:
re.exec(s); // ['JavaScript']
re.lastIndex; // 10

re.exec(s); // ['VBScript']
re.lastIndex; // 20

re.exec(s); // ['JScript']
re.lastIndex; // 29

re.exec(s); // ['ECMAScript']
re.lastIndex; // 44

re.exec(s); // null，直到结束仍没有匹配到
```

全局匹配类似搜索，因此不能使用<font color="red"><code>/^...$/</code></font>，那样只会最多匹配一次。

正则表达式还可以指定<font color="red"><code>i</code></font>标志，表示忽略大小写，<font color="red"><code>m</code></font>标志，表示执行多行匹配。

### 小结
正则表达式非常强大，要在短短的一节里讲完是不可能的。要讲清楚正则的所有内容，可以写一本厚厚的书了。如果你经常遇到正则表达式的问题，你可能需要一本正则表达式的参考书。

### 练习
请尝试写一个验证Email地址的正则表达式。版本一应该可以验证出类似的Email：

```javascript
'use strict';
var re = /^$/;

// 测试:
var
    i,
    success = true,
    should_pass = ['someone@gmail.com', 'bill.gates@microsoft.com', 'tom@voyager.org', 'bob2015@163.com'],
    should_fail = ['test#gmail.com', 'bill@microsoft', 'bill%gates@ms.com', '@voyager.org'];
for (i = 0; i < should_pass.length; i++) {
    if (!re.test(should_pass[i])) {
        console.log('测试失败: ' + should_pass[i]);
        success = false;
        break;
    }
}
for (i = 0; i < should_fail.length; i++) {
    if (re.test(should_fail[i])) {
        console.log('测试失败: ' + should_fail[i]);
        success = false;
        break;
    }
}
if (success) {
    console.log('测试通过!');
}
```

<button class="run" onclick="(() => {
    const answer = `
'use strict';
var re = /^[a-zA-Z]\w+?\.?\w+?\@\w+\.[a-zA-Z]+$/;
`;
    alert(answer);
})();">Show Answer</button>
### 

版本二可以验证并提取出带名字的Email地址：

```javascript
'use strict';
var re = /^$/;

// 测试:
var r = re.exec('<Tom Paris> tom@voyager.org');
if (r === null || r.toString() !== ['<Tom Paris> tom@voyager.org', 'Tom Paris', 'tom@voyager.org'].toString()) {
    console.log('测试失败!');
}
else {
    console.log('测试成功!');
}
```

<button class="run" onclick="(() => {
    const answer = `
'use strict';
var re = /^<([a-zA-Z]+\s+?[a-zA-Z]+)>\s+([a-zA-Z]\w+?\.?\w+?\@\w+\.[a-zA-Z]+)$/;
`;
    alert(answer);
})();">Show Answer</button>
### 
