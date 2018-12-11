## scp ssh 指定密钥
```
ssh -p 60022 root@39.108.37.168 -i gpu-ai.pem #ssh登录
scp -P 60022 -i gpu-ai.pem file_on_55 root@39.108.37.168:/data/yanzi #scp 拷贝文件
```
若运行时出错，出现很多@@@@，降低密钥文件的权限
```
chmod 600 gpu-ai.pem
```
