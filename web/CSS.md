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
----------------
### ID选择器
* 针对某一个特定的标签来使用，只能使用一次，css中的ID选择器用#来定义，例如：
```html
    <style>
        #test{
            color: red;
            font-size: 50px;
        }
    </style>
<body>
    <p id="test">Hello</p>
</body>
```
* ID是唯一的
* ID不能以数字开头
--------
### 合并选择器
* 提取共同的样式，减少重复代码，例如：
```html
    <style>
        p,h3{
            color: blue;
            font-size: 30px;
        }
    </style>
```
### 选择器优先级
1. 行内样式
2. ID选择器
3. 类选择器
4. 元素选择器
------------
## 字体属性
* CSS字体属性定义字体、颜色、大小、加粗，文字样式等
### 颜色：color
* 格式
1. color: red;
2. color: #ff0000;
3. color: rgb(255,0,0);
4. color: rgba(255,0,0,5);
--------
### 大小font-size
* 单位：px（像素）
---------
### 粗细  font-weight
* bold：定义粗体字符
* bolder：定义更粗的字符
* lighter：定义更细的字符
* 100~900：定义由细到粗400等同默认，700等同bold
---------
### 字体样式 font-style
* normal：默认值
* italic：定义斜字体
-------
### 字体 font-family
* 每个值用逗号分开
* 如果字体名称包含空格，它必须加上引号
--------------
## 背景属性  
### 背景颜色 
* 设置背景颜色：background-color
----------
### 背景图像
* 设置背景图像：background-image
* 记得加url
------
#### 如何平铺背景图像
* 设置平铺背景图像：background-repeat
1. repeat：默认值
2. repeat-x：只向水平方向平铺
3. repeat-y：只向垂直方向平铺
4. no-repeat：不平铺
#### 背景图像大小
* 设置背景图像大小：background-size
1. length：设置背景图片的宽度和高度，第一个值宽度，第二个高度，如果只设置一个，第二个值为auto
2. percentage：计算相对位置区域的百分比，第一个值宽度，第二个高度，如果只是设置一个，第二个值auto
3. cover：保持图片纵横比并将图片缩放成完全覆盖背景区域的最小大小
4. contain：保持图片纵横比并将图像缩放成适合背景定位区域的最大大小
------------
#### 设置该背景图像的起始位置
* background-position：设置背景图像的起始位置，其默认值为0% 0%
1. 两个属性,属性值包括：left、right、center、top、bottom,例如left top：左上角
2. x% y%：第一个值为水平位置，第二个值为垂直位置，如果只写了一个值另一个值默认为50%
3. xpos ypos：单位为像素
-----------
## 文本属性
### 对齐方式  
* text-align：指定元素文本的水平对齐方式：left、right、center默认为left
----------
### 文本修饰
* text-decoration：可以给文本添加，下划线、删除线、上划线等
1. underline：定义下划线
2. overline：定义上划线
3. line-through：定义删除线
-----------
### 控制文本大小写
* text-transform：控制文本大小写
1. capitalize：定义每个单词开头大写
2. uppercase：定义全部大写字母
3. lowercase：定义全部小写字母
-------
### 首行文本缩进
* text-indent：规定文本块中首行文本的缩进
* PS:负数是允许的，如过是负数，将第一行左缩进
---------------
## 表格属性
### 表格边框：border
* 3个属性空格间隔，第一个为表格粗细，第二个为线条类型，第三个为线条颜色
----------
### 折叠边框
* border-collapse：设置表格的边框是否被折叠成一个单一的边框或隔开
* 格式：border-collapse:collapse;
---------
### 表格大小
* width：表格宽度
* height：表格高度
-----
### 表格文本对齐方式
#### 水平对齐方式
* text-align：left、right、center：默认左对齐
#### 垂直对齐方式
* vertical-align：top、center、bottom：默认居中
--------
### 表格填充
* padding：控制单元格内容与单元格边框的距离，写在td和th中
--------
### 表格颜色
* background-color：表格的背景颜色
* color：文本颜色
---------------
## 关系选择器
### 后代选择器  
* 选择所有被E元素包含的F元素，中间用空格隔开，例如：E F{}
* 代码展示：
```html
    <style>
        ul li{
            color: red;
        }
    </style>
<body>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <div>
            <p>123</p>
            <ol>
                <li>4</li>
                <li>5</li>
            </ol>
        </div>
    </ul>
</body>
```
* 表示设置ul所包含的的li标签都为红色，p标签不受任何影响
-----------
### 子代选择器
* 选择所有作为E元素的直接子元素F，对更深一层的元素不起作用，用>表示，例如E>F{}
* 代码展示：
```html
    <style>
        div>p{
            color: red;
        }
    </style>
<body>
    <div>
    <p>nihao</p> //红色
    <ul>
        <li>
            <p>world</p> //默认
        </li>
    </ul>
    <p>hello</p> //红色
    </div>
</body>
```
* 影响了第一个和第三个p标签，由于第二个p标签由于在ul中，所以不受选择器的影响 
-----------
### 相邻兄弟选择器
* 选择紧跟E元素后的F元素，用+号表示，选择相邻的第一个兄弟元素,例如：E+F{}
* 代码展示：
```html
    <style>
        h3+p{
            color: red;
        }
    </style>
<body>
    <p>123</p> //默认
    <h3>456</h3>
    <p>789</p> //红色
    <p>999</p> //默认
</body>
```
* PS：只有第二个p标签生效，选择相邻只能向下找一个，不能向上找
---------
### 通用兄弟选择器
* 选择E元素之后的所有兄弟元素F，作用于多个元素，用~隔开，例如：E~F{}
* 代码展示：
```html
    <style>
        h3~p{
            color: red;
            
        }
    </style>
<body>
    <p>123</p> //默认
    <h3>456</h3>
    <p>789</p> //红色
    <p>999</p> //红色
</body>
```
* 第一个p标签不生效，在h3下面的p标签都生效