# GoWeb
## 搭建服务器
* 案例：搭建一个服务器，在网页上输出：Hello World！
* 代码展示：
```go
import (
	"fmt"
	"net/http"
)
//处理器函数
func handler(w http.ResponseWriter, r *http.Request) { 
	fmt.Fprintln(w, "Hello World!",r.URL.Path) //，后的可以不写
}

func main() {
	http.HandleFunc("/", handler) //传入映射地址“/”为根目录,后面为处理器函数
	//创建路由
    err := http.ListenAndServe(":8080", nil)//前面传入监听的端口号
	if err != nil {//错误处理
		fmt.Println("发现错误：", err)
}
```
* http默认端口号为80，https默认端口号为443
---------
## 通过详细配置创建服务器
* 代码展示：
```go
import (
	"fmt"
	"net/http"
	"time"
)

type MyHandler struct { //创建结构体
}
//创建方法，ServeHTTP为固定字段不可修改
func (m *MyHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) { 
	// fmt.Fprintln(w, "通过自己创建的处理器处理请求！")
	fmt.Fprintln(w, "通过详细配置服务器信息处理请求！")
}

func main() {
	myHandler := MyHandler{}

	// http.Handle("/myHandler", &myHandler)
	server := http.Server{ //详细配置
		Addr:        ":8080", //端口
		Handler:     &myHandler,//地址
		ReadTimeout: 2 * time.Second,//延时
	}

	server.ListenAndServe()//详细配置完不需要加参数
}
```
## 自己创建多路复用器
* 代码展示：
```go
import (
	"fmt"
	"net/http"
)
//创建处理器函数
func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "自己创建的多路复用器", r.URL.Path)
}

func main() {
    //创建多路复用器
	mux := http.NewServeMux()
    //http.HandleFunc("/",handler)
	mux.HandleFunc("/", handler)
    //创建路由
    //http.listenAndServe(":8080",nil)
	http.ListenAndServe(":8080", mux)

}
```
----------------
## 打开http接口实现对图书数据的增删改查（json）
### 代码展示：
```golang
package main

import (
    "encoding/json"
    "fmt"
    "net/http"
	"os"
)

type Book struct { //创建一个结构体
	Id     int     //编号
	Title  string  //书名
	Author string  //作者
	Price  float32 //价格
	State  bool    //状态
}

var allbook []*Book 

func handleAddBook(w http.ResponseWriter, r *http.Request) {//添加书籍
        var book Book 
        json.NewDecoder(r.Body).Decode(&book) //创建一个json解码器，从http请求的主体中读取json数据
        for _, b := range allbook { //遍历
            if b.Id == book.Id { //
                http.Error(w, "已有相同id的书，请换一个id", http.StatusConflict)//返回错误信息
                return
            }
        }
        allbook = append(allbook, &book)//没问题添加到切片中
        w.WriteHeader(http.StatusOK)//返回正确处理信息
        fmt.Fprintln(w, "添加成功")//正确提示信息
        daochu("allbook.json",allbook)//导出到json文件
        w.WriteHeader(http.StatusOK)//导出成功返回信息
}

func handleChakBooks(w http.ResponseWriter,r *http.Request){//查看所有书
    w.Header().Set("Content-Type", "application/json")//响应json文件内容
    json.NewEncoder(w).Encode(allbook)//将allbook数据对象转为json格式

}

func handleChaxBook(w http.ResponseWriter,r *http.Request){//查看指定书
    id := r.URL.Query().Get("id") //获取请求字符串
    for _, b := range allbook {
        if fmt.Sprintf("%d",b.Id) == id { 
            w.Header().Set("Content-Type", "application/json")
            json.NewEncoder(w).Encode(b)
            return
        }
    }
    http.Error(w,"没有找到这本书,请换一个编号看看",http.StatusNotFound) 
}

func handleJieBook(w http.ResponseWriter,r *http.Request){
    id := r.URL.Query().Get("id")
    for _, b := range allbook {
        if fmt.Sprintf("%d",b.Id) == id && b.State == false {
            fmt.Fprintln(w,"已成功借出")
            b.State = true
            daochu("allbook.json",allbook)
            w.WriteHeader(http.StatusOK)
            return
        }
    }
        http.Error(w,"此书不存在或已被借走",http.StatusNotFound)
}

func handlehuanBook(w http.ResponseWriter,r *http.Request){
    id := r.URL.Query().Get("id")
    for _, b := range allbook {
        if fmt.Sprintf("%d",b.Id) == id && b.State == true {
            fmt.Fprintln(w,"已成功归还")
            b.State = false
            return
        }
    }
        http.Error(w,"此书不存在或已经归还",http.StatusNotFound)
}

func handleDelBook(w http.ResponseWriter, r *http.Request){
    id := r.URL.Query().Get("id")
    var updatedBooks []*Book //定义一个新的切片
    found := false //初始found定义为false
    for _, b := range allbook {
        if fmt.Sprintf("%d", b.Id) == id {
            found = true //找到了改为true
            continue //退出本次循环继续下次
        }
        updatedBooks = append(updatedBooks, b)//没找到的添加到新的切片
    }

    if found {
        allbook = updatedBooks //将新切片的值赋给老的
        daochu("allbook.json", allbook)//导出最新获取的内容
        w.WriteHeader(http.StatusOK)
        fmt.Fprintln(w, "删除成功")
    } else {
        http.Error(w, "书籍未找到", http.StatusNotFound) //found为false返回错误信息
    }
}

func daochu(filename string, books []*Book) error { //导出json文件
    file,_ := os.Create(filename) //创建一个新文件，文件存在，他会被截断
    defer file.Close()//延迟关闭文件
    encoder := json.NewEncoder(file) //创建json编码器
    return encoder.Encode(books) //将books切片编码为json格式写入file文件
}

func daoru(filename string) { //导入json文件
	file, _ := os.Open(filename) //打开文件
	defer file.Close() //延迟关闭文件
	decoder := json.NewDecoder(file) //创建json解码器
	decoder.Decode(&allbook) //解码数据
	fmt.Println("导入数据成功")
}

func main() {
    daoru("allbook.json") //启动前导入数据
    http.HandleFunc("/add",handleAddBook)
    http.HandleFunc("/chak",handleChakBooks) //使用localhost:8080/chak调试接口
    http.HandleFunc("/chax",handleChaxBook)//使用localhost:8080/chax？id=1调试接口
    http.HandleFunc("/jie",handleJieBook)
    http.HandleFunc("/huan",handlehuanBook)
    http.HandleFunc("/del",handleDelBook)
    fmt.Println("端口已成功打开 :8080") //终端提示信息
    http.ListenAndServe(":8080", nil)
}

```
#### 组合含义：
1. NewDecoder(r.Body)//创建新的JSON解码器（后面跟的是http请求部分）
ps：r.Body 是 *http.Request 对象中的一个字段，表示请求的主体内容
2. Decode(&book) //解码器方法，将json数据解析到目标变量（后面跟地址）
3. http.StatusConflict // http状态码409，表示请求由于资源冲突无法完成
4. http.StatusNotFound // http状态码404，表示请求的资源无法服务器上找到
5. http.StatusOK // http状态码200，表示请求成功
6. Content-Type //是一个http头字段
7. r.URL.Query() //返回url.values类型对象

