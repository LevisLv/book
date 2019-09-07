<link rel="stylesheet" href="../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../static/css/console.css"/>

# 基本语法
---

### 语法
JavaScript的语法和Java语言类似，每个语句以<font color="red"><code>;</code></font>结束，语句块用<font color="red"><code>{...}</code></font>。但是，JavaScript并不强制要求在每个语句的结尾加<font color="red"><code>;</code></font>，浏览器中负责执行JavaScript代码的引擎会自动在每个语句的结尾补上<font color="red"><code>;</code></font>。

<p class="consoleError"><label class='consoleError'>让JavaScript引擎自动加分号在某些情况下会改变程序的语义，导致运行结果与期望不一致。在本教程中，我们不会省略;，所有语句都会添加;。</label></p>

例如，下面的一行代码就是一个完整的赋值语句：

```javascript
var x = 1;
```

下面的一行代码是一个字符串，但仍然可以视为一个完整的语句：

```javascript
'Hello, world';
```

下面的一行代码包含两个语句，每个语句用<font color="red"><code>;</code></font>表示语句结束：

```javascript
var x = 1; var y = 2; // 不建议一行写多个语句!
```

语句块是一组语句的集合，例如，下面的代码先做了一个判断，如果判断成立，将执行<font color="red"><code>{...}</code></font>中的所有语句：

```javascript
if (2 > 1) {
    x = 1;
    y = 2;
    z = 3;
}
```

注意花括号<font color="red"><code>{...}</code></font>内的语句具有缩进，通常是4个空格。缩进不是JavaScript语法要求必须的，但缩进有助于我们理解代码的层次，所以编写代码时要遵守缩进规则。很多文本编辑器具有“自动缩进”的功能，可以帮助整理代码。

<font color="red"><code>{...}</code></font>还可以嵌套，形成层级结构：

```javascript
if (2 > 1) {
    x = 1;
    y = 2;
    z = 3;
    if (x < y) {
        z = 4;
    }
    if (x > y) {
        z = 5;
    }
}
```

JavaScript本身对嵌套的层级没有限制，但是过多的嵌套无疑会大大增加看懂代码的难度。遇到这种情况，需要把部分代码抽出来，作为函数来调用，这样可以减少代码的复杂度。

###注释
以<font color="red"><code>//</code></font>开头直到行末的字符被视为行注释，注释是给开发人员看到，JavaScript引擎会自动忽略：

```javascript
// 这是一行注释
alert('hello'); // 这也是注释
```

另一种块注释是用<font color="red"><code>/*...*/</code></font>把多行字符包裹起来，把一大“块”视为一个注释：

```javascript
/* 从这里开始是块注释
仍然是注释
仍然是注释
注释结束 */
```

### 练习：

分别利用行注释和块注释把下面的语句注释掉，使它不再执行：

```javascript
// 请注释掉下面的语句:
alert('我不想执行');
alert('我也不想执行');
```

<button class="run" onclick="(() => {
    const answer = `
// 请注释掉下面的语句:
// alert('我不想执行');
/* alert('我也不想执行'); */
`;
    alert(answer);
})();">Show Answer</button>
 
### 大小写
请注意，JavaScript严格区分大小写，如果弄错了大小写，程序将报错或者运行不正常。
