# IDE问题
## 安装sbt
（或者安装intellij idea下安装scala插件）
在官网下载源文件后，执行`sudo tar -zxvf sbt-1.2.1.tgz -C /opt/scala/`
配置sbt环境变量，打开`sudo gedit ~/.bashrc`，在末尾增加:
```
export SPARK_HOME=/opt/spark/spark-2.3.1-bin-hadoop2.7
export PATH=${SPARK_HOME}/bin:$PATH
```
在/opt/scala/sbt文件夹下建立脚本文件sbt，文件内容为:
```
SBT_OPTS="-Xms512M -Xmx1536M -Xss1M -XX:+CMSClassUnloadingEnabled -XX:MaxPermSize=256M"
java $SBT_OPTS -jar /opt/scala/sbt/bin/sbt-launch.jar "$@"
```
修改脚本文件sbt的权限，`sudo chmod 774 sbt`
重启或者`source ~/.bashrc`，查看sbt sbtVersion

可参考https://blog.csdn.net/red_stone1/article/details/71330101

## sbt设置
在intellig IDEA运行scala程序时，要对运行进行配置

Run > Edit configurations；

点击 + 按键，选择SBT TASK；

命名为 “Run the program”；

在Tasks中，输入~run ；

确认，在Run菜单下，点击Run ‘Run the program’

[参考](https://docs.scala-lang.org/getting-started-intellij-track/building-a-scala-project-with-intellij-and-sbt.html)

## 在windows下解压缩文件时，无法创建符号链接
可能需要以管理员身份运行winrar
cmd进入需要解压文件的目录，运行start winrar x -y xxxxxx.tar.gz

## 运行eclipse demo
安装scala，在eclipse里面点击Help>Install New Software，点击Add按键

Name为scala，url为http://download.scala-ide.org/sdk/lithium/e47/scala212/stable/site

Maven build时会报错No goals have been specified for this build

在pom.xml文件的build节点后面添加<defaultGoal>compile</defaultGoal>

## 在sublime里选择多行
Ctrl-G to go to the last line you want to select, by number.

Edit > Mark > Set Mark to set your selection-end marker.

Ctrl-G (or Ctrl-Home) to go to the first line you want to select.

Edit > Mark > Select to Mark

## 在有多个主类的情况下，scala/sbt怎么选择来执行run操作
在built.sbt文件中添加
```
mainClass in (Compile, run) := Some("HbaseTest")
```

[参考](https://alvinalexander.com/scala/sbt-how-specify-main-method-class-to-run-in-project)

## idea: source not found attach
"Project Structure -> Library -> +Attach File or Directories -> Sources”

[参考](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206207249-Attach-sources-does-not-seem-to-work)

## sublime里删除空行

ctrl+H进入替换模式

alt+R或者点击左边第一行一个的正则表达式

替换部分为空

replace all
[参考](http://gohom.win/2015/08/21/sublime-delete-blankline/)

## 虚拟机作为服务器
[参考](https://blog.csdn.net/hk2291976/article/details/52983451)

## 更新win10下host文件
[参考](https://jingyan.baidu.com/article/624e7459b194f134e8ba5a8e.html)
