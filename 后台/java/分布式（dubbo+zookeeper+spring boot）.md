### Dubbo 

#### 架构

![dubbo-architucture](../../static/image/dubbo-architecture.jpg)

#### Dubbo-admin

> 一个监控管理后台

```yml
version: '3.1'
  
services:
   zookeeper:
      image: zookeeper
      container_name: myzookeeper
      ports:
        - 2181:2181
      volumes:
        - /usr/local/docker/zookeeper/data:/data/
      environment:
        privileged: "true"

   admin:
      image: chenchuxin/dubbo-admin
      container_name: dubbo-admin
      depends_on:
        - zookeeper
      ports:
        - 8080:8080
      environment:
        - admin.registry.address=zookeeper://zookeeper:2181
        - dubbo.admin.root.password=root
        - dubbo.admin.guest.password=guest
```



### Zookeeper

#### ZooKeeper目标

> ZooKeeper致力于为分布式应用提供一个高性能、高可用，且具有严格顺序访问控制能力的分布式协调服务
>
> 2.1 **高性能**
> ZooKeeper将全量数据存储在内存中，并直接服务于客户端的所有非事务请求，尤其适用于以读为主的应用场景
>
> 2.2 **高可用**
> ZooKeeper一般以集群的方式对外提供服务，一般3 ~ 5台机器就可以组成一个可用的Zookeeper集群了，每台机器都会在内存中维护当前的服务器状态，并且每台机器之间都相互保持着通信。只要集群中超过一般的机器都能够正常工作，那么整个集群就能够正常对外服务
>
> 2.3 **严格顺序访问**
> 对于来自客户端的每个更新请求，ZooKeeper都会分配一个全局唯一的递增编号，这个编号反映了所有事务操作的先后顺序

#### 五大特性

> ZooKeeper一般以集群的方式对外提供服务，一个集群包含多个节点，每个节点对应一台ZooKeeper服务器，所有的节点共同对外提供服务，整个集群环境对分布式数据一致性提供了全面的支持，具体包括以下五大特性：
>
> 3.1 **顺序一致性**
> 从同一个客户端发起的请求，最终将会严格按照其发送顺序进入ZooKeeper中
>
> 3.2 **原子性**
> 所有请求的响应结果在整个分布式集群环境中具备原子性，即要么整个集群中所有机器都成功的处理了某个请求，要么就都没有处理，绝对不会出现集群中一部分机器处理了某一个请求，而另一部分机器却没有处理的情况
>
> 3.3 **单一性**
> 无论客户端连接到ZooKeeper集群中哪个服务器，每个客户端所看到的服务端模型都是一致的，不可能出现两种不同的数据状态，因为ZooKeeper集群中每台服务器之间会进行数据同步
>
> 3.4 **可靠性**
> 一旦服务端数据的状态发送了变化，就会立即存储起来，除非此时有另一个请求对其进行了变更，否则数据一定是可靠的
>
> 3.5 **实时性**
> 当某个请求被成功处理后，ZooKeeper仅仅保证在一定的时间段内，客户端最终一定能从服务端上读取到最新的数据状态，即ZooKeeper保证数据的最终一致性

#### Docker 安装并且启动

- 安装

```bash
docker pull zookeeper
```

- 启动

```bash
docker run --privileged=true -d --name zookeeper --publish 2181:2181  -d zookeeper:latest -v /usr/local/docker/zookeeper/data/:/data/
```

- docker-compose

```yml
version: '3.1'
  
services:
   zookeeper:
      # restart: always
      image: zookeeper
      container_name: myzookeeper
      ports:
        - 2181:2181
      volumes:
        - /usr/local/docker/zookeeper/data:/data/
      environment:
        privileged: "true"
```

