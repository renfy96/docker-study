[toc]
#### 镜像
##### 拉取镜像
  ```
  docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
  ``` 
  + 具体的选项可以通过 docker pull --help 命令看到，这里我们说一下镜像名称的格式。
  + Docker 镜像仓库地址：地址的格式一般是 <域名/IP>[:端口号]。默认地址是 Docker Hub(docker.io)。
  + 仓库名：如之前所说，这里的仓库名是两段式名称，即 <用户名>/<软件名>。对于 Docker Hub，如果不给出用户名，则默认为 library，也就是官方镜像。
比如 
```
docker pull ubuntu:18.04

18.04: Pulling from library/ubuntu
88736512a147: Pull complete
Digest: sha256:43d8a96ef9acfbec2da181adc3b72ff1f30a555716c717e4a722b5031fa3d83e
Status: Downloaded newer image for ubuntu:18.04
docker.io/library/ubuntu:18.04
```
上面的命令中没有给出 Docker 镜像仓库地址，因此将会从 Docker Hub （docker.io）获取镜像。而镜像名称是 ubuntu:18.04，因此将会获取官方镜像 library/ubuntu 仓库中标签为 18.04 的镜像。docker pull 命令的输出结果最后一行给出了镜像的完整名称，即： docker.io/library/ubuntu:18.04。

###### 运行
有了镜像后，我们就能够以这个镜像为基础启动并运行一个容器。以上面的 ubuntu:18.04 为例，如果我们打算启动里面的 bash 并且进行交互式操作的话，可以执行下面的命令。
```
docker run -it --rm ubuntu:18.04 bash

cat /etc/os-release
NAME="Ubuntu"
VERSION="18.04.6 LTS (Bionic Beaver)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 18.04.6 LTS"
VERSION_ID="18.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=bionic
UBUNTU_CODENAME=bionic
```

`docker run` 就是运行容器的命令
+ `-it`：这是两个参数，一个是 -i：交互式操作，一个是 -t 终端。我们这里打算进入 bash 执行一些命令并查看返回结果，因此我们需要交互式终端。
+ `--rm`：这个参数是说容器退出后随之将其删除。默认情况下，为了排障需求，退出的容器并不会立即删除，除非手动 docker rm。我们这里只是随便执行个命令，看看结果，不需要排障和保留结果，因此使用 --rm 可以避免浪费空间。
+ `ubuntu:18.04`：这是指用 ubuntu:18.04 镜像为基础来启动容器。
+ `bash`：放在镜像名后的是 命令，这里我们希望有个交互式 Shell，因此用的是 bash。

##### 列出镜像
`docker image ls`  列出已经下载好的镜像
```docker
docker image  ls

REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
ubuntu       18.04     fdf0753c97a9   4 hours ago    63.2MB
mysql        latest    f2ad9f23df82   41 hours ago   521MB
alpine/git   latest    66704dc9cc56   46 hours ago   38.3MB
```
列表包含了 `REPOSITORY`仓库名、`TAG`标签、`IMAGE ID`镜像 ID、创建时间` CREATED` 以及 `SIZE`所占用的空间。

##### 删除本地镜像
如果要删除本地的镜像，可以使用 `docker image rm` 命令
后面可以跟着`镜像ID`，`镜像名`，`摘要`删除镜像
###### 用 docker image ls 命令来配合
想要批量删除某些镜像时
```
docker image rm $(docker image ls -q redis)
docker image rm $(docker image ls -q -f before=mongo:3.2)
```