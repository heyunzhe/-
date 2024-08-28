# JavaScript
## 概念：
1. JavaScript是一种轻量级的脚本语言，所谓“脚本语言”，指的是它不具备开发操作系统的能力，只是用来编写控制其他大型应用程序的“脚本”
2. JavaScript是一种嵌入式（embedded）语言，它本身提供的核心语法不算很多
---------
## 优点
1. 操控浏览器的能力
2. 广泛的使用领域
3. 易学性
---------
## JavaScript与ECMAScript的关系
* ECMAScript和JavaScript的关系是，前者是后者的规格，后者是前者的一种实现，在日常生活中，这两个词是可以互换的
----------
## 语句、标识符
### 语句
* JavaScript程序的单位是行（line），也就是一行一行的执行，一般情况下，每一行就是一个语句，语句以分号结尾，一个分号就表示一个语句结束
---------------
### 标识符
* 标识符（identifier）指的是用来识别各种值的合法名称，最常见的标识符就是变量名
* 标识符是由：字母、美元符号（$）、下划线（_）和数字组成，其中数字不能开头
* PS：中文是合法的标识符，可以用作变量名，但不推荐
---------------
#### 保留关键字
* 不可用于变量的声明
* arguments break case catch class const
* continue debugger default delete do else
* enum eval export extends false finally
* for function if implements import in
* instanceof interface let new null package
* private protected public return static
* super switch this throw true try typeof
* var void while with yield
---------
## js引入到文件
* 写在`<body>`中
1. 直接嵌入到文件：在html的`<script>`标签内中写
2. 引入本地文件：`<script src="./zhangsan.js">`
3. 引入网络来源：`<script src="http://~.js">`
-----------
## js输出方式
1. 在浏览器弹出一个对话框，然后把要输出的内容展示出来：alert("要输出的内容") 
2. 在页面输出： document.write("要输出的内容")
3. 在控制台输出：console.log("要输出的内容")
-------
## 数据类型
### 原始数据类型
* 数值型、字符串、布尔值
```js
var age = 18 //数值
var name = "张三" //字符
var flag = true //布尔
```
---------------
### 合成类型
* 对象
```js
 var user ={
    age:18,
    name:"zhangsan",
    status:false,
}
```
--------------
### 特殊值
* null、undefined
* null一般代表对象“没有”
* undefined一般代表数值“没有”
--------
## 运算符
### typeof
* typeof 变量名：返回变量的数值类型
------------
### 数值运算符
* 自增自减有两种写法：++num num++  ++在前，先自增在运算，++在后，先运算在自增，例如：
```js
var num = 10;
console.log(num++) //10
console.log(num) //11
console.log(++num) // 12
console.log(num) // 12
```
------------
### 比较运算符
#### 相等运算符
1. == ：相等运算符（只比较值不比较类型）
2. === ：严格相等运算符（比较值也比较值得类型）
* 调试：
```js
var num = 10
var num2 = "10"
console.log(num==num2) //true
console.log(num===num2) //false
```
--------
#### 不相等运算符
1. != ：不相等运算符
2. !== ：严格不相等运算符
```js
var num = 10
var num2 = "10"
console.log(num != num2) //false
console.log(num !== num2) //true
```
------------
### 布尔运算符
#### 取反运算符
* 取反运算符：!
```js
!false //true
!true //false
```
---------------
##### 非布尔值取反
* 对于非布尔值，取反运算符会将其转为布尔值，可以这样记忆，以下六个值取反后为true，其他都为false
1. undefined
2. null
3. false
4. 0
5. NaN
6. 空字符串("")
----------
## 条件语句
### if
* 格式
```
if (条件表达式){
    满足这个条件执行结果
}
```
* **条件表达式写在括号里面**
------
### 多个if...else
* 格式
```
if (条件表达式1){
    满足执行结果
}else if (条件表达式2){
    满足执行结果
}else if (条件表达式3){
    满足执行结果
}...else{
    都不满足上面执行结果
}
```
------------------
### swich
* 格式
```
switch (表达式){
    case 值1：
        执行语句1
        break
    case 值2：
        执行语句2
        break
    .......
    case 值n：
        执行语句n
        break
}
```
* PS：每一个执行语句后都得加一个break退出，否则会接下去执行下一个case代码块，而不是跳出switch结构，例如：
```js
var x = 3 
switch (x){
    case 1:
        console.log(1)
    case 2:
        console.log(2)
    case 3:
        console.log(3)
    default :
        console.log(4)
}
```
* 执行结果为3 4
---------------
### 三元运算符
* 该运算符需要三个运算子，其格式为：(条件) ？表达式1 : 表达式2
* 三元运算符可以被视为if...else的简写形式
* 例如：
```js
var a=10
var b=10
    if (a==b){
        console.log("yes") //输出
    }else{
        console.log("no")
    }
var result = a==b ? "yes" : "no"; //等价于上方
console.log(result) //yes
```
### for循环
* 打印99乘法表
```js
var sum =0
for (var i = 1;i<=9;i++){
    document.write("<br/>")
    for (var j = 1;j<=i;j++){
        sum = i*j
        document.write(j + "*" + i + "=" + sum + " ")
    }
}
```
----------
### while循环
```js
var i = 0;
while (i<=100){ 
    console.log(i) //满足条件执行
    i++
    }
```
------------
### break和continue
* 二者都具有跳转作用，可以让代码不按照既有的顺序执行
* break：退出整个循环
* continue：退出本次循环，继续下次
----------
## 图书管理系统目前需要解决的问题
1. 登录界面：点击登录按钮时，若用户输入的账号密码都正确，弹出登录成功提示框，并进入主界面，当输入的不对，弹出提示框，需重新输入，注册界面同理
2. 主界面：点击查看图书跳转到新网页，网页有几个输入框按钮，输入框可以输入，编号范围或者书名来查看书，也可以查看所有书
3. 添加图书：添加成功图书后跳出成功提示框，失败跳出错误提示框，但都保留在主界面
