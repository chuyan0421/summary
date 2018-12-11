## tensorflow CUDA cudnn 版本对应关系
[参考](https://blog.csdn.net/yuejisuo1948/article/details/81043962)

## 安装anaconda
安装完后，启动指令为 `anaconda-navigator`

激活环境 `source activate /home/yanzi/ENV_conda/pytorch （路径为实例，根据自己实际的放置）`

退出环境 `source deactivate`

安装pytorch指令 `conda install pytorch torchvision cuda90 -c pytorch`

出现不能下载安装pytorch安装包的问题

将安装包pytorch-0.4.0-py36_cuda9.0.176_cudnn7.1.2_1.tar.bz2下载好后

执行 `conda install /path/to/package`

[参考](https://docs.anaconda.com/anaconda/install/linux)

## 安装nvidia驱动
步骤如下：

1禁用nouveau驱动和相关的驱动包

打开配置文件`sudo gedit /etc/modprobe.d/blacklist.conf`

在文件的最后一行加入下面的命令
```
blacklist rivafb
blacklist vga16fb
blacklist nouveau
blacklist nvidiafb
blacklist rivatv
```
执行`sudo update-initramfs -u`使得blacklist.conf生效

重启

2卸载原本nvidia驱动
`sudo apt-get --purge remove nvidia-*`

3安装新的nvidia驱动

安装需要的依赖
```
sudo apt update
sudo apt install dkms build-essential linux-headers-generic
```
进入工作台界面`Ctrl+Alt+F1`

关闭图形服务 `sudo service lightdm stop`

更改驱动权限`sudo chmod a+x NVIDIA-Linux-x86_64-x.x.run`

执行安装`sudo ./NVIDIA-Linux-x86_64-x.x.run -no-x-check -no-nouveau-check -no-opengl-files`

重启

注1：在安装驱动会遇到：

The distribution-provided pre-install script failed! Are you sure you want to continue? 不用管它，继续安装。

Would you like to register the kernel module souces with DKMS? This will allow DKMS to automatically build a new module, if you install a different kernel later? 选择No继续

遇到是否安装 32位驱动 选择安装

Would you like to run the nvidia-xconfigutility to automatically update your x configuration so that the NVIDIA x driver will be used when you restart x? Any pre-existing x confile will be backed up. 这个选择Yes 继续

注2：CUDA9建议安装R384及以上版本的驱动，但是安装R384的驱动后，执行nvidia-smi指令不能显示显卡型号，将驱动更新成R390以上之后问题解决。

注3：安装驱动后可能会出现重复登录的问题，转入到工作台界面，卸掉原本的nvidia驱动和nouveau驱动

## tensorflow built with cuda support
[参考](https://www.tensorflow.org/install/install_linux?hl=zh-cn)

1安装cuda toolkit 9.0
```
sudo dpkg -i cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64.deb
sudo apt-key add /var/cuda-repo-<version>/7fa2af80.pub
sudo apt-get update
sudo apt-get install cuda
```

增加环境变量，在$HOME/.profile文件末尾添加下面两行
```
export PATH=${PATH:+${PATH}:}/usr/local/cuda-9.0/bin
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH:+${LD_LIBRARY_PATH}:}/usr/local/cuda-9.0/lib64
```
查询是否安装好 `nvcc -V`

[参考](https://developer.nvidia.com/cuda-90-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=deblocal)

2.安装nvidia驱动（在本文档中搜索）

3安装cudnn(nvidia账号 chuyan0421@126.com chuyan_0421)

Runtime library安装  `sudo dpkg -i libcudnn7_7.0.3.11-1+cuda9.0_amd64.deb`

Develop library安装  `sudo dpkg -i libcudnn7-dev_7.0.3.11-1+cuda9.0_amd64.deb`

帮助文档安装 `sudo dpkg -i libcudnn7-doc_7.0.3.11-1+cuda9.0_amd64.deb`

[参考](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html#installdriver)

4安装libcupti-dev库，这是NVIDIA CUDA分析工具接口

安装命令`sudo apt-get install cuda-command-line-tools`显示无法定位该软件

执行`sudo apt-cache search cuda-command-line-tools`显示已经安装 cuda-command-line-tools-9-0

将其路径添加到LD_LIBRARY_PATH环境变量上，在$HOME/.profile文件末尾添加：
```
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH:+${LD_LIBRARY_PATH}:}/usr/local/extras/CUPTI/lib64
```

注1：在安装cuda后出现屏幕分辨率不能更改的问题
```
sudo mv /etc/X11/xorg.conf /etc/X11/xorg.conf.backup
sudo touch /etc/X11/xorg.conf
sudo reboot
```
