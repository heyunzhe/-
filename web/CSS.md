# CSS
## CSS概念
* CSS(Cascading Style Sheets)层叠样式表，又叫级联样式表，简称样式表
* CSS文件后缀名为.css
* CSS用于HTML文档中元素样式的定义
* 使用CSS的目的是让网页具有美观一致的页面
## 语法
* CSS规则由两个主要的部分构成：选择器，以及一条或多条声明(样式)
1.选择器通常是你需要改变样式的html元素
2.每条声明由一个属性和一个值组成
3.属性（property）是你希望设置的样式属性（style attribute）每个属性都有一个值，属性和值被冒号分开，例如：
```html
<STYLE>
    h3{
        color: blue;
        font-size: 50px;
    }
    p{
        color: purple;
        font-size: 50px;
}
</STYLE>
<h3>123</h3>
<p>hahaha</p>
```
------------
## CSS引入方式
### 内联样式（行内样式）
* 要使用内联样式，你需要在相关的标签内使用样式（style）属性。style属性可以包含任何CSS属性，例如：
```html
<p style="color: red;font-size: 100px;">hahaha</p>
```
PS:不推荐使用，缺乏整体性和规划性，不利于维护，维护成本高

------------
### 内部样式
* 当单个文档需要特殊的样式时，就应该使用内部样式表，你可以使用`<style>`标签在文档头部定义内部样式表，例如：
```html

<head>
    <title>Document</title>
    <style>
        p{
            color: red;
            font-size: 50px;
        }
    </style>
</head>
<body>
    <p>hahaha</p>
    <p>lalala</p>
    <a href="./index.html"><p>123</p></a>
```
PS：单个页面内的CSS代码具有统一性和规划性，便于维护，但是在多个页面之间容易混乱

------------
### 外部样式（推荐）
* 当样式需要应用很多页面时，外部样式表将是理想的选择，在使用外部样式表的情况下，你可以通过改变一个文件来改变整个站点的外观，每个页面使用`<link>`标签链接到样式表，`<link>`标签在（文档的）头部，例如：
* CSS文件：
```css
p{
    color: blue;
    font-size: 40px;
}
```
* HTML头部写：
```html
<link rel="stylesheet" href="./外部样式/index.css">
```
---------------
## 选择器
* CSS语法规则由两个主要部分构成：选择器，以及一条或多条声明（样式）
### 全局选择器
* 可以与任何元素匹配，优先级最低，一般做样式初始化，例如：
```html
<head>
<style>
    *{
        color: red;
        font-size: 40px;
    }
</style>
</head>
```
----------
### 元素选择器
* HTML文档中的元素：p、b、div、a、img、body等都属于标签选择器，选择的是页面上所有这种种类的标签，所以经常描述“共性”，无法描述某一个元素的“个性”。例如：
```html
p{
    font-size: 14px;
}
```
又或者
```html
<style>
    p{
        color: blue;
        font-size: 10px;
    }
    span{
        color: red;
    }
</style>
</head>
<body>
    <p>123</p>
    <p>123<span>123</span></p>
    <p>456</p>
```
PS：所有的标签都可以是选择器，无论这个标签藏得多深一定能够被选择上，选择的所有，而不是一个

-------------
### 类选择器（灵活）
* 规定用圆点.来定义，针对你想要的所有标签使用，例如：
```html
    <style>
        p{
            color: purple;
            font-size: 50px;
        }
        .wuwuwu{
            color: red;
            font-size: 100px;
        }
    </style>
<body>
    <p>123</p>
    <p>234</p>
    <p class="wuwuwu">555</p>
</body>
```
* class属性的特点
1.类选择器可以被多种标签使用
2.类名不能以数字开头
3.同一个标签可以使用多个类选择器，用空格隔开，例如：
```html
    <style>
        p{
            color: purple;
            font-size: 50px;
        }
        .wuwuwu{
            color: red;
        }
        .size{
            font-size: 100px;
        }
    </style>
<body>
    <p>123</p>
    <p>234</p>
    <p class="wuwuwu size">555</p>
</body>
```