
## PIL读取图片，更给尺寸
```
from PIL import Image
test = Image.open('4.jpg')
gray_image = test.convert('L') # 'L'表示灰度图像，'1'表示二值图像，
new_image = test.resize((28,28))
```
[图形模式详情](https://blog.csdn.net/icamera0/article/details/50843172)

`resize`是PIL是自带的更改尺寸函数，但会造成图片的变形
```
def letterbox_image(image, size):
    '''resize image with unchanged aspect ratio using padding'''
    iw, ih = image.size
    w, h = size
    scale = min(w/iw, h/ih)
    nw = int(iw*scale)
    nh = int(ih*scale)

    image = image.resize((nw,nh), Image.BICUBIC)
    #Image.new生成新的图片，彩色图片mode='RGB',灰色图片mode='L'
    new_image = Image.new('L', size, 128)
    new_image.paste(image, ((w-nw)//2, (h-nh)//2))
    return new_image
```

## numpy读取图片，变更shape
```
from PIL import Image
import numpy as np
test = Image.open('test.jpg')
atrr = np.array(test)
#atrr = atrr/255 #归一化

#变更shape有多种方式，如下
atrr_3d = np.expand_dims(atrr, axis=0)
#atrr_3d = np.reshape(atrr,(-1,28,28)) #(28,28)为原本图片尺寸
#atrr_3d = atrr[np.newaxis, ...]

np.save('test.npy',atrr_3d)
```

## 提取路径中的文件名
```
filepath = ‘a/b/c’
filename = os.path.basename(filepath)
print(filename)
head, tail = os.path.split(filepath)
print(tail)
```
[参考](https://stackoverflow.com/questions/8384737/extract-file-name-from-path-no-matter-what-the-os-path-format)

## 添加python path
Windows下，在terminal下输入
```
set PYTHONPATH=E:\PycharmProjects\models
```
Linux下，在shell下输入
```
export PYTHONPATH=$PYTHONPATH:/path/to/models
```

## 通过源码安装python3
下载python3源码`wget https://www.python.org/ftp/python/3.6.7/Python-3.6.7rc2.tgz`

创建目标文件夹 `mkdir /usr/local/python3`

将源码解压到目标文件夹 `tar -zxvf Python-3.6.7rc2.tgz -C /usr/local/python3`

进入目标文件夹 `cd Python-3.7.0`

编译
```
./configure  --prefix=/usr/local/python3
make
make install
```
## virtualenv 安装
安装virtualenv  `pip install virtualenv`

创建解释器为python的虚拟环境 `virtualenv -p /path/to/python3 /path/to/virtualEnvFolder`

更改/path/to/virtualEnvFolder为普通用户权限
```
chown admin -R virtualEnvFolder
chgrp admin -R virtualEnvFolder
```
进入虚拟环境`Source virtualEnvFolder/bin/activate`
退出 `deactivate`

## pip安装
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py
```
[参考](https://pip.pypa.io/en/stable/installing/)

## 使用pip命令安装程序提示找不到ssl(pip安装软件前必装)
openssl只包含了可执行部分，openssl-devel才包含了头文件、头文件参考、某些库文件等跟开发相关的东西。所以只安装openssl是找不到相应的头文件的
```
yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel
```
[参考](https://www.cnblogs.com/minglee/p/9232673.html)

通过编译python安装包安装python后，通过pip安装软件，提示缺少ssl时，说明python3在编译时没有添加ssl，重新编译`./configure --with-ssl`
[参考](https://blog.csdn.net/jeryjeryjery/article/details/77880227)

### 通过pip安装scrapy，提示找不到合适的twisted版本，可通过源码编译安装
```
wget https://twistedmatrix.com/Releases/Twisted/17.1/Twisted-17.1.0.tar.bz2
tar -jxvf Twisted-17.1.0.tar.bz2
cd Twisted-17.1.0
python setup.py install
```
### 在tar解压缩tar.bz2文件时，提示找不到bz2子命令，安装bzip2和bzip2-libs
```
yum install bzip2
yum install bizp2-libs
```
### 若提示缺少zlib，安装zlib包`yum -y install zlib*`

安装zlib可能会发生64位和32位的冲突，根据系统的版本选择对应的版本

`yum -y install zlib* --setopt=protected_multilib=false `

[参考](https://www.cnblogs.com/kimyeee/p/7250560.html)
