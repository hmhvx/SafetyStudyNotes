### linux基础命令

#### 1.Linux 系统目录结构

系统目录结构
在 Windows 系统中，查看文件先进入相应的盘符，它是以单独分区，每个分区都单独命名，通常的命名有 c 盘、d 盘、e 盘、等等，先进入分区然后进入文件目录，如图 1-1 所示。

![img](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202206291016717.jpg)
图 1-1 Windows 文件系统

Linux 只有一个根目录、是整个文件系统的入口，如图 4-2 所示。
![img](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202206291016721.jpg)
图 1-2 Linux 文件系统

#### 2.绝对路径和相对路径

路径：在我们平时使用计算机时要找到需要的文件就必项知道文件的位置，而表示文件的位置的方式就是路径。
绝对路径：在 Linux 中，绝对路径是从”/”开始的，比如/usr、/etc/passwd。如果一个路径是从根
（/）开始的，它一定是绝对路径，简单来说就是从最开始处，直到结束处，称为绝对路径。

相对路径：相对路径是以 . 或 .. 开始的，相对于当前所处的位置路径，当需要找到目标文件时，起始点是从根不目标文件之间的查找路径，如果起点不在目的文件直枝上，需要回到根目录到目标文件所经过的某个节点。

[root@study etc]# pwd #判断用户当前所处的位置。
绝对路径： 从/开始的路径 /home/lin
相对路径： 相对于当前目录开始，a.txt ./a.txt ../miao/b.txt 

当前目录在/etc。

[root@study ~]# cd /etc/ #切换目录到/etc。
[root@study etc]# ll passwd #相对路径。
-rw-r--r-- 1 root root 2116 11 月 16 14:57 passwd 

[root@study etc]# ll /etc/passwd #绝对路径。 

-rw-r--r-- 1 root root 2116 11 月 16 14:57 /etc/passwd  

#### 3.基本命令之 ls

作用：列出指定路径或当前目录下的子目录和文件。
语法：ls 目录/文件 ，ls 不加参数时，只列出当前目录的子目录和文件。常用选项：
（1） -h ：显示大小单位
（2） -a ：all 显示包括隐藏文件目录，linux 下隐藏文件以 . 开头，在每个目录下都有 . 和 .. 这两个隐藏目录
（3） -l ：以长格式查看文件
（4） -r ：逆序显示文件，默认是顺序显示文件排列
( 5） -R ：递归（recursive）显示,此方式显示比较消耗资源，比如一个目录下有百个目录而每个目录下都有上百局，上万个文件，因为将要显示的数据都是先调入内存中，此时用这种方式显示 ，所以内存的大量缓存会用于显示目录

 

#### 4.基本命令之 cd


作用：切换目录（change directory）
命令使用格式：cd [目录]
说明：直接输入 cd 表示回到当前用户的宿主（家）目录。
[root@base ~]# cd /etc/sysconfig/network-scripts/ #切换目录。
[root@base network-scripts]# cd #直接使用 cd 命令回到用户家目录。
[root@base ~]# cd ~ #使用 cd ~和直接使用cd 是同样意义。
cd .. 表示返回到上级目录位置，也就是父目录。
[root@base ~]# cd .. #返回上一级目录。等同于cd ../
cd . 表示进入到当前用户所在的目录。等同于cd ./
[root@base /]# cd . #切换到当前目录。
[root@base ~]# cd - #切换到上一次切换的目录，在上次所处的目录切换到当前目录之间来回切换。
[root@base /]# cd /etc/sysconfig/network-scripts/
[root@base network-scripts]# cd - #使用 cd- 直接返回到上次切换至此的目录/。





#### 5.pwd 命令 （定位设备）

print work dir 打印当前的工作 绝对路径





#### 6.命令之touch ：用于创建文件。

作用：常用来创建空文件，如果文件存在，则修改这个文件的时间。补充：文件的三种时间，包括访问时间、修改时间、改变时间。
语法：touch 文件名
[root@base ~]# cd /opt/ #切换工作目录到/opt。
[root@base opt]# touch a.txt #创建 a.txt 文件。
[root@base opt]# touch file1 file2 #同时指定多个文件名称创建 file1 和 file2 文件。
[root@base opt]# touch file{6..20} #使用命令展开式创建 file6 到file20 的文件。
[root@base opt]# ls
a.txt file10 file12 file14 file16 file18 file2 file6 file8 rh file1 file11 file13 file15 file17 file19 file20 file7 file9



#### 7.命令之：mkdir 

作用：创建目录。
命令使用格式：mkdir (选项) 目录名

例 4.1：创建目录。
[root@base opt]# mkdir dir1 #创建 dir1 目录。
[root@base opt]# mkdir dir2 dir3 /home/dir4 #可以同时指多个目录。
[root@base opt]# ls /home/dir4
[root@base opt]# mkdir /tmp/a/b/c #级联创建目录。mkdir: 无法创建目录"/tmp/a/b/c": 没有那个文件或目录
[root@base opt]# mkdir -p /tmp/a/b/c #在创建一个目录的时候，如果这个目录的上一级不存在的话，要加参数-p，级联创建目录。
[root@base opt]# ls /tmp/a/b/ c



#### 8.删除文件和目录用到的命令：rm

命令使用格式：rm [选项] 文件/目录
作用：可以删除一个目录中的一个或多个文件或目录，对于链接文件，只是删除整个链接文件，而原文件保持不变的。
选项：
（1） -f 强制删除，没有提示。
（2） -r 删除目录。

例 4.2：删除文件。
[root@base opt]# rm -rf a.txt #强制删除 a.txt 文件。
[root@base opt]# rm -rf a.txt dir #强制删除 a.txt 文件和 dir 目录。
[root@base opt]# rm -rf file\* #强制删除 file 开头文件。
rm -rf (慎用,一定要在删除以前确定一下所在目录，防止误删除重要数据)



#### 9.复制文件

命令：cp 源文件/目录 目录文件/目录。
选项：-r：递归处理，将指定目录下的所有文件与子目录一并处理。

例 4.3：复制文件。
[root@base ~]# cp /etc/passwd /opt/ #复制/etc/passwd 文件到/opt 目录下。
[root@base ~]# cp -r /boot/grub /opt/ #复制/boot/grub 目录到/opt 目录下。

[root@study opt]# cp /etc/passwd /opt/passwd1#复制/etc/passwd 文件到/opt 目录下并修改名字

#### 10.移动文件

命令：mv 
作用：用于移动文件或目录，更改文件或目录名称。
[root@base opt]# mv passwd dir1 #移动当前工作目录下的 passwd 文件到工作目录dir1 目录下，如果没有 dir1 目录时，则把 passwd 文件重命名为 dir1。
在移动文件的时候支持改名操作：
[root@base opt]# mv xuegod.txt dir1/a.txt #移动当前工作目录下的 xuegod.txt 文件到当前工作目录下的 dir1 目录下，并重命名为 a.txt。
[root@base opt]# ls dir1/ 
a.txt





