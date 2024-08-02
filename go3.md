## 错误处理
### defer+recover机制处理错误
* 展示错误
</br>![image-1](D:/xxbj/image/image-1.png)
从图中可以看出：程序中出现错误/恐慌以后，程序被中断，无法继续执行
* 错误处理/捕获机制：go中追求代码优雅，引入机制:defer+recover机制处理错误
* 内置函数recover 
</br>![image-2](D:/xxbj/image/image-2.png)
* 代码展示：

```golang
func main() {
	test()
	fmt.Println("ok")
	fmt.Println("yes")

}
func test() {
    //利用defer+recover来捕获错误；defer后加上匿名函数的调用
	defer func() {
        //调用recover内置函数，可以捕获错误
		err := recover()
        //如果没有捕获错误，返回值为零值：ni1
		if err != nil {
			fmt.Println("错误已经捕获")
			fmt.Println("err是：", err)
		}
    }()//加括号直接调用匿名函数
	num1 := 10
	num2 := 0
	result := num1 / num2
	fmt.Println(result)
}
```
* 输出结果为：
```
错误已经捕获
err是： runtime error: integer divide by zero
ok
yes
```
* 优点：提高程序健壮性
---------
### 自定义错误
* 需要调用errors包下的New函数：函数返回error类型
![image-3](D:/xxbj/image/image-3.png)
* 代码展示
```golang
func main() {
	err := test()//将test()返回的值赋给err
	if err != nil {
		fmt.Println("自定义错误：", err)
	}
	fmt.Println("ok")
	fmt.Println("yes")

}
func test() (err error) {
	num1 := 10
	num2 := 0
	if num2 == 0 {//抛出自定义错误
		return errors.New("除数不能为0")
	}else {//如果除数不为0就正常执行
		result := num1 / num2
		fmt.Println(result)
		return nil//没有错误返回0值
	}
}
```
* 输出结果为：
```
自定义错误： 除数不能为0
ok
yes
```
------
有一种情况：程序出现错误以后，后续代码就没有必要执行，想让程序中断，退出程序：借助：builtin包下内置函数：panic
![image-4](D:/xxbj/image/image-4.png)
* 代码展示
```golang
func main() {
	err := test()//将test()返回的值赋给err
	if err != nil {
		fmt.Println("自定义错误：", err)
        panic(err)//发现错误中断程序
	}
	fmt.Println("ok")//若发现错误将不执行
	fmt.Println("yes")//同上

}
func test() (err error) {
	num1 := 10
	num2 := 0
	if num2 == 0 {//抛出自定义错误
		return errors.New("除数不能为0")
	}else {//如果除数不为0就正常执行
		result := num1 / num2
		fmt.Println(result)
		return nil//没有错误返回0值
	}
}
```
* 运行结果为：
```
自定义错误： 除数不能为0
panic: 除数不能为0

goroutine 1 [running]:
main.main()
        /home/user/GoPath/gocode3/go1/main/main.go:12 +0x125
exit status 2
```
------