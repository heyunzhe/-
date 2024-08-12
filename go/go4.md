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
	(*t).Name = "马士兵"//*的作用，根据地址取值，不加也不影响
	(*t).Age = 45
	t.School = "清华大学"//go的编译器将t.School转化成(*t).School = "清华大学"
	fmt.Println(*t)//加*输出内容，不加输出地址
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
	fmt.Println(*t)//加*输出内容，不加输出地址
}
```
-----------
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

---------
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
-----------
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
#### 封装
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
----------
### 继承
* 概念：当多个结构体存在相同的属性（字段）和方法时，可以从这些结构体中抽象出结构体，在该结构体中定义这些相同的属性和方法，其它的结构体不需要重新定义这些属性和方法，只需嵌套一个匿名结构体即可。也就是说在Golang中，如果一个struct嵌套了另一个匿名结构体，那么这个结构体可以直接访问匿名结构体的字段和方法，从而实现了继承特征。
* 代码展示：
```golang
//定义动物结构体
type Animal struct {
	Age    int
	Weight float32
}
//给Animal绑定方法
func (an *Animal) Shout() {
	fmt.Println("我可以大声喊叫")
}
//绑定方法
func (an *Animal) ShoutInfo() {
	fmt.Printf("动物的年龄是：%v,动物的体重是：%v\n", an.Age, an.Weight)
}
//定义结构体Cat
type Cat struct {
	//为了提高复用性，体现继承思维，嵌入匿名结构体：--》将Animal中的字段和方法都达到复用
	Animal
}
//对Cat绑定特有方法
func (c *Cat) mao() {
	fmt.Println("我是小猫")
}
func main() {
	cat := &Cat{}
	cat.Animal.Age = 3
	cat.Animal.Weight = 10.6
	cat.Animal.Shout()
	cat.Animal.ShoutInfo()
	cat.mao()
}
```
* 优点：提高了代码的复用性，扩展性
---------
#### 注意事项
1. 结构体可以使用嵌套匿名结构体所有的字段和方法，即：首字母大写或小写的字段，方法，都可以使用
2. 匿名结构体字段访问可以简化，例如：
```golang
func main() {
	cat := &Cat{}//创建cat结构体示例 
	cat.Age = 3 //等价于cat.Animal.Age = 3
	cat.Weight = 10.6//等价于cat.Animal.Weight = 10.6
	cat.Shout()//等价于cat.Animal.Shout()
	cat.ShoutInfo()//等价于cat.Animal.ShoutInfo()
	cat.mao()
}
```
ps：cat.Age--->cat对应的结构体中找是否有Age字段，如果有就直接使用，如果没有就去找嵌入的结构体类型中的Age
3. 当结构体和匿名结构体有相同的字段或方法时，编辑器采用就近访问原则访问，如果希望访问匿名结构体的字段和方法，可以通过匿名结构体名来区分，例如
```golang
type Animal struct {
	Age    int
	Weight float32
}
func (an *Animal) ShoutInfo() {
	fmt.Printf("动物的年龄是：%v,动物的体重是：%v\n", an.Age, an.Weight)
}
type Cat struct {
	Animal
	Age int
}
func (c *Cat) ShoutInfo() {
	fmt.Printf("动物的年龄是：%v,动物的体重是：%v\n", c.Age, c.Weight)
}
func main() {
	cat := &Cat{}
	cat.Weight = 9.4 //等价于cat.Animal.Weight = 9.4
	cat.Age = 10 //判定是给Cat结构体的age赋值
	cat.Animal.Age = 20//给Animal结构体的age赋值
	cat.ShoutInfo()//动物的年龄是：10,动物的体重是：9.4
	cat.Animal.ShoutInfo()//动物的年龄是：20,动物的体重是：9.4
}
```
4. Golang中支持多继承：如一个结构体嵌套了多个匿名结构体，那么该结构体可以直接访问嵌套的匿名结构体的字段和方法，从而实现多重继承，例如：
```golang
type A struct {
	a int
	b string
}
type B struct {
	c int
	d string
}
type C struct {
	A
	B
}
func main() {
	c := C{A{10, "aaa"}, B{20, "ccc"}}
	fmt.Println(c)
}
```
5. 如嵌入的匿名结构体有相同的字段名或方法名，则在访问时，需要通过匿名结构体类型名来区分，例如：
```golang
type A struct {
	a int
	b string
}
type B struct {
	c int
	d string
	a int
}
type C struct {
	A
	B
}
func main() {
	c := C{A{10, "aaa"}, B{20, "ccc", 70}}
	fmt.Println(c.b)//aaa
	fmt.Println(c.d)//ccc
	fmt.Println(c.A.a)//10 有俩相同字段必须要指定访问不能c.a
	fmt.Println(c.B.a)//70
}
```
6. 结构体的匿名字段可以使基本数据类型,例如：
```golang
type A struct {
	a int
	b string
}
type B struct {
	c int
	d string
	a int
}
type C struct {
	A
	B
	int //数据类型
}
func main() {
	c := C{A{10, "aaa"}, B{20, "ccc", 70}, 888}//赋值888
	fmt.Println(c.b)
	fmt.Println(c.d)
	fmt.Println(c.A.a)
	fmt.Println(c.B.a)
	fmt.Println(c.int)//888
}
```
7. 嵌套匿名结构体后，也可以在创建结构体变量（实施）时，直接指定各个匿名结构体字段的值，例如：
```golang
func main() {
	c := C{   //指定赋值
		A{
			a: 10,
			b: "aaa"},
		B{ 
			c: 20,
			d: "ccc",
			a: 70},
		888}
	fmt.Println(c.b)
	fmt.Println(c.d)
	fmt.Println(c.A.a)
	fmt.Println(c.B.a)
	fmt.Println(c.int)
}
```
8. 嵌入函数结构体的指针也是可以的，例如：
```golang
type A struct {
	a int
	b string
}
type B struct {
	c int
	d string
	a int
}
type C1 struct {
	*A //指针结构体
	*B
	int
}
func main() {
	c1 := C1{&A{10, "aaa"}, &B{20, "ccc", 50}, 888}
	fmt.Println(c1)//输出地址
	fmt.Println(*c1.A)//输出A
	fmt.Println(*c1.B)//输出B
}
```
9. 结构体的字段可以是结构体类型的（组合模式），例如：
```golang
type B struct {
	c int
	d string
	a int
}
type D struct {
	a int
	b string
	c B //组合模式
}
func main() {
	d := D{10, "ooo", B{20, "bbb", 30}}
	fmt.Println(d) 
	fmt.Println(d.c.d) //取D结构中的c包含B结构中的d，输出bbb
}
```
---------
### 接口
代码展示：
```golang
type SayHello interface {//定义接口
	sayHello()//声明没有实现的方法
}

