# go语言学习
* 学习网站：https://gitee.com/dopaminereceptor/over-golang/tree/master
</br>https://studygolang.com/pkgdoc
* 官网：https://golang.org/dl/
## 安装go
1. linux上输入：wget https://dl.google.com/go/go1.22.5.linux-amd64.tar.gz【下载压缩包,可以在官网上找版本】
2. 解压：sudo tar zxvf go1.22.5.linux-amd64.tar.gz -C /usr/local/ 【注意：如果解压到的目录是root权限一定要加**sudo**不然解压不进去,可以通过“**ls -la** 目录名” 查看目录的权限是哪个用户的】
3. 配置：sudo vim /etc/profile 打开目录【不加sudo可能打开了无法编辑】 底下添加export GOROOT=/usr/local/go  export PATH=$PATH:$GOROOT/bin
重启一下环境：source /etc/profile  应该就可以用了，可以使用go version 查询一下有无安装成功 
## 运行go程序：
1. go run 文件名 【记得是要在当前文件的目录下，适合运行一些简单的go程序】
2. 使用 go build 文件名 or go install 文件名 生成可执行文件使用 ./文件名 运行 【都要基于当前文件目录下】
## go的注意事项
1. 源文件以go为扩展名
2. 程序执行入口为main()函数
3. 严格区分大小写
4. 方法由一条条语句构成，每个语句后面不需要加；
5. go是一行行进行编译，所以一行只能写一条语句，不要写多个语句
6. 定义的变量或者import的包没有使用到，就不能编译成功
7. 大括号为成对出现缺一不可
8. 换行在行尾用，
9. 变量不要重复定义
------
## 注释
1. 单行注释：代码后加//输入注释 快捷键ctrl+/
2. 多行注释：/* 注释内容 */  快捷键shift+alt+a

-----
## 关键字
* const func import package type var 【用来声明各种代码元素】
* chan interface map struct 【用做一些组合类型的字面表示中】
* break case continue default else fallthrough for goto if range return select switch 【用在流程控制语句中】
* defer go 【也可以看做流程控制关键字】 

--------
## 变量
### 定义变量（单个变量）
1. var 变量名 变量类型 如：var age int = 18 【定义一个age变量为整数型并赋值18】
2. var 变量名 变量类型 如：var age int 【定义一个age变量为整数型不赋值，使用默认值】 
3. var 变量名 = 值 如：var age = 18 【定义一个age变量，不写变量类型根据等号后的值判断类型，但最好是知道变量类型就写上】 
4. 变量名 := 值 如：age := 18 【省略var注意:=不要写成=】  
### 定义变量（多个变量）
1. var 变量名1,变量名2....,变量名n 变量类型 如：var a,b,c int 【定义a,b,c三个变量为整型】
2. var 变量名1,变量名2,...,变量名n 值1,值2,...值n 如：var a,b,c = 1,2,3
3. 变量名1,变量名2,...,变量名n := 值1,值2,...,值n 如：a1,a2,a3 := 1,2,3
4.var(
    a = 10
    b = 20
) 【定义a变量为10，b为20】                     
</br>注：全局变量定义在函数外，局部变量定义在函数内

_______
### 变量类型
#### 整数类型（默认为int，默认值为0）：
1. 有符号位：int int8(-128~127) int16(-32768~32767) int32(-2^31~2^31-1) int64(-2^63~2^63-1)
2. 无符号位：uint uint8(0~255) uint16(0~2^16-1) uint32(0~2^32-1) uint64(0~2^63-1)
**ps**：尽量使用占用空间小的数据类型
#### 浮点类型(有小数点的数，默认值为0)：
* float32 float64 **ps**：浮点数可能会有精度的损失，通常情况下建议使用float64定义,默认为float64
#### 字符类型：
1. byte（0~255）其实就是uint8的内置编码，本质上就是一个整数，也可以直接参与运算，底层对应ASCII码，汉字字符底层对于Unicode码，Golang的字符对应UTF-8编码（Unicode包含UTF-8编码，ASCII码）
2. 转义字符：在输出的字符中间加上\参数 例如fmt.Println("abc\nabc") 其中\n为转义字符作用为换行输出:
abc</br>abc
* 换行：\n
* 光标往前一格:\b 例如：fmt.Println("abc\babc") 输出ababc
* 将光标回到本行开头：\r 例如：fmt.Println("aaaaa\rbbb") 输出bbbaa
* 表示一个制表符(8位)：\t 例如：fmt.Println("aaaaaaa\tbbb") 输出aaaaaaa bbb
* 将双引号输出：\" 例如：fmt.Println("\"golang\"") 输出"golang
#### 布尔型（默认值为false）：
* bool 【只有ture和false两个值】
#### 字符串（默认值为空）：
* string 【字符串定义完后，其中的字符的值不能改变，输出的字符有特殊字符是可以使用``(反引号)括起来，没有就""】 可以拼接字符例如：var s3 string = "abc" + "defg" 输出s3显示abcdefg

