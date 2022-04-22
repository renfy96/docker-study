[toc]
#### 数据卷
数据卷 是一个可供一个或多个容器使用的特殊目录，它绕过 UFS，可以提供很多有用的特性：
+ 数据卷 可以在容器之间共享和重用
+ 对 数据卷 的修改会立马生效
+ 对 数据卷 的更新，不会影响镜像
+ 数据卷 默认会一直存在，即使容器被删除

##### 创建一个数据卷
```
docker volume create my-vol
```
##### 查看所有数据卷
```
docker volume ls
```
###### 在主机里使用以下命令可以查看指定 数据卷 的信息
```
docker volume inspect my-vol
```
##### 启动一个挂载数据卷的容器
在用 `docker run` 命令的时候，使用 `--mount` 标记来将 `数据卷` 挂载到容器里。在一次 `docker run` 中可以挂载多个 `数据卷`。
下面创建一个名为 `project` 的容器，并加载一个 `数据卷` 到容器的 `/home/project` 目录。
```
docker run -d -P \
    --name project \
    --mount source=my-vol,target=/home/project \
    ubuntu:18.04
```
###### 查看容器的具体信息
```
docker inspect project
```
##### 删除数据卷
```
docker volume rm my-vol
```
如果需要在删除容器的同时移除数据卷。可以在删除容器的时候使用 `docker rm -v` 这个命令。
无主的数据卷可能会占据很多空间，要清理请使用以下命令
```
docker volume prune
```
##### 挂载一个主机目录作为数据卷
使用 `--mount` 标记可以指定挂载一个本地主机的目录到容器中去。
```
docker run -d -P `
    --name project `
    --mount type=bind,source=G:\owner,target=/home/project `
    ubuntu:18.04
```