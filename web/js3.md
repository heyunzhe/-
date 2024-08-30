## DOM
### 概念：
* DOM是JavaScript操作网页的接口，全称为"文档对象模型"（Document Object Model），它的作用是将网页转为一个JavaScript对象，从而可以用脚本进行各种操作（比如对元素的增删内容）
* 浏览器会根据DOM模型，将结构化文档HTML解析成一系列的节点，再由这些节点组成一个树状结构（DOMTree），所有的节点和最终的树状结构，都有规范的对外接口
* DOM只是一个接口规范，可以用各种语言实现，所以严格地说，DOM不是JavaScript语法的一部分，但是DOM操作时JavaScript最常见的任务，离开了DOM，JavaScript就无法控制网页，另一方面，JavaScript也是最常用于DOM操作的语言
-----------------
### 节点
* 概念：DOM的最小组成单位叫做节点（node），文档的树形结构（DOM树），就是由各种不同类型的节点组成，每个节点可以看作是文档数的一片叶子
------------
#### 节点类型
1. Document：整个文档树的顶层节点
2. DocumentType：doctype标签
3. Element：网页的各种HTML标签
4. Attribute：网页元素的属性（比如class="right"）
5. Text：标签之间或标签包含的文本
6. Comment：注释
7. DocumentFragment：文档的片段
--------
#### 节点树
* 概念：一个文档的所有节点，按照所在的层级，可以抽象成一种树状结构。这种树状结构就是DOM树，它有一个顶层节点，下一层都是顶层节点的子节点，然后子节点又有自己的子节点，就这样层层衍生出了一个金字塔结构，倒过来就像一棵树
* 浏览器原生提供document节点，代表整个文档
* 除了根节点，其他节点都有三种层级关系
1. 父节点关系(parentNode)：直接的那个上级节点
2. 子节点关系(childNodes)：直接的下级节点
3. 同级节点关系(sibling)：拥有同一个父节点的节点
---------------
##### Node.nodeType属性
* 不同节点的nodeType属性值和对应的常量如下
1. 文档节点（document）：9，对应常量Node.DOCUMENT_NODE
2. 元素节点（element）：1，对应常量Node.ELEMENT_NODE
3. 属性节点（attr）：2，对应常量Node.ATTRIBUTE_NODE
4. 文本节点（text）：3，对应常量Node.TEXT_NODE
5. 文档片断节点（DocumentFragment）：11，对应常量Node.DOCUMENT_FRAGMENT_NODE
-------------------
## document对象_方法/获取元素
### 搜索HTML标签名返回符合条件元素
* document.getElementsByTagName方法搜索HTML标签名，返回符合条件的元素，它的返回值是一个类似数组对象（HTML Collection实例），可以实时反映HTML文档的变化，如果没有任何匹配的元素，就返回一个空集
* 代码展示：
```js
var a = document.getElementsByTagName("div")[0] //获取对象赋值给a
console.log(a)
a.innerHTML = "hello world" //修改页面输出内容
```
-----------
### 根据class指定
* document.getElementsByClassName方法返回一个类似数组的对象（HTML Collection实例），包括所有class名字符合指定条件的元素，元素的变化实时反映在返回结果中
* 代码展示：
```js
<p class="text"></p>

var b = document.getElementsByClassName("text")[0] //多个参数空格间隔
console.log(b)
b.innerHTML = "nihao" //页面输出nihao
```
---------
### 根据name指定
* document.getElementsByName方法用于选择拥有name属性的HTML元素（比如`<form>、<radio>、<img>`等），返回一个类似数组的对象（NodeList实例），因为name属性相同的元素可能不止一个（不常用）
* 代码展示：
```js
<form name="login"></form>

var c = document.getElementsByName("login")
console.log(c)
```
---------
### 根据id指定
* document.getElementById方法返回匹配指定id属性的元素节点，如果没有发现匹配的节点，则返回null，注意该方法的参数是大小写敏感的，比如：如果某个节点的id属性是main，那么document.getElementById("Main")将返回null
* 代码展示：
```js
<div id="id"></div>

var d = document.getElementById("id")
console.log(d)
d.innerHTML = "yes"
```
-------------------
### 根据css选择器
* document.querySelector方法接收一个CSS选择器作为参数，返回匹配该选择器的元素节点，如果有多个节点满足匹配条件，则返回第一个匹配的节点，如果没有发现匹配的节点，则返回null
* 代码展示：
```js
 <div class="nav">nav1</div>
<div class="nav">nav2</div>

var nav = document.querySelector(".nav")
console.log(nav) //返回一个
```
-------
### 返回NodeList对象
* document.querySelectorAll方法与querySelector用法相似，区别是返回一个NodeList对象，包含所有匹配给定选择器的节点
* 代码展示
```js
<div class="nav">nav1</div>
<div class="nav">nav2</div>

var nav1 = document.querySelectorAll(".nav")
console.log(nav1) //返回多个
```
-----------------
## document对象_方法/创建元素
* 代码展示：
```js
<div id="container"></div>

var text = document.createElement("p"); //创建新标签
var content = document.createTextNode("我是文本") //创建文本内容
var id = document.createAttribute("id") //创建一个新的属性
id.value = "root" //给属性赋值
text.appendChild(content) //将内容或子元素放到容器中
text.setAttributeNode(id); //将属性放入标签中
console.log(text)
var container = document.getElementById("container") //读取指定id的元素节点
container.appendChild(text)
```
* 属性添加用：setAttributeNode()，内容添加用：appendChild()
-----------------
## Element对象_属性
* 概念：Element对象对应网页的HTML元素，每一个HTML元素，在DOM树上都会转化成一个ELement节点对象
* 代码展示：
```js
<div class="box" id="root">hello</div>

var root = document.getElementById("root") //根据id获取元素
root.id = "roots" //修改元素id的值为roots
root.className = "boxs" //修改元素class的值为boxs
root.classList.add("mybox") //向root元素中添加一个class
root.classList.remove("box") //在root元素中移除一个class
root.classlist.contains("box") //检查root元素是否包含名为box的class
root.classlist.toggle("box") //判断root元素有无名为box的class，有就删除，没有就添加
root.innerHTML = "大家好" //修改文本显示内容在页面上输出
root.innerText = "你们好" //修改文本显示内容在页面上输出
```
* innerHTML和innerText的区别：前者可以识别标签，后者会把标签识别成一个字符串，例如：
```js
var str = "<a href='http://www.baidu.com'>百度</a>"
root.innerHTML = str //页面上显示带有超链接的 "百度"
root.innerText = str //页面上显示代码中str的内容
```
### Element获取元素位置
* 如图所示：
![image-16](C:/Users/HYun/Desktop/文件/xxbj/image/image-16.png)
* 代码展示：
```js
var box =document.getElementById("box")
console.log(box.clientHeight) //获取元素高度包括padding部分
console.log(box.clientWidth) //获取元素宽度包括padding部分

console.log(document.documentElement.clientHeight) //获取视口高度
console.log(document.body.clientHeight) //页面总高度

console.log(box.scrollHeight) //元素总高度与第一条语句类似，但他包括溢出不可见的内容
console.log(box.scrollWidth) //元素总宽度

console.log(document.documentElement.scrollHeight) //视口高度
console.log(document.body.scrollHeight) //页面高度

console.log(document.documentElement.scrollTop) //获取滚动高度

console.log(box.offsetWidth) //元素的CSS垂直宽度，包括元素本身的宽度、padding和border
console.log(box.offsetHeight) //元素的CSS水平高度

console.log(box.offsetTop) //到定位父级元素上边界的间距，没有默认为<html>
console.log(box.offsetLeft)//到定位父级元素左边边界的间距
```
---------------
## CSS操作
### 通过HTML的style属性
* 操作CSS样式最简单的方法，就是使用网页元素节点的setAttribute方法直接操作网页元素的style属性，例如：
```js
var box = document.getElementById("box")
box.setAttribute("style","width:200px;height:200px;background:red;")
```
----------------------
### 通过元素节点的style属性
* 代码展示：
```js
var box = document.getElementById("box")
box.style.width = "300px"
box.style.height = "300px"
box.style.background = "red"
```
----------------
### 通过cssText属性
* 代码展示：
```js
var box = document.getElementById("box")
box.style.cssText = "width:200px;height:200px;background:red"
```
-------------
## 事件处理程序
### HTML事件处理
* 代码展示：
```js
<button onclick="dianji()">别点</button>
<script>
    function dianji(){
        alert("你干嘛!")
}
</script>
```
* 缺点：html没有和js分开
---------
### DOM0级事件处理
* 代码展示：
```js
<button id="btn">123</button>
<script>
    var btn = document.getElementById("btn")
    btn.onclick = function(){
        alert("哎哟!")
    }
</script>
```
* 优点：html和js是分离的
* 缺点：无法同时添加多个事件
-----------
### DOM2级事件处理
* 代码展示
```js
<button id="ntr">haha</button>
<script>
    var ntr = document.getElementById("ntr")
    ntr.addEventListener("click",function(){
        alert("lalala")
    })
    ntr.addEventListener("click",function(){
        console.log("lalala")
        // alert("heiheihei")
    })
</script>
```
* 优点：可以同时执行多个事件
----------
### 鼠标事件
1. click：按下鼠标时触发
2. dblclick：在同一个元素上双击鼠标时触发
3. mousedown：按下鼠标键时触发
4. mouseup：释放按下的鼠标键时触发
5. mousemove：当鼠标在节点内部移动时触发，当鼠标持续移动时，该事件会连触发
6. mouseenter：鼠标进入一个节点时触发，进入子节点不会触发这个事件
7. mouseleave：鼠标离开一个节点时触发，离开父节点不会触发这个事件
8. mouseover：鼠标进入一个节点时触发，进入子节点会再一次触发这个事件
9. mouseout：鼠标离开一个节点时触发，离开父节点也会触发这个事件
10. wheel：滚动鼠标的滚轮时触发
* **PS：这些方法在使用的时候，除了DOM2级事件，其余的都需要加前缀on**
* 代码展示：
```js
var btn1 = document.getElementById('dj') 
btn1.onclick = function(){
    console.log("dj")
}
var btn2 = document.getElementById('sj')
btn2.ondblclick=function(){
     console.log("sj")
}
var btn3 = document.getElementById('ax')
btn3.onmousedown=function(){
    console.log("ax")
    }
var btn4 = document.getElementById('tq')
btn4.onmouseup=function(){
    console.log("tq")
}
```
* 先获取元素信息，再给元素添加事件
------------
### Event事件对象
#### 对象属性
1. Event.Target
2. Event.type
------------
##### Event.Target
* 返回事件当前所在的节点，例如：
```js
<button id="btn">按钮</button>

<script>
    var btn = document.getElementById("btn")
    btn.onclick = function(event){
        console.log(event.target) //输出<button id="btn">按钮</button>
        event.target.innerHTML = "点击" //修改按钮名称为"点击"
}
</script>
```
----------
##### Event.type
* 返回一个字符串，表示事件类型，事件的类型是在生成事件的时候，该属性只读
```js
    <button id="btn">按钮</button>
    <script>
    var btn = document.getElementById("btn")
    btn.onclick = function(event){
        console.log(event.target)
        event.target.innerHTML = "dj"
        console.log(event.type) //click
    }
    </script>
```
--------------
#### 对象方法
1. Event.preventDefault()
2. Event.stopPropagation()
--------
##### Event.preventDefault()
* 取消浏览器对当前事件的默认行为，比如点击链接后，浏览器默认会跳转到另一个页面，使用这个方法以后，就不会跳转了，例如：
```js
<a href="https://www.baidu.com" id="bd">baidu</a>
<script>
    var bd = document.getElementById("bd")
    bd.onclick = function(e){ //单击事件
        e.preventDefault(); //阻止浏览器默认事件
        console.log("dj") //点击链接后只打印不跳转
}
```
-----------------
##### Event.stopPropagation()
* 阻止事件在DOM中继续传播，防止再触发定义在别的节点上的监听函数，但是不包括，在当前节点上其他的事件监听函数，例如：
```js
<div id="root" class="root">
<div id="box" class="box"></div>
</div>
<script>
    var root = document.getElementById("root") // 获取元素
    var box = document.getElementById("box") 
    root.onclick = function(){  //单击事件
        console.log("root")
    }
    box.onclick = function(e){ //单击事件
        e.stopPropagation() //阻止事件冒泡，阻止同时触发box和root的单击事件，只触发box的单击事件
        console.log("box") 
}
</script>
```
------------
### 键盘事件
* 概念：键盘事件由用户击打键盘触发，主要有keydown、keypress、keyup
1. keydown：按下键盘时触发
2. keypress：按下有值的键时触发，即按下Ctrl、Alt、Shift、Meta这样无值的键，这个事件不会触发，对于有值的键，按下时先触发keydown事件，再触发这个事件
3. keyup：松开键盘时触发改事件
* e.keycode：返回值所对应的ascll码
* 代码展示：
```js
<input type="text" id="username">
    <input type="text" id="password">
    <script>
        var username = document.getElementById("username")
        username.onkeydown = function(e){ //键盘按下事件
            console.log("anxiale")
        }
        username.onkeyup = function(e){ //键盘松开事件
            console.log(e.target.value)
        }
        username.onkeypress = function(){ //按下有值键返回
            console.log("lalala")
        }
        var password= document.getElementById("password")

        password.onkeyup = function(e){
            console.log(e.keyCode) //返回对应值的ascll码
        }
    </script>
```
----------------
### 表单事件
1. input事件：连续触发，用户每按下一次按键，就会触发
2. select事件：选中文本时触发
3. change事件：不会连续触发，完成全部修改（离开焦点）才会触发
4. reset事件：重置表单
5. submit事件：将数据提交给服务器
* 代码展示：
```js
<input type="text" id="username">
<input type="text" id="password">
<script>
    var username = document.getElementById("username") //获取元素
    username.oninput = function(e){ //定义事件
        console.log(e.target.value);   //实时获取用户输入的内容
    }
    username.onselect = function(){
        console.log("lalala"); //选中文本时打印
    }
    var password = document.getElementById("password")
    password.onchange = function(e){
        console.log(e.target.value) //离开焦点时打印
    }
</script>
```
```js
<form id="myForm" onsubmit="submitHandle"> //提交事件写在表单上
    <input type="text" name="username"> //name为提交内容
    <button id="resetbtn">清除</button>
    <button>提交</button>
</form>    
<script>
    var resetbtn = document.getElementById("resetbtn")
    var myForm = document.getElementById("myForm")
    resetbtn.onclick = function(){ //单击事件
        myForm.reset() //清空表单
    }
    function submitHandle(){ //定义提交事件
        console.log("想做的事情");  
    }
</script>
```
----------------
### 事件代理（事件委托）
* 概念：由于事件会在冒泡阶段向上传播到父节点，因此可以把子节点的监听函数定义在父节点上，由父节点的监听函数统一处理多个子元素的事件，这种方法叫做事件的代理（delegation）
* 代码展示
```js
    <ul id="list">
        <li>列表1</li>
        <li>列表2</li>
        <li>列表3</li>
        <li>列表4</li>
        <p>哈哈哈</p>
    </ul>
<script>
    var list = document.getElementById("list")
    list.addEventListener("click",function(e){ //DOM2级事件，单击事件
        if (e.target.tagName === "LI"){ //判断标签名是不是LI
            console.log(e.target.innerHTML); //是就答应点击标签自身的内容
    }
    })
```
* target.tagName：输出元素的标签名**大写显示**