**ps**：当赋予的值超过了定义类型本身的值就会报错，选择合适变量定义合适的值

#### 数据类型转换
* 数值类型转换
var n1 int = 100
	//var n2 float32 = n1 无法自动转换会报错
	fmt.Println(n1)
	var n2 float32 = float32(n1) //n1的类型还是int只是将n1的值100转换成了float32并赋给了n2

var n3 int64 = 888888
	var n4 int8 = int8(n3) //将int64转换为int8编译不会错，但会数据溢出
	fmt.Println(n4) //可以输出但输出结果为56
ps:当转换的数据大于定义类型的实际存储范围时，会数据溢出

* 转换为字符串 
方法：fmt.Sprintf("%参数",变量)
    var n1 int = 10
	var n2 float32 = 3.14
	var n3 bool
	var n4 byte = 'a' //定义
	var s1 string = fmt.Sprintf("%d", n1)//%d为十进制数值
	fmt.Printf("%T s1 = %q \n", s1, s1) //输出string s1 = "10"

	var s2 string = fmt.Sprintf("%f", n2)//%f为表示的浮点
	fmt.Printf("%T s2 = %q \n", s2, s2)//输出string s2 = "3.140000"

	var s3 string = fmt.Sprintf("%t", n3)//%t表示true或false
	fmt.Printf("%T s3 = %q \n", s3, s3)//输出string s3 = "false"

	var s4 string = fmt.Sprintf("%c", n4)//%c表示改值对应的unicode字符
	fmt.Printf("%T s4 = %q \n", s4, s4)//输出string s4 = "a"

_______
## 指针
var age int = 18
	fmt.Println(&age) //&age = 0xc0000120d0 //&+变量可以输出变量的地址
	var ptr *int = &age //定义一个名为ptr的指针变量 **ps**：指针接收的一定是一个地址值不要漏写&，此外变量定义什么类型 * 后就跟什么类型，当变量定义为int但 * 后面跟着float32 就会报错
	//其类型为*int为指针类型（可以理解为指向int类型的指针，这里是指向age）
	//&age就是一个地址，是ptr变量的值
	fmt.Println(*ptr)//输出指针指向内容的值
    	*ptr = 20 //修改指针指向的值
	fmt.Println(age)//输出20
}
**ps：** **&**：取内存地址 </br> *：根据地址取值

-----
## 包
* 格式：import "包的名字" 
### fmt 
* fmt.Println(内容) 输出
* fmt.Printf(内容)表示格式化输出 如：fmt.Printf("a的类型是：%T", a) 把a的变量类型填充到%T上 
</br>其常用格式输出有以下：
1. %%	%字面量
2. %b	二进制整数值，基数为2，或者是一个科学记数法表示的指数为2的浮点数
3. %c	该值对应的unicode字符
4. %d	十进制数值，基数为10
5. %e	科学记数法e表示的浮点或者复数
6. %E	科学记数法E表示的浮点或者复数
7. %f	标准计数法表示的浮点或者复数
8. %o	8进制度
9. %p	十六进制表示的一个地址值
10. %s	输出字符串或字节数组
11. %T	输出值的类型，注意int32和int是两种不同的类型，编译器不会自动转换，需要类型转换。
12. %v	值的默认格式表示
13. %+v	类似%v，但输出结构体时会添加字段名
14. %#v	值的Go语法表示
15. %t	单词true或false
16. %q	该值对应的单引号括起来的go语法字符字面值，必要时会采用安全的转义表示
17. %x	表示为十六进制，使用a-f
18. %X	表示为十六进制，使用A-F
19. %U	表示为Unicode格式：U+1234，等价于"U+%04X"  
* unsafe 【使用unsafe.Sizeof(变量名) 查询变量对应的字节数】