type Chinese struct {//结构体
}

func (person Chinese) sayHello() {//实现接口方法
	fmt.Println("你好")
}

type USA struct {
}

func (person USA) sayHello() {
	fmt.Println("hello")
}

func greet(s SayHello) {//接收SayHello接口能力的变量
	s.sayHello()
}

func main() {
	c := Chinese{}
	u := USA{}
	greet(u)
	greet(c)
}
```
* 总结
1.接口中可以定义一组方法，但不需要实现，不需要方法体。并且接口中不能包含任何变量。到某个自定义类型要使用的时候（实现接口的时候），再根据具体情况把这些方法具体实现出来
2.实现接口要实现所有的方法才是实现
3.Golang中的接口，不需要显示的实现接口
4.Golang中实现接口是基于方法的，不是基于接口的
--------
#### 注意事项
1. 接口本身不能创建实例，但是可以指向一个实现了该接口的自定义类型的变量，例如
```golang
func main() {
	c := Chinese{}
	u := USA{}
	greet(u)
	greet(c)
	//直接用接口创建实例，出错
	// var s SayHello
	// s.sayHello()
	var s SayHello = c //定义接口赋值一个实现的接口
	s.sayHello()//调用接口方法
}
```
2. 只要是自定义类型数据，都可以实现接口，不仅仅是结构体，例如：
```golang
type hello int
func (i hello) sayHello() {
	fmt.Println("say hi +", i)
}
func main() {
	var i hello = 10
	var s SayHello = i
	s.sayHello()
}
```
3. 一个自定义类型可以实现多个接口，例如：
```golang
type Hello interface {//接口
	a()
}

