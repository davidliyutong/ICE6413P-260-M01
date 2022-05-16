# 安装Minikube

如果只是学习API使用，安装Minikube是个很好的方法

## 安装本体

### 二进制安装

```shell
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

!!! note
    minikubu-linux-amd64 代表该minikube是为amd64平台编译的，只能用于amd64平台
    [Minikube start](https://minikube.sigs.k8s.io/docs/start/) 可以获取全平台安装的可执行文件

![Minikube installation](img/20220417230357.png)

!!! tip
    如果要删除安装的Minikube，需要使用`sudo rm /usr/local/bin/minikube`删除可执行文件

### DPKG 安装

与二进制安装类似

```shell
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
```

!!! tip
    如果要删除安装的Minikube，需要使用相应的`dpkg`命令卸载

如果想为`Minikube`添加终端的自动补全，可以执行如下命令

=== "Bash"

    ```shell
    echo 'source <(minikube completion bash)' >>~/.bashrc
    ```

=== "Zsh"

    ```shell
    echo 'source <(minikube completion zsh)' >>~/.zshrc
    ```

!!! tip

    可以通过`echo $SHELL`判断当前的Shell类型。Ubuntu默认使用的是Bash；最近的MacOS默认使用的是Zsh。

## Minikube 启动与停止

### 启动

```shell
minikube start
```

!!! tip
    如果出现连接问题导致无法下载镜像的问题，可以添加`--image-repository`参数执行镜像

    ```shell
    minikube start --image-mirror-country='cn' --driver docker --image-repository=registry.cn-hangzhou.aliyuncs.com/google_containers
    ```

!!! note
    Minikube 要求硬件必须满足

    - 2 vCPU
    - 2 GB RAM

    单核心的VPS，小内存的VPS是无法启动Minikube进行实验的

    ![Failed to start minikube](img/20220510132622.png)  

首次启动集群时，该命令会运行较长时间。这是因为要下载本地不存在的镜像。

![Minikube start](img/20220510133417.png)

集群启动后，可以使用`kubectl`工具检查集群。`kubectl`是K8S集群的管理工具。其安装教程在下一个章节

![Inspect Minikube](img/20220510133506.png)  

### 停止

停止的命令非常简单

```shell
minikube stop
```

## 安装kubectl

Minikube 只负责启动k8s实验集群，还需要安装`kubectl`工具管理，或者使用minikube自带的`kubectl`工具管理。

Minikube 自带的kubectl可以通过如下命令调用：

```shell
minikube kubectl -- get pod -A
```

该命令等同于

```shell
kubectl get pod -A
```

!!! tip
    你可以在终端配置文件中为该命令起一个别名，例如`alias kubectl="minikube kubectl --"`

若要安装kubectl则需要执行下列命令

```shell
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install ./kubectl /usr/local/bin/ # Use install command to replace move
```

!!! note
    这样安装的是最新的stable release，而`curl -L -s https://dl.k8s.io/release/stable.txt`被用来获取版本号

!!! warning
    kubectl需要从`dl.k8s.io`下载。该过程可能会遇到连接问题，因此，可以通过任何渠道获取该二进制文件，上传/拷贝到实验环境中进行安装

如果想为`kubectl`添加终端的自动补全，可以执行如下命令

=== "Bash"

    ```shell
    echo 'source <(kubectl completion bash)' >>~/.bashrc
    ```

=== "Zsh"

    ```shell
    echo 'source <(kubectl completion zsh)' >>~/.zshrc
    ```

![Kubctl installation](img/20220417225246.png)
