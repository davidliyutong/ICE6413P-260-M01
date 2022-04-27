# Installation

## 环境

## 安装Docker

关于如何安装Docker，可以参考[/ICE6405P-260-M01/scripts/ubuntu/20.04/setup-docker.sh](https://github.com/davidliyutong/ICE6405P-260-M01/blob/main/scripts/ubuntu/20.04/setup-docker.sh)脚本，在Ubuntu/Debian等使用APT作为包管理器的发行版中可以正常运行

```shell title="setup-docker.sh"
#!/bin/bash
# ------- BEGIN CONFIGURATION ------- #
PKG_MANAGER=apt-get
# -------- END CONFIGURATION -------- #

sudo $PKG_MANAGER clean
sudo $PKG_MANAGER autoclean

sudo $PKG_MANAGER update
if [[ $? -ne $(expr 0) ]]; then
echo "apt-get encountered problem, exit."
exit
fi

# Setup docker
if [[ ! $(docker --version | grep version) ]]; then
echo "Installing docker"
curl -fsSL https://get.docker.com -o get-docker.sh
set +e
sudo sh get-docker.sh
rm get-docker.sh
set -e
fi

# Add current user to docker group
# if [[ ! $(docker ps | grep CONTAINER) ]]; then 
# echo "Adding current user to docker group"
sudo usermod -aG docker $USER
newgrp docker
sudo systemctl restart docker

echo "Installation completed"

fi
```

!!! note
    该脚本目前只支持使用APT作为包管理的发行版，例如Debian/Ubuntu

## 安装docker-compose

在安装了Docker的基础上，只需要使用`apt`工具安装`docker-compose`

```shell
[speit@host] $ sudo apt-get install docker-compose
```

## 安装Minikube

如果只是学习API使用，安装Minikube是个很好的方法

### 安装本体

```shell
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

!!! note
    minikubu-linux-amd64 代表该minikube是为amd64平台编译的，只能用于amd64平台

![Minikube installation](img/20220417230357.png)

### 安装kubectl

Minikube 只负责启动k8s实验集群，还需要安装`kubectl`工具管理

```shell
[speit@host] $ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
[speit@host] $ sudo install ./kubectl /usr/local/bin/ # Use install command to replace move
```

!!! note
    这样安装的是最新的stable release，而`curl -L -s https://dl.k8s.io/release/stable.txt`被用来获取版本号

如果想为kubectl添加终端的自动补全，可以执行如下命令

=== "Bash"

    ```shell
    [speit@host] $ echo 'source <(kubectl completion bash)' >>~/.bashrc
    ```

=== "Zsh"

    ```shell
    [speit@host] $ echo 'source <(kubectl completion zsh)' >>~/.zshrc
    ```

![Kubctl installation](img/20220417225246.png)

## 杂项

### cfssl/cfssljson

[cloudflare/cfssl](https://github.com/cloudflare/cfssl/releases)仓库提供了编译好的二进制下载。以安装`v1.6.1`为例：

```shell
[speit@node0] $ wget https://github.com/cloudflare/cfssl/releases/download/v1.6.1/cfssl_1.6.1_linux_amd64 -O cfssl
[speit@node0] $ wget https://github.com/cloudflare/cfssl/releases/download/v1.6.1/cfssljson_1.6.1_linux_amd64 -O cfssljson
```

使用install命令安装这些工具

```shell
[speit@node0] $ sudo install ./cfssl /usr/local/bin/
[speit@node0] $ sudo install ./cfssljson /usr/local/bin/
```

!!!note
    这些工具可以不必安装在集群的节点上，而是可以部署在本地。证书生成后，应当将其上传