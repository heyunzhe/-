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
        //如果没有捕获错误，返回值为零值：nil
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
## 数组
### 定义
* 格式： var 数组名 [数个数]数据类型 例如：
```golang
func main() {
	var tests [5]int //定义数组
	tests[0] = 1 //赋值
	tests[1] = 2
	tests[2] = 3
	tests[3] = 4
	tests[4] = 5
    sum := 0
	for i := 0; i < len(tests); i++ {
		sum += tests[i]//根据下标提取数
	}
	fmt.Println(sum)
	avg := sum / (len(tests) - 1)
	fmt.Println(avg)
}
```
--------
### 内存分析
```golang
func main() {
	var arr [3]int //定义整型数组
	fmt.Println(len(arr))//输出3
	fmt.Println(arr)//输出[0,0,0]
	fmt.Printf("arr的地址为：%p\n", &arr)//输出0xc00001a1b0
	fmt.Printf("arr的地址为：%p\n", &arr[0])//输出0xc00001a1b0
	fmt.Printf("arr的地址为：%p\n", &arr[1])//输出0xc00001a1b8
	fmt.Printf("arr的地址为：%p\n", &arr[2])//输出0xc00001a1c0
}
```
ps：从输出结果可以看出数组长度为3，赋值默认值都为0，数组的地址就是数组中第一个数的地址，由于int类型占8个字节，所以数组中每下一个数的地址都要在上一个地址的基础值上加8（地址为十六进制），又或者：（整体数组的地址）+（定义的类型所占的字节数*数组中数的下标）求出数组中某个数的地址

---------
### 遍历
* 方法一：普通for循环遍历，例如：
```golang
func main() {
	var arr [10]int
	fmt.Println("请输入10个数")//提示信息
	for i := 0; i < len(arr); i++ {
		fmt.Printf("第%d个数：", i+1)//提示信息
		fmt.Scanln(&arr[i])//从键盘上输入数
	}
	for i := 0; i < len(arr); i++ {
		fmt.Printf("第%d个数为：%d\n", i+1, arr[i])//显示最终输入结果
	}
}
```
* 方法二：键值循环
* 格式：for key，val := range cell 
1. coll为你要遍历的数组
2. 每次遍历得到的索引用key接收，每次遍历得到的索引位置上的值用val
3. key,val的名字可以随便起
4. key，val属于在这个循环中的局部变量
* 代码展示：
```golang
func main() {
	var arr [10]int
	fmt.Println("请输入10个数")
	for i := 0; i < len(arr); i++ {
		fmt.Printf("第%d个数：", i+1)
		fmt.Scanln(&arr[i])
	}
	for key, val := range arr { 
		fmt.Printf("第%d个数的值为：%d\n", key+1, val)//显示最终输入结果
	}
}
```
--------
### 初始化
* 给数组赋值，例如：
```golang
func main() {
    //方法1
	var arr1 [3]int = [3]int{1, 2, 3}
	fmt.Println(arr1)//输出[1,2,3]
    //方法2
	var arr2 = [3]int{2, 3, 4}
	fmt.Println(arr2)//输出[2,3,4]
    //方法3
	var arr3 = []int{5, 3, 6, 1}
	fmt.Println(arr3)//输出[5,3,6,1]
    //方法4
	var arr4 = []int{1: 10, 0: 5, 3: 2, 2: 7}//指定下标的值
	fmt.Println(arr4)//输出[5,10,7,2]
}
```
* **注意：赋值用大括号{}**
---------
### 注意事项
1. 数组长度属于类型的一部分，例如：
```golang
func main() {
	var arr1 = [3]int{1, 2, 3}
	fmt.Println(arr1)
	fmt.Printf("数组的类型为：%T\n", arr1)//输出int[3]

	var arr2 = [6]int{1, 2, 3, 4, 5, 6}
	fmt.Println(arr2)
	fmt.Printf("数组的类型为：%T\n", arr2)//输出int[6]
}
```
2. go中数组属于值类型，在默认情况下是值传递，因此会进行拷贝，例如：
```golang
func main() {
	var arr = [3]int{1, 2, 3}
	test1(arr)
	fmt.Println(arr) //输出[1,2,3]
}
func test1(arr [3]int) {
	arr[0] = 7
}
```
ps:因为是拷贝，所以传入函数test1的是一个[1,2,3]的复制，改的也只是这个复制中的arr[0],不会影响arr[3]数组
3. 如果想在其他函数中，去修改原来的数组，可以引用传递（指针方式），例如：
```golang
func main() {
	var arr = [3]int{1, 2, 3}
	test1(&arr)//传入arr数组的地址
	fmt.Println(arr)//输出[7,2,3]
}
func test1(arr1 *[3]int) {//定义一个指针变量
	(*arr1)[0] = 7 //修改指针指向为7
}
```
--------
### 二维数组
#### 定义
* 代码展示：
```golang
func main() {
	var arr [2][3]int16//定义
	fmt.Println(arr)//输出[[0 0 0] [0 0 0]]
}
```
------
#### 内存分析
* 代码展示：
```golang
func main() {
	var arr [2][3]int16
	fmt.Printf("arr的地址是：%p\n", &arr)
	fmt.Printf("arr[0]的地址是：%p\n", &arr[0])
	fmt.Printf("arr[0][0]的地址是：%p\n", &arr[0][0])

	fmt.Printf("arr[1]的地址是：%p\n", &arr[1])
	fmt.Printf("arr[1][0]的地址是：%p\n", &arr[1][0])
}
```
* 输出结果：
```
arr的地址是：0xc0000a6010
arr[0]的地址是：0xc0000a6010
arr[0][0]的地址是：0xc0000a6010
arr[1]的地址是：0xc0000a6016
arr[1][0]的地址是：0xc0000a6016
```
PS:从输出结果可以看出arr的地址=arr[0]=arr[0][0],arr[1]的地址=arr[1][0]的地址，arr地址与arr[1]地址末位不同差了6,因为定义arr数组为int16类型，int16类型占两字节所以可以得出arr[0][1]地址末位为2，arr[0][2]末位地址为4，接下来就到了arr[1][0]，所以末位为6

