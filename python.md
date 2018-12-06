
## PIL读取图片，更给尺寸
```
from PIL import Image
test = Image.open('4.jpg')
new_image = test.resize((28,28))
```
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
head, tail = os.path.split(filepaht)
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
[参考](https://www.cnblogs.com/kimyeee/p/7250560.html)

## virtualenv 安装
安装virtualenv  `Pip install virtualenv`

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

## 使用pip命令安装程序提示找不到ssl
openssl只包含了可执行部分，openssl-devel才包含了头文件、头文件参考、某些库文件等跟开发相关的东西。所以只安装openssl是找不到相应的头文件的
```
yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel
```

[参考](https://www.cnblogs.com/minglee/p/9232673.html)
