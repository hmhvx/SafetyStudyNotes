### linux基础命令2

#### 1.查看文件

命令之：cat
命令使用格式：cat 文件名... #文件可同时指定多个。
作用：查看文件内容，连接并显示，如果指定多个目标，按指的顺序显示。例子：查看 passwd 文件内容。
[root@base ~]# cat /etc/passwd
注意：cat 命令一次显示整个文件的内容，但是在输入显示时，并非一次显示整个文件的内容，这个是 cat 命令程序的一个缺陷，查看大文件时，会造成一定的困难，尤其是在本地终端时，但是上下翻屏是有限的，一些大的文件，是未必能翻到首部，因为在查看文件时，是把文件加载到内存中再输出，而内存区域是有限的，比如一个文件查看总共 50 屏，内存只缓存了 20 屏，那么上下翻也就最多能翻到最后缓存的 20 屏，cat 命令程序只是把文件内容加载到内存中并输出，输出完成后 cat 命令程序则自动退出。

[root@study opt]# cat passwd1 |grep root #grep命令：搜索关键字



#### 2.命令之：more（一般用于大文件）

作用：以分页形式显示文件内容。命令使用格式：more 文件名
[root@base ~]# more /etc/httpd/conf/httpd.conf
说明： more 命令程序在查看文件，并且显示当前查看文本内容的百分比 ，实际上 more 不支持向前翻，只要没翻到未部，可以向前翻一屏，但不能向前翻一行。
翻屏操作：
向后翻一屏：SPACE （空格键）
向前翻一屏：b 键 （如果翻到最后一屏时，则不能向前翻，此时会自动退出） 向后翻一行：ENTER （回车键）
退出查看：q 键

#### 3.命令之：less（一般用于大文件）

作用：和 more 功能一样。命令使用格式：less 文件名
[root@base ~]# less /var/log/message
说明：实际上 man 手册是用 less 打开某个命令的使用手册的，命令都有一个使用手册，在使用 man 时，man 会到命令的使用手册所在位置用 less 打开命令的使用手册，所以 less 和使用 man 时基本相同操作。
翻屏操作：
向后翻一屏：SPACE （空格键） 或：pageup 按键。向前翻一屏：b 键 或：page down 按键。
向后翻一行：ENTER （回车键）。向前翻一行：k 键。

搜索/查找操作：2 种方法，默认不区分在小写
/关键字 #可以搜索关键字：在所处的位置向后查找。
？关键字 #可以搜索关键字：在所处的位置向前查找。按 n 键 跳到下一个关键字。
按 b 键 跳到上一个关键字。退出按 q 键。

#### 4.Linux 中 more 与 less 的区别：

more：不支持后退，但几乎不需要加参数，空格键是向后翻屏，Enter 键是向后翻一行，在不需要后退的情况下比较方便。
less：支持前后翻滚，既可以向前翻屏 SPACE （空格键）或（pageup 按键），也可以向后翻屏 b 键或（pagedown 按键），空格键是向前翻屏，Enter 键是向后翻一行。

#### 5.命令之：head

作用: 用于显示文件的开头的内容。在默认情况下，head 命令显示文件的头 10 行内容。

命令使用格式：head [选项] 文件名
选项：
-n 显示从文件头开始的行数，例：-n 3 查看文件中的开始 3 行内容，-n 的写法支持 -n3（选项不选项参数不需要空格），还可以直接 -3 指定查看文件的开始 3 行。
[root@base opt]# head /etc/passwd root:x:0:0:root:/root:/bin/bash bin:x:1:1:bin:/bin:/sbin/nologin daemon:x:2:2:daemon:/sbin:/sbin/nologin adm:x:3:4:adm:/var/adm:/sbin/nologin lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin sync:x:5:0:sync:/sbin:/bin/sync shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown halt:x:7:0:halt:/sbin:/sbin/halt mail:x:8:12:mail:/var/spool/mail:/sbin/nologin operator:x:11:0:operator:/root:/sbin/nologin

[root@base opt]# head -n 3 /etc/passwd #显示前 3 行root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin daemon:x:2:2:daemon:/sbin:/sbin/nologin

