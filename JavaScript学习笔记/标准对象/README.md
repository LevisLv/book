# 标准对象
---

在JavaScript的世界里，一切都是对象。

但是某些对象还是和其他对象不太一样。为了区分对象的类型，我们用<font color="red"><code>typeof</code></font>操作符获取对象的类型，它总是返回一个字符串：

```javascript
typeof 123; // 'number'
typeof NaN; // 'number'
typeof 'str'; // 'string'
typeof true; // 'boolean'
typeof undefined; // 'undefined'
typeof Math.abs; // 'function'
typeof null; // 'object'
typeof []; // 'object'
typeof {}; // 'object'
```

可见，<font color="red"><code>number</code></font>、<font color="red"><code>string</code></font>、<font color="red"><code>boolean</code></font>、<font color="red"><code>function</code></font>和<font color="red"><code>undefined</code></font>有别于其他类型。特别注意<font color="red"><code>null</code></font>的类型是<font color="red"><code>object</code></font>，<font color="red"><code>Array</code></font>的类型也是<font color="red"><code>object</code></font>，如果我们用<font color="red"><code>typeof</code></font>将无法区分出<font color="red"><code>null</code></font>、<font color="red"><code>Array</code></font>和通常意义上的object——<font color="red"><code>{}</code></font>。

### 包装对象
除了这些类型外，JavaScript还提供了包装对象，熟悉Java的小伙伴肯定很清楚<font color="red"><code>int</code></font>和<font color="red"><code>Integer</code></font>这种暧昧关系。

<font color="red"><code>number</code></font>、<font color="red"><code>boolean</code></font>和<font color="red"><code>string</code></font>都有包装对象。没错，在JavaScript中，字符串也区分<font color="red"><code>string</code></font>类型和它的包装类型。包装对象用<font color="red"><code>new</code></font>创建：

```javascript
var n = new Number(123); // 123,生成了新的包装类型
var b = new Boolean(true); // true,生成了新的包装类型
var s = new String('str'); // 'str',生成了新的包装类型
```

虽然包装对象看上去和原来的值一模一样，显示出来也是一模一样，但他们的类型已经变为<font color="red"><code>object</code></font>了！所以，包装对象和原始值用<font color="red"><code>===</code></font>比较会返回<font color="red"><code>false</code></font>：

```javascript
typeof new Number(123); // 'object'
new Number(123) === 123; // false

typeof new Boolean(true); // 'object'
new Boolean(true) === true; // false

typeof new String('str'); // 'object'
new String('str') === 'str'; // false
```

所以<font color="red"><code>闲的蛋疼也不要使用包装对象</code></font>！尤其是针对<font color="red"><code>string</code></font>类型！！！

如果我们在使用<font color="red"><code>Number</code></font>、<font color="red"><code>Boolean</code></font>和<font color="red"><code>String</code></font>时，没有写<font color="red"><code>new</code></font>会发生什么情况？

此时，<font color="red"><code>Number()</code></font>、<font color="red"><code>Boolean()</code></font>和<font color="red"><code>String()</code></font>被当做普通函数，把任何类型的数据转换为<font color="red"><code>number</code></font>、<font color="red"><code>boolean</code></font>和<font color="red"><code>string</code></font>类型（注意不是其包装类型）：

```javascript
var n = Number('123'); // 123，相当于parseInt()或parseFloat()
typeof n; // 'number'

var b = Boolean('true'); // true
typeof b; // 'boolean'

var b2 = Boolean('false'); // true! 'false'字符串转换结果为true！因为它是非空字符串！
var b3 = Boolean(''); // false

var s = String(123.45); // '123.45'
typeof s; // 'string'
```

是不是感觉头大了？这就是JavaScript特有的催眠魅力！

总结一下，有这么几条规则需要遵守：
* 不要使用<font color="red"><code>new Number()</code></font>、<font color="red"><code>new Boolean()</code></font>、<font color="red"><code>new String()</code></font>创建包装对象；
* 用<font color="red"><code>parseInt()</code></font>或<font color="red"><code>parseFloat()</code></font>来转换任意类型到<font color="red"><code>number</code></font>；
* 用<font color="red"><code>String()</code></font>来转换任意类型到<font color="red"><code>string</code></font>，或者直接调用某个对象的<font color="red"><code>toString()</code></font>方法；
* 通常不必把任意类型转换为<font color="red"><code>boolean</code></font>再判断，因为可以直接写<font color="red"><code>if (myVar) {...}</code></font>；
* <font color="red"><code>typeof</code></font>操作符可以判断出<font color="red"><code>number</code></font>、<font color="red"><code>boolean</code></font>、<font color="red"><code>string</code></font>、<font color="red"><code>function</code></font>和<font color="red"><code>undefined</code></font>；
* 判断<font color="red"><code>Array</code></font>要使用<font color="red"><code>Array.isArray(arr)</code></font>；
* 判断<font color="red"><code>null</code></font>请使用<font color="red"><code>myVar === null</code></font>；
* 判断某个全局变量是否存在用<font color="red"><code>typeof window.myVar === 'undefined'</code></font>；
* 函数内部判断某个变量是否存在用<font color="red"><code>typeof myVar === 'undefined'</code></font>。

最后有细心的同学指出，任何对象都有<font color="red"><code>toString()</code></font>方法吗？<font color="red"><code>null</code></font>和<font color="red"><code>undefined</code></font>就没有！确实如此，这两个特殊值要除外，虽然<font color="red"><code>null</code></font>还伪装成了<font color="red"><code>object</code></font>类型。

更细心的同学指出，<font color="red"><code>number</code></font>对象调用<font color="red"><code>toString()</code></font>报SyntaxError：

```javascript
123.toString(); // SyntaxError
```

遇到这种情况，要特殊处理一下：

```javascript
123..toString(); // '123', 注意是两个点！
(123).toString(); // '123'
```

不要问为什么，这就是JavaScript代码的乐趣！
