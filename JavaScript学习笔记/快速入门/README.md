# 快速入门
---

JavaScript代码可以直接嵌在网页的任何地方，不过通常我们都把JavaScript代码放到<font color="red"><code>&lt;head&gt;</code></font>中：

```
<html>
<head>
  <script>
    alert('Hello, world');
  </script>
</head>
<body>
  ...
</body>
</html>
```

由<font color="red"><code>&lt;script&gt;...&lt;/script&gt;</code></font>包含的代码就是JavaScript代码，它将直接被浏览器执行。

第二种方法是把JavaScript代码放到一个单独的<font color="red"><code>.js</code></font>文件，然后在HTML中通过<font color="red"><code>&lt;script src="..."&gt;&lt;/script&gt;</code></font>引入这个文件：

```
<html>
<head>
  <script src="/static/js/abc.js"></script>
</head>
<body>
  ...
</body>
</html>
```

这样，<font color="red"><code>/static/js/abc.js</code></font>就会被浏览器执行。

把JavaScript代码放入一个单独的<font color="red"><code>.js</code></font>文件中更利于维护代码，并且多个页面可以各自引用同一份<font color="red"><code>.js</code></font>文件。

可以在同一个页面中引入多个<font color="red"><code>.js</code></font>文件，还可以在页面中多次编写<font color="red"><code>&lt;script&gt; js代码... &lt;/script&gt;</code></font>，浏览器按照顺序依次执行。

有些时候你会看到<font color="red"><code>&lt;script&gt;</code></font>标签还设置了一个<font color="red"><code>type</code></font>属性：

```
<script type="text/javascript">
  ...
</script>
```

但这是没有必要的，因为默认的type就是JavaScript，所以不必显式地把type指定为JavaScript。

### 如何编写JavaScript
可以用任何文本编辑器来编写JavaScript代码。这里我们推荐以下几种文本编辑器：

#### Visual Studio Code
微软出的[Visual Studio Code](https://code.visualstudio.com/)，可以看做迷你版Visual Studio，免费！跨平台！内置JavaScript支持，强烈推荐使用！

#### Sublime Text
[Sublime Text](https://www.sublimetext.com/)是一个好用的文本编辑器，免费，但不注册会不定时弹出提示框。

#### Notepad++
[Notepad++](https://notepad-plus-plus.org/)也是免费的文本编辑器，但仅限Windows下使用。

<font color="red">注意</font>：不可以用Word或写字板来编写JavaScript或HTML，因为带格式的文本保存后不是<font color="red">纯文本文件</font>，无法被浏览器正常读取。也尽量不要用记事本编写，它会自作聪明地在保存UTF-8格式文本时添加BOM头。

### 如何运行JavaScript
要让浏览器运行JavaScript，必须先有一个HTML页面，在HTML页面中引入JavaScript，然后，让浏览器加载该HTML页面，就可以执行JavaScript代码。

你也许会想，直接在我的硬盘上创建好HTML和JavaScript文件，然后用浏览器打开，不就可以看到效果了吗？

这种方式运行部分JavaScript代码没有问题，但由于浏览器的安全限制，以<font color="red"><code>file://</code></font>开头的地址无法执行如联网等JavaScript代码，最终，你还是需要架设一个Web服务器，然后以<font color="red"><code>http://</code></font>开头的地址来正常执行所有JavaScript代码。

不过，开始学习阶段，你无须关心如何搭建开发环境的问题，我们提供在页面输入JavaScript代码并直接运行的功能，让你专注于JavaScript的学习。

### 调试
俗话说得好，“工欲善其事，必先利其器。”，写JavaScript的时候，如果期望显示<font color="red"><code>ABC</code></font>，结果却显示<font color="red"><code>XYZ</code></font>，到底代码哪里出了问题？不要抓狂，也不要泄气，作为小白，要坚信：JavaScript本身没有问题，浏览器执行也没有问题，有问题的一定是我的代码。

如何找出问题代码？这就需要调试。

怎么在浏览器中调试JavaScript代码呢？

首先，你需要安装Google Chrome浏览器，Chrome浏览器对开发者非常友好，可以让你方便地调试JavaScript代码。从这里[下载Chrome浏览器](https://www.google.com/chrome/browser/desktop/index.html?system=true&standalone=1)。打开网页出问题的童鞋请移步[国内镜像](http://pan.baidu.com/s/1qWMaZSg#path=%252Fpub%252Fchrome)。

安装后，随便打开一个网页，然后点击菜单“查看(View)”-“开发者(Developer)”-“开发者工具(Developer Tools)”，浏览器窗口就会一分为二，下方就是开发者工具：

![](https://www.liaoxuefeng.com/files/attachments/1025934180217184/l)

先点击“控制台(Console)“，在这个面板里可以直接输入JavaScript代码，按回车后执行。

要查看一个变量的内容，在Console中输入<font color="red"><code>console.log(a);</code></font>，回车后显示的值就是变量的内容。

关闭Console请点击右上角的“×”按钮。请熟练掌握Console的使用方法，在编写JavaScript代码时，经常需要在Console运行测试代码。

如果你对自己还有更高的要求，可以研究开发者工具的“源码(Sources)”，掌握断点、单步执行等高级调试技巧。

### 练习
打开[新浪首页](http://www.sina.com.cn/)，然后查看页面源代码，找一找引入的JavaScript文件和直接编写在页面中的JavaScript代码。然后在Chrome中打开开发者工具，在控制台输入<font color="red"><code>console.log('Hello');</code></font>，回车查看JavaScript代码执行结果。
