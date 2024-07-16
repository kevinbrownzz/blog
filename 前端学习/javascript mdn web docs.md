# javascript mdn web docs

### [1.HTML 文档详解](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/HTML_basics#html_文档详解)

以上介绍了一些基本的 HTML 元素，但孤木不成林。现在来看看单个元素如何彼此协同构成一个完整的 HTML 页面。回顾 [文件处理](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/Dealing_with_files) 小节中创建的 `index.html` 示例：

HTMLCopy to Clipboard

```
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>My test page</title>
  </head>
  <body>
    <img src="images/firefox-icon.png" alt="My test image" />
  </body>
</html>
```

这里有：

- `<!DOCTYPE html>`——[文档类型](https://developer.mozilla.org/zh-CN/docs/Glossary/Doctype)。这是必不可少的开头。混沌初分，HTML 尚在襁褓（大约是 1991/92 年）之时，这个元素用来关联 HTML 编写规范，以供自动查错等功能所用。而在当今，它作用有限，可以说仅用于保证文档正常读取。现在知道这些就足够了。
- `<html></html>`——[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/html) 元素。该元素包含整个页面的所有内容，有时候也称作根元素。里面也包含了 `lang` 属性，写明了页面的主要语种。
- `<head></head>`——该元素。所有那些你加到页面中，且*不*向用户展示的页面内容，都以这个元素为容器。其中包含诸如提供给搜索引擎的[关键字](https://developer.mozilla.org/zh-CN/docs/Glossary/Keyword)和页面描述、用于设置页面样式的 CSS、字符集声明等等。
- `<meta charset="utf-8">`——该元素指明你的文档使用 UTF-8 字符编码，UTF-8 包括世界绝大多数书写语言的字符。它基本上可以处理任何文本内容。以它为编码还可以避免以后出现某些问题，没有理由再选用其他编码。
- `<meta name="viewport" content="width=device-width">`——[视口元素](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Viewport_concepts#移动设备的视口)可以确保页面以视口宽度进行渲染，避免移动端浏览器上因页面过宽导致缩放。
- `<title></title>`—— 该元素设置页面的标题，显示在浏览器标签页上，也作为收藏网页的描述文字。
- `<body></body>`——该元素包含期望让用户在访问页面时看到的*全部*内容，包括文本、图像、视频、游戏、可播放的音轨或其他内容。



## 2.[标记文本](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/HTML_basics#标记文本)

本段包含了一些最常用的文本标记 HTML 元素。

### [标题（Heading）](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/HTML_basics#标题（heading）)

标题元素可用于指定内容的标题和子标题。就像一本书的书名、每章的大标题、小标题，等。HTML 文档也是一样。HTML 包括六个级别的标题， <h1> -- <h6>，一般最多用到 3-4 级标题。

### [段落（Paragraph）](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/HTML_basics#段落（paragraph）)

如上文所讲，<p>元素是用来指定段落的。通常用于指定常规的文本内容：

```
<p>这是一个段落</p>
```

### [列表（List）](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/HTML_basics#列表（list）)

Web 上的许多内容都是列表，HTML 有一些特别的列表元素。标记列表通常包括至少两个元素。最常用的列表类型为：

1. **无序列表**（Unordered List）中项目的顺序并不重要，就像购物列表。用一个 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/ul) 元素包围。
2. **有序列表**（Ordered List）中项目的顺序很重要，就像烹调指南。用一个 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/ol) 元素包围。

列表的每个项目用一个列表项目（List Item）元素 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/li) 包围。

## [链接](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/HTML_basics#链接)

链接非常重要 — 它们赋予 Web 网络属性。要植入一个链接，我们需要使用一个简单的元素 — [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a) — a 是 "anchor" （锚）的缩写。要将一些文本添加到链接中.

1. ```
       <a href="https://www.mozilla.org/zh-CN/about/manifesto/">
           Mozilla Manifesto
       </a>
   ```



### 3. CSS基础

CSS（Cascading Style Sheets，层叠样式表）是为 web 内容添加样式的代码。本节将介绍 CSS 的基础知识，并解答像这样的问题：怎样将文本设置为红色？怎样将内容显示在屏幕的特定位置？怎样用背景图片或颜色来装饰网页？

## [什么是 CSS？](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/CSS_basics#什么是_css？)

和 HTML 类似，CSS 也不是真正的编程语言，甚至不是标记语言。**CSS 是一门样式表语言**，这也就是说人们可以用它来选择性地为 HTML 元素添加样式。举例来说，以下 CSS 代码选择了所有的段落文字，并将它们设置为红色。

```
p {
  color: red;
}
```

文件命名为 `style.css` 并保存到 `styles` 文件夹下。

需要将上述 CSS 样式应用到你的 HTML 文档中。否则，这些样式不会改变 HTML 的外观。

1. 打开

    

   ```
   index.html
   ```

   粘贴

   ```
   <link href="styles/style.css" rel="stylesheet" />
   ```

![](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/CSS_basics/css-declaration-small.png)

整个结构称为**规则集**（*规则集*通常简称*规则*），注意各个部分的名称：

- [选择器（Selector）](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/CSS_basics#选择器（selector）)

  HTML 元素的名称位于规则集开始。它选择了一个或多个需要添加样式的元素（在这个例子中就是 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/p) 元素）。要给不同元素添加样式，只需要更改选择器。