type Hi interface {//接口
	b()
}

type stu struct { //一个类型
}

func (s stu) a() { //方法实现
	fmt.Println("123")
}

func (s stu) b() {
	fmt.Println("abc")
}

func main() {
	var a stu 
	var b Hello = a
	var c Hi = a
	b.a()
	c.b()
}
```
4. 一个接口（比如A接口）可以继承多个别的接口（比如B，C接口），这时如果要实现A接口，也必须将B，C接口的方法也全部实现，例如：
```golang
type Cat interface { //接口1
	c()
}
type Book interface {//接口2
	b()
}
type Apple interface {//接口3
	Book
	Cat
	a()
}
type Stu struct { //定义结构体
}
func (s Stu) a() { //方法
	fmt.Println("a")
}
func (s Stu) b() {
	fmt.Println("b")
}
func (s Stu) c() {
	fmt.Println("c")
}
func main() {
	var s Stu
	var a Apple = s
	a.a()
	a.b()
	a.c()
}
```
5. 接口（interface）类型默认是一个指针（引用类型），如果没有对interface初始化就使用，那么会输出nil
6. 空接口没有任何方法，所以可以理解为所有类型都实现了空接口，也可以理解为我们可以把任何一个变量赋给空接口，例如
```golang
type Kong interface {
}
func main() {
	var e Kong = s
	fmt.Println(e) //输出{}
	var e2 interface{} = s
	fmt.Println(e2) //输出{}
	var num float64 = 9.3
	var e3 interface{} = num
	fmt.Println(e3)//输出9.3
}
```
---------
#### 多态
* 概念：变量（实例）具有很多种形态。面向对象的第三大特征，在Go语言，多态特征是通过接口实现的。可以按照统一的接口来调用不同的实现。这时接口变量就呈现不同的形态，例如：
```golang
func greet(s SayHello) { //S为多态参数
	s.sayHello()
}
```
ps：s可以通过上下文来识别具体是什么类型的实例，就体现出多态
* 接口体现多态特征
1.多态参数：s叫多态参数
2.多态数组：
定义SayHello数组，存放中国人结构体，美国人结构体
```golang
type SayHello interface {
	sayHello()
}

type Chinese struct {
	name string
}

func (person Chinese) sayHello() {
	fmt.Println("你好")
}

type USA struct {
	name string
}

func (person USA) sayHello() {
	fmt.Println("hello")
}

type hello int

func (i hello) sayHello() {
	fmt.Println("say hi +", i)//[{rose} {莉莉} {露露}]
}

func greet(s SayHello) {
	s.sayHello()
}