[root@base ~]# head -n3 /etc/passwd #-n3 （选项不选项参数不需要空格）的写法。
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin daemon:x:2:2:daemon:/sbin:/sbin/nologin

[root@base ~]# head -3 /etc/passwd #直接 -3 指定查看文件前 3 行。
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin

#### 6.命令之：tail

作用: 用于显示文件中的尾部内容。默认在屏幕上显示指定文件的末尾 10 行。

语法：tail [选项] 文件名
参数：
（1） -n 显示文件尾部多少行的内容（n 为数字）。
（2） -f 动态查看，在查看某个文件时默认查看后 10 行，显示后并不退查看，等待显示后续追加至此文件的新增内容，并立即显示出来，一般用于查看日志文件。
[root@base ~]# tail -n 3 /var/log/secure #查看最后 3 行记录。
[root@base ~]# tail -f /var/log/secure #动态查看到登录成功的日志。
[root@base ~]# ssh [root@192.168.1.63](http://mailto:root@192.168.1.63) #在另一个终端进程登录 Linux，登录成功后。
[root@base ~]# tail -f /var/log/secure #可以动态查看到登录成功的日志。
Nov 17 00:08:32 base sshd[2924]: Accepted password for root from 192.168.1.63 port 39904 ssh2

 

重定向

```
> 覆盖重定向
>> 追加重定向
只要是执行命令有结果的都可以后接重定向

```





 

#### 命令之df

用于显示目前在 Linux 系统上的文件系统磁盘使用情况统计。
语法：df [选项]

参数：

（1） -h 通过它可以产生可读的格式

\# df -h 

Filesystem   Size Used  Avail Use% Mounted on 

/dev/sda6    29G  4.2G  23G  16%   / 

udev       1.5G 4.0K  1.5G  1%   /dev 

tmpfs      604M 892K  603M  1%   /run 

none       5.0M   0  5.0M  0%   /run/lock 

none       1.5G 156K  1.5G  1%   /run/shm 

 

 

 

#### Linux 下快捷键

在 Linux 下的快捷键都是使用 Ctrl+下面的单词， ^表示 Ctrl。

1. ^C
   终止前台运行的程序 , 如：ping g.cn 后，想停止按下 Ctrl+C。

2. ^L
   清屏 clear 命令功能一样。
3. tab 键
   补全命令使用 tab 键，Tab 只能补全命令和文件。Tab补全出来的结果带“/”的是目录，不带“/”的是文件

4. ^u

   把命令行光标前的内容删掉

   5.^k

​	   把命令行光标后的内容删掉





#### 开关机命令及 7 个启动级别

常用的几个关机，重启命令。shutdown、init、reboot、poweroff
关机命令之 shutdown、init 0 作用：关机，重启，定时关机
命令使用格式：shutdown [选项] 参数：
（1） -r #新启动计算机。
（2） -h #关机。
（3） -h 时间 #定时关机。

例 3.2:
[root@base ~]# shutdown -h +10 #10 分钟之后关机。
[root@base ~]# shutdown -h 23:30 #指定具体的时间点进行关机。
[root@base ~]# shutdown -h now #立即关机。
[root@base ~]#shutdown -r 22:22 #22:22 以后重启。





#### Vi 编辑器

vim 主要模式介绍。
vim 是文本编辑器，只编辑纯 ASCII 码的文档，没有任何多余的修饰符。：

vim 是 vi 的增加版，最明显的区别就是 vim 可以语法加亮，它完全兼容 vi。vim 编辑器模式，如图 5-1 所示。

 

 

![img](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202206291024948.jpg) 

图 5-1 vim 模式之间切换图
模式介绍：
（1） 首次进入文件 处于“命令模式”。
（2） 出现 “Insert” 处于“编辑模式”。
（3） 输入 : 处于“命令行模式”。

从编辑模式到命令行模式之间的切换：
在“命令模式”进入“编辑模式”可按下键盘上字母键 ---》 a、i、o 、A、I、O 键。
在“编辑模式”下转换为“命令模式”下 ---》按下 Esc 键。
在“命令模式”下转换为“命令行模式”下 ---》输入 : （直接输入：冒号）。

