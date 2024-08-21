### 图书管理系统
* 功能：添加图书、借书、还书、查询图书信息
* 说明：编号是唯一的，其余不定
#### 代码展示
```golang
package main

import (
	"encoding/json"//处理json数据
	"fmt"
	"os" //创建文件
    "sort" //排序切片
)

type book struct { //创建一个结构体
	Id     int  //编号
	Title  string  //书名
	Author string  //作者
	Price  float32 //价格
	State  bool    //状态
}

func newbook(id int, title string, author string, price float32, state bool) *book { //定义一个函数返回一个指针
	return &book{ //函数体，返回以下这些
		Id:     id,
		Title:  title,
		Author: author,
		Price:  price,
		State:  state,
	}
}
 //导出数据到json
func daochu(filename string) {
	file, err := os.Create(filename)//创建一个新文件
	if err != nil { //判断有错
		fmt.Println("Error creating file:", err)//打印错误信息
		return
	}
	defer file.Close()//延迟文件关闭

	encoder := json.NewEncoder(file)//创建新的json编码器，将数据写入到file文件
	if err := encoder.Encode(allbook); err != nil {//将变量allbook编码为json文件并写入到文件，判断有无错误
		fmt.Println("Error encoding JSON:", err)
		return
	}
	fmt.Println("已成功导出数据")
}
//导入json数据
func daoru(filename string) { 
	file, err := os.Open(filename)//打开指定的filename文件
	if err != nil {
		fmt.Println("Error opening file:", err)
		return
	}
	defer file.Close()//延迟关闭文件

	decoder := json.NewDecoder(file)//创建一个json解码器，使用打开的文件作为输入源
	if err := decoder.Decode(&allbook); err != nil {//尝试将文件中的json数据编码解码到allbook变量中
		fmt.Println("Error decoding JSON:", err)
		return
	}
	fmt.Println("导入数据成功")
}

var allbook = make([]*book, 0, 200) //定义切片

func addbook() *book { //输入图书信息
	var (
		id     int  //编号
		title  string  //书名
		author string  //作者
		price  float32 //价格
		state  bool    //状态
	)
	fmt.Println("请输入编号：")
	fmt.Scanln(&id)
	fmt.Println("请输入书名：")
	fmt.Scanln(&title)
	fmt.Println("请输入作者：")
	fmt.Scanln(&author)
	fmt.Println("请输入价格：")
	fmt.Scanf("%f", &price)
	fmt.Println("是否借出：")
	fmt.Scanln(&state)
	fmt.Printf("编号：%d 书名：%s 作者：%s 价格：%f  是否借出：%t\n", id, title, author, price, state)
	book := newbook(id, title, author, price, state)
	return book
}
func xuanze() { //提示信息
	fmt.Println("欢迎使用图书管理系统，请选择你的需求")
	fmt.Println("1.添加图书")
	fmt.Println("2.查看所有图书")
	fmt.Println("3.查询指定图书")
	fmt.Println("4.借书")
	fmt.Println("5.还书")
	fmt.Println("6.退出")
}

func jiebook() { //借书
	var id int
	fmt.Println("请输入你要借书的编号：")
	fmt.Scanln(&id)
	found := false
	for _, b := range allbook {
		if b.Id == id && !b.State {
			fmt.Printf("已成功借出，记得及时归还\n")
			b.State = true
			found = true
			break
		}
	}

	if !found {
		fmt.Println("此书不存在或已被借出，换一本看看吧")
	}
}

func huanbook() { //还书
	var id int
	fmt.Println("请输入你要还书的编号：")
	fmt.Scanln(&id)
	found := true
	for _, b := range allbook {
		if b.Id == id && b.State {
			fmt.Printf("已成功归还\n")
			b.State = false
			found = false
			break
		}
	}

	if found {
		fmt.Println("未查询到此书信息或已归还")
	}
}

func tianjbook() { //添加书到切片
	book := addbook()
	for _, b := range allbook {
		if b.Id == book.Id {
			fmt.Printf("编号：%d已存在，请换个编号\n", b.Id)
			return
		}
	}
	allbook = append(allbook, book) //添加
	fmt.Println("书籍添加成功！")
}

func chakbook() { //查看所有书
	if len(allbook) == 0 {
		fmt.Println("还没有书")
	}
    sort.Slice(allbook, func(i, j int) bool { //i，j的值会随着指执行函数sort.Slice时传入
	    return allbook[i].Id < allbook[j].Id //按照书的编号大小进行排序
	})
	for _, b := range allbook {
		fmt.Printf("编号：%d 书名：%s 作者：%s 价格：%f  是否借出：%t\n", b.Id, b.Title, b.Author, b.Price, b.State)
	}

}

func chaxbook() {
	var title string
	fmt.Println("请输入你要查找的书名：")
	fmt.Scanln(&title)
	found := false
    sort.Slice(allbook, func(i, j int) bool {
	    return allbook[i].Id < allbook[j].Id
	})
	for _, b := range allbook {
		if title == b.Title {
			found = true
			fmt.Printf("编号：%d 书名：%s 作者：%s 价格：%f  是否借出：%t\n", b.Id, b.Title, b.Author, b.Price, b.State)
		}
	}
	if !found {
		fmt.Println("目前库中没有这本书")
	}
}

func main() { //主函数
	daoru("allbook.json")
	for {
		xuanze()
		var x int
		fmt.Scanln(&x)
		switch x {
		case 1:
			tianjbook()
		case 2:
			chakbook()
		case 3:
			chaxbook()
		case 4:
			jiebook()
		case 5:
			huanbook()
		case 6:
			daochu("allbook.json")
			os.Exit(0)
		}
	}
}
```
#### 注意事项
1. 要想被外界调用，创建结构体定义的时候就必须以**大写字母**开头，否则无法被外界调用，传入json的数据也为空数据
2. 对切片进行排序最好将排序的变量设置为数值型，字符型虽然也可以排，但数值型的优先级高，导入sort包
3. 错误报告可写可不写，能写最好还是写上，提高代码健壮性
4. for rang遍历切片，第一个参数可以省略，表示索引值，第二个参数才是切片的内容
5. switch表达式记得也要加{}，然后case后面跟的是值加:,不是case :值
-----------------
### 计算器
#### 代码展示
```go
import (
	"bufio"
	"fmt"
	"os"
	"strconv"
	"strings"
)

func main() {
	//逐行读取输入
	Scanner := bufio.NewScanner(os.Stdin)
	fmt.Println("请输入一个算数表达式，例如：3 + 4")

	for {
		Scanner.Scan() //读取输入，直到遇到换行符或其他指定的分隔符，并将其存储在扫描器内部，以便后续处理
		line := Scanner.Text() //获取当前扫描到的文本行

		if strings.TrimSpace(line) == "exit" {
			fmt.Println("退出计算器")
			break //退出循环
		}
		a := strings.Fields(line) //分割成一个字符串切片
		if len(a) != 3 { //判断是否有3个字符
			fmt.Println("您输入的表达式有错误，请重新输入")
			continue //退出本次循环继续下次
		}
		num1, err1 := strconv.ParseFloat(a[0], 64) //将字符串解析为float64格式
		fuhao := a[1] //提取符号
		num2, err2 := strconv.ParseFloat(a[2], 64)
		if err1 != nil || err2 != nil { //转换字符串时有无错误
			fmt.Println("无效的数字，请重新输入")
			continue //有错结束本次循环继续下次
		}
		var result float64 
		switch fuhao { //判读符号
		case "+":
			result = num1 + num2
		case "-":
			result = num1 - num2
		case "*":
			result = num1 * num2
		case "/":
			if num2 == 0 {
				fmt.Println("除数不能为0")
				continue
			}
			result = num1 / num2
		default: //没有上面这四个符号时
			fmt.Println("无效的操作符，请使用 +，-,*,/")
			continue
		}
		fmt.Println("结果为", result)
	}
}
```
#### 语句具体含义
1. Scanner := bufio.NewScanner(os.Stdin) //用于创建一个新的扫描器，它从标准输入 (os.Stdin) 读取数据，适用于处理大块数据或需要逐行处理的场景
2. Scanner.Scan() //读取输入，直到遇到换行符或其他指定的分隔符，并将其存储在扫描器内部，以便后续处理
3. Scanner.Text() // 获取上方语句读取的文本行
4. strings.Fields(line) //用于将字符串按空白字符（空格、制表符、换行符等）分割成一个字符串切片（[]string）。它会自动去除多余的空白，并忽略连续的空白字符
5. num1, err1 := strconv.ParseFloat(a[0], 64) //用于将字符串 tokens[0] 解析为浮点数。64 指定了返回值的精度为 64 位双精度浮点数（float64）。如果解析成功，它返回解析后的 float64 值和一个 nil 错误；如果失败，它返回 0 和一个错误值
6. strings.TrimSpace(line) //去除字符两边空格
--------------------
