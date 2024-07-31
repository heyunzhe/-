## 流程控制
* 概念：流程控制语句是用来控制程序中各语句执行顺序的语句，可以把语句组合成能完成一定功能的小逻辑模块。
* 控制语句可分为三类：顺序、选择和循环
1. 顺序结构：先执行a，在执行b
2. 条件判断结构：如果...，则...
3. 循环结构：如果...则再继续...

------
### 分支结构
#### 单分支
* 基本语法：
```go
if 条件表达式 {
    逻辑代码
}
```
**注意：** if后要跟空格在写条件表达式写完条件表达式一定要有{ }

------
#### 双分支
* 基本语法：
```
if 条件表达式 {
    逻辑代码1
}
else {
    逻辑代码2
}
```
ps：当表达式成立执行逻辑代码1，不成立执行逻辑代码2

------
#### 多分支
* 基本语法
```golang
if 条件表达式1 {
    逻辑代码1
} else if 条件表达式2{
    逻辑代码2
}...
else{
    逻辑代码n
}
```
ps：哪个表达式成立，就执行对应的逻辑代码，都不成立就执行逻辑代码n

------
#### switch分支
* 基本语法
```golang
switch 表达式(即：常量、变量、一个有返回值的函数都行) {
	case 值1，值2...:
        语句块1
    case 值3，值4...:
		语句块2
    ...
    default:
        语句块
```
**注意**：
1. defalut不是必须的，defalut表示如果上面case都没执行到就执行defalut，位置随意
2. case后面跟的值不可重复，值之间用，隔开
3. case后面的值的数据类型必须与switch一致

**注意：** 写完代码自己试着读一读逻辑通不通顺

-------
### 循环结构
#### for循环
* 基本语法：
```golang
for 初始表达式；布尔表达式(条件判断)；迭代因子{
    循环体；//反复执行的内容
}
```
ps：
1. for的初始表达式不能用var定义变量的形式，要用:=
2. for后面不加东西可以进入死循环
3. for后跟着的三个表达式可以不写，但要加上；
##### 关键字
1. **break**：结束正在执行的循环(离他最近的循环)，如果想结束指定循环，可以给这个循环添加一个标签，然后break 标签名 就可以结束指定循环，定义标签直接写在语句上面例如：
label1://标签（记得加:）
for ...
break label1
1. **continue**：结束本次循环，继续下次循环（离他最近的循环），想指定结束也是使用标签：continue 标签名
2. **goto**：无条件的转移到程序中指定的行，一般不建议使用，例如：
```golang
fmt.Println("1")
fmt.Println("2")
goto label1
fmt.Println("3")
fmt.Println("4")
label1:
fmt.Println("5")
fmt.Println("6")
```
**ps**：程序执行将会输出:1 2 5 6 因为goto跳到了label1跳过了两条指令，所以这两条指令不输出
4.**return**：结束当前函数
#### for range结构介绍
* 概念：for range 结构是Go语言特有的一种迭代结构，在许多情况下都非常有用，for range 可以遍历数组、切片、字符串、map及通道，for range 语法上类似于其他语言中的foreach语句
* 基本语法
```golang
for key,val:= range coll{

}
```
**如**：
```golang
var a string = "Hello Golang你好"
for i , value := range a{
		fmt.Printf("索引为: %d,具体的值为: %c \n",i,value)
    }
```
//对a进行遍历，遍历每个结果的索引值被i接收，每个结果的具体数值被value接收
//遍历对字符进行遍历

