<link rel="stylesheet" href="../../../static/css/button.css"/>
<link rel="stylesheet" href="../../../static/css/console.css"/>

# class继承
---

在上面的章节中我们看到了JavaScript的对象模型是基于原型实现的，特点是简单，缺点是理解起来比传统的类－实例模型要困难，最大的缺点是继承的实现需要编写大量代码，并且需要正确实现原型链。

有没有更简单的写法？有！

新的关键字<font color="red"><code>class</code></font>从ES6开始正式被引入到JavaScript中。<font color="red"><code>class</code></font>的目的就是让定义类更简单。

我们先回顾用函数实现<font color="red"><code>Student</code></font>的方法：

```javascript
function Student(name) {
    this.name = name;
}

Student.prototype.hello = function () {
    alert('Hello, ' + this.name + '!');
}
```

如果用新的<font color="red"><code>class</code></font>关键字来编写<font color="red"><code>Student</code></font>，可以这样写：

```javascript
class Student {
    constructor(name) {
        this.name = name;
    }

    hello() {
        alert('Hello, ' + this.name + '!');
    }
}
```

比较一下就可以发现，<font color="red"><code>class</code></font>的定义包含了构造函数<font color="red"><code>constructor</code></font>和定义在原型对象上的函数<font color="red"><code>hello()</code></font>（注意没有<font color="red"><code>function</code></font>关键字），这样就避免了<font color="red"><code>Student.prototype.hello = function () {...}</code></font>这样分散的代码。

最后，创建一个<font color="red"><code>Student</code></font>对象代码和前面章节完全一样：

```javascript
var xiaoming = new Student('小明');
xiaoming.hello();
```

### class继承
用<font color="red"><code>class</code></font>定义对象的另一个巨大的好处是继承更方便了。想一想我们从<font color="red"><code>Student</code></font>派生一个<font color="red"><code>PrimaryStudent</code></font>需要编写的代码量。现在，原型继承的中间对象，原型对象的构造函数等等都不需要考虑了，直接通过<font color="red"><code>extends</code></font>来实现：

```javascript
class PrimaryStudent extends Student {
    constructor(name, grade) {
        super(name); // 记得用super调用父类的构造方法!
        this.grade = grade;
    }

    myGrade() {
        alert('I am at grade ' + this.grade);
    }
}
```

注意<font color="red"><code>PrimaryStudent</code></font>的定义也是<font color="red"><code>class</code></font>关键字实现的，而<font color="red"><code>extends</code></font>则表示原型链对象来自<font color="red"><code>Student</code></font>。子类的构造函数可能会与父类不太相同，例如，<font color="red"><code>PrimaryStudent</code></font>需要<font color="red"><code>name</code></font>和<font color="red"><code>grade</code></font>两个参数，并且需要通过<font color="red"><code>super(name)</code></font>来调用父类的构造函数，否则父类的<font color="red"><code>name</code></font>属性无法正常初始化。

<font color="red"><code>PrimaryStudent</code></font>已经自动获得了父类<font color="red"><code>Student</code></font>的<font color="red"><code>hello</code></font>方法，我们又在子类中定义了新的<font color="red"><code>myGrade</code></font>方法。

ES6引入的<font color="red"><code>class</code></font>和原有的JavaScript原型继承有什么区别呢？实际上它们没有任何区别，<font color="red"><code>class</code></font>的作用就是让JavaScript引擎去实现原来需要我们自己编写的原型链代码。简而言之，用<font color="red"><code>class</code></font>的好处就是极大地简化了原型链代码。

你一定会问，<font color="red"><code>class</code></font>这么好用，能不能现在就用上？

现在用还早了点，因为不是所有的主流浏览器都支持ES6的<font color="red"><code>class</code></font>。如果一定要现在就用上，就需要一个工具把<font color="red"><code>class</code></font>代码转换为传统的<font color="red"><code>prototype</code></font>代码，可以试试 [Babel](https://babeljs.io/) 这个工具。

### 练习
请利用<font color="red"><code>class</code></font>重新定义<font color="red"><code>Cat</code></font>，并让它从已有的<font color="red"><code>Animal</code></font>继承，然后新增一个方法<font color="red"><code>say()</code></font>，返回字符串<font color="red"><code>'Hello, xxx!'</code></font>：

```javascript
'use strict';

class Animal {
    constructor(name) {
        this.name = name;
    }
}
class Cat ???

// 测试:
var kitty = new Cat('Kitty');
var doraemon = new Cat('哆啦A梦');
if ((new Cat('x') instanceof Animal) && kitty && kitty.name === 'Kitty' && kitty.say && typeof kitty.say === 'function' && kitty.say() === 'Hello, Kitty!' && kitty.say === doraemon.say) {
    console.log('测试通过!');
} else {
    console.log('测试失败!');
}
```

<button class="run" onclick="(() => {
    const answer = `
class Cat extends Animal {
    constructor(name) {
        super(name);
    }
    say() {
        return \`Hello, \${this.name}!\`;
    }
}
`;
    alert(answer);
})();">Show Answer</button>
### 

这个练习需要浏览器支持ES6的<font color="red"><code>class</code></font>，如果遇到SyntaxError，则说明浏览器不支持<font color="red"><code>class</code></font>语法，请换一个最新的浏览器试试。
