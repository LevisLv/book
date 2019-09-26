# 面向对象编程
---

JavaScript的所有数据都可以看成对象，那是不是我们已经在使用面向对象编程了呢？

当然不是。如果我们只使用<font color="red"><code>Number</code></font>、<font color="red"><code>Array</code></font>、<font color="red"><code>string</code></font>以及基本的<font color="red"><code>{...}</code></font>定义的对象，还无法发挥出面向对象编程的威力。

JavaScript的面向对象编程和大多数其他语言如Java、C#的面向对象编程都不太一样。如果你熟悉Java或C#，很好，你一定明白面向对象的两个基本概念：
1. 类：类是对象的类型模板，例如，定义<font color="red"><code>Student</code></font>类来表示学生，类本身是一种类型，<font color="red"><code>Student</code></font>表示学生类型，但不表示任何具体的某个学生；
2. 实例：实例是根据类创建的对象，例如，根据<font color="red"><code>Student</code></font>类可以创建出<font color="red"><code>xiaoming</code></font>、<font color="red"><code>xiaohong</code></font>、<font color="red"><code>xiaojun</code></font>等多个实例，每个实例表示一个具体的学生，他们全都属于<font color="red"><code>Student</code></font>类型。

所以，类和实例是大多数面向对象编程语言的基本概念。

不过，在JavaScript中，这个概念需要改一改。JavaScript不区分类和实例的概念，而是通过原型（prototype）来实现面向对象编程。

原型是指当我们想要创建<font color="red"><code>xiaoming</code></font>这个具体的学生时，我们并没有一个<font color="red"><code>Student</code></font>类型可用。那怎么办？恰好有这么一个现成的对象：

```javascript
var robot = {
    name: 'Robot',
    height: 1.6,
    run: function () {
        console.log(this.name + ' is running...');
    }
};
```

我们看这个<font color="red"><code>robot</code></font>对象有名字，有身高，还会跑，有点像小明，干脆就根据它来“创建”小明得了！

于是我们把它改名为<font color="red"><code>Student</code></font>，然后创建出<font color="red"><code>xiaoming</code></font>：

```javascript
var Student = {
    name: 'Robot',
    height: 1.2,
    run: function () {
        console.log(this.name + ' is running...');
    }
};

var xiaoming = {
    name: '小明'
};

xiaoming.__proto__ = Student;
```

注意最后一行代码把<font color="red"><code>xiaoming</code></font>的原型指向了对象<font color="red"><code>Student</code></font>，看上去<font color="red"><code>xiaoming</code></font>仿佛是从<font color="red"><code>Student</code></font>继承下来的：

```javascript
xiaoming.name; // '小明'
xiaoming.run(); // 小明 is running...
```

<font color="red"><code>xiaoming</code></font>有自己的<font color="red"><code>name</code></font>属性，但并没有定义<font color="red"><code>run()</code></font>方法。不过，由于小明是从<font color="red"><code>Student</code></font>继承而来，只要<font color="red"><code>Student</code></font>有<font color="red"><code>run()</code></font>方法，<font color="red"><code>xiaoming</code></font>也可以调用：

![](https://www.liaoxuefeng.com/files/attachments/1024674367146144/l)

JavaScript的原型链和Java的Class区别就在，它没有“Class”的概念，所有对象都是实例，所谓继承关系不过是把一个对象的原型指向另一个对象而已。

如果你把<font color="red"><code>xiaoming</code></font>的原型指向其他对象：

```javascript
var Bird = {
    fly: function () {
        console.log(this.name + ' is flying...');
    }
};

xiaoming.__proto__ = Bird;
```

现在<font color="red"><code>xiaoming</code></font>已经无法<font color="red"><code>run()</code></font>了，他已经变成了一只鸟：

```javascript
xiaoming.fly(); // 小明 is flying...
```

在JavaScrip代码运行时期，你可以把<font color="red"><code>xiaoming</code></font>从<font color="red"><code>Student</code></font>变成<font color="red"><code>Bird</code></font>，或者变成任何对象。

<font color="red"><code>请注意</code></font>，上述代码仅用于演示目的。在编写JavaScript代码时，不要直接用<font color="red"><code>obj.__proto__</code></font>去改变一个对象的原型，并且，低版本的IE也无法使用<font color="red"><code>__proto__</code></font>。<font color="red"><code>Object.create()</code></font>方法可以传入一个原型对象，并创建一个基于该原型的新对象，但是新对象什么属性都没有，因此，我们可以编写一个函数来创建<font color="red"><code>xiaoming</code></font>：

```javascript
// 原型对象:
var Student = {
    name: 'Robot',
    height: 1.2,
    run: function () {
        console.log(this.name + ' is running...');
    }
};

function createStudent(name) {
    // 基于Student原型创建一个新对象:
    var s = Object.create(Student);
    // 初始化新对象:
    s.name = name;
    return s;
}

var xiaoming = createStudent('小明');
xiaoming.run(); // 小明 is running...
xiaoming.__proto__ === Student; // true
```

这是创建原型继承的一种方法，JavaScript还有其他方法来创建对象，我们在后面会一一讲到。