- [声明（Declaration）](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/CSS_basics#声明（declaration）)

  一个单独的规则，如 `color: red;` 用来指定添加样式元素的**属性**。

- [属性（Properties）](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/CSS_basics#属性（properties）)

  改变 HTML 元素样式的途径（本例中 `color` 就是<p> 元素的属性）。CSS 中，由编写人员决定修改哪个属性以改变规则。

- [属性的值（Property value）](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/CSS_basics#属性的值（property_value）)

  在属性的右边，冒号后面即**属性的值**，它从指定属性的众多外观中选择一个值（我们除了 `red` 之外还有很多属性值可以用于 `color` ）。

注意其他重要的语法：

- 除了选择器部分，每个规则集都应该包含在成对的大括号里（`{}`）。
- 在每个声明里要用冒号（`:`）将属性与属性值分隔开。
- 在每个规则集里要用分号（`;`）将各个声明分隔开。

如果要同时修改多个属性，只需要将它们用分号隔开，就像这样：

CSSCopy to Clipboard

```javascript
p {
  color: red;
  width: 500px;
  border: 1px solid black;
}
```

### [选择多个元素](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/CSS_basics#选择多个元素)

也可以选择多种类型的元素并为它们添加一组相同的样式。将不同的选择器用逗号分开。例如：

CSSCopy to Clipboard

```
p,
li,
h1 {
  color: red;
}
```

### [不同类型的选择器](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/CSS_basics#不同类型的选择器)

选择器有许多不同的类型。上面只介绍了**元素选择器**，用来选择 HTML 文档中给定的元素。但是选择操作可以更加具体。下面是一些常用的选择器类型：

| 选择器名称                           | 选择的内容                                                   | 示例                                                         |
| :----------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 元素选择器（也称作标签或类型选择器） | 所有指定类型的 HTML 元素                                     | `p` 选择 `<p>`                                               |
| ID 选择器                            | 具有特定 ID 的元素。单一 HTML 页面中，每个 ID 只对应一个元素，一个元素只对应一个 ID | `#my-id` 选择 `<p id="my-id">` 或 `<a id="my-id">`           |
| 类选择器                             | 具有特定类的元素。单一页面中，一个类可以有多个实例           | `.my-class` 选择 `<p class="my-class">` 和 `<a class="my-class">` |
| 属性选择器                           | 拥有特定属性的元素                                           | `img[src]` 选择 `<img src="myimage.png">` 但不是 `<img>`     |
| 伪类选择器                           | 特定状态下的特定元素（比如鼠标指针悬停于链接之上）           | `a:hover` 选择仅在鼠标指针悬停在链接上时的 `<a>` 元素        |

### 4.javascripts

### [变量（Variable）](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/JavaScript_basics#变量（variable）)

[变量](https://developer.mozilla.org/zh-CN/docs/Glossary/Variable) 是存储值的容器。要声明一个变量，先输入关键字 `let` 或 `var`，然后输入合适的名称：

JSCopy to Clipboard

```
let myVariable;
```

变量定义后可以进行赋值：

JSCopy to Clipboard

```
myVariable = "李雷";
```

也可以将定义、赋值操作写在同一行：

JSCopy to Clipboard

```
let myVariable = "李雷";
```

注意变量可以有不同的 [数据类型](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Data_structures) ：

| 变量                                                         | 解释                                                         | 示例                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| [String](https://developer.mozilla.org/zh-CN/docs/Glossary/String) | 字符串（一串文本）：字符串的值必须用引号（单双均可，必须成对）括起来。 | `let myVariable = '李雷';`                                   |
| [Number](https://developer.mozilla.org/zh-CN/docs/Glossary/Number) | 数字：无需引号。                                             | `let myVariable = 10;`                                       |
| [Boolean](https://developer.mozilla.org/zh-CN/docs/Glossary/Boolean) | 布尔值（真 / 假）： `true`/`false` 是 JS 里的特殊关键字，无需引号。 | `let myVariable = true;`                                     |
| [Array](https://developer.mozilla.org/zh-CN/docs/Glossary/Array) | 数组：用于在单一引用中存储多个值的结构。                     | `let myVariable = [1, '李雷', '韩梅梅', 10];` 元素引用方法：`myVariable[0]`, `myVariable[1]` …… |
| [Object](https://developer.mozilla.org/zh-CN/docs/Glossary/Object) | 对象：JavaScript 里一切皆对象，一切皆可储存在变量里。这一点要牢记于心。 | `let myVariable = document.querySelector('h1');` 以及上面所有示例都是对象。注意变量可以有不同的 [数据类型](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Data_structures) ： |

### [运算符](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/JavaScript_basics#运算符)

[运算符](https://developer.mozilla.org/zh-CN/docs/Glossary/Operator) 是一类数学符号，可以根据两个值（或变量）产生结果。以下表格中介绍了一些最简单的运算符，可以在浏览器控制台里尝试一下后面的示例。

**备注：** 这里说“根据**两个**值（或变量）产生结果”是不严谨的，计算两个变量的运算符称为“二元运算符”，还有一元运算符和三元运算符，下表中的“取非”就是一元运算符。

| 运算符     | 解释                                                         | 符号          | 示例                                                         |
| :--------- | :----------------------------------------------------------- | :------------ | :----------------------------------------------------------- |
| 加         | 将两个数字相加，或拼接两个字符串。                           | `+`           | `6 + 9;"Hello " + "world!";`                                 |
| 减、乘、除 | 这些运算符操作与基础算术一致。只是乘法写作星号，除法写作斜杠。 | `-`, `*`, `/` | `9 - 3;8 * 2; //乘法在 JS 中是一个星号9 / 3;`                |
| 赋值运算符 | 为变量赋值（你之前已经见过这个符号了）                       | `=`           | `let myVariable = '李雷';`                                   |
| 等于       | 测试两个值是否相等，并返回一个 `true`/`false` （布尔）值。   | `===`         | `let myVariable = 3;myVariable === 4; // false`              |
| 不等于     | 和等于运算符相反，测试两个值是否不相等，并返回一个 `true`/`false` （布尔）值。 | `!==`         | `let myVariable = 3;myVariable !== 3; // false`              |
| 取非       | 返回逻辑相反的值，比如当前值为真，则返回 `false`。           | `!`           | 原式为真，但经取非后值为 `false`： `let myVariable = 3;!(myVariable === 3); // false` |

### [函数（Function）](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/JavaScript_basics#函数（function）)

[函数](https://developer.mozilla.org/zh-CN/docs/Glossary/Function) 用来封装可复用的功能。如果没有函数，一段特定的操作过程用几次就要重复写几次，而使用函数则只需写下函数名和一些简短的信息。之前已经涉及过一些函数，比如：

```
let myVariable = document.querySelector("h1");
```

```
alert("hello!");
```

`document.querySelector` 和 `alert` 是浏览器内置的函数，随时可用。

如果代码中有一个类似变量名后加小括号 `()` 的东西，很可能就是一个函数。函数通常包括[参数](https://developer.mozilla.org/zh-CN/docs/Glossary/Argument)，参数中保存着一些必要的数据。它们位于括号内部，多个参数之间用逗号分开。

比如， `alert()` 函数在浏览器窗口内弹出一个警告框，还应为其提供一个字符串参数，以告诉它警告框里要显示的内容。

好消息是：人人都能定义自己的函数。下面的示例是为两个参数进行乘法运算的函数：

```
function multiply(num1, num2) {
  let result = num1 * num2;
  return result;
}
```

## [完善示例网页](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/JavaScript_basics#完善示例网页)

现在你已经具备了一些 JavaScript 基础，下面来为示例网页添加一些更酷的特性。

### [添加一个图像切换器](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/JavaScript_basics#添加一个图像切换器)

这里将用新的 DOM API 为网页添加另一张图片，并用 JavaScript 使图片在点击时进行切换。

1. 首先，找到另一张你想要在你的页面上展示的图片，且尺寸与第一张图片尽可能相同。

2. 将这张图片储存在你的`images`目录下。

3. 将图片重命名为'firefox2.png'（去掉引号）。

4. 打开

   ```
   let myImage = document.querySelector("img");
   
   myImage.onclick = function () {
     let mySrc = myImage.getAttribute("src");
     if (mySrc === "images/firefox-icon.png") {
       myImage.setAttribute("src", "images/firefox2.png");
     } else {
       myImage.setAttribute("src", "images/firefox-icon.png");
     }
   };
   ```

5. 保存所有文件并用浏览器打开 `index.html` 。点击图片可以发现它能够切换了！

## [到底发生了什么？](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/How_the_Web_works#到底发生了什么？)

当你在浏览器里输入一个网址时（在我们的例子里就是走向商店的路上时）：

1. 浏览器在域名系统（DNS）服务器上找出存放网页的服务器的实际地址（找出商店的位置）。
2. 浏览器发送 HTTP 请求信息到服务器来请拷贝一份网页到客户端（你走到商店并下订单）。这条消息，包括其他所有在客户端和服务器之间传递的数据都是通过互联网使用 TCP/IP 协议传输的。
3. 服务器同意客户端的请求后，会返回一个“200 OK”信息，意味着“你可以查看这个网页，给你～”，然后开始将网页的文件以数据包的形式传输到浏览器（商店给你商品，你将商品带回家）。
4. 浏览器将数据包聚集成完整的网页然后将网页呈现给你（商品到了你的门口——新东西，好棒！）。

## [解析组成文件的顺序](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/How_the_Web_works#解析组成文件的顺序)

当浏览器向服务器发送请求获取 HTML 文件时，HTML 文件通常包含 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/link) 和 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/script) 元素，这些元素分别指向了外部的 [CSS](https://developer.mozilla.org/zh-CN/docs/Learn/CSS) 样式表文件和 [JavaScript](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript) 脚本文件。了解这些文件被[浏览器解析](https://developer.mozilla.org/zh-CN/docs/Web/Performance/How_browsers_work#解析)的顺序是很重要的：

- 浏览器首先解析 HTML 文件，并从中识别出所有的 `<link>` 和 `<script>` 元素，获取它们指向的外部文件的链接。
- 继续解析 HTML 文件的同时，浏览器根据外部文件的链接向服务器发送请求，获取并解析 CSS 文件和 JavaScript 脚本文件。
- 接着浏览器会给解析后的 HTML 文件生成一个 [DOM](https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model) 树（在内存中），会给解析后的 CSS 文件生成一个 [CSSOM](https://developer.mozilla.org/zh-CN/docs/Glossary/CSSOM) 树（在内存中），并且会[编译和执行](https://developer.mozilla.org/zh-CN/docs/Web/Performance/How_browsers_work#其他过程)解析后的 JavaScript 脚本文件。
- 伴随着构建 DOM 树、应用 CSSOM 树的样式、以及执行 JavaScript 脚本文件，浏览器会在屏幕上绘制出网页的界面；用户看到网页界面也就可以跟网页进行交互了。

## [链接的解析](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Creating_hyperlinks#链接的解析)

通过将文本或其他内容包裹在 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a) 元素内，并给它一个包含网址的 [`href`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a#href) 属性（也称为**超文本引用**或**目标**，它将包含一个网址）来创建一个基本链接。

```
<p>
  我创建了一个指向
  <a href="https://www.mozilla.org/zh-CN/">Mozilla 主页</a>的链接。
</p>
```

### [文档片段](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Creating_hyperlinks#文档片段)

超链接除了可以链接到文档外，也可以链接到 HTML 文档的特定部分（被称为**文档片段**）。要做到这一点，你必须首先给要链接到的元素分配一个 [`id`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes#id) 属性。通常情况下，链接到一个特定的标题是有意义的，这看起来就像下面这样：

```
<h2 id="Mailing_address">邮寄地址</h2>
```

为了链接到那个特定的 `id`，要将它放在 URL 的末尾，并在前面包含井号（`#`），例如：

```
<p>
  要提供意见和建议，请将信件邮寄至<a href="contacts.html#Mailing_address"
    >我们的地址</a
  >。
</p>
```

你甚至可以在同一份文档下，通过链接文档片段，来链接到*当前文档的另一部分*：

```
<p>本页面底部可以找到<a href="#Mailing_address">公司邮寄地址</a>。</p>
```

## [HTML 布局元素细节](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure#html_布局元素细节)

理解所有 HTML 区段元素具体含义是很有益处的，这一点将随着个人 web 开发经验的逐渐丰富日趋显现。更多细节请查阅 [HTML 元素参考](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element)。现在，你只需要理解以下主要元素的意义：

- main存放每个页面独有的内容。每个页面上只能用一次 `<main>`，且直接位于 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/body) 中。最好不要把它嵌套进其他元素。
- article包围的内容即一篇文章，与页面其他部分无关（比如一篇博文）。
- section与 `<article>` 类似，但 `<section>` 更适用于组织页面使其按功能（比如迷你地图、一组文章标题和摘要）分块。一般的最佳用法是：以 [标题](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Howto/Set_up_a_proper_title_hierarchy) 作为开头；也可以把一篇 `<article>` 分成若干部分并分别置于不同的 `<section>` 中，也可以把一个区段 `<section>` 分成若干部分并分别置于不同的 `<article>` 中，取决于上下文。
- aside包含一些间接信息（术语条目、作者简介、相关链接，等等）。
- 
- nav 包含页面主导航功能。其中不应包含二级链接等内容。
- footer包含了页面的页脚部分。

### [无语义元素](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure#无语义元素)

有时你会发现，对于一些要组织的项目或要包装的内容，现有的语义元素均不能很好对应。有时候你可能只想将一组元素作为一个单独的实体来修饰来响应单一的用 [CSS](https://developer.mozilla.org/zh-CN/docs/Glossary/CSS) 或 [JavaScript](https://developer.mozilla.org/zh-CN/docs/Glossary/JavaScript)。为了应对这种情况，HTML 提供了div 和span元素。应配合使用 [`class`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes#class) 属性提供一些标签，使这些元素能易于查询。

span 是一个内联的（inline）无语义元素，最好只用于无法找到更好的语义元素来包含内容时，或者不想增加特定的含义时。

### [换行与水平分割线](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure#换行与水平分割线)

有时会用到br 和 hr两个元素，需要介绍一下。

#### br ：换行元素

`<br>` 可在段落中进行换行；`<br>` 是唯一能够生成多个短行结构（例如邮寄地址或诗歌）的元素。比如：

```
<p>
  从前有个人叫小高<br />
  他说写 HTML 感觉最好<br />
  但他写的代码结构语义一团糟<br />
  他写的标签谁也懂不了。
</p>
```

#### hr ：主题性中断元素

`<hr>` 元素在文档中生成一条水平分割线，表示文本中主题的变化（例如话题或场景的改变）。一般就是一条水平的直线。

## [怎样将图片放到网页上？](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/Images_in_HTML#怎样将图片放到网页上？)

要想在网页上放置简单的图像，我们需要使用 img 元素。这个元素是[空元素](https://developer.mozilla.org/zh-CN/docs/Glossary/Void_element)（即无法包含任何子内容和结束标签），它需要两个属性才能起作用：`src` 和 `alt`。`src` 属性包含一个 URL，该 URL 指向要嵌入页面的图像。`src` 属性可以是相对 URL 或绝对 URL，这与

a 元素的 `href` 属性类似。如果没有 `src` 属性，`img` 元素就没有图像可加载。

**备注：** 为了更容易理解下面的内容，建议你阅读 [URL 和路径简明入门](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Creating_hyperlinks#统一资源定位符（url）与路径（path）快速入门)来复习一下相对和绝对 URL 的概念。

例如，如果你的图像叫做 `dinosaur.jpg`，并且它位于与 HTML 页面相同的目录中，你可以这样嵌入图像：

```
<img src="dinosaur.jpg" alt="恐龙" />
```

如果图像在名为 `images` 的子目录中，该子目录位于与 HTML 页面相同的目录中，你可以这样嵌入它：

```
<img src="images/dinosaur.jpg" alt="恐龙" />
```

以此类推。

**备注：** 搜索引擎还会读取图像文件名并将其计入 SEO。因此，你应该为图像起一个描述性的文件名；`dinosaur.jpg` 比 `img835.png` 更好。

你也可以使用图像的绝对 URL 进行嵌入，例如：

```
<img src="https://www.example.com/images/dinosaur.jpg" alt="恐龙" />
```

然而，不建议使用绝对 URL 进行链接。你需要托管你想要在网站上使用的图像，在比较简单的情况下，通常我们会把网站的图像保存在与 HTML 相同的服务器上。此外，从维护的角度来说，使用相对 URL 比绝对 URL 更有效率（当你将网站迁移到不同的域名时，你不需要更新所有 URL，使其包含新域名）。在更高级的设置中，你可能会使用[内容分发网络（CDN）](https://developer.mozilla.org/zh-CN/docs/Glossary/CDN)来传递图像。

### [备选文本](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/Images_in_HTML#备选文本)

我们接下来要看的属性是 `alt`。它的值应该是图片的文本描述，用于在图片无法显示或者因为网速慢而加载缓慢的情况下使用。例如，我们可以把上面的代码修改成这样： 

# 视频和音频内容

现在我们已经可以轻松地为网页添加简单的图像，下一步我们开始为 HTML 文档添加音频和视频播放器。在这篇文章中，我们会使用video 和 audio 元素来完成这件事；然后我们还会了解如何为视频添加标题/字幕。

### video [ 元素](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/Video_and_audio_content#video_元素)

你可以借助video 元素来轻松地嵌入视频。一个简单的例子如下：

```
<video src="rabbit320.webm" controls>
  <p>
    你的浏览器不支持 HTML 视频。可点击<a href="rabbit320.mp4">此链接</a>观看。
  </p>
</video>
```

当中值得注意的有：

当中值得注意的有：

- [`src`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video#src)

  同 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img) 元素的使用方式相同，`src`（来源）属性指向你想要嵌入到网页中的视频资源，它们的运作方式完全相同。

- [`controls`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video#controls)

  用户应当能够控制视频和音频的播放（这对于患有[癫痫](https://zh.wikipedia.org/wiki/癫痫#病因)的人来说尤为重要）。你必须使用 `controls` 属性来让视频或音频包含浏览器自带的控制界面，或者使用适当的 [JavaScript API](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLMediaElement) 构建自己的界面。至少，界面必须包括启动和停止媒体以及调整音量的方法。

- video 元素内的段落

  这个叫做**后备内容**，当浏览器不支持 `<video>` 元素的时候，就会显示这段内容，借此我们能够对旧的浏览器提供回退。你可以添加任何后备内容，在这个例子中我们提供了一个指向这个视频文件的链接，从而使用户至少可以访问到这个文件，而不会局限于浏览器的支持。

# CSS

## [样式化 HTML 元素](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/First_steps/Getting_started#样式化_html_元素)

通过上一节将大标题变成红色的例子，我们已经展示了我们可以选中并且样式化 HTML 元素。我们通过触发元素选择器实现这一点——元素选择器，即直接匹配 HTML 元素的选择器。例如，若要样式化一个文档中所有的段落，只需使用选择器 `p`。若要将所有段落变成绿色，你可以利用如下方式：

CSSCopy to Clipboard

```
p {
  color: green;
}
```

用逗号将不同选择器隔开，即可一次使用多个选择器。譬如，若要将所有段落与列表变成绿色，只需：

CSSCopy to Clipboard

```
p,
li {
  color: green;
}
```

举个例子吧，咱们把 [class 属性](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/class)加到表里面第二个对象。你的列表看起来应该是这样的：

HTMLCopy to Clipboard

```
<ul>
  <li>项目一</li>
  <li class="special">项目二</li>
  <li>项目 <em>三</em></li>
</ul>
```

在 CSS 中，要选中这个 `special` 类，只需在选择器的开头加个西文句点（.）。在你的 CSS 文档里加上如下代码：

CSSCopy to Clipboard

```
.special {
  color: orange;
  font-weight: bold;
}
```

CSSCopy to Clipboard

```
li.special {
  color: orange;
  font-weight: bold;
}
```

这个意思是说，“选中每个 `special` 类的 `li` 元素”。你真要这样，好了，它对 `<span>` 还有其他元素不起作用了。你可以把这个元素再添上去就是了：

CSSCopy to Clipboard

```
li.special,
span.special {
  color: orange;
  font-weight: bold;
}
```

## [根据元素在文档中的位置确定样式](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/First_steps/Getting_started#根据元素在文档中的位置确定样式)

有时候，你希望某些内容根据它在文档中的位置而有所不同。这里有很多选择器可以为你提供帮助，但现在我们只介绍几个选择器。在我们的文档中有两个 `<em>`元素——一个在段落内，另一个在列表项内。仅选择嵌套在`<li>` 元素内的`<em>`我们可以使用一个称为**包含选择符**的选择器，它只是单纯地在两个选择器之间加上一个空格。

将以下规则添加到样式表。

CSSCopy to Clipboard

```
li em {
  color: rebeccapurple;
}
```

该选择器将选择`<li>`内部的任何`<em>`元素（`<li>`的后代）。因此在示例文档中，你应该发现第三个列表项内的`<em>`现在是紫色，但是在段落内的那个没发生变化。

另一些可能想尝试的事情是在 HTML 文档中设置直接出现在标题后面并且与标题具有相同层级的段落样式，为此需在两个选择器之间添加一个 `+` 号 (成为 **相邻选择符**)

也将这个规则添加到样式表中：

CSSCopy to Clipboard

```
h1 + p {
  font-size: 200%;
}
```

## [根据状态确定样式](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/First_steps/Getting_started#根据状态确定样式)

在这篇教程中我们最后要看的一种修改样式的方法就是根据标签的状态确定样式。一个直观的例子就是当我们修改链接的样式时。当我们修改一个链接的样式时我们需要定位（针对） [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a) （锚）标签。取决于是否是未访问的、访问过的、被鼠标悬停的、被键盘定位的，亦或是正在被点击当中的状态，这个标签有着不同的状态。你可以使用 CSS 去定位或者说针对这些不同的状态进行修饰——下面的 CSS 代码使得没有被访问的链接颜色变为粉色、访问过的链接变为绿色。

CSSCopy to Clipboard

```
a:link {
  color: pink;
}

a:visited {
  color: green;
}
```

你可以改变链接被鼠标悬停的时候的样式，例如移除下划线，下面的代码就实现了这个功能。

CSSCopy to Clipboard

```
a:hover {
  text-decoration: none;
}
```

## [CSS 究竟是怎么工作的？](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/First_steps/How_CSS_works#css_究竟是怎么工作的？)

当浏览器展示一个文件的时候，它必须兼顾文件的内容和文件的样式信息，下面我们会了解到它处理文件的标准的流程。需要知道的是，下面的步骤是浏览加载网页的简化版本，而且不同的浏览器在处理文件的时候会有不同的方式，但是下面的步骤基本都会出现。

1. 浏览器载入 HTML 文件（比如从网络上获取）。
2. 将 HTML 文件转化成一个 DOM（Document Object Model），DOM 是文件在计算机内存中的表现形式，下一节将更加详细的解释 DOM。
3. 接下来，浏览器会拉取该 HTML 相关的大部分资源，比如嵌入到页面的图片、视频和 CSS 样式。JavaScript 则会稍后进行处理，简单起见，同时此节主讲 CSS，所以这里对如何加载 JavaScript 不会展开叙述。
4. 浏览器拉取到 CSS 之后会进行解析，根据选择器的不同类型（比如 element、class、id 等等）把他们分到不同的“桶”中。浏览器基于它找到的不同的选择器，将不同的规则（基于选择器的规则，如元素选择器、类选择器、id 选择器等）应用在对应的 DOM 的节点中，并添加节点依赖的样式（这个中间步骤称为渲染树）。
5. 上述的规则应用于渲染树之后，渲染树会依照应该出现的结构进行布局。
6. 网页展示在屏幕上（这一步被称为着色）

结合下面的图示更形象：

![img](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/First_steps/How_CSS_works/rendering.svg)

## [ID 选择器](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors/Type_Class_and_ID_Selectors#id_选择器)

ID 选择器开头为`#`而非句点，不过基本上和类选择器是同种用法。可是在一篇文档中，一个 ID 只会用到一次。它能选中设定了`id`的元素，你可以在 ID 前面加上类型选择器，只指向元素和 ID 都匹配的类。在下面的示例里，你可以看看这两种用法。

### [指向特定元素的类](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors/Type_Class_and_ID_Selectors#指向特定元素的类)

你能建立一个指向应用一个类的特定元素。在下一个示例里面，我们将会用不同方式高亮一个带有`highlight`类的`<span>`和带有 `highlight`类的`<h1>`标题。我们通过使用附加了类的欲选元素的选择器做到这点，其间没有空格。

```javascript
span.highlight {
  background-color: yellow;
}

h1.highlight {
  background-color: pink;
}
```

```javascript
<h1 class="highlight">Class selectors</h1>
<p>Veggies es bonus vobis, proinde vos postulo essum magis <span class="highlight">kohlrabi welsh onion</span> daikon amaranth tatsoi tomatillo
    melon azuki bean garlic.</p>

<p class="highlight">Gumbo beet greens corn soko <strong>endive</strong> gumbo gourd. Parsley shallot courgette tatsoi pea sprouts fava bean collard
    greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini.</p>
```

## [存否和值选择器](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors/Attribute_selectors#存否和值选择器)

这些选择器允许基于一个元素自身是否存在（例如`href`）或者基于各式不同的按属性值的匹配，来选取元素。

| 选择器              | 示例                            | 描述                                                         |
| :------------------ | :------------------------------ | :----------------------------------------------------------- |
| `[*attr*]`          | `a[title]`                      | 匹配带有一个名为*attr*的属性的元素——方括号里的值。           |
| `[*attr*=*value*]`  | `a[href="https://example.com"]` | 匹配带有一个名为*attr*的属性的元素，其值正为*value*——引号中的字符串。 |
| `[*attr*~=*value*]` | `p[class~="special"]`           | 匹配带有一个名为*attr*的属性的元素，其值正为*value*，或者匹配带有一个*attr*属性的元素，其值有一个或者更多，至少有一个和*value*匹配。注意，在一列中的好几个值，是用空格隔开的。 |
| `[*attr*|=*value*]` | `div[lang|="zh"]`               | 匹配带有一个名为*attr*的属性的元素，其值可正为*value*，或者开始为*value*，后面紧随着一个连字符。 |

下面的示例中，你可以看到这些选择器是怎样使用的。

- 使用`li[class]`，我们就能匹配任何有 class 属性的选择器。这匹配了除了第一项以外的所有项。
- `li[class="a"]`匹配带有一个`a`类的选择器，不过不会选中一部分值为`a`而另一部分是另一个用空格隔开的值的类，它选中了第二项。
- `li[class~="a"]`会匹配一个`a`类，不过也可以匹配一列用空格分开、包含`a`类的值，它选中了第二和第三项。

## [子字符串匹配选择器](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors/Attribute_selectors#子字符串匹配选择器)

这些选择器让更高级的属性的值的子字符串的匹配变得可行。例如，如果你有`box-warning`和`box-error`类，想把开头为“box-”字符串的每个物件都匹配上的话，你可以用`[class^="box-"]`来把它们两个都选中。

| 选择器          | 示例                | 描述                                                         |
| :-------------- | :------------------ | :----------------------------------------------------------- |
| `[attr^=value]` | `li[class^="box-"]` | 匹配带有一个名为*attr*的属性的元素，其值开头为*value*子字符串。 |
| `[attr$=value]` | `li[class$="-box"]` | 匹配带有一个名为*attr*的属性的元素，其值结尾为*value*子字符串 |
| `[attr*=value]` | `li[class*="box"]`  | 匹配带有一个名为*attr*的属性的元素，其值的字符串中的任何地方，至少出现了一次*value*子字符串。 |

# 关系选择器

- [上一页](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors/Pseudo-classes_and_pseudo-elements)
- [概述：CSS 构建](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks)
- [下一页](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance)

我们要了解的最后一种选择器被命名为关系选择器（Combinator），这是因为它们在其他选择器之间和其他选择器与文档内容的位置之间建立了一种有用的关系的缘故。

| 学习前提： | 基础电脑知识、[安装了基本的软件](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/Installing_basic_software)、[文件处理](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/Dealing_with_files)基本知识、HTML 基础（学习 [HTML 介绍](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML)），以及对 CSS 工作原理的了解（学习[CSS 初步](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/First_steps)） |
| :--------- | ------------------------------------------------------------ |
| 目标：     | 了解 CSS 里面可用的不同关系选择器。                          |

## [后代选择器](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors/Combinators#后代选择器)

后代选择器——典型用单个空格（" "）字符——组合两个选择器，比如，第二个选择器匹配的元素被选择，如果他们有一个祖先（父亲，父亲的父亲，父亲的父亲的父亲，等等）元素匹配第一个选择器。选择器利用后代组合符被称作后代选择器。

```
body article p
```

下面的示例中，我们只会匹配处于带有`.box`类的元素里面的`<p>`元素。

<iframe width="100%" height="500" src="https://mdn.github.io/css-examples/learn/selectors/descendant.html" loading="lazy" style="box-sizing: initial; border: 1px solid var(--border-primary); max-width: 100%; width: calc(100% - 2px - 2rem); background: rgb(255, 255, 255); border-radius: var(--elem-radius); padding: 1rem;"></iframe>

[子代关系选择器](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors/Combinators#子代关系选择器)

子代关系选择器是个大于号（`>`），只会在选择器选中直接子元素的时候匹配。继承关系上更远的后代则不会匹配。例如，只选中作为`<article>`的直接子元素的`<p>`元素：

```
article > p
```

下个示例中，我们弄了个有序列表，内嵌于另一个无序列表里面。我用子代关系选择器，只选中为`<ul>`的直接子元素的`<li>`元素，给了它们一个顶端边框。

如果你移去指定子代选择器的`>`的话，你最后得到的是后代选择器，所有的`<li>`会有个红色的边框。

<iframe width="100%" height="600" src="https://mdn.github.io/css-examples/learn/selectors/child.html" loading="lazy" style="box-sizing: initial; border: 1px solid var(--border-primary); max-width: 100%; width: calc(100% - 2px - 2rem); background: rgb(255, 255, 255); border-radius: var(--elem-radius); padding: 1rem;"></iframe>

## [邻接兄弟](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors/Combinators#邻接兄弟)

邻接兄弟选择器（`+`）用来选中恰好处于另一个在继承关系上同级的元素旁边的物件。例如，选中所有紧随`<p>`元素之后的`<img>`元素：

```
p + img
```

常见的使用场景是，改变紧跟着一个标题的段的某些表现方面，就像是我下面的示例那样。这里我们寻找一个紧挨`<h1>`的段，然后样式化它。

如果你往`<h1>`和`<p>`之间插入其他的某个元素，例如`<h2>`，你将会发现，段落不再与选择器匹配，因而不会应用元素邻接时的前景和背景色。

<iframe width="100%" height="800" src="https://mdn.github.io/css-examples/learn/selectors/adjacent.html" loading="lazy" style="box-sizing: initial; border: 1px solid var(--border-primary); max-width: 100%; width: calc(100% - 2px - 2rem); background: rgb(255, 255, 255); border-radius: var(--elem-radius); padding: 1rem;"></iframe>

## [通用兄弟](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors/Combinators#通用兄弟)

如果你想选中一个元素的兄弟元素，即使它们不直接相邻，你还是可以使用通用兄弟关系选择器（`~`）。要选中所有的`<p>`元素后*任何地方*的`<img>`元素，我们会这样做：

```
p ~ img
```

在下面的示例中，我们选中了所有的 `<h1>`之后的`<p>`元素，虽然文档中还有个 `<div>`，其后的`<p>`还是被选中了。

<iframe width="100%" height="600" src="https://mdn.github.io/css-examples/learn/selectors/general.html" loading="lazy" style="box-sizing: initial; border: 1px solid var(--border-primary); max-width: 100%; width: calc(100% - 2px - 2rem); background: rgb(255, 255, 255); border-radius: var(--elem-radius); padding: 1rem;"><[使用关系选择器](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors/Combinators#使用关系选择器)

你能用关系选择器，将任何在我们前面的学习过程中学到的选择器组合起来，选出你的文档中的一部分。例如如果我们想选中为`<ul>`的直接子元素的带有“a”类的列表项的话，我可以用下面的代码。



```
ul > li[class="a"] {
}
```

## [冲突规则](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance#冲突规则)

CSS 代表**层叠样式表**（Cascading Style Sheets），理解第一个词*层叠*（cascade）很重要——层叠的表现方式是理解 CSS 的关键。

在某些时候，在做一个项目过程中你会发现一些应该产生效果的样式没有生效。通常的原因是你创建了两个应用于同一个元素的规则。与[**层叠**](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Cascade)密切相关的概念是[**优先级**（specificity）](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Specificity)，决定在发生冲突的时候应该使用哪条规则。设计元素样式的规则可能不是期望的规则，因此需要了解这些机制是如何工作的。

这里也有[**继承**](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Inheritance)的概念，也就是在默认情况下，一些 css 属性继承当前元素的父元素上设置的值，有些则不继承。这也可能导致一些和期望不同的结果。

我们来快速地看下正在处理的关键问题，然后依次了解它们是如何相互影响的，以及如何和 CSS 交互的。虽然这些概念难以理解，但是随着不断的练习，你会慢慢熟悉它的工作原理。

### [层叠](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance#层叠)

样式表[**层叠**](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Cascade)——简单的说，就是 CSS 规则的顺序很重要；当应用两条同级别的规则到一个元素的时候，写在后面的就是实际使用的规则。

### [优先级](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance#优先级)

浏览器是根据[优先级](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Specificity)来决定当多个规则有不同选择器对应相同的元素的时候需要使用哪个规则。它基本上是一个衡量选择器具体选择哪些区域的尺度：

- 一个元素选择器不是很具体，则会选择页面上该类型的所有元素，所以它的优先级就会低一些。
- 一个类选择器稍微具体点，则会选择该页面中有特定 `class` 属性值的元素，所以它的优先级就要高一点。

### [继承](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance#继承)

继承也需要在上下文中去理解——一些设置在父元素上的 CSS 属性是可以被子元素继承的，有些则不能。

举一个例子，如果你设置一个元素的 `color` 和 `font-family`，每个在里面的元素也都会有相同的属性，除非你直接在元素上设置属性。

### [控制继承](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance#控制继承)





CSS 为控制继承提供了五个特殊的通用属性值。每个 CSS 属性都接收这些值。

- [`inherit`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/inherit)

  设置该属性会使子元素属性和父元素相同。实际上，就是“开启继承”。

- [`initial`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/initial)

  将应用于选定元素的属性值设置为该属性的[初始值](https://developer.mozilla.org/zh-CN/docs/Web/CSS/initial_value)。

- [`revert`](https://developer.mozilla.org/en-US/docs/Web/CSS/revert)

  将应用于选定元素的属性值重置为浏览器的默认样式，而不是应用于该属性的默认值。在许多情况下，此值的作用类似于 [`unset`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/unset)。

- [`revert-layer`](https://developer.mozilla.org/en-US/docs/Web/CSS/revert-layer)

  将应用于选定元素的属性值重置为在上一个[层叠层](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@layer)中建立的值。

- [`unset`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/unset)

  将属性重置为自然值，也就是如果属性是自然继承那么就是 `inherit`，否则和 `initial` 一样

# 盒模型

## [区块盒子与行内盒子](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/The_box_model#区块盒子与行内盒子)

在 CSS 中，我们有几种类型的盒子，一般分为**区块盒子**（block boxes）和**行内盒子**（inline boxes）。类型指的是盒子在页面流中的行为方式以及与页面上其他盒子的关系。盒子有**内部显示**（inner display type）和**外部显示**（outer display type）两种类型。

一般来说，可以使用 [`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 属性为显示类型设置各种值，该属性可以有多种值。

## [外部显示类型](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/The_box_model#外部显示类型)

一个拥有 `block` 外部显示类型的盒子会表现出以下行为：

- 盒子会产生换行。
- [`width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/width) 和 [`height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height) 属性可以发挥作用。
- 内边距、外边距和边框会将其他元素从当前盒子周围“推开”。
- 如果未指定 [`width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/width)，方框将沿行向扩展，以填充其容器中的可用空间。在大多数情况下，盒子会变得与其容器一样宽，占据可用空间的 100%。

某些 HTML 元素，如 `<h1>` 和 `<p>`，默认使用 `block` 作为外部显示类型。

一个拥有 `inline` 外部显示类型的盒子会表现出以下行为：

- 盒子不会产生换行。
- [`width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/width) 和 [`height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height) 属性将不起作用。
- 垂直方向的内边距、外边距以及边框会被应用但是不会把其他处于 `inline` 状态的盒子推开。
- 水平方向的内边距、外边距以及边框会被应用且会把其他处于 `inline` 状态的盒子推开。

某些 HTML 元素，如 `<a>`、 `<span>`、 `<em>` 以及 `<strong>`，默认使用 `inline` 作为外部显示类型。

### [盒模型的各个部分](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/The_box_model#盒模型的各个部分)

CSS 中组成一个区块盒子需要：

- **内容盒子**：显示内容的区域；使用 [`inline-size`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/inline-size) 和 [`block-size`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/block-size) 或 [`width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/width) 和 [`height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height) 等属性确定其大小。
- **内边距盒子**：填充位于内容周围的空白处；使用 [`padding`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/padding) 和相关属性确定其大小。
- **边框盒子**：边框盒子包住内容和任何填充；使用 [`border`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border) 和相关属性确定其大小。
- **外边距盒子**：外边距是最外层，其包裹内容、内边距和边框，作为该盒子与其他元素之间的空白；使用 [`margin`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin) 和相关属性确定其大小。

下图显示了这些层次：

![盒模型示意图](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/The_box_model/box-model.png)

### [CSS 替代盒模型](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/The_box_model#css_替代盒模型)

在替代盒模型中，任何宽度都是页面上可见方框的宽度。内容区域的宽度是该宽度减去填充和边框的宽度（见下图）。无需将边框和内边距相加，即可获得盒子的实际大小。

要为某个元素使用替代模型，可对其设置 `box-sizing: border-box`：

## [外边距、内边距和边框](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/The_box_model#外边距、内边距和边框)

在上面的示例中，你已经看到了 [`margin`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin)、[`padding`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/padding) 和 [`border`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border) 属性的作用。该示例中使用了**简写属性**，允许我们一次性设置盒子的所有边。这些简写属性也有等效的普通属性，可以单独控制盒子的不同边。

接下来，让我们更详细地探究这些属性。

### [外边距](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/The_box_model#外边距)

外边距是盒子周围一圈看不到的空间。它会把其他元素退推离盒子。外边距属性值可以为正也可以为负。在盒子一侧设置负值会导致盒子和页面上的其他内容重叠。无论使用标准模型还是替代模型，外边距总是在计算可见部分后额外添加。

我们可以使用 [`margin`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin) 属性一次性控制一个元素的所有外边距，或者每边单独使用等价的普通属性控制：

- [`margin-top`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-top)
- [`margin-right`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-right)
- [`margin-bottom`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-bottom)
- [`margin-left`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-left)

**在下面的示例中，尝试更改外边距的值，来查看当前元素和其包含元素，在外边距设置为正时如何推开周边元素，以及设置为负时，是如何收缩空间的。**

<iframe width="100%" height="800" src="https://mdn.github.io/css-examples/learn/box-model/margin.html" loading="lazy" style="box-sizing: initial; border: 1px solid var(--border-primary); max-width: 100%; width: calc(100% - 2px - 2rem); background: rgb(255, 255, 255); border-radius: var(--elem-radius); padding: 1rem;"></iframe>