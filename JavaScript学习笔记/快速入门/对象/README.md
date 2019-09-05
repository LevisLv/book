<link rel="stylesheet" href="../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../static/css/console.css"/>

# 对象
---

JavaScript的对象是一种无序的集合数据类型，它由若干键值对组成。

JavaScript的对象用于描述现实世界中的某个对象。例如，为了描述“小明”这个淘气的小朋友，我们可以用若干键值对来描述他：

```javascript
var xiaoming = {
    name: '小明',
    birth: 1990,
    school: 'No.1 Middle School',
    height: 1.70,
    weight: 65,
    score: null
};
```

JavaScript用一个<font color="red"><code>{...}</code></font>表示一个对象，键值对以<font color="red"><code>xxx: xxx</code></font>形式申明，用<font color="red"><code>,</code></font>隔开。注意，最后一个键值对不需要在末尾加<font color="red"><code>,</code></font>，如果加了，有的浏览器（如低版本的IE）将报错。

上述对象申明了一个<font color="red"><code>name</code></font>属性，值是<font color="red"><code>'小明'</code></font>，<font color="red"><code>birth</code></font>属性，值是<font color="red"><code>1990</code></font>，以及其他一些属性。最后，把这个对象赋值给变量<font color="red"><code>xiaoming</code></font>后，就可以通过变量<font color="red"><code>xiaoming</code></font>来获取小明的属性了：

```javascript
xiaoming.name; // '小明'
xiaoming.birth; // 1990
```

访问属性是通过.操作符完成的，但这要求属性名必须是一个有效的变量名。如果属性名包含特殊字符，就必须用<font color="red"><code>''</code></font>括起来：

```javascript
var xiaohong = {
    name: '小红',
    'middle-school': 'No.1 Middle School'
};
```

<font color="red"><code>xiaohong</code></font>的属性名<font color="red"><code>middle-school</code></font>不是一个有效的变量，就需要用<font color="red"><code>''</code></font>括起来。访问这个属性也无法使用.操作符，必须用<font color="red"><code>['xxx']</code></font>来访问：

```javascript
xiaohong['middle-school']; // 'No.1 Middle School'
xiaohong['name']; // '小红'
xiaohong.name; // '小红'
```

也可以用<font color="red"><code>xiaohong['name']</code></font>来访问<font color="red"><code>xiaohong</code></font>的<font color="red"><code>name</code></font>属性，不过<font color="red"><code>xiaohong.name</code></font>的写法更简洁。我们在编写JavaScript代码的时候，属性名尽量使用标准的变量名，这样就可以直接通过<font color="red"><code>object.prop</code></font>的形式访问一个属性了。

实际上JavaScript对象的所有属性都是字符串，不过属性对应的值可以是任意数据类型。

如果访问一个不存在的属性会返回什么呢？JavaScript规定，访问不存在的属性不报错，而是返回<font color="red"><code>undefined</code></font>：

```javascript
'use strict';

var xiaoming = {
    name: '小明'
};
console.log(xiaoming.name);
console.log(xiaoming.age); // undefined
```

<button class="run" onclick="(() => {
    const element = document.getElementById('undefined');
    try {
        'use strict';
        var xiaoming = {
            name: '小明'
        };
        console.log(xiaoming.name);
        console.log(xiaoming.age); // undefined
        element.classList.remove(['consoleError']);
        element.classList.add('consoleLog');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleLog'>小明<br>undefined</label>`;
    } catch (e) {
        element.classList.remove(['consoleLog']);
        element.classList.add('consoleError');
        element.removeAttribute('hidden');
        element.innerHTML = `<label class='consoleError'>${e}</label>`;
    }
})();">Run</button>
<p id="undefined" hidden></p>

由于JavaScript的对象是动态类型，你可以自由地给一个对象添加或删除属性：

```javascript
var xiaoming = {
    name: '小明'
};
xiaoming.age; // undefined
xiaoming.age = 18; // 新增一个age属性
xiaoming.age; // 18
delete xiaoming.age; // 删除age属性
xiaoming.age; // undefined
delete xiaoming['name']; // 删除name属性
xiaoming.name; // undefined
delete xiaoming.school; // 删除一个不存在的school属性也不会报错
```

如果我们要检测<font color="red"><code>xiaoming</code></font>是否拥有某一属性，可以用<font color="red"><code>in</code></font>操作符：

```javascript
var xiaoming = {
    name: '小明',
    birth: 1990,
    school: 'No.1 Middle School',
    height: 1.70,
    weight: 65,
    score: null
};
'name' in xiaoming; // true
'grade' in xiaoming; // false
```

不过要小心，如果<font color="red"><code>in</code></font>判断一个属性存在，这个属性不一定是<font color="red"><code>xiaoming</code></font>的，它可能是<font color="red"><code>xiaoming</code></font>继承得到的：

```javascript
'toString' in xiaoming; // true
```

因为<font color="red"><code>toString</code></font>定义在<font color="red"><code>object</code></font>对象中，而所有对象最终都会在原型链上指向<font color="red"><code>object</code></font>，所以<font color="red"><code>xiaoming</code></font>也拥有<font color="red"><code>toString</code></font>属性。

要判断一个属性是否是<font color="red"><code>xiaoming</code></font>自身拥有的，而不是继承得到的，可以用<font color="red"><code>hasOwnProperty()</code></font>方法：

```javascript
var xiaoming = {
    name: '小明'
};
xiaoming.hasOwnProperty('name'); // true
xiaoming.hasOwnProperty('toString'); // false
```