\1. 命令模式---》编辑模式： 
（1） I：当前字符之前插入 （光标前）
（2） I：行首插入 （行首）
（3） a：当前字符之后插入 （光标后）
（4） A：行尾插入（行尾）
（5） o：下一行插入 （另起一行）
（6） O：上一行插入（上一行插入）

\2. 在命令模式下做的操作：
（1） h：光标向左移动
（2） j ：光标向下移动
（3） k：光标向上移动
（4） l ：光标向右移动
（5） 0 和 home 键：切换到行首， $和 end 键表示切换到行尾
（6） gg：快速定位到文档的首行 , G 定位到未行
（7） 3gg：或者 3G 快速定位到第 3 行
（8） u：撤销一步，每按一次就撤销一次
（9） r ：替换
（10） /string(字符串)：找到或定位你要找的单词或内容，如果相符内容比较多，我们可以通过 N、n 来进行向上向下查找，并且 vi 会对查找到的内容进行高亮显示，取消用 :noh
（11） /^d：^符号表示以什么开头，查找以字母 d 开头的内容
（12） /t$：$符号表示以什么结尾，查找以字母 t 结尾的内容
（13） vim + a.txt：打开文件后，光标会自动位于文件的最后一行

\3. 对文本进行编辑：
对于文本的内容编辑主要有：删除、复制、粘贴、撤销、等等常见操作。
（1） y：复制（以字符为单位）表示对单个字符进行复制，如果要复制整行，用 yy（以行为单位）
（2） Nyy：复制 N 行，比如： 2yy ，表示在光标所在的行往下复制 2 行
（3） dd：删除，以行为单位，删除当前光标所在行
（4） Ndd：删除 N 行，比如：2dd，表示光标所在行往下删除 2 行
（5） p：在光标所在位置往下插入粘贴的内容
（6） dd：剪切
（7） x：向后删除一个字符，等同于 Delete
（8） X：向前删除一个字符
（9） D：从光标处删除到行尾
（10） u：撤销操作
（11） ctrl+r：还原撤销过的操作，将做过的撤销操作再还原回去，也就是说撤销前是什么样，再还原成什么样
（12） r：替换，或者说用来修改一个字符



总结：vim 如何进入其它模式。
（1） a、A、o、O、i、I 都是可以进行插入，编辑模式
（2） ： 输入冒号进入命令行模式
（3） v 进入可视模式
（4） ctrl+v 进入可视块模式
（5） R 擦除、改写，进入替换模式
（6） 进入以上模式后，想要退出 ，按 esc



可视化模式
进入 v 模式 移动光标选择区域，编程的时候需要进行多行注释，ctrl+v 进入列编辑模式。
（1） 向下或向上移动光标，把需要注释、编辑的行的开头选中起来
（2） 然后按大写的字母 I
（3） 再插入注释符或者你需要插入的符号,比如"#"
（4） 再按 Esc,就会全部注释或添加了
（5） 删除：再按 ctrl+v 进入列编辑模式；向下或向上移动光标 ；选中注释部分,然后按 d, 就会删除选中的注释符号

命令行模式操作
（1） :w 保 存 save
（2） :w! 强制保存
（3） :q 没有进行任何修改，退出 quit
（4） :q! 修改了，不保存，强制退出
（5） :wq 保存并退出
（6） :wq! 强制保存并退出
（7） :x 保存退出

例 5.2：强制保存并退出操作示例。
[root@base ~]# ll /etc/shadow #使用 ll 命令可查看得到，shadow 文件权限为 000。
----------. 1 root root 1179 9 月 19 12:57 /etc/shadow
[root@base ~]# vim /etc/shadow #使用 root 用户对 shadow 文件进行编辑，在保存退出时， 由于权限不够无法正常保存退出，但 root 用户可以使用:wq! 强制保存并退出。

自定义 vim 使用环境。
\1. 临时设置：
（1） :set nu 设置行号
（2） :set nonu 取消设置行号
（3） :noh 取消高亮显示命令，默认高亮