-------
### 函数
* 基本语法：
```golang
func 函数名 (形参列表) (返回值类型列表){
    执行语句
    return 返回值列表
}
```
例如：
```golang
func cal(num1 int) int { //自定义函数名字如cal（形参）
	var sum int = 1 //执行语句
	for i := 1; i <= num1; i++ {//执行语句
		sum *= i//执行语句
	}
	return sum //返回值
}
func main() {
	sum := cal(10) //调用函数 （实参）
	fmt.Println(sum)//输出
}
```
#### 函数细节点：
##### 基础模块
1. 函数和函数是并列的关系，所以我们定义的函数不能写在main函数中(匿名函数可以写在main中)
2. 函数名：驼峰命名，首位不能为数字，首字母大写可以被本包和其他包调用，首字母小写只能被本包用，不可被其他包文件调用。（可以参照标识符模块）
3. 形参列表(参数)：可以是一个参数，也可以是n个参数中间用，间隔，也可以是0个参数，用于接受外来数据  
4. 实际参数：实际传入的数据
5. 返回值类型列表：函数的返回值对应的类型应该写在这个列表中 
* 返回0个：可以不写反回值列表类型，不写return 返回值
* 返回1个：需要写返回值类型列表，写return 返回值，但返回值列表中的()可以省略
* 返回多个：返回几个写几个返回值类型列表和返回值例如：
```golang
func cal(num1 int, num2 int) (int, int) {
	var sum int = num1 + num2
	var x int = num1 - num2
	return sum, x
}
func main() {
	var a int
	var b int
	fmt.Scanf("%d %d", &a, &b)
	c, d := cal(a, b)//如果不想接收某个值可以用_忽略例如：c,_ := cal(a,b) 这样就只会收sum，下划线功能具体参考标识符
	fmt.Println(c)
	fmt.Println(d)
}
```
6. go函数不支持重载：不能定义两个相同名字的函数
7. go中支持可变参数 例如：func test (nums...int) //定义一个函数，函数的参数为：可变参数...参数的数量可变，可以传入任意个int类型的数据。函数内部处理可变参数的时候，将可变参数当做切片来处理，例如：
```golang
package main

import "fmt"

func test(nums ...int) {
	for i := 0; i < len(nums); i++ {//遍历可变参数
		fmt.Println(nums[i])
	}
}

func main() {
	test(8)//调用函数输出8
	test(3)//同上输出3
	test(6)//同上输出6
}
```
##### 数据类型模块
8. 基本数据类型和数组默认都是值传递的，即进行值拷贝。在函数内修改，不会影响到原来的值，例如：
```golang
func test(num int){
	num = 30
	fmt.Println("test---",num)//num=30
}
func main() {
	var num int = 10
	test(num)
	fmt.Println("main---",num)// num=10
}
```
9. 以值传递方式的数据类型，如果希望在函数内的变量能修改函数外的变量，可以传入变量的地址&，函数内以指针的方式操作变量，从效果来看类似引用传递，例如：
```golang    
    func test(num *int) {
	*num = 30//指针变量

}

func main() {
	var num int = 10
	fmt.Println(&num)//输出num存放的地址
	test(&num)//执行函数test将num的指针指向30
	fmt.Println(num)//输出30
}
```
10. 在go中，函数也是一种数据类型，可以赋值给一个变量，则该变量就是一个函数类型的变量了。通过该变量可以对函数调用
```golang
func shu(num int) {
	fmt.Println(num)
}
func main() {
	a := shu //定义a为shu
	fmt.Printf("a %T , shu %T\n", a, shu)//输出的%T都为func(int)
	a(10)//等价于 shu(10)
}
```
11. 函数既然是一种数据类型，因此在go中，函数可以作为形参，并且调用(把函数本身当做一种数据类型)例如：
```golang
func shu(num int) {
	fmt.Println(num)
}
func chu(a int, b float32, c func(int)) {// 可以定义func(int)类型
	fmt.Println("OK")
}
func main() {
	a := shu //定义a为shu
	a(10)//等价于shu(10)
	chu(1, 3.14, a)//可以调用函数并正常输出OK
	chu(1, 3.14, shu)//同上
}
```
12. go支持自定义数据类型
* 基本语法 type 自定义数据类型名 数据类型，就是给数据类型起一个别名，例如：
```golang
func main() {
	type a int //起名一个类型a为int类型
	var x a = 10//定义x变量为a类型
	var y int = 20//定义y变量为int类型
	c := x + a(y)//若改为y不是a(y)将会报错，或者改为int(x) + y
    【ps：虽然说是别名，但在go的编译识别的时候还是会认为a和int不是同一种数据类型】
	fmt.Println(c)//输出30
}
```
* 也支持给函数数据类型起别名，例如：
```golang
func test(num int) { 
	fmt.Println("OK")
}
type myfunc func(int) //起一个名为myfunc的func(int)变量
func test1(a int, b float32, c myfunc) { //调用myfunc变量
	fmt.Println("ok")//输出ok
}
func main() {
	test1(1, 0.9, test)//test变量类型为myfunc，可以调用函数
}
```
13. 支持对函数返回值命名
* **传统写法**要求：返回值和返回值的类型对应顺序不能变
```golang
func cal(num1 int, num2 int) (int, int) {
	var sum int = num1 + num2
	var x int = num1 - num2
	return sum, x
}
```
* **升级写法**：对函数返回值命名，里面的顺序就无所谓了，不需要11对应
```golang
func cal(num1 int, num2 int) (sum int, x int) {
	x := num1 - num2
    sum := num1 + num2 
	return
}
```
#### 包的引入
* 为什么用包？
1. 我们不可能把所有的函数文件放在同一个源文件夹中，可以分门别类的把韩式放在不同的原文件中
2. 解决同名问题，当两个人都想定义一个同名的函数，在同一个文件中是不可以定义相同名字的函数的，此时可以用包来区分
导入一个包例如：
```golang
package main
import (
	"fmt"
	"goproject/go/crm/aaaa"  //导入包所在的位置在linux中一般是安装go的目录的src处，将go文件移动到src就可以调用，导入时可以省略/src/前面的内容，只要后面的   
)
func main() {
	aaaa.Test()//调用外部包函数，ps：要调用外部包，外部包开头字母必须要**大写**
	fmt.Println("zheshiyige")
}
```
#### 包的细节点
1. package 进行包的声明，建议：包的声明这个包和所在的文件夹同名
2. main包是程序的入口，一般main函数会放在这个包下 
ps：main函数一定要放在main包下，否则不能编译执行
3. 打包语法：package 包名
4. 引入包语法：import "包的路径"例如：import "goproject/go/crm/aaaa"
5. 在函数调用的时候前面要定位到所在的包例如：aaaa.Test()
6. 首字母大写，函数才能被其他包访问
7. 一个目录下不能有重复的函数
8. 包和文件夹的名字可以不一样
9. 一个目录下的同级文件归属一个包，同级别的源文件的包的声明必须一致
10. 包：在程序层面，所有使用相同package包名的源文件组成的代码模块
11. 包：在源文件层面就是一个文件夹
12. 可以给包起别名，但原先的名就不能使用了,例如：
```golang
import (
	"fmt"
	abc "goproject/go/crm/aaaa" //给aaaa包起别名为text
)
func main() {
	aaaa.Test()//若不将aaaa改为abc运行时将会报错
	fmt.Println("zheshiyige")
}
```
#### init函数
* 概念：初始化函数，可以用来进行一些初始化的操作，每一个源文件都可以包含一个init函数，该函数会在main函数执行前，被Go运行框架调用
1. 当全局变量定义，init函数，main函数的执行流程，例如：
``` golang
package main

import "fmt"

var num int = test()//1.先执行全局变量的定义

func init() { //2.init函数的调用
	fmt.Println("ok")//第二个输出
}

func main() { // 3.main函数的调用
	fmt.Println("OK")//最后一个输出
}

func test() int {
	fmt.Println("ok!")//最先输出
	return 10
}
```
ps：先执行全局定义输出test函数，再然后执行init，最后执行main函数
2. 当多个源文件都有init函数的时候，例如：
* main.go文件：
```golang
import (
	"fmt"
	"goproject/go/crm/aaaa"
)
var num int = test()//3.执行

func init() { //4执行
	fmt.Println("ok")
}
func main() { //5执行
	fmt.Println("OK")
	fmt.Println(aaaa.Age)
	fmt.Println(aaaa.Sex)
	fmt.Println(aaaa.Name)
}
func test() int { 
	fmt.Println("ok!")
	return 10
}
```
* aaaa.go文件：
```golang
package aaaa

import "fmt"

var Age int
var Sex string  //1.最先执行
var Name string

func init() { //2.执行
	fmt.Println("执行")
	Age = 19
	Sex = "男"
	Name = "波波"
}
```
ps：先执行aaaa.go文件里的变量定义然后在执行aaaa.go文件里的init，再然后执行main.go文件里的变量定义，然后执行init函数，最后main执行函数

#### 匿名函数
* 概念：如果某个函数只是希望使用一次，可以考虑用匿名函数
1. 在定义匿名函数时就直接调用，这种方式匿名函数只能调用一次，例如：
```golang
func main() {
	age := func(num1 int, num2 int) int { //匿名函数
		return num1 + num2 //返回值
	}(10, 20)//给num1和num2的值
	fmt.Println(age)//输出
}
```
2. 将匿名函数赋给一个变量,再通过变量来调用匿名函数（用得少），例如：
```golang
func main{
sub := func(num1 int, num2 int) int { //sub等价于匿名函数
		return num1 - num2
	}
	age := sub(30, 70)//直接调用sub
	fmt.Println(age)
}
```
3. 让匿名函数在全局有效,给他定义一个全局变量就行，例如：
```golang
package main
import "fmt"
var Func01 = func(num1 int, num2 int) int { //全局变量
	return num1 * num2
}
func main() {
	age := Func01(10, 2)
	fmt.Println(age)
}
```
------
### 拓展：
1. 从键盘上输入一个数
格式：
```golang
var x int
fmt.Scanln(&x)//方法1
fmt.Scanf("%d",&x)//方法2
```
2. len(str):可以得出字符串长度