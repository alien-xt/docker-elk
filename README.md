# Docker ELK Stack

将ELK (Elasticseach, Logstash, Kibana)部署在Docker

# 准备

1. 安装docker
	* [Windows安装](https://docs.docker.com/docker-for-windows/)
	* [Mac安装](https://docs.docker.com/docker-for-mac/)
	* [Ubuntu安装](https://docs.docker.com/docker-for-mac/)
	* [Centos安装](https://docs.docker.com/engine/installation/linux/centos/)
2. 安装docker-compose [安装](https://docs.docker.com/compose/install/)

# 基础镜像

* [elasticsearch](https://hub.docker.com/_/elasticsearch/)
* [logstash](https://hub.docker.com/_/logstash/)
* [kibana](https://hub.docker.com/_/kibana/)

**Note**: 默认配置将会从国内镜像仓库下载，以加速镜像的下载 [国内仓库](https://hub.daocloud.io/)

# 使用

## hello, elk

下载该项目

```bash
git clone https://github.com/alienxt/docker-elk.git
```

启动

```bash
cd docker-elk
docker-compose up
```

浏览器打开http://localhost:5601/，是不是可以访问到Kibana了

logstash写入数据

```bash
telnet localhost 5000 <<-'EOF'
hello,elk
EOF
```

刷新Kibana，是不是就可以看见index[logstash-*]的数据了

## 集群配置

疑惑

* 怎么看服务的日志呢
* 怎么修改服务的配置呢
* 容器之间怎么通信呢
 
### 神奇的Docker

我们可以很简单的通过Docker提供的功能，解决上述问题

#### 挂载宿主机目录

**Note**: -v

```bash
docker run -v "path":container/path ...
```

该命令可以将宿主机的目录挂载到容器里，也就是当宿主机的目录或文件发生变化，容器内也将会变化。我们可以通过该功能，动态配置容器的配置

#### 容器间通信

**Note**: --link

```bash
docker run --name logstash --link elasticsearch:elasticsearch ...
```

该命令会将两个容器的网络建立桥接，也就是我们在启动logstash的时候指定elasticsearch的地址，于是我们就可以在logstash这样配置

```conf
elasticsearch {
  hosts => "elasticsearch:9200"
}
```

是不是觉得上面的疑惑全解决了呢


### docker-compose

本示例是通过docker-compose该工具来构建elk stack的各个服务。简单说，docker-compose是docker官方提供的一个基于docker之上的部署复杂服务的工具

在根目录的docker-compose.yml文件中，描述各个服务的配置，建议简单先看看docker-compose的[帮助文档](https://docs.docker.com/compose/gettingstarted/)




