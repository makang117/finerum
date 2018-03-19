# 获取go安装包
-   获取go安装包

```
  wget https://dl.google.com/go/go1.10.linux-amd64.tar.gz
```

-   解压缩到/usr/local目录下

```
sudo tar -C /usr/local -xzf go1.10.linux-amd64.tar.gz
```

-   配置环境变量，修改/etc/profile文件，在文件中加入以下语句

```
export GOPATH=/home/makang/project/go
export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
```

-   重新加载profile文件

```
source /etc/profile
```
