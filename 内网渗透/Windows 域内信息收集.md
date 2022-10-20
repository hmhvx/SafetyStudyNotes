
## 1、判断是否存在域

### ipconfig

查看网关 IP 地址、DNS 的 IP 地址、域名、本机是否和 DNS 服务器处于同一网段。

```text
ipconfig /all
```

>C:\Users\daniel10> ipconfig /all
Windows IP 配置
   主 DNS 后缀 . . . . . . . . . .  : teamssix.com
   DNS 后缀搜索列表  . . . . . . . . : teamssix.com
以太网适配器 Ethernet0:
   IPv4 地址 . . . . . . . . . . .. : 192.168.7.110
   子网掩码  . . . . . . . . . . . . : 255.255.255.0
   默认网关. . . . . . . . . . . . . : 192.168.7.1
   DNS 服务器  . . . . . . . . . . . : 192.168.7.7

接着使用 nslookup 解析域名的 IP 地址，查看是否与 DNS 服务器为同一 IP

```text
nslookup teamssix.com
```

>C:\Users\daniel10> nslookup teamssix.com
>服务器:  UnKnown
>Address:  192.168.7.7
>名称:    teamssix.com
>Address:  192.168.7.7

### 系统详细信息

```text
systeminfo
```

>C:\Users\daniel10> systeminfo | findstr 域:
>域: teamssix.com

### 当前登录域与域用户

```text
net config workstation
```

>C:\Users\daniel10> net config workstation | findstr 域
>工作站域                    TEAMSSIX
>工作站域 DNS 名称            teamssix.com
>登录域                      TEAMSSIX

### 判断主域

```text
net time /domain
```

>C:\Users\daniel10> net time /domain
>\\dc.teamssix.com 的当前时间是 2021/2/13 20:49:56
>命令成功完成。

## 2、收集域内基础信息

### 查看域

```text
net view /domain
```

>C:\Users\daniel10> net view /domain
>Domain
>TEAMSSIX
>命令成功完成。


### 查看域内计算机

```text
net view /domain:domain_name
```

>C:\Users\daniel10> net view /domain:teamssix
>服务器名称            注解
>\\DANIEL10
>\\DANIEL7
>\\DC
>命令成功完成。

### 查看域内用户组列表

```text
net group /domain
```

>C:\Users\daniel10> net group /domain
>这项请求将在域 teamssix.com 的域控制器处理。
>\\dc.teamssix.com 的组帐户
>*Admins
>*Domain Admins
>*Domain Computers
>*Domain Users
>*Enterprise Admins
>命令成功完成。

### 查看域用户组信息

```text
net group "Enterprise Admins" /domain
```

>C:\Users\daniel10> net group "Enterprise Admins" /domain
>这项请求将在域 teamssix.com 的域控制器处理。
>组名     Enterprise Admins
>注释     指定的公司系統管理員
>成员
>Administrator
>命令成功完成。

### 查看域密码策略信息

```text
net accounts /domain
```

>C:\Users\daniel10> net accounts /domain
>这项请求将在域 teamssix.com 的域控制器处理。
>强制用户在时间到期之后多久必须注销?:     从不
>密码最短使用期限(天):                  1
>密码最长使用期限(天):                  42
>密码长度最小值:                        7
>保持的密码历史记录长度:                 24
>锁定阈值:                            从不
>锁定持续时间(分):                      30
>锁定观测窗口(分):                      30
>计算机角色:                           PRIMARY
>命令成功完成。

### 查看域信任信息

```text
nltest /domain_trusts
```

>C:\Users\daniel10> nltest /domain_trusts
>域信任的列表:
>	0: TEAMSSIX teamssix.com (NT 5) (Forest Tree Root) (Primary Domain) (Native)
>此命令成功完成

## 3、收集域用户和管理员信息

### 查询域用户列表

```text
net user /domain
C:\Users\daniel10> net user /domain
这项请求将在域 teamssix.com 的域控制器处理。
\\dc.teamssix.com 的用户帐户
-------------------------------------------------------------------------------
admin                    Administrator                    daniel10
```

### 查询域用户详细信息

```text
wmic useraccount get /all
C:\Users\daniel10> wmic useraccount get /all
AccountType  Caption                        Description                                                     Disabled  Domain    FullName                               InstallDate  LocalAccount  Lockout  Name                  PasswordChangeable  PasswordExpires  PasswordRequired  SID                                            SIDType  Status
512          DANIEL10\Administrator         管理计算机(域)的内置帐户                                        TRUE      DANIEL10                                                      TRUE          FALSE    Administrator         TRUE                FALSE            TRUE              S-1-5-21-1097120846-822447287-3576165687-500   1        Degraded
512          DANIEL10\DefaultAccount        系统管理的用户帐户。                                            TRUE      DANIEL10                                                      TRUE          FALSE    DefaultAccount        TRUE                FALSE            FALSE             S-1-5-21-1097120846-822447287-3576165687-503   1        Degraded
```

### 查询存在的用户

```text
dsquery user
C:\Users\daniel10> dsquery user
"CN=Administrator,CN=Users,DC=teamssix,DC=com"
"CN=Guest,CN=Users,DC=teamssix,DC=com"
```

常用的 dsquery 命令

```text
dsquery computer - 查找目录中的计算机
dsquery contact - 查找目录中的联系人
dsquery subnet - 查找目录中的子网
dsquery group - 查找目录中的组
dsquery ou - 查找目录中的组织单位
dsquery site - 查找目录中的站点
dsquery server - 查找目录中的域控制器
dsquery user - 查找目录中的用户
dsquery quota - 查找目录中的配额
dsquery partition - 查找目录中的分区
dsquery * - 用通用的 LDAP 查询查找目录中的任何对象
```

## 4、查找域控制器

### 查看域控器机器名

```text
nltest /DCLIST:teamssix
C:\Users\daniel10> nltest /DCLIST:teamssix

获得域“teamssix”中 DC 的列表(从“\\DC”中)。
    dc.teamssix.com [PDC]  [DS] 站点: Default-First-Site-Name
此命令成功完成
```

### 查看域控器主机名

```text
nslookup -type=SRV _ldap._tcp
C:\Users\daniel10> nslookup -type=SRV _ldap._tcp

_ldap._tcp.teamssix.com SRV service location:
          priority       = 0
          weight         = 100
          port           = 389
          svr hostname   = dc.teamssix.com
dc.teamssix.com internet address = 192.168.7.7
netdom query pdc
C:\Users\daniel10> netdom query pdc
域的主域控制器:
DC
命令成功完成。
```

### 查看域控器组

```text
net group "domain controllers" /domain
C:\Users\daniel10> net group "domain controllers" /domain
这项请求将在域 teamssix.com 的域控制器处理。
组名     Domain Controllers
注释     在網域所有的網域控制站
成员
-------------------------------------------------------------------------------
DC$
命令成功完成。
```