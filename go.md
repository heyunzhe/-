## go语言学习
* 学习网站：https://gitee.com/dopaminereceptor/over-golang/tree/master
* 官网：https://golang.org/dl/
### 安装go
1. linux上输入：wget https://dl.google.com/go/go1.22.5.linux-amd64.tar.gz【下载压缩包,可以在官网上找版本】
2. 解压：sudo tar zxvf go1.22.5.linux-amd64.tar.gz -C /usr/local/ 【注意：如果解压到的目录是root权限一定要加**sudo**不然解压不进去,可以通过“**ls -la** 目录名” 查看目录的权限是哪个用户的】
3. 配置：sudo vim /etc/profile 打开目录【不加sudo可能打开了无法编辑】 底下添加export GOROOT=/usr/local/go  export PATH=$PATH:$GOROOT/bin
重启一下环境：source /etc/profile  应该就可以用了，可以使用go version 查询一下有无安装成功 
### 运行go程序：
1. go run 文件名 【记得是要在当前文件的目录下，适合运行一些简单的go程序】
2. 使用 go build 文件名 or go install 文件名 生成可执行文件使用 ./文件名 运行 【都要基于当前文件目录下】
### go的注意事项
1. 源文件以go为扩展名
2. 程序执行入口为main()函数
3. 严格区分大小写
4. 方法由一条条语句构成，每个语句后面不需要加；
5. go是一行行进行编译，所以一行只能写一条语句，不要写多个语句
6. 定义的变量或者import的包没有使用到，就不能编译成功
7. 大括号为成对出现缺一不可
8. 一行内最长不超过80个字符，超过了换行在行尾用，
### 注释
1. 单行注释：代码后加//输入注释 快捷键ctrl+/
2. 多行注释：/* 注释内容 */  快捷键shift+alt+a