func main() {
	var arr [3]SayHello
	arr[0] = USA{"rose"}
	arr[1] = Chinese{"莉莉"}
	arr[2] = Chinese{"露露"}

	fmt.Println(arr)

	var i hello = 10
	var s SayHello = i
	s.sayHello()//say hi + 10
	c := Chinese{}
	u := USA{}
	greet(u)//hello
	greet(c)//你好
}
```
----------
#### 断言
* 概念：Go语言里面有一个语法，可以直接判断是否是该类型的变量：value，ok = element.(T)，这里的value就是变量的值，ok是一个bool类型，element是interface变量，T是断言的类型。
* 代码展示：
```golang
type SayHello interface {
	sayHello()
}
type Chinese struct {
}
func (person Chinese) sayHello() {
	fmt.Println("你好")
}
//中国人特有的方法
func (person Chinese) niuYanGe() {
	fmt.Println("东北文化-扭秧歌")
}
type USA struct {
}
func (person USA) sayHello() {
	fmt.Println("hello")
}
func greet(s SayHello) {
	s.sayHello()
	//断言
	var ch Chinese = s.(Chinese)//看s是否能转成Chinese类型并且赋给ch变量
	ch.niuYanGe()
}
func main() {
	c := Chinese{}
	greet(c)
}
```
* if语句
```golang
func greet(s SayHello) {
	s.sayHello()
	if ch, flag := s.(Chinese); flag { //通过分号隔开，前面对变量进行初始化，后面进行判断
		ch.niuYanGe()
	} else {
		fmt.Println("NO")
	}
}
```
* Type Switch语句
```golang
func greet(s SayHello) {
	s.sayHello()
	switch s.(type) {
	case Chinese:
		ch := s.(Chinese)
		ch.niuYanGe()
	case USA:
		am := s.(USA)
		am.tiaowuw()
	}
}
```
---------
## 文件的操作
### File结构体
1. 打开文件，用于读取：(函数)
```go
file, err := os.Open("/home/user/test/abc.txt")//打开文件
if err != nil { //判断有没有错误
	fmt.Println("文件打开错误对应错误为：", err)
}
fmt.Printf("文件=%v\n", file)
```
2. 关闭文件
```go
err2 := file.Close()//关闭文件
if err2 != nil {
	fmt.Println("关闭失败")
}
```
-------
### io引入
#### 读取文件（一次性）
* 代码展示：
```golang
import (
	"fmt"
	"io/ioutil"
)
func main() {
	//在下面的程序中不需要进行Open\Close操作，因为文件的打开和关闭操作被封装在ReadFile函数内部了
	//读取文件
	file, err := ioutil.ReadFile("/home/user/test/abc.txt")
	//判断错误
	if err != nil {
		fmt.Println("读取出错，错误为：", err)
	}
	fmt.Printf("%v", string(file))//转换为字符串输出
}
```
----------
#### 读取文件（带缓冲区）
* 读取文件的内容并显示在终端（带缓冲区的方式-4096），适合读取比较大的文件，使用os.Open，file.Close，bufio.NewReader()，reader.ReadString函数和方法
* 代码展示：
```go
import (
	"bufio"
	"fmt"
	"io"
	"os"
)
func main() {
	//打开文件
	file, err := os.Open("/home/user/test/abc.txt")
	if err != nil {
		fmt.Println("打开失败，错误是：", err)
	}
	//当函数退出时，让file关闭
	defer file.Close()
	//创建一个流
	reader := bufio.NewReader(file)
	//读取操作
	for {
		str, err := reader.ReadString('\n')//读取到一个换行就结束
		if err == io.EOF {//io.EOF表示已经读取到文件的末尾
			break
		}
		fmt.Printf("%v", str)
	}
}

```
---------
#### 写入文件
* 指令展示，打开文件：
![image-6](C:/Users/HYun/Desktop/文件/xxbj/image/image-6.png)
* 三个参数：
1.要打开的文件路径
2.文件打开模式（可以利用|组合使用）
![image-7](C:/Users/HYun/Desktop/文件/xxbj/image/image-7.png)
3.权限控制
* 代码展示：
```go
import (
	"bufio"
	"fmt"
	"os"
)
func main() {
	file, err := os.OpenFile("/home/user/test/abc.txt", os.O_RDWR|os.O_APPEND, 0666) //打开文件操作
	if err != nil {
		fmt.Println("打开文件失败", err)
		return
	}
	defer file.Close()
	//写入文件操作：---》IO流---》缓冲输出流（带缓冲区）
	writer := bufio.NewWriter(file)
	for i := 0; i <= 5; i++ {
		writer.WriteString("你好\n")
	}
	//流带缓冲区，刷新数据--->真正写入文件中
	writer.Flush()

	s := os.FileMode(0666).String()
	fmt.Println(s)//0666权限为-rw-rw-rw-
}
```
---------
#### 文件复制
* 代码展示
```go
import (
	"fmt"
	"io/ioutil"
)

