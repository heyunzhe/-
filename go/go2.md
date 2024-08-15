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

--------
##### 关键字
1. **break**：结束正在执行的循环(离他最近的循环)，如果想结束指定循环，可以给这个循环添加一个标签，然后break 标签名 就可以结束指定循环，定义标签直接写在语句上面例如：
label1://标签（记得加:）
for ...
break label1
2. **continue**：结束本次循环，继续下次循环（离他最近的循环），想指定结束也是使用标签：continue 标签名
3. **goto**：无条件的转移到程序中指定的行，一般不建议使用，例如：
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

-------
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
## 函数
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
-------
### 函数细节点：
#### 基础模块
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
	test(6,7,14)//输出6,输出7，输出14
}
```

---------
#### 数据类型模块
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

----------
### 包的引入
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
------
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
	abc "goproject/go/crm/aaaa" //给aaaa包起别名为abc
)
func main() {
	aaaa.Test()//若不将aaaa改为abc运行时将会报错
	fmt.Println("zheshiyige")
}
```

--------
### init函数
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

-------
### 匿名函数
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
---------
#### 闭包
* 概念：闭包就是一个函数和与其相关的引用环境组合的一个整体，例如：
```golang
func getSum() func(int) int {//返回值为一个函数，这个函数的参数是一个int类型的参数，返回值也是int类型
	var sum int = 0
	return func(num int) int { //返回一个匿名函数
		sum = sum + num //求和
		return sum //返回sum
	}
}//闭包：返回的匿名函数+匿名函数以外的变量num
func main() {
	f := getSum()//调用函数
	fmt.Println(f(1))//把1赋给num，sum=1
	fmt.Println(f(2))//把2赋给num，由于上面将sum变成1了，这里结果sum=3
}
```
ps：匿名函数中引用的那个变量会一直保存在内存中，可以一直使用
**闭包的本质：**
* 闭包本质依旧是一个匿名函数，只是这个函数引入外界的变量/参数
* 匿名函数 + 引用的变量/参数 = 闭包
**闭包的特点：**
* 返回的是一个匿名函数，但是这个匿名函数引用到函数外的变量/参数，因此这个匿名函数就和变量/参数形成一个整体，构成闭包。
* 闭包中使用的变量/参数会一直保存在内存中，所以会一直使用--》意味着闭包不可滥用（对内存消耗大）
**闭包的作用：**
* 不使用闭包的时候，我想保留的值，不可以反复使用
* 闭包应用场景:闭包可以保留上次引用的某个值，我们传入一次就可以反复使用了

--------
### 关键字
#### defer关键字：
* 概念：在函数中，程序员经常需要创建资源，为了在函数执行完毕后，及时的释放资源，Go的设计者提供defer关键字
* 在Golang中，程序遇到defer关键字，不会立即执行defer后的语句，而是将defer后的语句压入下一个栈中，继续执行函数后面的语句，例如：
```golang
func main(){
	fmt.Println(add(30,60))//4.最后一个输出
}
func add(num1 int,num2 int) int {
	defer fmt.Println(num1)//3.第三个输出
	defer fmt.Println(num2)//2.第二个输出
//栈的特点是先进后出，在这个函数执行完毕后，再从栈中取出语句开始执行
	var sum int = num1 + num2
	fmt.Println(sum)//1.最先输出
	return sum
}
```
* defer拓展，例如：
```golang
func main() {
	fmt.Println(add(30, 60))//输出sum=260
}
func add(num1 int, num2 int) int {
	defer fmt.Println(num1) //输出num1=30
	defer fmt.Println(num2)//输出num2=60
	num1 += 90
	num2 += 80
	var sum int = num1 + num2
	fmt.Println(sum) //输出sum=260
	return sum
}
```
ps:遇到defer关键字，会将后面的代码存入栈中，也同时会将值拷贝进栈中，不会随着函数后面的变化而变化
* defer应用场景：
当你要关闭某个使用的资源，在使用的时候直接随手defer，因为defer有延迟机制（函数执行完毕再执行defer压入栈的语句），所以用完时随手写了关闭，比较省心，省事

