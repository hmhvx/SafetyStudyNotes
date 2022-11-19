
|参数名|参数含义|使用示例|
|:-:|:-:|:-:|
|-f|从指定文件中读取主机名或地址|arp-scan -f ip.txt|
|-l|从网络接口配置生成地址|arp-scan -l|
|-i|各扫描之间的时间差|arp-scan -l -i 1000|
|-r|每个主机扫描次数|arp-scan -l -r 5|
|-V|显示程序版本并退出|arp-scan -l -V|
|-t|设置主机超时时间|arp-scan -t 1000 192.168.75.0/24|
|-L|使用网络接口|arp-scan -L eth0|
|-g|不显示重复的数据|arp-scan -l -g|
|-D|显示数据包往返时间|arp-scan -l -D|