func main() {
	//定义源文件
	file1Path := "/home/user/test/abc.txt"
	//定义目标文件
	file2Path := "/home/user/test/abc1.txt"
	//对文件进行读取
	content, err := ioutil.ReadFile(file1Path)
	if err != nil {
		fmt.Println("读取有问题")
		return
	}
	//写出文件
	err = ioutil.WriteFile(file2Path, content, 0666)
	if err != nil {
		fmt.Println("写出失败")
	}
}
```
---------
## 协程和管道
* 程序（program）：是为了完成特定任务、用某语言编写的一组指令的集合，是一段静态的代码。（程序是静态的）
* 进程（process）：是程序执行一次的过程，正在运行的一个程序，进程作为资源分配的单位，在内存中会为每个进程分配不同的内存区域。（进程是动态的）是一个动的过程，进程的生命周期：有它自身的产生、存在和消亡的过程
* 线程（thread）：进程可进一步细化，是一个程序内部的一条执行路径，若一个进程同一时间并执行多个线程，就是支持多线程的
### 协程（gcroutine）：
* 概念：又称为微线程，纤程，协程是一种用户态轻量级线程
* 作用：在执行A函数的时候，可以随时中断，去执行B函数，然后中断继续执行A函数（可以自动切换），注意这一切换过程并不是函数调用（没有调用语句），过程很想多线程，然而协程中只有一个线程在执行（协程的本质是个单线程）
* 案例展示：
```go
import (
	"fmt"
	"strconv"
	"time"
)

func test() {
	for i := 1; i <= 10; i++ {
		fmt.Println("hello golang +" + strconv.Itoa(i))//strconv.Itoa(i)将i转换成字符串，以便和其他字符串连接起来完整输出
		//
		time.Sleep(time.Second)//一秒输出一次，两秒输出一个可以改成：time.Sleep(time.Second*2)
	}
}

func main() {
	go test() //开启协程,在函数前面加go就可以了
	for i := 1; i <= 10; i++ {
		fmt.Println("hello python +" + strconv.Itoa(i))
		//
		time.Sleep(time.Second)
	}
}
```
* 执行流程：1.主线程开始执行--->2.go test()开启协程--->3.执行协程中代码逻辑--->3.主线程代码继续执行--->程序结束/主线程结束
**PS:当主线程运行结束时，若协程中还未执行完也会强制结束**
--------
#### 启动多个协程
* 代码展示：
```go
import (
	"fmt"
	"time"
)
func main() {
	//匿名函数+外部变量 = 闭包
	for i := 1; i <= 5; i++ {
		go func(n int) {
			fmt.Println(n) //每次输出的结果不同，不按顺序输出
		}(i)
	}
	time.Sleep(time.Second)
}
```
----------
#### 使用WaitGroup控制协程退出
* WaitGroup概念：WaitGroup用于等待一组线程的结束。父线程调用Add方法来设定应等待的线程数量。每个被等待的线程在结束时应调用Done方法。同时，主线程里可以调用Wait方法阻塞至所有线程结束。---》解决了主线程在子协程结束后自动结束
* 主要方法：![image-8](C:/Users/HYun/Desktop/文件/xxbj/image/image-8.png)
* 案例分析：
Add/Done/Wait
```go
import (
	"fmt"
	"sync"
)
var wg sync.WaitGroup //只定义无须赋值
func main() {
	for i := 1; i <= 5; i++ {
		wg.Add(1) //启动加一
		go func(n int) {
			fmt.Println(n)
			wg.Done()//完成减一
		}(i)
	}
	wg.Wait()//wg为0结束
}
```
---------
#### 互斥锁
* 概念：Mutex为互斥锁，lock()加锁，Unlock()解锁，使用Lock()加锁后，便不能再次对其进行加锁，直到利用Unlock()解锁对其解锁后，才能再次加锁，适用于读写不确定场景，即读写次数没有明显的区别，性能、效率相对来说较低
* 代码展示：
```go
import (
	"fmt"
	"sync" //导包
)
var a int
var wg sync.WaitGroup
var lock sync.Mutex //加入互斥锁
func add() {
	defer wg.Done()
	for i := 0; i < 10000; i++ {
		lock.Lock()//加锁
		a = a + 1
		lock.Unlock()//开锁
	}
}
func sub() {
	defer wg.Done()
	for i := 0; i < 10000; i++ {
		lock.Lock()
		a = a - 1
		lock.Unlock()
	}
}
func main() {
	wg.Add(2)
	go add()
	go sub()
	wg.Wait()
	fmt.Println(a)
}

