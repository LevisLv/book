<link rel="stylesheet" href="../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../static/css/console.css"/>

# 方法
---

在一个对象中绑定函数，称为这个对象的方法。

在JavaScript中，对象的定义是这样的：

```javascript
var xiaoming = {
    name: '小明',
    birth: 1990
};
```

但是，如果我们给<font color="red"><code>xiaoming</code></font>绑定一个函数，就可以做更多的事情。比如，写个<font color="red"><code>age()</code></font>方法，返回<font color="red"><code>xiaoming</code></font>的年龄：

```javascript
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var y = new Date().getFullYear();
        return y - this.birth;
    }
};

xiaoming.age; // function xiaoming.age()
xiaoming.age(); // 今年调用是25,明年调用就变成26了
```

绑定到对象上的函数称为方法，和普通函数也没啥区别，但是它在内部使用了一个<font color="red"><code>this</code></font>关键字，这个东东是什么？

在一个方法内部，<font color="red"><code>this</code></font>是一个特殊变量，它始终指向当前对象，也就是<font color="red"><code>xiaoming</code></font>这个变量。所以，<font color="red"><code>this.birth</code></font>可以拿到<font color="red"><code>xiaoming</code></font>的<font color="red"><code>birth</code></font>属性。

让我们拆开写：

```javascript
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25, 正常结果
getAge(); // NaN
```

单独调用函数<font color="red"><code>getAge()</code></font>怎么返回了<font color="red"><code>NaN</code></font>？<font color="red"><code>请注意</code></font>，我们已经进入到了JavaScript的一个大坑里。

JavaScript的函数内部如果调用了<font color="red"><code>this</code></font>，那么这个<font color="red"><code>this</code></font>到底指向谁？

答案是，视情况而定！

如果以对象的方法形式调用，比如<font color="red"><code>xiaoming.age()</code></font>，该函数的<font color="red"><code>this</code></font>指向被调用的对象，也就是<font color="red"><code>xiaoming</code></font>，这是符合我们预期的。

如果单独调用函数，比如<font color="red"><code>getAge()</code></font>，此时，该函数的<font color="red"><code>this</code></font>指向全局对象，也就是<font color="red"><code>window</code></font>。

坑爹啊！

更坑爹的是，如果这么写：

```javascript
var fn = xiaoming.age; // 先拿到xiaoming的age函数
fn(); // NaN
```

也是不行的！要保证<font color="red"><code>this</code></font>指向正确，必须用<font color="red"><code>obj.xxx()</code></font>的形式调用！

由于这是一个巨大的设计错误，要想纠正可没那么简单。ECMA决定，在strict模式下让函数的<font color="red"><code>this</code></font>指向<font color="red"><code>undefined</code></font>，因此，在strict模式下，你会得到一个错误：

```javascript
'use strict';

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var y = new Date().getFullYear();
        return y - this.birth;
    }
};

var fn = xiaoming.age;
fn(); // Uncaught TypeError: Cannot read property 'birth' of undefined
```

这个决定只是让错误及时暴露出来，并没有解决<font color="red"><code>this</code></font>应该指向的正确位置。

有些时候，喜欢重构的你把方法重构了一下：

```javascript
'use strict';

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        function getAgeFromBirth() {
            var y = new Date().getFullYear();
            return y - this.birth;
        }
        return getAgeFromBirth();
    }
};

xiaoming.age(); // Uncaught TypeError: Cannot read property 'birth' of undefined
```

结果又报错了！原因是<font color="red"><code>this</code></font>指针只在<font color="red"><code>age</code></font>方法的函数内指向<font color="red"><code>xiaoming</code></font>，在函数内部定义的函数，<font color="red"><code>this</code></font>又指向<font color="red"><code>undefined</code></font>了！（在非strict模式下，它重新指向全局对象<font color="red"><code>window</code></font>！）

修复的办法也不是没有，我们用一个<font color="red"><code>that</code></font>变量首先捕获<font color="red"><code>this</code></font>：

```javascript
'use strict';

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var that = this; // 在方法内部一开始就捕获this
        function getAgeFromBirth() {
            var y = new Date().getFullYear();
            return y - that.birth; // 用that而不是this
        }
        return getAgeFromBirth();
    }
};

xiaoming.age(); // 25
```

用<font color="red"><code>var that = this</code></font>;，你就可以放心地在方法内部定义其他函数，而不是把所有语句都堆到一个方法中。

### apply
虽然在一个独立的函数调用中，根据是否是strict模式，<font color="red"><code>this</code></font>指向<font color="red"><code>undefined</code></font>或<font color="red"><code>window</code></font>，不过，我们还是可以控制<font color="red"><code>this</code></font>的指向的！

要指定函数的<font color="red"><code>this</code></font>指向哪个对象，可以用函数本身的<font color="red"><code>apply</code></font>方法，它接收两个参数，第一个参数就是需要绑定的<font color="red"><code>this</code></font>变量，第二个参数是<font color="red"><code>Array</code></font>，表示函数本身的参数。

用<font color="red"><code>apply</code></font>修复<font color="red"><code>getAge()</code></font>调用：

```javascript
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空
```

另一个与<font color="red"><code>apply()</code></font>类似的方法是<font color="red"><code>call()</code></font>，唯一区别是：

* <font color="red"><code>apply()</code></font>把参数打包成<font color="red"><code>Array</code></font>再传入；
* <font color="red"><code>call()</code></font>把参数按顺序传入。

比如调用<font color="red"><code>Math.max(3, 5, 4)</code></font>，分别用<font color="red"><code>apply()</code></font>和<font color="red"><code>call()</code></font>实现如下：

```javascript
Math.max.apply(null, [3, 5, 4]); // 5
Math.max.call(null, 3, 5, 4); // 5
```

对普通函数调用，我们通常把<font color="red"><code>this</code></font>绑定为<font color="red"><code>null</code></font>。

### 装饰器
利用<font color="red"><code>apply()</code></font>，我们还可以动态改变函数的行为。

JavaScript的所有对象都是动态的，即使内置的函数，我们也可以重新指向新的函数。

现在假定我们想统计一下代码一共调用了多少次<font color="red"><code>parseInt()</code></font>，可以把所有的调用都找出来，然后手动加上<font color="red"><code>count += 1</code></font>，不过这样做太傻了。最佳方案是用我们自己的函数替换掉默认的<font color="red"><code>parseInt()</code></font>：

```javascript
'use strict';

var count = 0;
var oldParseInt = parseInt; // 保存原函数

window.parseInt = function () {
    count += 1;
    return oldParseInt.apply(null, arguments); // 调用原函数
};
// 测试:
parseInt('10');
parseInt('20');
parseInt('30');
console.log('count = ' + count); // 3
```

<button class="run" onclick="(() => {
    const element = document.getElementById('decorator');
    try {
        'use strict';
        var count = 0;
        var oldParseInt = parseInt; // 保存原函数
        window.parseInt = function () {
            count += 1;
            return oldParseInt.apply(null, arguments); // 调用原函数
        };
        // 测试:
        parseInt('10');
        parseInt('20');
        parseInt('30');
        console.log('count = ' + count); // 3
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>count = ${count}</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="decorator" hidden></p>
