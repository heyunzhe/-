# 学习笔记
## 网站推荐
apt命令：https://baike.baidu.com/item/apt/20109246?fr=aladdin
inux文件系统介绍：https://www.mr-wu.cn/linux-file-systems/
bash备忘清单：https://quickref.aibk.cn/docs/bash.html
搜索linux命令：https://wangchujiang.com/linux-command/hot.html
Linux命令行：https://github.com/jlevy/the-art-of-command-line/blob/master/README-zh.md

-------------
## git
### 1.安装git
在linux上安装：先输入git，看系统有无git，没有git输入：sudo apt-get install git 安装 
**安装完成后需要在命令行输入下方命令方可使用**
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
**学习网站**：https://www.liaoxuefeng.com/wiki/896043488029600/897271968352576
### 2.基本命令用法
* git init 目录名 ：把目录变成git可管理的仓库
* ~ add 文件名：将文件添加到仓库
* ~ commit -m "说明": 将文件提交到仓库 
* ~ status ：仓库当前状态
* ~ diff 文件名：查看文件修改内容
* ~ log ：显示最近到最远的提交日志
* ~ reset --hard HEAD^ ：回退到上一个版本
* ~ reset --hard (前四位)：回到指定版本
* ~ reflog：记录每一次命令
* ~ checkout -- 文件名：把文件在工作区的修改撤销
* ~ reset HEAD 文件名：把暂存区的修改撤销
### 3.远程仓库操作
* 添加到远程库：首先需登录github。创建一个新仓库根据github所给提示操作 
  本地库进入仓库：git remote add origin`https://github.com/heyunzhe/test.git` 【根据官网上给的指令来，每个人不一样】
  重命名当前分支：git branch -M main 
**注意**：没有ssh密钥无法添加进去，需在终端输入：ssh-keygen -t rsa -b 4096 -C "电子邮件"。运行完后输入：cat ~/.ssh/id_rsa.pub 查看公钥，然后在github的设置里复制输入。（window上查看密钥会发到邮箱上）
* 提交到远程库：git push -u origin main(取决于远程仓库的分支名一般为main) (本地只要提交了就可以输入这个指令上传到github)
* 删除远程库：git remote rm origin （其实就是断开和远程库的连接，这样就无法上传到远程库，真要物理删除自己官网上操作）
* 克隆远程库： git clone git@github.com:(库主人的名字)/(库的名字).git
### 4.分支操作
* 创建分支：git branch (分支名) 
* 切换分支：git switch (分支名) 
* 创建并切换分支：git switch -c (分支名)
* 查看所有分支：git branch  其中当前分支前会标注*
* 合并某分支到当前分支：git merge (分支名) 
**注意**：当git无法自动合并分支时，就必须要解决冲突，解决完在合并，可以先用git status看一下哪个文件有问题然后cat 文件名看一下冲突点在哪，冲突的点会用:<<<<<<<,=======,>>>>>>>囊括起来，最后手动打开文件修改
* 删除分支：git branch -d (分支名)
* 查看分支合并情况：git log --graph --pretty=oneline --abbrev-commit

-----------                           
## Markdown 使用
* 网址：https://blog.csdn.net/qq_40818172/article/details/126260661
* 1~6级标题：多少级跟多少个# 最后一个井号后面加空格
* 加粗倾斜：用*和_ 位于文字两侧，倾斜一边一个，粗体一边两个，粗斜体一边三个
* 引用：用>,可以嵌套>>、>>>使用 。效果如下
>1
>>2
>>>3

* 图片：可以直接复制图片粘贴 
*   ![alt text](image-1.png)
* 无序列表：开头用*，+，-都行记得加个空格
* 有序列表：开头使用个数字并加上.和空格
* 分割线：空一行用 _ 或 * 三个以上都可以
* 删除线：在文字前后添加俩~
* 下划线：在文字收尾添加 `<U></U>`
* 代码块:一行引用用反引号`引起来，多行首尾各用三个
* 表格：|分隔单元格，-分隔表头和其他行。在-前添加：为左对齐，右为右对齐，都添加为居中。
* 特殊符号：语法符号前加\可以显示符号本身 \*    
[^1]:  这是一个脚注
--------------------------------------------------------------------------------
## 输入法解决
网站：https://www.linuxdashen.com/linux-mint%E5%AE%89%E8%A3%85ibus%E4%BA%94%E7%AC%94%E8%BE%93%E5%85%A5%E6%B3%95%E7%AE%80%E6%98%8E%E6%95%99%E7%A8%8B
sudo apt-get install ibus ibus-table-wubi ibus-pinyin （安装）
im-config -n ibus(设置框架)
ibus-setup(配置窗口)

-----------
## ssh本地连接
sudo apt-get install openssh-server(安装)
本机软件操作配置ip(输入ifconfig查询ip)输入用户名密码即可

-------------
## linux基本命令
* 部分命令：https://blog.csdn.net/weixin_39665076/article/details/79095540
* mkdir 名称：创建一个目录
* touch 名称：创建一个文件
* cd 名称：切换目录
* vi 文件名：打开文本编辑器编辑
* cat 文件名：打印文件
* clear：清屏
* pwd：显示当前路径
* ls：显示当前目录下所有文件
* cp：复制文件
* rm：删除文件
* mv:移动文件
* chomd：改变文件权限 【读(r)，写(w)，执行(x)】
* tar：打包文件
----------

