### docker-compose

#### 下载

```sh
# 下载二进制文件
sudo curl -L https://github.com/docker/compose/releases/download/1.17.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

# 将文件提权至可执行文件
sudo chmod +x /usr/local/bin/docker-compose

# 检验是否安装成功
docker-compose version
```

#### 数据卷

> `-v`宿主机的文件路径 : 容器的文件路径

```sh
docker run -p 8080:8080 -d -v /usr/local/docker/myshop/ROOT/:/usr/local/tomcat/webapps/ROOT/ tomcat
```

#### docker 配置mysql

[教程](https://blog.csdn.net/socct_yj/article/details/103606173)

##### 拉取mysql镜像

```sh
docker pull mysql
```

##### 运行容器，然后将配置文件拷贝出来

```sh
docker run  -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD="123456" -d mysql
docker cp mysql:/etc/mysql/my.cnf /usr/local/docker/mysql/conf/my.cnf
# 删除容器
docker rm -f mysql
```

##### 挂载数据卷运行

```sh
# 数据卷还没数据时，需要初始化登录密码
docker run -p 3305:3306 --name mysql \
-v /usr/local/docker/mysql/conf/my.cnf:/etc/mysql/my.cnf \
-v /usr/local/docker/mysql/logs:/var/log/mysql \
-v /usr/local/docker/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=123456 \
-d mysql

# 配置好后就使用这个新建容器
docker run -p 3305:3306 --name mysql \
-v /usr/local/docker/mysql/conf/my.cnf:/etc/mysql/my.cnf \
-v /usr/local/docker/mysql/logs:/var/log/mysql \
-v /usr/local/docker/mysql/data:/var/lib/mysql \
-d mysql
```

##### 数据库连接失败时

```sql
//使用mysql数据库
use mysql;
//查看root用户的加密方式
select host,user,plugin from user;
//若root加密方式为caching_sha2_password
//修改为mysql_native_password
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'admin';
//如果navicat 提示“1045 access denied for user 'root'@'localhost' ”，则执行：
alter user 'root'@'localhost' identified by 'admin';
//如果navicat 提示“1045 access denied for user 'root'@'%' ”，则执行：
alter user 'root'@'%' identified by 'admin';
//刷新权限
flush privileges;
```

```sql
# 修改远程登录root密码
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'admin';
# 修改登录权限
grant all privileges on *.* to 'root'@'%' with grant option;
# 刷新数据库
flush privileges;
```

```sql
# 修改密码
ALTER user 'root' IDENTIFIED BY 'admin'; 
```