----------
### 系统函数
#### 字符串函数
1. 统计字符串长度：len(str) 
ps:使用内置函数不需要导包，可以直接用
2. 字符串遍历：
方法1.r:=[]rune(str) 例如：
```golang
var str string = "abcd呵呵"
r := []rune(str)
for i := 0; i < len(r); i++ {
	fmt.Printf("%c\n", r[i])
}
```
方法2.for -range健值循环，例如：
```golang
var str string = "abcd呵呵"
for i, value := range str {
	fmt.Printf("索引值为：%d，具体的值为：%c \n", i, value)
}
```
-------
注意：使用以下这两种方式需要导入**strconv**包才能使用
1. 字符串转整数：n,err:= strconv.Atoi("66") 例如：
```golang
num1, _ := strconv.Atoi("6666")
fmt.Printf("%T", num1) //输出int
```
1. 整数转字符串：str = strconv.ltoa(6887) 例如：
```golang
str1 := strconv.Itoa(88)
fmt.Printf("%T", str)//输出string
```
--------
**注意**：使用以下这几种方式的需要导入**strings**包才能使用
1. 查找子串是否在指定字符串中，例如：
```golang
x := strings.Contains("goatoayes", "go")//判断go在不在前面的字符串中
fmt.Println(x)//输出true
```
2. 统计一个字符串有几个指定子串，例如：
```golang
y := strings.Count("golango", "go")//判断go在前面的字符串出现了几次
fmt.Println(y)//输出2
```
3. 不区分大小写的字符串比较,例如：
```golang
z := strings.EqualFold("hello", "HELLO")//判断是否相等
fmt.Println(z)//输出ture
```
4. 返回子串在字符串中第一次出现的索引值，如果没有返回-1，例如：
```golang
p := strings.Index("golanggoto", "go")//判断go在前面的字符串中第一次出现的索引值
fmt.Println(p)//输出0
```
5. 字符串的替换，例如：
```golang
a1 := strings.Replace("golanggotogorun", "go", "11", -1)//在前面的字符串中查找并替换go为11，-1表示全部替换
fmt.Println(a1)//输出11lang11to11run
a2 := strings.Replace("golanggotogorun", "go", "11", 2)//同上，2表示替换两个
fmt.Println(a2)//输出11lang11togorun
```
6.  按照指定的某个字符，为分割标识，将一个字符串拆分成字符串数组，例如：
```golang
a3 := strings.Split("go-python-java", "-")//根据"-"切割
fmt.Println(a3)//输出[go python java]
```
7.  将字符串的字母进行大小写转换，例如：
```golang
a5 := strings.ToLower("Go")//转换为小写
fmt.Println(a5)//输出go
a6 := strings.ToUpper("go")//转换为大写
fmt.Println(a6)//输出GO
```
8.  将字符串左右两边的空格去掉，例如：
```golang
a7 := strings.TrimSpace("  go and java    ")
fmt.Println(a7)//输出go and java
```
9.  将字符串左右两边指定的字符去掉，例如：
```golang
a8 := strings.Trim("--golang--", "-")
fmt.Println(a8)//输出golang
```
10.   将字符串左边指定的字符去掉，例如：
```golang
a9 := strings.TrimLeft("--golang--", "-")
fmt.Println(a9)//输出golang--
```
11.   将字符串右边指定的字符去掉，例如：
```golang
a10 := strings.TrimRight("--golang--", "-")
fmt.Println(a10)//输出--golang
```
12.   判断字符串是否以指定的字符串开头，例如：
```golang
a11 := strings.HasPrefix("image.jpg", "image")
fmt.Println(a11)//输出true
```
13.   判断字符串是否以指定的字符串结尾，例如：
```golang
a12 := strings.HasSuffix("image.jpg", "jpg")
fmt.Println(a12)//输出true
```
---------
#### 日期和时间函数
注意：需要导入**time**包
* 获取当前时间，调用Now函数，例如：
```golang
func main() {
	now := time.Now()  //Now()返回值是一个结构体
	fmt.Printf("%v ~~~ 对应的类型为：%T\n", now, now) //输出2024-08-01 13:18:28.006164325 +0800 CST m=+0.000041700 ~~~ 对应的类型为：time.Time
	fmt.Printf("年：%v \n", now.Year())//输出当前年份
	fmt.Printf("月：%v \n", now.Month())//输出当前月份（英文显示）
	fmt.Printf("月：%v \n", int(now.Month()))//输出当前月份
	fmt.Printf("日：%v \n", now.Day())//输出当前日期
	fmt.Printf("时：%v \n", now.Hour())//小时
	fmt.Printf("分：%v \n", now.Minute())//分钟
	fmt.Printf("秒：%v \n", now.Second())//秒
}
```
* 日期的格式化
1. 将日期以年月日时分秒按照格式输出为字符串，例如：
```golang
// Printf将字符穿直接输出
fmt.Printf("当前年月日:%d-%d-%d 时分秒：%d:%d:%d  \n", now.Year(), now.Month(),now.Day(), now.Hour(), now.Minute(), now.Second())
//Sprintf可以得到这个字符串，以便后续使用
datestr := fmt.Sprintf("当前年月日:%d-%d-%d 时分秒:%d:%d:%d  \n", now.Year(),now.Month(),now.Day(), now.Hour(), now.Minute(), now.Second())
fmt.Println(datestr)
```
2.按照指定格式输出，例如：
```golang
//这个参数字符串的各个数字必须固定，必须这样写
datestr2 := now.Format("2006/01/02 15/04/05")
fmt.Println(datestr2)
//可以选择任意组合，但数字不能变
datestr3 := now.Format("2006 15:04")
fmt.Println(datestr3)
```
-------
#### 内置函数
* 概念：Golang设计者为了编程方便，提供了一些函数，这些函数不用导包可以直接使用，我们称为Go的内置函数/内建函数
* 内置函数存放的位置在builtin包下
* 常用函数
1. len函数：统计字符串长度，按字节进行统计，例如：
```golang
func main() {
	var str string = "golang"
	fmt.Println(len(str))//输出结果为6
}
```
2. new函数：分配内存，主要用来分配值类型（int、float、bool、string、数组和结构体struct）,例如：
```golang
func main() {
	num := new(int)
	fmt.Printf("num的类型是：%T，num的值是：%v，num的地址：%v,num指针指向的值是：%v",num, num, &num, *num)
}
```
输出结果为：num的类型是：*int，num的值是：0xc0000120d8，num的地址：0xc000048028,num指针指向的值是：0 
说明：num是一个指针变量，指针里存放的就是num的值，但指针本身也有一个内存地址，所以num的地址就是他的储存地址，指针指向的值就是num的值所以为0

3. make函数：分配内存，主要用来分配引用类型（指针、slice切片、map、管道chan、interface等）
------