```
* 作用：一个协程在执行逻辑的时候另外的协程不执行
-------
#### 读写锁
* 概念：RWMutex是一个读写锁，其经常用于读次数远远多于写次数的场景，在读的时候，数据之间不产生影响，写和读之间才会产生影响
* 代码展示：
```golang
import (
	"fmt"
	"sync"
	"time"
)
var lock sync.RWMutex
var wg sync.WaitGroup
func read() {
	defer wg.Done()
	lock.RLock() //如果只是读数据，那么这个锁不产生影响，但是读写同时发生的时候，就会有影响
	fmt.Println("开始读取")
	time.Sleep(time.Second)
	fmt.Println("读取成功")
	lock.RUnlock()
}
func write() {
	defer wg.Done()
	lock.Lock()
	fmt.Println("开始写入")
	time.Sleep(time.Second * 10)
	fmt.Println("写入成功")
	lock.Unlock()
}
func main() {
	wg.Add(6)
	//启动协程---》场合：读多写少
	for i := 0; i < 5; i++ {
		go read()
	}
	go write()
	wg.Wait()
}
```
-------------
### 管道
* 特征：
1.管道本质就是一个数据结构-队列
2.数据是先进先出
3.自身线程安全，多协程访问时，不需要加锁，channel本身就是线程安全的
4.管道有类型的，一个string的管道只能存放string类型数据
* 定义：
var 变量名 chan 数据类型
1.chan管道关键字
2.数据类型指的是管道的类型，里面放入数据的类型，管道是有类型的，int类型的管道只能写入整数int
3.管道是引用类型，必须初始化才能写入数据，即make后才能使用
* 代码展示：
```go
func main() {
	var intChan chan int //定义一个int类型的管道
	intChan = make(chan int, 3) //初始化：管道可以存放三个int类型的数据，存多会报错
	fmt.Printf("intChan的值=%v\n", intChan)//返回的是一个地址值
	intChan <- 10 //给管道赋值
	num := 20 
	intChan <- num//将变量值赋给管道
	close(intChan) //关闭管道
	// intChan <- 30 当管道关闭以后不能写入数据会报错,但可以读
	// intChan <- 50 存放的数据超过容量会报错
	num1 := <-intChan //取出第一个写入给管道的值
	fmt.Println(num1)//10
	num2 := <-intChan//取出第二个写入管道的值
	// num3 := <-intChan
	fmt.Println(num2) //20
	// fmt.Println(num3)
	//num4 := <_intChan //没数据取出报错
	//fmt.Println(num4)//报错
	fmt.Printf("管道的实际长度：%v，容量是：%v\n", len(intChan), cap(intChan))//取出数据后，实际长度减少，容量不变
}
```
ps：在没有使用协程的情况下，如果管道的数据全部取出，那么再取就会报错

-------
#### 管道的遍历
* 管道支持for-range的方式进行遍历，请注意两个细节
1.在遍历时，如果管道没有关闭，则会出现deadlock的错误
2.在遍历时，如果管道已经关闭，则会正常遍历数据，遍历完后，就会退出遍历
* 代码展示：
```go
func main() {
	var intChan chan int //定义一个管道
	intChan = make(chan int, 100) //初始化
	for i := 0; i < 100; i++ { 
		intChan <- i //向管道添加数据
	}
	close(intChan) //关闭管道
	for b := range intChan { //for-range遍历，记得遍历前关闭管道不然会报错
		fmt.Println("b = ", b)//输出管道内容
	}
}
```
#### 协程和管道同时工作
* 代码展示：
```go
import (
	"fmt"
	"sync"
	"time"
)
var wg sync.WaitGroup //同步工具
func main() { //主线程
//写协程和读协程共同操作同一个管道-》定义管道
	intChan := make(chan int, 100)//定义管道
	wg.Add(2) 
	//开启俩协程
	go read(intChan)
	go write(intChan)
	wg.Wait()
}
func write(intChan chan int) { //写入管道
	defer wg.Done()
	for i := 0; i < 100; i++ {
		intChan <- i
		fmt.Println("写入的数据为=", i)
		time.Sleep(time.Second)//延时一秒
	}
	close(intChan)//关闭管道
}
func read(intChan chan int) { //读取管道
	defer wg.Done()
	for b := range intChan {
		fmt.Println("读取的数据为 = ", b)
		time.Sleep(time.Second)
	}
}
```
#### 声明
* 管道可以声明为只读或者只写性质
* 代码展示：
```go
func main() {
	//默认情况下管道是双向的，可以读可以写
	// var intChan2 chan int
	//声明为只写
	var intChan chan<- int 
	intChan = make(chan<- int, 3)
	intChan <- 10
	fmt.Println(intChan)
	// num1 := <-intChan 只写不能读报错
	// fmt.Println(num1) 报错
	//声明为只读
	var intChan1 <-chan int
	if intChan1 != nil { //判断是否为空管道
		num1 := <-intChan1
		fmt.Println(num1)
	}
	// intChan1 <- 30 只读不能写报错
}
```
#### 阻塞
* 概念：当管道只写入数据，没有读取时，超过管道的最大容量，就会出现阻塞，但写得快读的慢不会，只要有读就行，例如：
```go
import (
	"fmt"
	"sync"
	"time"
)
var wg sync.WaitGroup
func main() {
	intChan := make(chan int, 10)//容量为10
	wg.Add(2)
	go read(intChan)
	go write(intChan)
	wg.Wait()
}
func write(intChan chan int) {
	defer wg.Done()
	for i := 0; i < 15; i++ { //写15个不延时
		intChan <- i
		fmt.Println("写入的数据为=", i)
		// time.Sleep(time.Second / 10)
	}
	close(intChan)
}