---------------
#### 语句用法
1. http.Error(w, "书籍未找到", http.StatusNotFound) //简化错误响应的生成过程，返回错误状态码和错误消息
ps：适用于返回错误信息
2. w.WriteHeader(http.StatusOK) //设置 HTTP 响应的状态码为 200（成功）,意味着服务器成功处理了请求，并且返回了正常的响应
ps：适用于返回正确信息
3. w.Header().Set("Content-Type", "application/json") //表示将 HTTP 响应的 Content-Type 头字段设置为 application/json，这告诉客户端响应的内容是 JSON 格式的数据
4. json.NewEncoder(w).Encode(allbook) //将 allbook 这个数据对象转换为 JSON 格式，并将其作为 HTTP 响应的一部分发送给客户端，自动设置响应的 Content-Type 为 application/json（如果没设置过）
5. id := r.URL.Query().Get("id") //将从 HTTP 请求的查询字符串中提取名为 "id" 的查询参数的值，并将其赋值给 id 变量
--------------
## 打开http接口实现对数据的增删改查（数据库）
### 代码展示
```go
package main

import (
	"database/sql" //数据库包
	"encoding/json"
	"fmt"
	"log"
	"net/http"
	"os"
	"strings"
	_ "github.com/mattn/go-sqlite3" //外部包
)

type Book struct { //定义一个结构体
	Id     int     `json:"id"`
	Title  string  `json:"title"`
	Author string  `json:"author"`
	Price  float32 `json:"price"`
	State  bool    `json:"state"`
}

var db *sql.DB //定义一个数据库变量
var errorLog *log.Logger //定义一个日志文件变量

func init() { //准备工作，主函数还没执行前运行
	var err error //定义错误变量
	db, err = sql.Open("sqlite3", "books.db") //打开名为books.db类型为sqlite3的数据库
	if err != nil {
		log.Fatal(err) //有错就记录错误并退出程序
	}
    //如果没有books.db的数据表就创建一个
	createTableQuery := `CREATE TABLE IF NOT EXISTS books ( 
	id INTEGER PRIMARY KEY,
	title TEXT,
	author TEXT,
	price REAL,
	state INTEGER);`

	_, err = db.Exec(createTableQuery) //执行一次数据库命令
	if err != nil { 
		log.Fatal(err) //有错就记录错误并退出程序
	}

	logfile, err := os.OpenFile("books.log", os.O_CREATE|os.O_APPEND|os.O_WRONLY, 0666) //创建并打开一个日志文件，0666为任何人可读写，不可执行，后面参数类型参考组合含义
	if err != nil {
		log.Fatal("无法打开日志文件:", err)
	}
	errorLog = log.New(logfile, "错误是：", log.Ldate|log.Ltime|log.Lshortfile) //创建一个日志记录器，第一个参数为文件，第二个是日志消息的前缀，后面参数参考组合含义
}
//创建http接口
func handleAddBook(w http.ResponseWriter, r *http.Request) { 
	var book Book //定义book为Book结构体
	decoder := json.NewDecoder(r.Body) //创建一个json解码器，准备从http请求的主体中读取json数据
	err := decoder.Decode(&book) //获取结构体信息
	if err != nil { 
		errorLog.Println("获取图书失败：", err) //写入日志
		http.Error(w, "获取图书失败", http.StatusBadRequest) //http上显示错误信息
		return
	}

	var existingId int
	err = db.QueryRow("SELECT id FROM books WHERE id = ?", book.Id).Scan(&existingId) //执行数据库语句，QueryRow执行一次查询，返回最多一行结果，将查询的结果给existingId，没查到就返回错误信息
	if err == nil { //如果没错
		errorLog.Println("该Id的书本已存在") //说明已经这个ID了，能被查到
		http.Error(w, "该ID的书本已存在", http.StatusConflict)
		return
	}

	_, err = db.Exec("INSERT INTO books (id, title, author, price, state) VALUES (?, ?, ?, ?, ?)", //添加语句，执行一次命令，不返回执行结果
		book.Id, book.Title, book.Author, book.Price, boolToInt(book.State))
	if err != nil { 
		errorLog.Println("添加书本失败", err) //添加到日志文件
		http.Error(w, "添加书本失败", http.StatusInternalServerError)
		return
	}

	w.WriteHeader(http.StatusOK) //没问题输出
	fmt.Fprintln(w, "书本添加成功")//没问题输出
}
//查看所有书的http接口
func handleChakBooks(w http.ResponseWriter, r *http.Request) {
	rows, err := db.Query("SELECT * FROM books")//执行一次查询语句返回多行
	if err != nil {
		errorLog.Println("无法获取书本", err)
		http.Error(w, "无法获取书本", http.StatusInternalServerError)
		return
	}
	defer rows.Close() //确保rows处理完关闭

	var htmlContent strings.Builder//声明了一个 strings.Builder 类型的变量 htmlContent
	htmlContent.WriteString( //向htmlContent插入字符串数据
		`<html>
			<body>
				<h3>书本列表</h3>
					<table border='1' width = 30%%>
						<tr><th>ID</th><th>标题</th><th>作者</th><th>价格</th><th>状态</th></tr>`)

	for rows.Next() { //遍历查询结果的每一行
		var book Book //定义Book类型的变量book
		if err := rows.Scan(&book.Id, &book.Title, &book.Author, &book.Price, &book.State); err == nil { //从结果集中扫描到当前行的数据并赋值到book结构体的字段中
			htmlContent.WriteString(fmt.Sprintf("<tr><th>%v</th><th>%v</th><th>%v</th><th>%v</th><th>%v</th></tr>",
				book.Id, book.Title, book.Author, book.Price, book.State))
		}//格式化当前书籍的详细信息，并将其作为html表格一行添加到htmlConten中
	}

	htmlContent.WriteString("</table></body></html>") //完成html内容的构建
	w.Header().Set("Content-Type", "text/html; charset=utf-8") //设置html响应头的Content-Type为text/html，指定字符为utf-8（可以显示中文）
	w.Write([]byte(htmlContent.String()))//构建好后将html的内容作为响应的主体发送到客户端
}

func handleChaxBook(w http.ResponseWriter, r *http.Request) {
	id := r.URL.Query().Get("id") // 从查询参数中获取 ID
	var book Book
	row := db.QueryRow("SELECT id, title, author, price, state FROM books WHERE id = ?", id) //执行一次查询语句，返回一条结果
	err := row.Scan(&book.Id, &book.Title, &book.Author, &book.Price, &book.State) //将执行结果状态赋给err，读取的值赋给后面的变量
	if err != nil { //如果有错
		if err == sql.ErrNoRows { //如果错误是没有查询到任何记录
			errorLog.Println("书本不存在")
			http.Error(w, "书本不存在", http.StatusNotFound)
		} else { //其他未知的错误
			errorLog.Println("查询书本失败：", err)
			http.Error(w, "查询书本失败", http.StatusInternalServerError)
		}
		return
	}
	w.Header().Set("Content-Type", "text/html; charset=utf-8")
	//生成一个包含书籍信息的html表格，通过格式化字符串将数据添加到表格中
    htmlContent := fmt.Sprintf(` 
		<html>
			<body>
				<h3>书本信息</h3>
				<table border='1' width = 30%%>
					<tr><th>id</th><th>书名</th><th>作者</th><th>价格</th><th>是否借出</th></tr>
					<tr><th>%v</th><th>%v</th><th>%v</th><th>%v</th><th>%v</th></tr>
				</table>
			</body>
		</html>`, book.Id, book.Title, book.Author, book.Price, book.State)

	w.Write([]byte(htmlContent))//发送到客户端
	w.WriteHeader(http.StatusOK)
}
//删除图书接口
func handleDelBook(w http.ResponseWriter, r *http.Request) {
	id := r.URL.Query().Get("id") // 从查询参数中获取 ID
	// 执行删除操作
	result, err := db.Exec("DELETE FROM books WHERE id = ?", id)
	if err != nil {
		errorLog.Println("删除书本失败：", err)
		http.Error(w, "删除书本失败", http.StatusInternalServerError)
		return
	}
	row, err := result.RowsAffected() //返回受影响的行数
	if err != nil { 
		errorLog.Println("无法获取删除结果：", err)
		http.Error(w, "无法获取删除结果", http.StatusInternalServerError)
		return
	}

	if row == 0 { //返回行数为0
		errorLog.Println("书本不存在")//提交日志
		http.Error(w, "书本不存在", http.StatusNotFound)//客户端上传错误信息
		return
	}

	w.WriteHeader(http.StatusOK)
	w.Write([]byte("书本删除成功"))
}
//创建一个借书的http端口
func handleJieBook(w http.ResponseWriter, r *http.Request) {
	id := r.URL.Query().Get("id")
	var state int //定义状态变量
    //查询状态信息
	row := db.QueryRow("SELECT state FROM books WHERE id = ?", id)
	err := row.Scan(&state) //将查询结果赋值给state，状态给err
	if err != nil { 
		if err == sql.ErrNoRows { //如果错误是没有查询到任何记录
			errorLog.Println("书本不存在")
			http.Error(w, "书本不存在", http.StatusNotFound)
		} else {
			errorLog.Println("查询书本失败：", err)
			http.Error(w, "查询书本失败", http.StatusInternalServerError)
		}
		return
	}
	if state != 0 { //状态不等于0也就是为True
		errorLog.Println("书本状态无效") 
		http.Error(w, "书本已被借出，换一本看看", http.StatusBadRequest)
		return
	}
    //修改状态为1，将false改为true
	_, err = db.Exec("UPDATE books SET state = 1 WHERE id = ?", id)
	if err != nil { //如果有未知错误
		errorLog.Println("更新书本状态失败：", err)
		http.Error(w, "更新书本状态失败", http.StatusInternalServerError)
		return
	}

	w.WriteHeader(http.StatusOK)
	w.Write([]byte("书本借出成功"))
}
//还书接口
func handleHuanBook(w http.ResponseWriter, r *http.Request) {
	id := r.URL.Query().Get("id")
	var state int
	row := db.QueryRow("SELECT state FROM books WHERE id = ?", id)
	err := row.Scan(&state)
	if err != nil {
		if err == sql.ErrNoRows {
			errorLog.Println("书本不存在")
			http.Error(w, "书本不存在", http.StatusNotFound)
		} else {
			errorLog.Println("查询书本失败：", err)
			http.Error(w, "查询书本失败", http.StatusInternalServerError)
		}
		return
	}
	if state == 0 {
		errorLog.Println("书本状态无效")
		http.Error(w, "此书未被借出", http.StatusBadRequest)
		return
	}
	_, err = db.Exec("UPDATE books SET state = 0 WHERE id = ?", id)
	if err != nil {
		errorLog.Println("更新书本状态失败：", err)
		http.Error(w, "更新书本状态失败", http.StatusInternalServerError)
		return
	}
	w.WriteHeader(http.StatusOK)
	w.Write([]byte("书本归还成功"))
}
//将bool值以整形返回，因为在数据库中没有bool，表示逻辑型的INTEGER只有0,1两个值
func boolToInt(b bool) int {
	if b { //传入的值为true时
		return 1 //返回1
	}
	return 0
}

func main() {
    //创建各个接口
	http.HandleFunc("/add", handleAddBook)
	http.HandleFunc("/chak", handleChakBooks)
	http.HandleFunc("/chax", handleChaxBook)
	http.HandleFunc("/del", handleDelBook)
	http.HandleFunc("/jie", handleJieBook)
	http.HandleFunc("/huan", handleHuanBook)
	//提示信息
    fmt.Println("端口已打开 :8080")
	//设置端口号，配上日志文件检查错误，有错上传日志退出程序
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```
-----------------
#### 组合含义
* *sql.DB ：sql包中的一个类型，表示数据库的连接池
* *log.Logge：log包中的一个类型，用于日志记录
* log.Fatal()：检查错误，有错误上传到日志文件并退出程序
* 277行后面参数的含义：
![image-13](C:/Users/HYun/Desktop/文件/xxbj/image/image-13.png)
* 281行后面参数的含义：
![image-14](C:/Users/HYun/Desktop/文件/xxbj/image/image-14.png)
* strings.Builder ： 是Go语言中的一个高效字符串构建工具，适用于需要频繁拼接字符串的场景
* WriteString：是strings.Builder 类型的一部分，它用于将字符串追加到构建器的当前内容中
* w.Write()：要求写入的数据是一个字节切片（[]byte）
* row.Scan()：从sql.Row 对象中读取一行数据，并将其赋值给提供的变量
* sql.ErrNoRows：没有查询到任何记录
* result.RowsAffected()：数据库函数，通常用于查询删除、更新或插入操作后，返回受影响的行数
-------------
#### 语句含义
* _, err = db.Exec(createTableQuery) //执行一次数据库命令，不返回任何值
* w.Write([]byte(htmlContent.String())) //将构建好的 HTML 内容（以字符串形式）转换为字节切片，然后通过 HTTP 响应写入到客户端
* w.Header().Set("Content-Type", "text/html; charset=utf-8") //设置html响应头的Content-Type为text/html，指定字符为utf-8（可以显示中文）