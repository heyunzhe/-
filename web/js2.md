## 字符串
* 单引号中可以嵌套双引号
* 双引号中可以嵌套单引号
```js
var a = "nihao'ma'"
var b = 'wohen"hao"'
console.log(a) //nihao'ma'
console.log(b) //wohen"hao"
```
* 但双引号/单引号中想嵌套双引号/单引号，需要加反斜杠，例如：
```js
var a = "nihao\"ma\""
var b = 'wohen\'hao\''
console.log(a) //nihao"ma" 
console.log(b) //wohen'hao'
```
* 默认情况下字符串只能写在一行，如果要换行末尾加"\"
* 求字符串长度:length
```js
var a = "abcdefg"
console.log(a.length) //7
```
------------
### 返回指定字符串
* charAt()
* 传入的参数不符合字符的下标会返回空值
* 代码展示：
```js
var x = "hello js"
console.log(x.length) //8
console.log(x.charAt(2)) //l
```
---------------
### 连接两个字符串
* concat()
* 代码展示：
```js
var a = "hello"
var b = "world"
var c = "!"
var d = a.concat(b,c)
console.log(d) //helloworld!
```
* 如果只连接字符串可以用+
* concat()中传入的参数可以是数字，他会转换数字为字符串添加到字符串中
----------
### 从原字符取出字符串并返回,不改变原字符串1
* substring()
* 代码展示：
```js
var a = "helloworld"
var b = a.substring(0,5) //第一个参数为开始位置，第二个参数为结束位置[0,5),结束位置取不到
var c = a.substring(5)
console.log(b) //hello
console.log(c) //world
```
* 若第一个参数>第二个参数则substring会自动更换两个参数的位置
* 省略第二个参数，表示子字符串一直到原字符串结束
* 如果参数为负数，substring会转换为0
------------------
### 从原字符取出字符串并返回,不改变原字符串2
* substr() 跟substring方法作用相同，但substr的第一个参数是字符串的开始位置，第二个字符串为子字符串的长度
* 代码展示：
```js
var a = "helloworld"
var b = a.substr(5,4)
var c = a.substr(4,5)
var d = a.substr(7)
var e = a.substr(-7)
console.log(b) //worl
console.log(c) //oworl
console.log(d) //rld
console.log(e) //loworld
```
* 省略第二个参数，表示子字符串一直到原字符串结束
* 如果第一个参数为负数，则表示倒数计算字符位置，若第二个为负数就转换成0，因此会返回空字符
----------------
### indexOf() 查找字符
* 概念：indexOf方法用于确定一个字符串在另一个字符串中第一次出现的位置，返回结果是匹配开始的位置，如果返回-1，就表示不匹配
* 代码展示：
```js
var a = "hello world"
var b = a.indexOf("o")
var c = a.indexOf("o",5)
var d = a.indexOf("m")
console.log(b) // 4
console.log(c) // 7
console.log(d) // -1
```
* 第一个参数为查找的值，第二个参数为从第几个位置开始查找
----------------------
### 去除字符串两边空格
* trim()：去除字符串两端的空格，还可以去除制表符，返回一个新字符串，不改变原字符串
* 代码展示：
```js
var a = "   helloworld    "
var b = a.trim()
var c = "\r\nhelloworld \t"
var d = c.trim()
console.log(b) //helloworld
console.log(d) //helloworld
```
-------------
#### 去除头部或尾部空格
* trimStart()：去掉头部空格
* trimEnd()：去掉尾部空格
-------------
### 分割字符串为数组
* split()：按照给定规则分割字符串，返回一个由分割出来的子字符串组成的数组
* 代码展示：
```js
var a = "ni wo ta ha la hei"
console.log(a) 
var b = a.split(" ")
console.log(b) //[ "ni", "wo", "ta", "ha", "la", "hei" ]
var c = "hello"
var d = c.split("")
console.log(d) //[ "h", "e", "l", "l", "o" ]
```
* PS：split()还可以指定第二个参数，为返回数组的最大成员数
------------
## 数组
* 返回数组成员数量：length
```js
var a = [] //定义数组
a[0] = 1 //赋值
a[1] = 2 
console.log(a) // [1,2]
console.log(a.length) // 2
console.log(a[3]) //超出了数组的范围，打印undefined
```
--------------
### 数组的遍历
* for...in：遍历数组
```js
var username = ["1","2","3","4","5"]
for (var i in username){
    console.log(username[i])
}
```
-------------------
### Array.isArray() 判断是否为数组
* 概念：Array.isArray()方法返回一个布尔值，表示参数是否为数组，它可以弥补typeof运算符的不足
* 代码展示：
```js
var username = ["1","2","3","4","5"] //数组
var num = 10 
console.log(Array.isArray(username)) //true
console.log(typeof username) //数组类型应该为Array 但这里返回object
console.log(Array.isArray(num)) //false
```
-------------
### push()/pop() 末端添加/删除元素
#### push()
* 概念：push方法用于在数组的末端添加一个或多个元素，并返回添加新元素后的数组长度，**注意，该方法会改变原数组**
* 代码展示:
```js
var arr = ["hello","world"] 
var mylength = arr.push("!",100,true) //可以随意添加
console.log(arr.length) //5
console.log(arr) //["hello","world","!","100","true"]
```
-------------
#### pop()
* 概念：pop方法用于删除数组的最后一个元素，并返回该元素，**注意：该方法会改变原数组**
* 代码展示：
```js
var arr = ["hello","world"]
arr.pop() //删除最后一个元素
console.log(arr) //["hello"]
```
--------------
### shift()/unshift() 头部添加/删除元素
#### shift()
* 概念：shift方法用于删除数组的第一个元素，并返回该元素，**注意：该方法会改变原数组**
* 代码展示：
```js
var arr = ["hello","world"]
arr.shift() //删除第一个元素
console.log(arr); //["world"]
```
-------------
#### unshift()
* 概念：unshift方法用于在数组的第一个位置添加元素，可以添加多个元素，并返回添加新元素后的数组长度，**注意：该方法会改变原数组**
* 代码展示：
```js
var arr = ["hello","world"]
console.log(arr.unshift("ni","hao")) //输出：4 向数组第一个位置添加两个元素 
console.log(arr); //["ni","hao","hello","world"]
```
-------------
### join() 返回字符串
* 概念：join方法以指定参数作为分隔符，将所有数组成员连接为一个字符串返回，如果不提供参数，默认用逗号分隔
* 代码展示
```js
var a = [10,20,30,40]
console.log(a.join()) //不加参数默认用逗号，间隔 10,20,30,40
console.log(a.join("|")) //10|20|30|40
console.log(a.join(" ")) //10 20 30 40
var b = a.join(" ") // 将转换成的字符串赋值给b
console.log(b.split(" ")) //在将b转换为数组
```
--------------------
### concat() 数组合并
* 概念：concat方法用于多个数组的合并。它将新数组的成员，添加到原数组成员的后部，然后返回一个新数组，原数组不变
* 代码展示：
```js
var a = [1,4,5]
var b = [6,9]
var c = [10,11]
console.log(a.concat(b,c)) //[1,4,5,6,9,10,11]
```
------------
### reverse() 颠倒排列数组
* reverse方法用于颠倒排列数组元素，返回改变后的数组，注意，该方法将改变原数组
* 代码展示：
```js
var a = [1,2,3,4]
a.reverse() //颠倒数组
console.log(a) //[4,3,2,1]
```
--------------
#### 字符串颠倒
* 利用split()将字符串先分割成数组，在通过reverse()将数组翻转，最后在用join()转回字符串
* 代码展示：
```js
var b = "hello world"
var c = b.split("") //将字符转换为数组
console.log(c) 
c.reverse() //翻转数组
console.log(c)
console.log(c.join("")) //将数组转换为字符串输出
```
### indexof() 查找字符
* 与字符串indexof()功能相同
------------
## 函数
* 函数的声明：function命令：function命令声明的代码区块，就是一个函数，function命令后面跟函数名，函数名后面是一对圆括号，里面是传入函数的参数
* JavaScript将函数名视同变量名，所以采用function命令声明函数时，整个函数会像变量声明一样，被提升到代码头部
---------------
### 函数参数
* 函数运行时，有时候需要提供外部参数，不同的外部数据会得到不同的结果，这种外部数据就叫做参数，例如：
```js
function add(x,y){ //函数
    console.log(x+y) //作用
}
add(1,3) //调用 4
add(2,5) // 7
```
---------------
## 对象
* 概念：对象(object)是JavaScript语言的核心概念，也是最重要的数据类型，简单说，对象就是一组"键值对"(key-value)的集合，是一种无序的复合数据集合
* 代码展示：
```js
var a = { //定义一个对象
    name : "lili", //字符串
    age : 13, //数值
    jos :[1,2,3,4],  //数组
    fun:function(){ //函数
        console.log("haha")
    }
}
console.log(a.name) //对象名.对象调用
a.fun() //调用函数
```
--------------------
### Math对象
* Math是JavaScript的原生对象，提供各种数学功能
---------------
#### Math.abs() 返回绝对值
* 返回参数值的绝对值，例如：
```js
Math.abs(1) // 1
Math.abs(-1) // 1
```
----------------
#### Math.max(),Math.min()
* 返回参数之中最大的值和最小的值，如果参数为空，Math.min返回lnfinity，Math.max返回-lnfinity，例如：
```js
Math.max(10,20,50,88) // 88
Math.min(20,10,70,5) // 5
```
---------------
#### Math.floor(),Math.ceil() 返回整数
##### Math.floor()
* 返回小于参数值的最大整数，例如：
```js
Math.floor(3.2) // 3
Math.floor(-3.2) // 4
```
----------
##### Math.ceil()
* 返回大于参数值的最小整数，例如：
```js
Math.floor(3.2) // 4
Math.floor(-3.2) // -3
```
----------------
#### Math.random()
* 返回0到1之间的一个伪随机数范围`[0,1)`
* 在任意范围生成随机数，代码展示：
```js
function suiji(min,max){
    return Math.rando() * (max-min) + min;
}
var a = suiji(5,10)
console.log(a) //例如 8.430679863952001
```
* 想要取整数就向上或者向下取整
-----------------------
### Date对象
* 概念：Date对象是JavaScript原生的时间库，它以1970年1月1日00:00:00作为时间的零点，可以表示的时间范围是前后各1亿天（单位为毫秒）
------------
#### Date.now()
* 返回当前时间距离时间零点（1970年1月1日00:00:00 UTC）的毫秒数，例如：
```js
console.log(Date.now()) // 例如：1724723753154
```
--------
#### 时间戳
* 时间戳是指格林威治时间1970年1月1日00:00:00（北京时间1970年1月1日08:00:00）起至现在的总秒数，格林威治和北京时间就是时区的不同
* Unix是20世纪70年代初出现的一个操作系统，Unix认为1970年1月1日00:00:00是时间纪元，JavaScript也就遵循了这一约束
* Date对象提供了一系列get*方法，用来获取实例对象某个方面的值，例如：
1. getTime()：返回实例距离1970年1月1日00:00:00的毫秒数
2. getDate()：返回实例对象对应每个月的几号(从1开始)
3. getDay()：返回星期几，星期日为0，星期一为1，以此类推
4. getYear()：返回距离1900的年数
5. getFullYear()：返回四位的年份
6. getMonth()：返回月份（0表示1月，11表示12月）
7. getHours()：返回小时（0~23）
8. getMilliseconds()：返回毫秒（0~999）
9. getMinutes()：返回分钟（0~59）
10. getSeconds()：返回秒（0~59）
---------
#### 网页上显示当前时间
* 代码展示：
```js 
<script>
function mydate(){
    var Year = new Date().getFullYear() 
    var month = new Date().getMonth() + 1
    var day = new Date().getDate()
    var hour = new Date().getHours()
    var minut = new Date().getMinutes()
    var second = new Date().getSeconds()
    if (hour < 10){
        hour = "0" + hour
    }
    if (minut < 10){
        minut = "0" + minut
    }
    if (second < 10){
        second = "0" + second
    }
    document.getElementById("div").innerHTML =  hour + ":" + minut + ":" + second + "<br>" + Year + "-" + month + "-" + day
}
setInterval(mydate,1000)    
</script>
<div id = "div"></div>
```
-----------