func read(intChan chan int) {
	defer wg.Done()
	for b := range intChan {
		fmt.Println("读取的数据为 = ", b)
		time.Sleep(time.Second)//延时一秒读取一个
	}
}
```
PS：首先程序会先写入10数个到管道，当达到管道容量上限，就会等待管道读取，读取完一个继续写入下一个，不会发生阻塞

---------
#### select
* 功能：解决多个管道的选择问题，也可以叫做多路复用，可以从多个管道中随机公平地选择一个来执行
* 代码展示：
```go
func main() {
	intChan := make(chan int, 1)
	go func() {
		time.Sleep(time.Second * 5)
		intChan <- 10
	}()
	stringChan := make(chan string, 1)
	go func() {
		time.Sleep(time.Second * 12)
		stringChan <- "123456"
	}()
	select {
	case v := <-intChan:
		fmt.Println(v)
	case v := <-stringChan:
		fmt.Println(v)
	default :
		fmt.Println("防止select被阻塞")
	}
}
```
ps:case后面必须进行的是io操作，不能是等值，随机去选择一个io操作，default防止select被阻塞塞住，加入default

--------
#### refer + recover机制处理错误
* 问题原因：当多个协程工作，其中一个协程出现panic时，导致程序崩溃
* 解决办法：利用refer + recover捕获panic处理，即使协程出现问题，主线程仍然不受影响，可以继续执行,可以参考go3的错误处理
* 代码展示：
```go
func printNum() {
	for i := 1; i <= 10; i++ {
		fmt.Println(i)
	}
}
func devide() {
	defer func() {
		err := recover()
		if err != nil {
			fmt.Println("devide()有错,错误是:", err)
		}
	}()
	num1 := 10
	num2 := 0
	result := num1 / num2
	fmt.Println(result) 
}
func main() {
	go printNum()
	go devide()
	time.Sleep(time.Second * 5)
}
```