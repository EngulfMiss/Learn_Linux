# Learn_Linux
学习linux
## 常用的基本命令
### 目录管理
**绝对路径 相对路径**  
cd ：切换目录命令 绝对路径以/开头 相对路径 ../上一级是目录 根据与当前目录的相对位置查找  
./ ：当前目录  
cd.. ：返回上一级目录  
  
ls ：列出目录  
-a参数：all，查看全部的文件，包括隐藏文件  
-l参数：列出所有的文件，包含文件的属性和权限，没有隐藏文件  
可以组合使用 -al   

psd ：显示当前所在目录  

mkdir ：创建一个目录  
mkdir -p ：创建层级目录(递归创建)  

rm ：移除文件或者文件夹  
-f：忽略不存在的文件，不会出现警告，强制删除
-r：递归删除目录  
-i：互动，删除时询问是否删除


rmdir ：删除目录，只能删除空目录，如果下面存在文件，需要先删除文件  
rmdir -p ：递归删除多个目录  

cp ：复制文件或者目录  
cp 拷贝文件 新的地方  

mv ：移动文件或文件夹/重命名文件夹  
-f 强制  
-u 只替换更新过的文件  

### 文件属性

![image](https://user-images.githubusercontent.com/61497283/131207803-0f1cd742-3603-4756-8db0-be78c349b29b.png)  

在Linux中开头第一个字符代表这个文件是目录，文件或者链接文件等：  
- [d]：表示目录
- [-]：表示文件
- [l]：表示为链接文档
- [b]：表示为装置文件里面的可供存储的接口设备
- [c]：表示为装置文件里面的串行端口设备，例如键盘鼠标(一次性读取装置)  

接下来的字符中，以三个为一组，且均为[rwx]的三个参数的组合  
其中，[r]表示可读(read),[w]表示可写(write),[x]表示可执行(execute)  
这三个权限的位置不会改变，如果没有权限，就会出现减号[-]而已  
每个文件的属性由左边第一部分的10个字符来确定，如上图  

![image](https://user-images.githubusercontent.com/61497283/131208069-2171d0e8-ecbe-4c89-a468-a3d34b8aa65f.png)

### 修改属性
1. chgrp：更改文件属组  
chgrp [-R] 属组名 文件名  
2. chown：更改文件属主，也可以同时更改文件属组  
chown [-R] 属主名 文件名  
chown [-R] 属主名: 属组名 文件名  
**3. chmod：更改文件9个属性(必须掌握)**  
当你没权限操作文件时就可能需要修改权限了  
chmod [-R] xyz 文件或目录  
例如：chmod 777 filename  , 7 = 4(r) + 2(w) + 1(x)  
Linux文件属性有两种设置方法，一种是数字()，一种是符号  
Linux文件的基本权限有9个，分别是owner/group/others三种身份和各自的read/write/execute权限  
```
r:4  w:2  x:1
```

### 查看文件
- cat：由第一行开始显示文件内容
- tac：从最后一行开始显示
- nl：显示的时候，输出行号
- more：一页一页的显示文件内容(空格表示翻页，enter代表向下看一行，:f行号)
- less：和more类似，但它可以往前翻页(空格表示翻页，上下键，pageUP，pageDown代表翻动页面，退出是q命令，就是按下q)  
(文件中搜索 /搜索内容 要查找的字符向下查询，向上查询使用 ?查询内容 ,n/N继续搜寻下一个/上一个，和你选择的查询方式有关)  

- head：只看头几行，通过-n参数控制显示多少行 例如：head -n 20
- tail：只看尾几行，通过-n参数控制显示多少行 例如：tail -n 20

### Linux链接的概念
Linux的链接分为两种：硬链接和软链接  
硬链接：A---B，假设B是A的硬链接，那么它们两个指向了同一个文件，允许一个文件拥有多个路径(在删除源文件的时候，系统则将链接数减1，当链接数为0的时候，inode就会被系统回收，文件的内容才会被删除。)  
软链接：类似Windows下的快捷方式，删除源文件，快捷方式也访问不了  
创建硬链接ln 谁 被谁链接   
创建软连接ln -s 谁 被谁链接  
创建文件 touch  
输入字符串 echo  ,echo "内容" >> 文件名  
```
[root@iZ2zedwea74ejj95zb9sh0Z /]# cd home
[root@iZ2zedwea74ejj95zb9sh0Z home]# clear
[root@iZ2zedwea74ejj95zb9sh0Z home]# touch f1  # 创建一个文件f1
[root@iZ2zedwea74ejj95zb9sh0Z home]# ls
f1  mildlamb  myimage  tomcat
[root@iZ2zedwea74ejj95zb9sh0Z home]# ln f1 f2  # 创建一个硬链接f2
[root@iZ2zedwea74ejj95zb9sh0Z home]# ls
f1  f2  mildlamb  myimage  tomcat
[root@iZ2zedwea74ejj95zb9sh0Z home]# ln -s f1 f3  # 创建一个软链接f3
[root@iZ2zedwea74ejj95zb9sh0Z home]# ls
f1  f2  f3  mildlamb  myimage  tomcat
[root@iZ2zedwea74ejj95zb9sh0Z home]# ll
total 12
-rw-r--r-- 2 root root    0 Aug 28 15:16 f1
-rw-r--r-- 2 root root    0 Aug 28 15:16 f2
lrwxrwxrwx 1 root root    2 Aug 28 15:17 f3 -> f1
drwxr-xr-x 2 root root 4096 Aug 28 11:54 mildlamb
drwxr-xr-x 2 root root 4096 Aug 28 11:29 myimage
drwxr-xr-x 3 root root 4096 Aug 28 11:41 tomcat
[root@iZ2zedwea74ejj95zb9sh0Z home]# echo "I love Kindred" >> f1  # 给f1文件中写入一些字符串
[root@iZ2zedwea74ejj95zb9sh0Z home]# clear
[root@iZ2zedwea74ejj95zb9sh0Z home]# ll
total 20
-rw-r--r-- 2 root root   15 Aug 28 15:17 f1
-rw-r--r-- 2 root root   15 Aug 28 15:17 f2
lrwxrwxrwx 1 root root    2 Aug 28 15:17 f3 -> f1
drwxr-xr-x 2 root root 4096 Aug 28 11:54 mildlamb
drwxr-xr-x 2 root root 4096 Aug 28 11:29 myimage
drwxr-xr-x 3 root root 4096 Aug 28 11:41 tomcat
[root@iZ2zedwea74ejj95zb9sh0Z home]# cat f1   # 查看各个文件内容
I love Kindred
[root@iZ2zedwea74ejj95zb9sh0Z home]# cat f2
I love Kindred
[root@iZ2zedwea74ejj95zb9sh0Z home]# cat f3
I love Kindred
[root@iZ2zedwea74ejj95zb9sh0Z home]# rm -rf f1   # 删除源文件f1
[root@iZ2zedwea74ejj95zb9sh0Z home]# ls
f2  f3  mildlamb  myimage  tomcat
[root@iZ2zedwea74ejj95zb9sh0Z home]# cat f2     # 硬链接依旧可以查看
I love Kindred
[root@iZ2zedwea74ejj95zb9sh0Z home]# cat f3     # 软连接不能查看了
cat: f3: No such file or directory
```

## Vim
vi/vim共分为三种模式，分别是命令模式(Command mode),输入模式(Insert mdoe)和底线命令模式(Lastline mode)  
**命令模式：**  
- i:切换到输入模式，以输入字符
- x：删除当前光标所在处的字符
- : ：切换到底线命令模式，如果是编辑模式需要先退出编辑模式(ESC)

命令模式只有一些最基本的命令，因此要依靠底线命令模式获取更多命令  

**输入模式:**  
可以编辑

**底线命令模式:**  
在命令模式下按:(英文冒号)进入底线命令模式，光标移动到最后，可以输入一些底线命令了  
- q:退出程序
- w:保存文件
- 直接wq:保存并退出
