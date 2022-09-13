```



 wget http://repo.mysql.com/yum/mysql-5.7-community/el/7/x86_64/mysql57-community-release-el7-10.noarch.rpm
rpm -ivh mysql57-community-release-el7-10.noarch.rpm 
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
yum -y install mysql-community-server

create database XSK character set utf8;

use XSK;
CREATE TABLE Customers(Cid char(10) not null,Cname char(12) not null,City char(8) not null);
CREATE TABLE Orders(Ordno char(12) not null,Cid char(10) not null,Pid char(12) not null,Qty char(12),Month char(6) not null);
CREATE TABLE Products(Pid char(12) not null,Pname char(12) not null,Quantity int(12),Price float not null);
insert into Customers value("081101","王向荣","北 京 ");
insert into Customers value("081102","李丽","珠海 ");
insert into Customers value("081103","刘明","上海 ");
insert into Customers value("081104","雷敏","南京 ");
insert into Customers value("081105","张石兵","杭州 ");
insert into Customers value("081106","雷林琳","柳州 ");
select * from Customers;
insert into Orders value("031101","081101","021101","250","1");
insert into Orders value("031102","081102","021101","300","1");
insert into Orders value("031103","081103","021103","120","1");
insert into Orders value("031104","081104","021103","150","1");
insert into Orders value("031105","081105","021104","180","1");
insert into Orders value("031106","081106","021105","200","1");
insert into Orders value("031107","081101","021102","300","1");
insert into Orders value("031108","081102","021103","400","1");
insert into Orders value("031109","081103","021104","320","1");
insert into Orders value("0311010","081102","021104","230","1");
insert into Orders value("0311011","081103","021102","325","1");
insert into Orders value("0311012","081104","021103","235","1");
insert into Orders value("0311013","081105","021104","345","1");
insert into Orders value("0311014","081106","021105","432","1");
select * from Orders;
insert into Products value("021101","空调","3500","200");
insert into Products value("021102","冰箱","2500","100");
insert into Products value("021103","彩电","4000","50");
insert into Products value("021104","洗衣机","1500","150");
insert into Products value("021105","微波炉","2800","50");
select * from Products;
mysqldump -uroot -p -B XSK >/usr/local/src/XSK.sql
mysql -uroot -p < /usr/local/src/XSK.sql
```