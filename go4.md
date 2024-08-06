## 面向对象
### 概念
1.Golang也支持面向对象编程（OOP），但和传统的面向对象编程有区别，并不是纯粹的面向对象语言，所以我们说Golang支持面向对象编程特性是比较准确的
2.Golang没有类（class），Go语言的结构体（struct）和其他编程语言的类（class）有同等的地位，你可以理解Golang是基于struct来实现OOP特性的
3.Golang面向对象编程非常简洁，去掉传统OOP语言的方法重载、构造函数和析构函数、隐藏的this指针等等
4.Golang仍然有面向对象编程的继承，封装和多态的特征，只是实现的方式和其他OOP语言不一样，比如继承Golang没有extends关键，继承是通过匿名字段来实现

------
### 结构体
* 代码展示：
```golang
package main

import "fmt"

type Teacher struct { //定义老师结构体，将老师的各个属性放入结构体中
//变量大写外界可以访问
	Name   string //姓名
	Age    int //年龄 
	School string //学校
}
func main() {
	var t1 Teacher
	t1.Name = "马士兵"
	t1.Age = 45
	t1.School = "清华大学"
	fmt.Println(t1)
}
```
--------
#### 内存分析
* 代码参考上方，t1指向Name,Age,School

----------
#### 结构体创建方式
* 方法一直接创建：
```golang
type Teacher struct { //定义老师结构体，将老师的各个属性放入结构体中
//变量大写外界可以访问
	Name   string //姓名
	Age    int //年龄 
	School string //学校
}
func main() {
	var t1 Teacher
	t1.Name = "马士兵"
	t1.Age = 45
	t1.School = "清华大学"
	fmt.Println(t1)
}
```
* 方法二直接创建：
```golang
type Teacher struct { //定义老师结构体，将老师的各个属性放入结构体中
//变量大写外界可以访问
	Name   string //姓名
	Age    int //年龄 
	School string //学校
}
func main() {
	var t1 Teacher = Teacher{"马士兵", 45, "清华大学"}
	fmt.Println(t1)
}
```
* 方法三返回结构体指针：
```golang
type Teacher struct {
	Name   string
	Age    int
	School string
}

func main() {
	var t *Teacher = new(Teacher)//定义一个指针变量t
//t指向的就是地址，应该给这个地址的指向对象的字段赋值
	(*t).Name = "马士兵"//*的作用，根据地址取值
	(*t).Age = 45
	t.School = "清华大学"//go的编译器将t.School转化成(*t).School = "清华大学"
	fmt.Println(*t)
}
```
* 方法四返回结构体指针：
```golang
type Teacher struct {
	Name   string
	Age    int
	School string
}
func main(){
	var t *Teacher = &Teacher{"马士兵",45,"清华大学"}//一一对应前面所定义的变量
	fmt.Println(*t)
}
```
#### 结构体之间的转换
* 结构体是用户单独定义的类型，和其他类型进行转换时需要有完全相同的字段（名字、个数和类型），例如：
```golang
type Student struct {
	Age int
}
type Person struct {
	Age int
}
func main() {
	var s Student = Student{10} //同下
	var p Person = Person{10} //定义类型并赋值
	s = Student(p) //改为s = p 会报错
	fmt.Println(s)
	fmt.Println(p)
}
```
* 结构体进行type重新定义（相当于取别名），Golang认为是新的数据类型，但是相互间可以强转，例如：
```golang
type Student struct {
	Age int
}
type Stu Student
func main() {
	var s1 Student = Student{10}
	var s2 Stu = Stu{10}
	s1 = Student(s2)//改为s1=s2报错，虽然定义了Student的别名为Stu，但系统底层认为不同
	fmt.Println(s1)
	fmt.Println(s2)
}
```
--------
#### 方法
* 概念：方法是作用在指定的数据类型上和指定的数据类型绑定，因此自定义类型，都可以有方法，而不仅仅是struct
* 方法的声明和调用格式：
```golang
type A struct {
    Num int
}
func (a,A) test() {//相当于A结构体有一个方法叫test
//(a,A)体现方法test和结构体A绑定关系
    fmt.Println(a.num)
}
```
* 代码展示：
```golang
// 定义Person结构体
type Person struct {
	Name string
}
// 给Person结构体绑定方法test
func (p Person) test() { //参数名字随便起
    p.Name = "娜娜"
    fmt.Println(p.Name) //输出娜娜
}
func main() {
	//创建结构体对象
	var p Person
	p.Name = "露露"
	p.test()
    fmt.Println(p.Name) //输出露露
}
```
ps:如果其他类型变量调用test方法一定会报错，结构体对象传入test方法中，值传递，和函数参数传递一致。
##### 注意事项
1. 结构体类型是值类型，在方法调用中，遵守值类型的传递机制，是值拷贝传递方式
2. 如果希望在方法中，改变结构体变量的值，可以通过指针的方式来处理，例如：
```golang
type Person struct {
	Name string
}

// 给Person结构体绑定方法test
func (p *Person) test() { //参数名字随便起
	(*p).Name = "娜娜"//可以简化
	fmt.Println((*p).Name) //输出娜娜，可以简化
}
func main() {
	//创建结构体对象
	var p Person
	p.Name = "露露"
	(&p).test()//可以简化
	fmt.Println(p.Name) //输出娜娜
}
```
ps：底层编译器做了优化，(&p)、(*p),都可以改为p,底层会自动帮我们加上& *
3. Golang中的方法作用在指定的数据类型上的和指定的数据类型绑定，因此自定义类型，都可以有方法，而不仅仅是struct，比如int，float32等都可以有方法
* 代码展示：
```golang
type integer int
func (i integer) print() {
    i = 30
	fmt.Println(i)//i = 30
}
func main() {
	var i integer = 20
	i.print()
    fmt.Println(i)// i = 20
}
```
ps：若想改变i的值就得引用指针变量
4. 方法的访问范围控制的规则和函数一样，方法名首字母小写，只能在本包使用，方法首字母大写，可以再本包和其他包访问。
5. 如果一个类型实现了String()这个方法，那么fmt.Println默认会调用这个变量的String()进行输出以后定义结构体的话，常定义String()作为输出结构体信息的方法，在fmt.Println会自动调用，代码展示：
```golang
type Student struct {
	Name string
	Age  int
}
func (s *Student) String() string {
	str := fmt.Sprintf("Name = %v , Age = %v", s.Name, s.Age)
	return str //返回str
}
func main() {
	stu := Student{
		Name: "lili",
		Age:  20,
	}
	fmt.Println(&stu)//自动调用String
}
```
-------------
##### 方法和函数的区别
1. 绑定数据类型：
方法：需要绑定指定的数据类型
函数：不需要绑定数据类型
2. 调用方式不一样：
函数：函数名(实参列表)
方法：变量.方法名(实参列表)
代码展示：
```golang
type Student struct {
	Name string
}
func (s Student) test01() {//方法
	fmt.Println(s.Name)
}
func method01(s Student) {//函数
	fmt.Println(s.Name)
}
func main() {
	var s Student = Student{"lili"}
	method01(s)//调用函数
	s.test01()//调用方法
}
```
3. 对于函数来说，参数类型对应是什么就要传入什么，例如：
```golang
type Student struct {
	Name string
}
func method02(s *Student) {
	fmt.Println((*s).Name)
}
func method01(s Student) {
	fmt.Println(s.Name)
}
func main() {
	var s Student = Student{"lili"}
	method01(s)
    //method01(&s)错误
	method02(&s)
    //method02(s)错误
}
```
4. 对于方法来说，接收者为值类型，可以传入指针类型，接收者为指针类型，可以传入值类型，例如
```golang
type Student struct {
	Name string
}

func (s Student) test01() {
	fmt.Println(s.Name)
}
func (s *Student) test02() {
	fmt.Println(s.Name)
}

func main() {
	var s Student = Student{"lili"}
	(&s).test01() //都可以正常输出lili。看似是指针，但你传递都是按照值传递
	s.test01()
	s.test02()
	(&s).test02()
}
```
----------
#### 创建结构体时指定字段值
* 方法一：按照顺序赋值操作，例如：
```golang
type Student struct {
	Name string
	Age  int
}
func main() {
	var s1 Student = Student{"lili", 18}
	fmt.Println(s1)
}
```
缺点：必须按照顺序，有局限性
* 方法二：按照指定类型，例如：
```golang
type Student struct {
	Name string
	Age  int
}

func main() {
	var s2 Student = Student{
		Name: "lili",
		Age:  18,
	}
	fmt.Println(s2)
}
```
* 方法三：想要返回结构体的指针类型，例如：
```golang
type Student struct {
	Name string
	Age  int
}
func main() {
	var s3 *Student = &Student{"lili", 18}//定义指针类型
	fmt.Println(*s3)
	var s4 *Student = &Student{
		Name: "lili",
		Age:  18,
	}
	fmt.Println(*s4)
}
```
#### 跨包创建结构体
1. 创建两个不同的包：
![alt text](C:/Users/HYun/Desktop/文件/xxbj/image/image-5.png)
* main.go的内容为：
```golang
import (
	"fmt"
	"goproject/go2/mode1"
)
func main() {
	//var s mode1.Student = mode1.Student{"lili", 18}
	s := mode1.Student{"lili", 18}
	fmt.Println(s)
}
```
* modo1.go的内容为：
```golang
type Student struct {
	Name string
	Age  int
}
```
ps:结构体首字母大写，其他包可以访问，但如果结构体小写想访问的话--》工厂模式，例如：
* modo1.go的内容：
```golang
type student struct {
	Name string
	Age  int
}
func NewStudent(n string, a int) *student {//赋给函数
	return &student{n, a}
}
```
* main.go的内容
```golang
import (
	"fmt"
	"goproject/go2/mode1"
)
func main() {
	s := mode1.NewStudent("nana", 20)//调用函数
	fmt.Println(*s)
}
```
-------
### 封装
* 概念：
1.封装(encapsulation)就是把抽象出的字段和对字段的操作封装在一起，数据被保护在内部，程序的其他包只有通过被授权的操作方法，才能对字段进行操作。
* 好处：
1.隐藏实现细节
2.提可以对数据进行验证，保证安全合理
* Golang如何实现封装
1. 建议将结构体、字段（属性）的首字母小写（其他包不能使用），类似private，实际开发不小写也可能，因为封装没有那么严格
2. 给结构体所在包提供一个工厂模式的函数，首字母大写（类似一个构造函数）
3. 提供一个首字母大写的Set方法（类似其他语言的public），用于对属性判断并赋值：
```go
func (var结构体类型名)SetXxx（参数列表）{
    //加入数据验证的业务逻辑
    var.Age = 参数
}
```
1. 提供一个首字母大写的Get方法（类似其他语言的public），用于获取属性的值：
```go
func (var结构体系类型名)GetXxx()(返回值列表){
    return var.字段；
}
```
代码结构：
![alt text](C:/Users/HYun/Desktop/文件/xxbj/image/image-5.png)
* mode1.go代码展示：
```golang
type person struct {
	Name string
	age  int
}
func NewPerson(name string) *person {
	return &person{
		Name: name,
	}
}
// 定义set和get方法，对age字段进行封装，因为在方法中可以加一系列的限制操作，确保被封装字段的安全合理性
func (p *person) SetAge(age int) {
	if age > 0 && age < 150 {
		p.age = age
	} else {
		fmt.Println("对不起，你传入的年龄范围不正确")
	}
}
func (p *person) GetAge() int {
	return p.age
}
```
* main.go代码展示：
```golang
import (
	"fmt"
	"goproject/go9/mode1"
)
func main() {
	p := mode1.NewPerson("lili")
	p.SetAge(20)
	fmt.Println(p.Name)
	fmt.Println(p.GetAge())
	fmt.Println(*p)
}
```