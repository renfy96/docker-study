[toc]
#### 容器
##### 新建&启动
举个栗子
```
docker run ubuntu:18.04 /bin/echo 'Hello world'

Hello world
```
意思是用ubuntu:18.04这个镜像创建一个容器 执行后面的命令

```
docker run -t -i ubuntu:18.04 /bin/bash
```
这个命令这是启动一个bash终端 允许用户交互
`-t` 让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上
`-i` 则让容器的标准输入保持打开。
当利用 docker run 来创建容器时，Docker 在后台运行的标准操作包括：
+ 检查本地是否存在指定的镜像，不存在就从 registry 下载
+ 利用镜像创建并启动一个容器
+ 分配一个文件系统，并在只读的镜像层外面挂载一层可读写层
+ 从宿主主机配置的网桥接口中桥接一个虚拟接口到容器中去
+ 从地址池配置一个 ip 地址给容器
+ 执行用户指定的应用程序
+ 执行完毕后容器被终止
###### 启动已终止容器
可以利用 `docker container start` 命令，直接将一个已经终止（exited）的容器启动运行。
###### 守护态运行
启动容器是加入`-d`参数，启动后会返回一个唯一的 id，也可以通过 `docker container ls` 命令来查看容器信息。
要获取容器的输出信息，可以通过 `docker container logs` 命令。
```
docker container logs [container ID or NAMES]
```
##### 终止容器
可以使用 `docker container stop` 来终止一个运行中的容器。
`docker container restart` 命令会将一个运行态的容器终止，然后再重新启动它。

##### 进入容器
主要是使用`exec`命令
`docker exec` 后边可以跟多个参数，这里主要说明 `-i` `-t` 参数。
只用 `-i` 参数时，由于没有分配伪终端，界面没有我们熟悉的 Linux 命令提示符，但命令执行结果仍然可以返回。
当 `-i` `-t` 参数一起使用时，则可以看到我们熟悉的 Linux 命令提示符。
##### 导入导出容器
如果要导出本地某个容器，可以使用 `docker export` 命令。
```
docker export 5199e9f567bb > ubuntu.tar
```
可以使用 `docker import` 从容器快照文件中再导入为镜像，例如
```
cat ubuntu.tar | docker import - own/ubuntu:v1.0
```
##### 删除容器
可以使用 `docker container rm` 来删除一个处于终止状态的容器。
```
ocker container rm trusting_newton
```
用 `docker container ls -a` 命令可以查看所有已经创建的包括终止状态的容器，如果数量太多要一个个删除可能会很麻烦，用下面的命令可以清理掉所有处于终止状态的容器。
```
docker container prune
```