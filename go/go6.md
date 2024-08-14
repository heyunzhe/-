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
## 打开http接口实现对图书数据的增删改查
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
## 拓展
* 运行Postman：/opt/Postman/Postman/Postman
