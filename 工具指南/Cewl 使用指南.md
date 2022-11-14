[参考](https://blog.csdn.net/systemino/article/details/90114610)

**常规参数选项：**

-h, –help：显示帮助。
-k, –keep：保存下载文件。
-d <x>, –depth <x>：爬行深度，默认2。
-m, –min_world_length：最小长度，默认最小长度为3。
-o, –offsite：允许爬虫访问其他站点。
-w, –write：将输出结果写入到文件。
-u, –ua <agent>：设置user agent。
-n, –no-words：不输出字典。
–with-numbers：允许单词中存在数字，跟字母一样。
-a, –meta：包括元数据。
–meta_file file：输出元数据文件。
-e, –email：包括email地址。
–email_file <file>：输入邮件地址文件。
–meta-temp-dir <dir>：exiftool解析文件时使用的临时目录，默认是/temp
-c, –count：显示发现的每个单词的数量。
-v, –verbose：verbose。
–debug:提取调试信息。

**认证 **

–auth_type：Digest或者basic认证。
–auth_user：用户名认证。
–auth_pass：密码认证。

**代理**

–proxy_host：代理主机。
–proxy_port：代理端口，默认8080。
–proxy_username：用户名代理。
–proxy_password：密码代理。

`cewl <url> -w password.txt`