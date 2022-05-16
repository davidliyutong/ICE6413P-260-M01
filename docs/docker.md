# 搭建集群的准备工作

## 安装Docker

关于如何安装Docker，可以参考[/ICE6405P-260-M01/scripts/ubuntu/20.04/setup-docker.sh](https://github.com/davidliyutong/ICE6405P-260-M01/blob/main/scripts/ubuntu/20.04/setup-docker.sh)脚本，在Ubuntu/Debian等使用APT作为包管理器的发行版中可以正常运行

!!! note
    该脚本目前只支持使用APT作为包管理的发行版，例如Debian/Ubuntu

如果手动安装，则最佳实践是

```shell title="run.sh"
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
# Add current user to docker group
$ sudo usermod -aG docker $USER
$ newgrp docker
$ sudo systemctl restart docker
```

!!! note
    这些命令首先调用docker官方脚本安装docker，然后授予当前用户`$USER`docker的使用权限，更新权限后重启docker

## 安装docker-compose

在安装了Docker的基础上，只需要使用`apt`工具安装`docker-compose`

```shell
[speit@host] $ sudo apt-get install docker-compose
```
