### docker

#### 1.安装

##### 1.apt-get安装

###### 安装必要的系统工具

```shell
sudo apt-get update
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
```

###### 安装 GPG 证书

```shell
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```

###### 写入数据源信息

```shell
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
```

###### 更新并安装 Docker CE

```shell
sudo apt-get -y update
sudo apt-get -y install docker-ce
```

> 以上命令会添加稳定版本的 Docker CE APT 镜像源，如果需要最新或者测试版本的 Docker CE 请将 stable 改为 edge 或者 test。从 Docker 17.06 开始，edge test 版本的 APT 镜像源也会包含稳定版本的 Docker。

###### 检查是否安装成功

```shell
docker version
```

##### 2.使用脚本自动安装

在测试或开发环境中 Docker 官方为了简化安装流程，提供了一套便捷的安装脚本，Ubuntu 系统上可以使用这套脚本安装：

```bash
$ curl -fsSL get.docker.com -o get-docker.sh
# 可能会出现 404 错误，请移步下面的特别说明
$ sudo sh get-docker.sh --mirror Aliyun
```

#### 2.docker镜像加速

登录阿里元账号，搜索`容器镜像服务`找到`镜像加速器`，然后复制里面的加速连接

> 下面的是淘宝账号的镜像链接

```text
https://npsyu8s9.mirror.aliyuncs.com
```

##### 1.一键修改指令

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://npsyu8s9.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

##### 2.检查是否配置成功

```shell
docker info
```

> 在 Registry Mirrors: 中能看到自己配置的镜像地址，就算是配置成功

#### 3.镜像操作

##### 拉取镜像

```shell
别人的笔记
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]

自己运行的
docker pull tomcat
```

- Docker 镜像仓库地址：地址的格式一般是 `<域名/IP>[:端口号]`。默认地址是 Docker Hub。
- 仓库名：如之前所说，这里的仓库名是两段式名称，即 `<用户名>/<软件名>`。对于 Docker Hub，如果不给出用户名，则默认为 `library`，也就是官方镜像。

##### 查看已安装镜像

```shell
查看已经安装的镜像
docker images
查看所有镜像和容器
docker system df
```

##### 删除

```sh
rm: 删除	i: 镜像
删除镜像: docker rmi -f tomcat
```

#### 4.容器操作

##### 运行容器

> 基于一个`镜像`，创建一个容器然后再去运行。
>
> 容器是镜像的一个实例，容器不会影响镜像。

```sh
别人的笔记
$ docker run -it --rm \
    ubuntu:16.04 \
    bash
自己运行的
docker run -p 8080:8080 tomcat
```

- `-it`：这是两个参数，一个是 `-i`：交互式操作，一个是 `-t` 终端。我们这里打算进入 `bash` 执行一些命令并查看返回结果，因此我们需要交互式终端。
- `--rm`：这个参数是说容器退出后随之将其删除。默认情况下，为了排障需求，退出的容器并不会立即删除，除非手动 `docker rm`。我们这里只是随便执行个命令，看看结果，不需要排障和保留结果，因此使用 `--rm` 可以避免浪费空间。
- `ubuntu:16.04`：这是指用 `ubuntu:16.04` 镜像为基础来启动容器。
- `bash`：放在镜像名后的是**命令**，这里我们希望有个交互式 Shell，因此用的是 `bash`。
- `-d`：守护态运行（后台运行）
- `-p`：（**宿主机端口** : **容器端口**）可以指定要映射的IP和端口，但是在一个指定端口上只可以绑定一个容器。支持的格式有

https默认端口**443**，http默认端口**80**

##### 查看容器

###### 运行中的容器

```sh
docker ps
或
docker container ls
```

###### 所有容器（运行的和没有运行的）

```sh
docker ps -a
```

##### 停止容器

```sh
docker stop [容器id/容器名(不能重复)]
```

##### 删除容器

> 与删除镜像的名字只少了个`i`

```sh
docker rm -f [容器id/容器名]
```

> 查询所有容器的`id`（只有id）

```sh
docker ps -a -q
```

> 强制删除所有容器

```sh
docker rm -f $(docker ps -a -q)
```

> 删除所有已停止的容器

```sh
docker container prune
```

##### 进入容器

```sh
docker exec -it [容器id] bash
```

##### 退出容器

快捷键：`Ctrl + d`

#### 5.dockerfile

##### 配置文件，和自定义文件

> 在Linux系统下新家 `/usr/local/docker` 文件夹存放定制的镜像项目
>
> 项目名：`myshop`，下面新建Dockerfile
>
> 项目名：`myshop`，放入自己的index.jsp

##### **构建镜像**

在`myshop`的文件夹下执行

```sh
docker build -t myshop .
```

> 这里的`.`表示上下文路径

#### 6.虚悬镜像

#### 7.中间层镜像

> 中间层镜像不能删除，这是其他镜像的一些依赖

#### 8.关于