--------
#### 赋值操作
* 代码展示：
```golang
func main() {
	var arr [2][3]int16
    arr[0][0] = 100
	arr[0][1] = 85
	arr[1][2] = 75
	fmt.Println(arr)//输出[[100 85 0] [0 0 75]]
}
```
-------
#### 初始化
* 代码展示：
```golang
func main() {
    //方法1
	var arr1 [2][3]int = [2][3]int{{1, 2, 3}, {4, 5, 6}}
	fmt.Println(arr1)//[[1 2 3] [4 5 6]]
    //方法2
	var arr2 = [2][3]int{{7, 8, 9}, {2, 3, 4}}
	fmt.Println(arr2)//[[7 8 9] [2 3 4]]
    //方法3
	var arr3 = [][]int{{1, 2, 3, 4}, {5, 3, 6, 1}}
	fmt.Println(arr3)//[[1 2 3 4] [5 3 6 1]]
    //方法4
	var arr4 = [][]int{{1: 10, 0: 5}, {1: 6, 0: 8}}
	fmt.Println(arr4)//[[5 10] [8 6]]
}
```
ps：初始化时数组中间用逗号间隔，整体外围在套一个大括号{}

------
#### 遍历
* 方法1：普通for循环遍历，例如：
```golang
func main() {
	var arr [3][3]int = [3][3]int{{1, 2, 3}, {4, 5, 6}, {7, 8, 9}}
	fmt.Println(arr)
	for i := 0; i < len(arr); i++ {//len(arr)=3
		for j := 0; j < len(arr[i]); j++ {//len(arr[0])=3
			fmt.Print(arr[i][j], "\t")
		}
		fmt.Println()
	}
}
```
* 方法2：健值循环，例如：
```golang
func main() {
	var arr [3][3]int = [3][3]int{{1, 2, 3}, {4, 5, 6}, {7, 8, 9}}
	fmt.Println(arr)
	for key, value := range arr {
		for k, v := range value {
			fmt.Printf("arr[%v][%v]=%v\t", key, k, v)
		}
		fmt.Println()
	}
}
```
PS：
1. key：表示当前遍历到的外层数组（即二维数组的“行”）的索引
2. value：表示当前遍历到的外层数组元素的值，即一个包含3个整数的数组（[3]int）
3. k：表示当前遍历到的内层数组（即二维数组的“行”中的“列”）的索引
4. v：表示当前遍历到的内层数组元素的值，即arr中某一行某一列的整数值
---------
## 切片
**概念**：
* 切片（slice）是golang中一种特有的数据类型，切片建立在数组类型之上的抽象，它构建在数组之上并且提供更强大的能力和便捷。
* 切片（slice）是对数组一个连续片段的引用，所以切片是一个引用类型，这个片段可以是整个数组，或者是由起始和终止索引标识的一些项的子集，需要注意的是，终止索引标识的项不包括在切片内，切片提供了一个相关数组的动态窗口
* 格式：var 切片名 []类型 = 数组的一个片段引用
* 代码展示：
```golang
func main() {
	//定义数组
	var intarr [6]int = [6]int{1, 3, 5, 7, 9, 11}
	//切片构建在数组之上
	//定义一个切片名字为slice，[]动态变化的数组长度不写，int类型，intarr是原数组
	//[1:3]切片 - 切出的一段片段 - 索引：从1开始，到3结束（不包含3）- [1,3)
	//var slice []int = intarr[1:3]
	var slice []int = intarr[1:3]
    //输出数组
	fmt.Println("intarr：", intarr)
    // 输出切片
	fmt.Println("slice：", slice)
    //切片元素个数
	fmt.Println("slice的元素个数：", len(slice))
    //获取切片的容量，容量可以动态变化
	fmt.Println("slice的容量：", cap(slice))
}
```
* 输出结果
```
intarr： [1 3 5 7 9 11]
slice： [3 5]
slice的元素个数： 2
slice的容量： 5
```
-------
### 内存分析
* 切片有3个字段的数据结构：一个是指向底层数组的指针，一个是切片的长度，一个是切片的容量
* 代码展示：
```golang
func main() {
	var intarr [6]int = [6]int{1, 3, 5, 7, 9, 11}
	var slice []int = intarr[1:3]//取数组中下标为1和2的数组成的数组
	fmt.Printf("%p\n", &intarr[1])//数组中下标为1的内存地址与下相等
	fmt.Printf("%p\n", &slice[0])//切片中下标为0的内存地址与上相等
	slice[1] = 16 //改变指针指向
	fmt.Println(intarr)//[1,3,16,7,9,11]
	fmt.Println(slice)//[3,16]
}
```
### 定义
* 格式：var 切片名 []类型 = 数组的一个片段引用
* 方法1：定义一个切片，然后让切片去引用一个已经创建好的数组，例如：
```golang
var intarr [6]int = [6]int{1, 3, 5, 7, 9, 11}//定义数组
var slice []int = intarr[1:3]//定义切片
```
* 方法2：通过make内置函数来创建切片
基本语法：var 切片名[type = make([],len,[cap])]
* 代码展示：
```golang
func main() {
	//定义切片：make函数的三个参数：1.切片类型 2.切片长度 3.切片的容量
	slice := make([]int, 4, 20)
	fmt.Println(slice)//[0 0 0 0]
	fmt.Println("切片的长度：", len(slice)) //4
	fmt.Println("切片的容量：", cap(slice)) //20
	slice[0] = 66
	slice[1] = 88
	fmt.Println(slice)//[66 88 0 0]
}
```
ps:make底层创建一个数组，对外不可见，所以不可以直接操作这个数组，要通过slice去间接的访问各个元素，不可以直接对数组进行维护/操作
* 方法3：定义一个切片，直接就指定具体数组，使用原理类似make的方式
* 代码展示：
```golang
func main() {
	slice2 := []int{1, 4, 7}
	fmt.Println("切片的长度：", len(slice2))//3
	fmt.Println("切片的容量：", cap(slice2))//3
	fmt.Print(slice2)//[1 4 7]
}
```
-------------
### 遍历
* 方法1：普通for遍历，例如：
```golang
func main() {
	slice := make([]int, 4, 20)//定义切片
	slice[0] = 66
	slice[1] = 88
	slice[2] = 77
	slice[3] = 99//赋值
	for i := 0; i < len(slice); i++ {
		fmt.Printf("slice[%v]= %v \t", i, slice[i])
	}
}
```
* 方法2：for range，例如：
```golang 
func main() {
	slice := make([]int, 4, 20)
	slice[0] = 66
	slice[1] = 88
	slice[2] = 77
	slice[3] = 99
	for key, val := range slice {
		fmt.Printf("slice[%v]= %v \t", key, val)
	}
}
```
ps：key表示索引值/下标，下标中的值给val，对slice切片进行遍历

