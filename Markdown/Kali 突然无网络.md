方法一：
```copy
ifconfig eth0 up
ifconfig -a
```

方法二：

```copy
vim /etc/network/interface
```


添加以下内容
```copy
auto eth0
iface eth0 inet dhcp
```
重启网络
```copy
/etc/init.d/networking restart
```