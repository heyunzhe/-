## 错误处理
1. 展示错误
</br>![alt text](D:/xxbj/image/image-1.png)
发现：程序中出现错误/恐慌以后，程序被中断，无法继续执行
2. 错误处理/捕获机制：go中追求代码优雅，引入机制:defer+recover机制处理错误
3. 内置函数recover 