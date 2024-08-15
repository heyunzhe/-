## 网络编程
* 概念：把分布在不同地理区域的计算机与专门的外部设备用通信线路互连成一个规模达、功能强的网络系统，从而使众多的计算机可以方便地互相传递信息、共享硬件、软件、数据信息等资源，设备之间在网络中进行数据的传输，发送/接收数据
### 创建客户端
* 调用Dial函数（net包下）：
![image-9](C:/Users/HYun/Desktop/文件/xxbj/image/image-9.png)
* 代码展示：
```go
import (
	"fmt"
	"net" //所需的网络编程
)
func main() {
	fmt.Println("客户端启动")
    //调用Dial函数：参数需要指定tcp协议，需要指定服务器端的IP+PORT
	coon, err := net.Dial("tcp", "127.0.0.1:8888")
	if err != nil {
		fmt.Println("错误信息：", err)
		return
	}
	fmt.Println("连接成功,coon=", coon)
}
```
### 创建服务端
* 进行监听：（listen函数在net包下）
![image-10](C:/Users/HYun/Desktop/文件/xxbj/image/image-10.png)
* 代码展示
```go
import (
	"fmt"
	"net"
)
func main() {
	fmt.Println("服务器启动")
    //进行监听：需要指定服务器端tcp协议，服务器端的IP+PORT
	listen, err := net.Listen("tcp", "127.0.0.1:8888")
	if err != nil {//监听失败
		fmt.Println("监听失败：", err)
		return
	}
	for {
        //监听成功后
        //循环客户端的连接
		conn, err2 := listen.Accept()
		if err2 != err {
			fmt.Println("等待失败：", err)
		} else {
            //连接成功
			fmt.Printf("等待连接成功，con=%v ,接收到的客户消息是：%v", conn, conn.RemoteAddr().String())
		}
	}
}
```
## 反射
### 作用
1. 反射可以在运行时动态获取变量的各种信息，比如变量的类型，类别等信息
2. 如果是结构体变量，还可以获取到结构体本身的信息（包括结构体的字段、方法）
3. 通过反射，可以修改变量的值，可以调用关联的方法
4. 使用反射需要导入“reflect”包
--------
### 相关函数
1. reflect。TypeOf(变量名)，获取变量的类型，返回reflect。Type类型
2. reflect。ValueOf(变量名)，获取变量的值，返回reflect.Value类型（reflect.Value是一个结构体类型），通过reflect.Value,可以获取到关于该变量的很多信息
------------
### 对基本数据类型进行反射
* 代码展示：
```go
import (
	"fmt"
	"reflect"
)
// 利用一个函数，函数定义为空接口
//可以把任何一个变量赋给空接口
func testReflect(i interface{}) {
	reType := reflect.TypeOf(i)
	fmt.Printf("reType的类型是：%T\n", reType) //*reflect.rtype
	fmt.Println("reType:", reType) //int
	reValue := reflect.ValueOf(i)
	fmt.Printf("reValue的类型是：%T\n", reValue) //reflect.Value
	fmt.Println("reValue:", reValue) //100
	num2 := 80 + reValue.Int()
	fmt.Println(num2) //180

	i2 := reValue.Interface() //转换为空接口
	//断言
	n := i2.(int)
	n2 := n + 30
	fmt.Println(n2)
}

func main() {
	//对基本数据类型进行反射：
	var num int = 100
	testReflect(num)
}
```
------------
### 对结构体进行反射
* 代码展示：
```go
import (
	"fmt"
	"reflect"
)

type student struct {
	name string
	age  int
}

// 利用一个函数，函数定义为空接口
func testReflect(i interface{}) {
	reType := reflect.TypeOf(i)
	fmt.Println("reType:", reType)
	reValue := reflect.ValueOf(i)
	fmt.Println("reValue:", reValue)

	i2 := reValue.Interface()
	//断言
	n := i2.(student)
	fmt.Printf("学生的姓名是：%v,学生的年龄是：%v\n", n.name, n.age)
}

func main() {
	//对结构体数据类型进行反射：
	stu := student{
		name: "lili",
		age:  18,
	}
	testReflect(stu)
}
```
----------
### 获取变量类别
* 方法
1.reflect.Type.Kind()
2.reflect.Value.Kind()
* kind的值是常量值，例如：
![image-11](C:/Users/HYun/Desktop/文件/xxbj/image/image-11.png)
* 代码展示：
```go
import (
	"fmt"
	"reflect"
)
type student struct {
	name string
	age  int
}
// 利用一个函数，函数定义为空接口
func testReflect(i interface{}) {
	reType := reflect.TypeOf(i)
	reValue := reflect.ValueOf(i)
	//获取变量类别
	k1 := reType.Kind()
	fmt.Println(k1) //struct
	k2 := reValue.Kind()
	fmt.Println(k2) //struct
	//获取变量类型
	i2 := reValue.Interface()
	n := i2.(student)
	fmt.Printf("%T\n", n) // main.student
}
func main() {
	//对基本数据类型进行反射：
	stu := student{
		name: "lili",
		age:  18,
	}
	testReflect(stu)
}
```
----------------
### 反射修改变量的值
![image-12](C:/Users/HYun/Desktop/文件/xxbj/image/image-12.png)
#### 修改基本变量值
* 代码展示：
```go
import (
	"fmt"
	"reflect"
)
// 利用一个函数，函数定义为空接口
func testReflect(i interface{}) {
	reValue := reflect.ValueOf(i) //获取变量的值
	reValue.Elem().SetInt(40) //对指针指向的value值进行封装，setnt将指针指向的值改为40
}
func main() {
	var num int = 100
	testReflect(&num) //传入一个地址值
	fmt.Println(num) //40
}
```
------------
#### 熟悉API
* 代码展示：
```go
import (
	"fmt"
	"reflect"
)
//定义结构体
type student struct {
	name string
	age  int
}
//方法3，下标2
func (s student) CPrint() {
	fmt.Println(s.name)
}
//方法2，下标1
func (s student) BGetsum(n1, n2 int) int {
	fmt.Println("BGetsum")
	return n1 + n2
}
//方法1，下标0
func (s student) ASet(name string, age int) {
	s.name = name
	s.age = age
}

// 利用一个函数，函数定义为空接口
func testReflect(i interface{}) {
	reValue := reflect.ValueOf(i) //获取变量值
	fmt.Println(reValue)
	//返回结构体类型的字段数
	n := reValue.NumField()
	fmt.Println(n)// 2
	for i := 0; i < n; i++ { //遍历结构体
		fmt.Printf("第%d个字段的值为：%v\n", i, reValue.Field(i)) //返回结构体第i个字段
	}

	n2 := reValue.NumMethod() //返回持有的方法数量
	fmt.Println(n2) // 3
	//调用CPrint()方法
	//方法的首字母必须大写才能有对应的反射的访问权限
	//方法的顺序按照ASCII的顺序排列的 a,b,c,,,,,索引 0,1,2,,,,,,,,
	reValue.Method(2).Call(nil) //调用第三个方法，没有参数可以不写Call，有参数Call后面跟传入的参数

	var params []reflect.Value //定义一个新的切片
	params = append(params, reflect.ValueOf(10)) //传入数据
	params = append(params, reflect.ValueOf(20))
	result := reValue.Method(1).Call(params) //调用第二个方法，call表示用params作为参数调用该方法，将返回值存在result
	fmt.Println("BGetsum方法的返回值为：", result[0].Int()) //取切片下标为0的值，并转换成int形式输出
}
func main() {
	//对基本数据类型进行反射：
	s := student{
		name: "lili",
		age:  18,
	}
	testReflect(s) 
}
```
---------
#### 修改结构体变量值
* 代码展示：
```go
import (
	"fmt"
	"reflect"
)
//定义结构体
type student struct {
	Name string
	Age  int
}
// 利用一个函数，函数定义为空接口
func testReflect(i interface{}) {
	reValue := reflect.ValueOf(i) //传入结构体参数
	fmt.Println(reValue)
	//
	n := reValue.Elem().NumField() //结构体字段数
	fmt.Println(n) // 2

	reValue.Elem().Field(0).SetString("张三") //修改结构体第0个字段指向值为“张三”
}

func main() {
	//对基本数据类型进行反射：
	s := student{
		Name: "lili",
		Age:  18,
	}
	testReflect(&s) //传入指针变量
	fmt.Println(s) //{张三，18}
}
```
* PS:变量名一定要大写不然ELem()无法封装修改，跟方法名一样开头字母一定要大写！！！