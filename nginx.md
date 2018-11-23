# nginx
## 安装pcre：
[参考](https://sourceforge.net/projects/pcre/files/pcre/8.42/pcre-8.42.tar.gz)

## 安装nginx：
[下载地址](http://nginx.org/download/nginx-1.9.9.tar.gz)

[参考](http://www.runoob.com/linux/nginx-install-setup.html)

## nginx 重新加载命令./nginx -s reload，出现nginx: [error] invalid PID number "" in "/usr/local/nginx/logs/nginx.pid"

杀掉nginx 进程  
```
killall -9 nginx（若找不到killall指令，yum install psmisc）
```
重新加载配置文件
```
/usr/local/webserver/nginx/sbin/nginx -c /usr/local/webserver/nginx/conf/nginx.conf （具体根据nginx的安装路径而定）
```
重新启动nginx
```
./nginx -s reload
```
