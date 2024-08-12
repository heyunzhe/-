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

http://localhost:8080

func main() {
	http.HandleFunc("/", handler)
	fmt.Println("Server is running on port 8080")
	http.ListenAndServe(":8080", nil)
}

http://223.94.36.124:8080/update