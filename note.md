# 容器化部署工具Docker学习笔记

学习过程中使用Centos7虚拟机

## 1. Docker的卸载

```shell
yum remove docker \
			docker-client \
			docker-client-latest \
			docker-common \
			docker-latest \
			docker-latest-logrotate \
			docker-logrotate \
			docker-selinux \
			docker-engine-selinux \
			docker-engine \
			docker-ce
```

## 2. 安装Docker

### 2.1 安装yum工具

```shell
yum install -y yum-utils \
				device-mapper-persistent-data \
				lvm2 --skip-broken
```

### 2.2 更新本地镜像源

```shell
yum-config-manager \
	--add-repo \
	https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
	
sed -i 's/download.docker.com/mirrors.aliyun.com\/docker-ce/g' /etc/yum.repos.d/docker-ce.repo

yum makecache fast
```

### 2.3 安装docker-ce（社区免费版）

```shell\
yum -y install docker-ce
```

### 2.4 关闭防火墙

docker运行时需要用到各种各样的端口，在企业中，需要逐个配置到防火墙中

而现在是学习阶段，直接关闭防火墙就可以了

```shell
# 关闭防火墙
systemctl stop firewalld
# 禁用开机自启
systemctl disable firewalld
```

### 2.5 启动docker

```shell
systemctl start docker

# 查看版本号验证是否开启成功
docker -v
```

### 2.6 配置阿里云镜像

```shell
sudo mkdir -p /etc/docker
```

```shell
sudo tee /etc/docker/daemon.json <<- 'EOF'
{
  "registry-mirrors": ["https://n0dwemtq.mirror.aliyuncs.com"]
}
EOF
```

```shell
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 3. Docker基本操作

### 3.1 镜像操作

构建镜像

```shell
docker build
```

从服务端拉取镜像

```
docker pull
```

推送镜像到服务

```
docker push
```

查看镜像

```
docker images
```

删除镜像

```
docker rmi
```

保存镜像为一个压缩包

```
docker save
```

加载压缩包为镜像

```
docker load
```



**案例：拉取nginx镜像并查看**

可以从DockerHub网站`hub.docker.com`查找镜像

```shell
docker pull nginx
docker images
```

![](./assets/拉取nginx镜像.png)



**案例：将nginx镜像保存为压缩包，并重新加载**

使用docker save --help命令可以查看详细使用方法

- 保存镜像

![](./assets/保存nginx镜像.png)

- 删除当前的nginx镜像

![](./assets/删除nginx镜像.png)

- 重新加载镜像

![](./assets/重新加载nginx镜像.png)

