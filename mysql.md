# mysql
## 登录mysql
```
mysql -uwgb -hXXX.XXX.XXX.XXX -p
```
-u指定用户名，-h指定ip地址，-p表示需要输入密码

## mysql数据迁移
```
mysqldump -uroot -pdbpasswd -d dbname >db.sql; #导出dbname的所有表结构
mysqldump -uroot -pdbpasswd dbname >db.sql; #导出dbname的所有表结构和表数据，不加-d

mysqldump -uroot -pdbpasswd dbname test>db-test.sql; #导出特定表，在dbname后面添加table名字

create database dbname_new; #在新的位置建库
mysqldump -uroot -pdbpasswd dbname_new < db.sql;
```


## 导入csv文件
当csv文件字段名和表字段名不一致时,导入表格出现乱码
```
load data infile '/var/lib/mysql-files/test.csv' into table products character set utf8 fields terminated by ',' optionally enclosed by '"' escaped by '"' lines terminated by '\r\n' (general_agency, images_urls, images_urls_local, name, net_weight, odor_type, package, price, sku, slogan, url, volume);
```
[参考](https://blog.csdn.net/quiet_girl/article/details/71436108)

若csv的某几行不需要导入mysql，将末尾的对应字段用@dumpy代替
[参考](https://blog.csdn.net/etimechen/article/details/50521636)

## 清空，查询，更改字段长度操作
清空表`truncate products;`

删除表中元素`delete * from products;`

查询表结构`desc products;`

查找某个字段包含特定字符的记录`select sku, name from products where name LIKE "%贵州茅台%";`

更改字段长度`ALTER TABLE products MODIFY images_urls VARCHAR(1000);`

[参考](https://stackoverflow.com/questions/1279568/how-can-i-modify-the-size-of-column-in-a-mysql-table)

## 报错--secure-file-priv
The MySQL server is running with the --secure-file-priv option so it cannot execute this statement

查询secure-file-priv文件夹位置：`SELECT @@GLOBAL.secure_file_priv;`将csv文件拷贝到该文件夹下

[参考1](https://www.jianshu.com/p/0f10c218d6fc)
[参考2](https://stackoverflow.com/questions/32737478/how-should-i-tackle-secure-file-priv-in-mysql)

## 在python中使用mysql
先引入pymysql包
```
import pymysql as pq
conn = pq.connect(host='localhost', user='',passwd='', db='ir', charset='utf8')
cur = conn.cursor()

sql = "insert into products(sku, url, images_urls, images_urls_local, slogan, name, odor_type, volume, net_weight, general_agency, package, price) VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s)"

cur.execute(sql, ('a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l'))
conn.commit()
```
