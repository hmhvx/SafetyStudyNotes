[参考1](https://cloud.tencent.com/developer/article/1850703)
[参考2](https://cloud.tencent.com/developer/article/1850705?from=article.detail.1850703)

[参考3](https://blog.csdn.net/weixin_44604541/article/details/116743835)
## 黄金票据

黄金票据就是伪造krbtgt用户的TGT票据，krbtgt用户是域控中用来管理发放票据的用户，拥有了该用户的权限，就可以伪造系统中的任意用户

利用前提：

-   拿到域控(没错就是拿到域控QAQ),适合做权限维持
-   有krbtgt用户的hash值(aeshash ntlmhash等都可以,后面指定一下算法就行了)

条件要求：

-   域名
-   域的SID 值
-   域的KRBTGT账户NTLM密码哈希
-   伪造用户名

## 白银票据

黄金票据是伪造TGT（门票发放票），而白银票据则是伪造ST（门票），这样的好处是门票不会经过KDC，从而更加隐蔽，但是伪造的门票只对部分服务起作用,如cifs（文件共享服务），mssql，winrm（windows远程管理），DNS等等

利用前提：

-   拿到目标机器hash(是目标机,不一定是域控)

条件要求：

-   域名
-   域sid
-   目标服务器FQDN
-   可利用的服务
-   服务账号的NTML HASH
-   需要伪造的用户名