-------
### 注意事项
1. 切片定义后不可以直接使用，需要将其引用到一个数组，或者make一个空间供切片来使用
2. 切片使用的时候不能越界，例如：
```golang
func main() {
	var abc [6]int = [6]int{1, 2, 3, 4, 5, 6}
	var slice []int = abc[1:4]
    fmt.Println(slice[3])//越界报错
}//切片只有三个数[2,3,4]下标为0,1,2没有3
```
3. 简写方式：
* var slice = arr[0:end] ---》 var slice = arr[:end]
* var slice = arr[start:len(arr)]----》 var slice = arr[start:]
* var slice = arr[0:len(arr)]---》 var slice = arr[:]
4. 切片可以继续切片，例如：
```golang
func main() {
	var abc [6]int = [6]int{1, 2, 3, 4, 5, 6}//定义数组
	var slice []int = abc[1:4]//定义第一个切片
	fmt.Println(slice)//[2 3 4]
	var slice2 []int = slice[1:2]//定义第二个切片
	fmt.Println(slice2)//[3]
    slice2[0] = 66 //修改数值为66
	fmt.Println(slice)//[2 66 4]
	fmt.Println(abc)//[1 2 66 4 5 6]
}
```
5. 切片可以动态增长，例如：
```golang
func main() {
	var abc [6]int = [6]int{1, 2, 3, 4, 5, 6}
	var slice []int = abc[1:4]//[2 3 4]
	fmt.Println(len(slice))//3
	slice2 := append(slice, 88, 50)
	fmt.Println(slice2)//[2 3 4 88 50]
// 底层原理：
// 1.底层追加元素的时候对数组进行扩容，老数组扩容为新数组
// 2.创建一个新数组，将老数组中的2，3，4复制到新的数组中，在新数组中追加88，50
// 3.slice2 底层数组的指向 指向的是新数组
// 4.往往我们在使用追加的时候其实想要做的效果给slice追加
	slice = append(slice, 88, 50)
	fmt.Println(slice)//[2 3 4 88 50]
//5.底层的新数组不能直接维护，需要通过切片间接维护操作
//可以通过append函数将切片追加给切片，例如：
slice3 := []int{99, 44}
slice = append(slice, slice3...)//...一定要加
fmt.Println(slice)//[2 3 4 88 50 99 44]
}
```
6. 切片的拷贝，例如
```golang
func main() {
	var a []int = []int{1, 2, 3, 4, 5, 6}//定义切片
	var b []int = make([]int, 10)//在定义一个切片
	copy(b, a)//将a中对应数组中元素内容复制到b中对应的数组中
	fmt.Println(b)//[1 2 3 4 5 6 0 0 0 0]
}
```
## 映射
* **概念：** go语言中内置的一种类型，他将**健值对**相关联，我们可以通过健key来获取对应的值value。类似其他语言的集合
* 健值对：一对匹配信息，类似于学生的学号对应学生的姓名，例如：18 - 张三；18为健key，张三为值value
* 基本语法：var map变量名 map[keytype]valuetype
1.key,value的类型：bool、数字、string、指针、channel、还可以是只包含前面几个类型的接口、结构体、数组。
2.key通常为int、string类型，value经常为数字（整数、浮点数）、string、map、结构体。
3.key中：slice map function不可以
* 特点：
1.map集合在使用前一定要make
2.map的key-value是无序的
3.key是不可以重复的，如果遇到重复，后一个value会替换前一个value
4.value可以重复
5.make函数的第二个参数size可以省略，默认就分配一个内存
* 代码展示：
```golang
func main() {
	var a map[int]string//定义map变量
	a = make(map[int]string, 10)
	a[100] = "你"
	a[400] = "你"
	a[500] = "好"
	a[600] = "吗"
	a[200] = "好"
	a[300] = "吗"
	fmt.Println(a)
}
```
### map创建方式
* 方法1：
```golang
func main() {
    var a map[int]string//定义map变量
	a = make(map[int]string, 10)
	a[100] = "你"
	a[400] = "你"
}
```
* 方法2：
```golang
b := make(map[int]string)
b[400] = "你"
b[500] = "好"
fmt.Println(b)
```
* 方法3
```golang
c := map[int]string{
	123: "abc",
	234: "fhd",
}
c[1231] = "nihao"
fmt.Println(c)
```
### map操作
* 修改操作：map["key"] = value --->如果有key就是修改，没有就是增加，例如：
```
b := make(map[int]string)
b[400] = "你"
b[400] = "hao"
fmt.Println(b)//输出map[400:hao]
```
* 删除操作：delete(map,"key),delete是一个内置函数，如果key存在，就删除key-value，如果k的y不存在，不操作，但是也不会报错
```
b[400] = "ni"
fmt.Println(b)
delete(b, 400)
fmt.Println(b)
```
* 清空操作：
1.如果我们要删除map的所有key，没有一个专门的方法一次性删除，可以遍历一下key，逐个删除
2.或者map = make(...),make一个新的，让原来的成为垃圾，被gc回收
* 查找操作：value ,bool = map[key]
value为返回的value，bool为是否返回，要么true要么false，例如：
```
value, flag := b[500]
fmt.Println(value)
fmt.Println(flag)
```
* 获取长度len函数
* 遍历：for-range,例如：
```golang
func main() {
	var a map[int]string
	a = make(map[int]string, 10)
	a[100] = "你"
	a[400] = "你"
	a[500] = "好"
	a[600] = "吗"
	a[200] = "好"
	a[300] = "吗"
	fmt.Println(len(a))
	for k, v := range a {
		fmt.Printf("key: %v value: %v \t", k, v)
		fmt.Println()
	}
}
```
* 进阶遍历,当定义map后，把value在定义成map，代码展示：
```golang
func main() {
	a := make(map[string]map[int]string)

	a["班级1"] = make(map[int]string, 3)
	a["班级1"][33112201] = "ll"
	a["班级1"][33112202] = "mm"
	a["班级1"][33112203] = "ff"

	a["班级2"] = make(map[int]string, 3)
	a["班级2"][33112204] = "hh"
	a["班级2"][33112205] = "bb"
	a["班级2"][33112206] = "ww"

	for k1, v1 := range a {
		fmt.Println(k1)
		for k2, v2 := range v1 {
			fmt.Printf("学生的学号为：%v 学生的姓名为：%v \t", k2, v2)
		}
		fmt.Println()
	}
}
```
输出结果为：
```
班级1
学生的学号为：33112201 学生的姓名为：ll         学生的学号为：33112202 学生的姓名为：mm         学生的学号为：33112203 学生的姓名为：ff 
班级2
学生的学号为：33112204 学生的姓名为：hh         学生的学号为：33112205 学生的姓名为：bb         学生的学号为：33112206 学生的姓名为：ww 
```
--------