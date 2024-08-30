## 定时器
* js提供定时执行代码的功能，叫做定时器(timer),主要由setTimeout()和setlnterval()这两个函数来完成，它们向任务队列添加定时任务
### setTimeout()
* setTimeout函数用来指定某个函数或某段代码，在多少毫秒之后执行，它返回一个整数，表示定时器的编号，以后可以用来取消这个定时器
* 格式：var timerId = setTimeout(func|code,delay);
* setTimeout函数接收两个参数，第一个参数func|code是将要推迟执行的函数名或一段代码，第二个参数delay是推迟执行的毫秒数，例如:
```js
<script>
    setTimeout(function(){
        console.log("哈哈哈")
    },3000) //3秒后在控制台打印“哈哈哈”
</script>
```
* PS：如果回调函数是对象的方法，那么setTimeout使得方法内部的this关键字指向全局环境，而不是定义时所在的那个对象，例如：
```js
<script>
    var name = "abc"
    var user = {
        name: "ABC",
        getname: function(){
            // console.log(this.name) ABC
            setTimeout(function(){ //添加定时器
                console.log(this.name);   //3秒后abc
            },3000)
        }
    }
        user.getname()
</script>
```
#### 清除定时器
* clearTimeout()：清除定时器
* 代码展示
```js
var time = setTimeout(function(){
    console.log("3miao") //定时器被取消无法在控制台打印
},3000)

clearTimeout(time) //清除定时器
```
------------
### setlnterval()
* setlnterval函数的用法与setTimeout完全一致，区别仅仅在于setlnterval指定某个任务间隔一段时间就执行一次，也就是无限次的定时执行，例如：
```js
var i = 0
setInterval(function(){
    i++
    console.log(i);  //打印i
},1000) //每隔一秒执行一次
```
-----------------------
#### 呼吸灯
* 代码展示：
```js
<style>
    #somediv{
        width: 100px;
        height: 100px;
        background-color: red;
        opacity: 1;  //透明度
    }
</style>
<div id="somediv"></div>    
<script>
    var div = document.getElementById("somediv") //获取元素
    var opacity = 1 //定义初始透明度为1
    var f = true //定义一个状态
    var fade = setInterval(function(){ //定义定时器
        if (f){ //当状态为True
            opacity -= 0.05 //透明度每次减少0.05
            if (opacity<=0){ //当透明度小于0时
                opacity = 0 //改为0
                f = false //状态为false
            }
        }else{ //当状态为false
            opacity +=0.05 //透明度每次加0.05
            if (opacity>=1){ //当透明度大于1
                opacity = 1 //改为1
                f = true //状态该为true
            }
        }
        div.style.opacity = opacity //修改div的style的opaity(透明度)属性
    },50) //50毫秒触发一次
</script>
```
----------------
## 防抖(debounce)
* 定义：对于短时间内连续触发的事件（滚动事件），防抖的含义就是让某个时间期限（200ms）内，事件处理函数值执行一次
* 防抖严格算起来应该属于性能优化的知识，但实际上遇到的频率相当高，处理不当或者放任不管就容易引起浏览器卡死，例如：
```js
window.onscroll = function(){ //定义一个窗口滚动条事件
    var scrolltop = document.documentElement.scrollTop //获取当前滚动高度
    console.log(scrolltop) //打印
}
```
* 效果：每一次滑动滚动条的时候，会打印一大堆滚动高度，执行的频率太高了
----------
### 利用防抖优化执行频率
* 例如：在第一次执行触发事件时不立即执行函数，而是给一个延时例如200ms，当200ms内没有再次触发滚动事件，执行函数，当200ms内再次触发滚动事件，取消当前计时，重新开始计时，借助闭包来实现，例如：
```js
function debounce(fn,delay){ //定义一个函数，两个参数fn为函数，delay为延时
    var timer = null //定义初始为没有计时器
    return function(){ //闭包
        if(timer){ //如果有计时器
            clearTimeout(timer) //结束计时器
        }
        timer = setTimeout(fn,delay) //没有开启一个定时器
    }
}

window.onscroll = debounce(scrollHandle,200) //开启一个窗口滚动事件传入参数调用函数

function scrollHandle(){
    var scrolltop = document.documentElement.scrollTop
    console.log(scrolltop)
}
```
* 效果：如果短时间内大量触发同一时间，只会执行一次函数
----------
## 节流（throttle）
* 节流严格算起来应该属于性能优化的知识，但实际上遇到的频率相当的高，如果处理不当或放任不管就容易引起浏览器卡死
* 代码展示：
```js
function throttle(fn,delay){  //定义函数
    var valid = true; //默认状态为true
    return function(){ //闭包
        if(!valid){ //当状态为false
            return false //返回false
        }
        valid = false //状态不为false改为false
        setTimeout(function(){ //延时函数
            fn() //传入的函数执行
            valid = true //在将状态改为true
        },delay) //传入的延时
    }
}

window.onscroll = throttle(scrollHandle,2000) //调用节流函数

function scrollHandle(){ //获取页面当前滚动高度
    var scrolltop = document.documentElement.scrollTop
    console.log(scrolltop)
}
```
* 效果：如果一直拖着滚动条进行滚动，那么会以2000ms的时间间隔，持续输出当前位置和顶部的距离
----------------------------
### 应用场景
* 搜索框input事件：如果要支持输入实时搜索，就可以使用节流（间隔一段时间就必须查询相关内容），或者实现输入间隔大于某个值（如500ms），就当做用户输入完成，然后开始搜索，具体使用哪种方案要看业务需求
*  页面resize事件，常见于需要做页面适配的时候，需要根据最终呈现的页面情况进行dom渲染（这种情形一般是使用防抖，因为只需要判断最后一次的变化情况）
--------------------
## ECMAScript
### JavaScript与ECMAScript的关系
* ECMAScript和JavaScript的关系是，前者是后者的规格，后者是前者的一种实现，在日常生活中，这两个词是可以互换的
-----------------
### 新特性
1. let和const命令
2. 变量的解构赋值
3. 字符串扩展
4. 函数扩展
5. 对象扩展
6. 数组扩展
7. 运算符扩展
8. Promise对象
9. Class
10. Class继承
11. ...
--------
### let命令
* ES6新增了let命令，用来声明变量，它用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效（也就是花括号内），例如：
```js
if(true){
    let i = 10
}
console.log(i); //报错
```
* let是块级作用域，var是函数级作用域，作用域不同有时候会有不同效果，例如：
```js
var a = []
for(var i = 0;i<10;i++){
    a[i] = function(){
        console.log(i);
        }
}
a[6]() //输出10
```
* 原因：
1. var 声明的变量是函数级作用域，且在整个函数（或全局）范围内有效。所以在 for 循环外部的所有函数都共享一个 i 变量。
2. 每一次访问的都是同一个i
3. 所以当i变为10时，循环终止，此时，i 的值是 10，所有的 a[i] 函数都引用了这个值不管`a[6]()还是a[2]()`都会是10
```js
var a = []
for(let i = 0;i<10;i++){
    a[i] = function(){
        console.log(i);        
    }
}
a[6]() //输出6
```
* 原因
1. 变量不提升，只在花括号内有效
2. 每一次循环都是一个新的i
-----------
#### 不允许重复声明
* 代码展示：
```js
let i =10
let i =20
console.log(i) //报错
```
* 不同的作用域可以，同一个就不行
-----------
* var可以重复声明，例如：
```js
var i = 10
var i = 20
console.log(i) //输出最后声明的值 20
```
------------
### const命令
* const命令声明一个只读的常量，一旦声明，常量的值就不能改变，例如：
```js
const PI = 3.14
console.log(PI) //3.14
PI = 3 //报错
console.log(PI) //无法输出
```
* const一旦声明变量就必须初始化，不能留到后面赋值，例如：
```js
const a
a = 20
console.log(a) //无法输出
```
* const是块级作用域和let一样
```js
{
    const n = 20
}
console.log(n) //报错
```
* 也不允许重复声明，不同作用域可以
* **const和let概念基本类似，但一个是常量一个是变量**
-------------------------
### 对象解构赋值
* 代码展示：
```js
var user = { //定义一个对象
    name: "lulu",
    age: 18,
}
const{ name , age } = user //将对象解构出来
console.log(name,age); //直接打印，不需要在前面加user.name
```
* PS：对象的属性没有次序，变量必须与属性同名，才能得到正确的值
-----------------
### 字符串扩展
* ES6加强了对Unicode的支持，允许采用\uxxx形式表示一个字符，其中xxxx表示字符的Unicode码点
--------------
#### Unicode
* 统一码（Unicode），也叫万国码、单一码，是计算机科学领域里的一项业界标准，包括字符集，编码方案等，Unicode是为了解决传统的字符编码方案的局限而产生的，它为每种语言中的每个字符设定了统一并且唯一的二进制编码，以满足跨语言、跨平台进行文本转换，处理的要求
----------
#### 字符串遍历接口
* for...of循环遍历，例如：
```js
var str = "hello"
for(let i of str){
    console.log(i) //遍历输出hello
}
```
----------
#### 模板字符串
* 模板字符串(template string)是增强版的字符串，用反引号(`)表示，它可以当做普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量，例如：
```js
var href = "http://mi.com"
var text = "小米"
var a1 = "<a herf = '"+ href +"' > "+ text +" </a> " //老方法
console.log(a1)
var a2 = `<a href="${href}">${text}</a>` //模板字符串
console.log(a2)
```
----------------
#### 字符串新增方法
##### 判断是否包含某个字符串
1. includes()：返回布尔值，表示是否找到了参数字符串
2. startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部
3. endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部
* 代码展示：
```js
var allstr = "hello world"
var str = "world"
console.log(allstr.includes(str)) //true
console.log(allstr.startsWith(str)) //从头部查找 false
console.log(allstr.endsWith(str)) //从尾部查找 true
```
* PS：这几个方法也支持第二个参数，表示从第几个位置查找
----------
##### repeat()
* repeat方法返回一个新字符串，表示将原字符串重复n次，例如：
```js
var x = "x"
console.log(x.repeat(3)) //输出xxx
```
------------
##### padStart() padEnd() 字符串补全
* ES2017引入了字符串补全长度的功能，如果某个字符串不够指定长度，会在头部或尾部补全，padStart()用于头部补全，padEnd()用于尾部补全，例如：
```js
var a = "hello"
console.log(a.padStart(10,"abc")) // abcabhello
console.log(a.padEnd(10,"abc")) // helloabcab
```
-------------
##### trimStart() trimEnd() 去除空格
* ES2019对字符串实例新增了trimStrat()和trimEnd()这两个方法，它们的行为与trim()一致，trimStrat()消除字符串头部的空格，trimEnd()消除尾部的空格，它们返回的都是新的字符串，不会修改原始字符串，例如：
```js
var a = "   hello   "
console.log(a.trim()) //输出hello
console.log(a.trimStart()) //hello   输出
console.log(a.trimEnd()) //输出   hello
```
---------------
##### at()
* at()方法接受一个整数作为参数，返回参数指定位置的字符，支持负索引(即倒数的位置)，若超过了字符本身的长度会返回undefined，例如：
```js
var a = "hello" //定义一个字符
console.log(a.at(3)) //输出l
console.log(a.at(-4)) //输出e
console.log(a.at(7)) //undefined
```
----------------
### 数组扩展
#### 扩展运算符
* 扩展运算符(spread)是三个点(...)，将一个数组转为用逗号分隔的参数序列，例如：
```js
var arr = [10,20,30,40]
for(let i = 0;i<arr.length;i++){
    console.log(arr[i]) //在四行内获得数组的值
}
console.log(...arr) //10 20 30 40
```
------------
##### 代替apply方法
* 由于扩展运算符可以展开数组，所以不在需要apply方法将数组转为函数的参数了，例如：
```js
var arr1 = [10,20,30,40]
console.log(Math.max.apply(null,arr1)) // 40
console.log(Math.max(...arr1)) // 40
```
* 这两方法等同于Math.max(10,20,30,40)
---------
##### 合并数组
* 原本的方法是利用concat来合并两个数组，现在可以用扩展运算符来合并，例如：
```js
var arr1 = [10,20,30,40]
var arr2 = [50,60,70,80]
console.log(arr1.concat(arr2)) 
console.log([...arr1,...arr2])
```
----------------
#### 新增方法
##### Array.from()
* Array.from方法用于将类数组(伪数组)转为真正的数组
* 常见的类数组
1. arguments
2. 元素集合
3. 类似数组的对象
--------
###### arguments伪数组
* 函数的可选参数
* 类数组，伪数组，只能使用数组的读取方式和length属性，不能使用数组方法比如：push
* 代码展示：
```js
function add(){
    console.log(arguments)
    console.log(arguments[0]) //10
    console.log(arguments[1]) //20
    console.log(arguments[2]) //30
    arguments.push(40) //报错

    var result = Array.from(arguments)
    result.push(40)
    console.log(result) //[10,20,30,40]
}
add(10,20,30)
```
--------
###### 元素集合伪数组
* 代码展示：
```js
<h3>1</h3>
<h3>2</h3>
<h3>3</h3>
<script>
let title = document.querySelectorAll("h3")
console.log(title)
console.log(Array.from(title)) //[h3,h3,h3]
</script>
```
-------------
###### 类数组的对象
* 代码展示：
```js
var user = {
    '0':"lili",
    '1': 18,
    '2': 100,
    length:3
}
    console.log(user["0"])
    console.log(user["1"])
    console.log(user["2"])
    console.log(user.length)

    console.log(Array.from(user)) //["li",18,100]
```
----------------
##### Array.of()
* Array.of()方法将一组值转换为数组
```js
console.log(Array.of(10,20,30)) //[10,20,30]
```