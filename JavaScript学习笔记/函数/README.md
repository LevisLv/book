# 函数
---

我们知道圆的面积计算公式为：

S = πr2

当我们知道半径<font color="red"><code>r</code></font>的值时，就可以根据公式计算出面积。假设我们需要计算3个不同大小的圆的面积：

```javascript
var r1 = 12.34;
var r2 = 9.08;
var r3 = 73.1;
var s1 = 3.14 * r1 * r1;
var s2 = 3.14 * r2 * r2;
var s3 = 3.14 * r3 * r3;
```

当代码出现有规律的重复的时候，你就需要当心了，每次写<font color="red"><code>3.14 * x * x</code></font>不仅很麻烦，而且，如果要把<font color="red"><code>3.14</code></font>改成<font color="red"><code>3.14159265359</code></font>的时候，得全部替换。

有了函数，我们就不再每次写<font color="red"><code>s = 3.14 * x * x</code></font>，而是写成更有意义的函数调用<font color="red"><code>s = area_of_circle(x)</code></font>，而函数<font color="red"><code>area_of_circle</code></font>本身只需要写一次，就可以多次调用。

基本上所有的高级语言都支持函数，JavaScript也不例外。JavaScript的函数不但是“头等公民”，而且可以像变量一样使用，具有非常强大的抽象能力。

### 抽象
抽象是数学中非常常见的概念。举个例子：

计算数列的和，比如：<font color="red"><code>1 + 2 + 3 + ... + 100</code></font>，写起来十分不方便，于是数学家发明了求和符号∑，可以把<font color="red"><code>1 + 2 + 3 + ... + 100</code></font>记作：

> 100<br>
> <span style="font-size:3em">∑</span><span style="font-size:2em">n</span><br>
> n=1<br>

这种抽象记法非常强大，因为我们看到 ∑ 就可以理解成求和，而不是还原成低级的加法运算。

而且，这种抽象记法是可扩展的，比如：

> 100<br>
> <span style="font-size:3em">∑</span><span style="font-size:2em">(n<sup>2</sup>+1)</span><br>
> n=1<br>

还原成加法运算就变成了：

> (1 x 1 + 1) + (2 x 2 + 1) + (3 x 3 + 1) + ... + (100 x 100 + 1)

可见，借助抽象，我们才能不关心底层的具体计算过程，而直接在更高的层次上思考问题。

写计算机程序也是一样，函数就是最基本的一种代码抽象的方式。
