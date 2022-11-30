## 编码格式

># -*- coding: utf-8 -*-     
># coding = utf-8

## 变量名

-   第一个字符必须是字母表中字母或下划线 _ 。
-   标识符的其他的部分由字母、数字和下划线组成。
-   标识符对大小写敏感。

在 Python 3 中，可以用中文作为变量名，非 ASCII 标识符也是允许的了。

## 字符串

可以使用 ‘ / “ 

>'This is a string'
>"This is also a string"

### 修改字符串大小写

>name = "abc"
>print(name.upper()) # 字符串大写
>print(name.lower()) # 字符串小写

### 拼接字符串

>first_name = "abc"
>last_name = "def"
>full_name = first_name + last_name

### 制表符和换行符

>\\t # 制表符
>\\n # 换行符

### 删除空白

>rstrip() 

## 数字

可直接对整数加 减 乘 除运算

> 2 + 3
> 3 - 2
> 2 * 3
> 3 / 2

使用两个乘号表示乘方

>3 ** 2
>9