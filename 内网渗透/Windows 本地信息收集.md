## 手动信息收集


-   查看当前权限

```text
whoami
```

-   本机网络配置信息

```text
ipconfig /all
```

-   操作系统和版本信息（英文版）

```text
systeminfo | findstr /B /C:"OS Name" /C:"OS Version"
```

-   操作系统和版本信息（中文版）

```text
systeminfo | findstr /B /C:"OS 名称" /C:"OS 版本"
```

-   查看系统体系结构

```text
echo %PROCESSOR_ARCHITECTURE%
```

-   查看系统所有环境变量

```text
set
```

-   查看安装的软件及版本和路径等信息

```text
wmic product get name,version
```

-   利用 PowerShell 收集软件版本信息

```text
powershell "Get-WmiObject -class Win32_Product |Select-Object -Property name,version"
```

-   查询本机服务信息

```text
wmic service list brief
```

-   查询进程列表

```text
tasklist /v
```

-   wmic 查看进程信息

```text
wmic process list brief
```

-   查看启动程序信息

```text
wmic startup get command,caption
```

-   查看计划任务

```text
schtasks /query /fo LIST /v
```

-   查看主机开启时间

```text
net statistics workstation
```

-   查询用户列表

```text
net user
```

-   查看指定用户的信息

```text
net user teamssix
```

-   查看本地管理员用户

```text
net localgroup administrators
```

-   查看当前在线用户

```text
query user || qwinsta
```

-   列出或断开本地计算机和连接的客户端的会话

```text
net session
```

-   查看端口列表

```text
netstat –ano
```

-   查看补丁列表

```text
systeminfo
```

-   使用 wmic 查看补丁列表

```text
wmic qfe get Caption,Description,HotFixID,InstalledOn
```

-   查看本机共享

```text
net share
```

-   使用 wmic 查看共享列表

```text
wmic share get name,path,status
```

-   查询路由表及所有可用接口的ARP 缓存表

```text
route print
arp –a
```

-   查询防火墙相关配置  

```text
netsh firewall show config
```

-   关闭防火墙

```text
netsh firewall set opmode disable (Windows Server 2003 系统及之前版本) 
netsh advfirewall set allprofiles state off   (Windows Server 2003 系统及之后版本)
```

-   查看防火墙配置

```text
netsh firewall show config
```

-   修改防火墙配置

```text
(Windows Server 2003 系统及之前版本)   允许指定程序全部连接   
netsh firewall add allowedprogram c:\nc.exe "allow nc" enable

(Windows Server 2003 之后系统版本)   允许指定程序连入   
netsh advfirewall firewall add rule name="pass nc" dir=in action=allow program="C: \nc.exe"

允许指定程序连出  
netsh advfirewall firewall add rule name="Allow nc" dir=out action=allow program="C: \nc.exe"

允许 3389 端口放行   
netsh advfirewall firewall add rule name="Remote Desktop" protocol=TCP dir=in localport=3389 action=allow   ```
```

-   自定义防火墙日志储存位置

```text
netsh advfirewall set currentprofile logging filename "C:\windows\temp\fw.log"
```

-   查看计算机代理配置情况

```text
reg query "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
```

-   查询并开启远程连接服务  
    
-   查看远程连接端口（0xd3d换成10进制即3389）

```text
REG QUERY "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /V PortNumber
```

-   在Windows Server 2003 中开启3389 端口

```text
wmic path win32_terminalservicesetting where (__CLASS !="") call setallowtsconnections 1
```

-   在Windows Server 2008 和Windows Server 2012 中开启3389 端口

```text
wmic /namespace:\root\cimv2\terminalservices path win32_terminalservicesetting where (__CLASS !="") call setallowtsconnections 1
wmic /namespace:\root\cimv2\terminalservices path win32_tsgeneralsetting where (TerminalName='RDP-Tcp') call setuserauthenticationrequired 1
reg add "HKLM\SYSTEM\CURRENT\CONTROLSET\CONTROL\TERMINAL SERVER" /v fSingleSessionPerUser /t REG_DWORD /d 0 /f
```

## 自动信息收集

