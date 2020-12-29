#### 4.docker容器
aaa
cccbbb
11111
11223344
11111
##### 4.1.docker配置阿里镜像

- 获取阿里云的容器配置

  - 登录阿里云：https://cr.console.aliyun.com/cn-hangzhou/instances

  - 获取阿里云在docker中配置文件：

    ```json
    {
      "registry-mirrors": ["https://h45068lf.mirror.aliyuncs.com"]
    }
    ```

- 在虚拟集中配置

  - /etc/docker/daemon.json

    ```json
    {
      "registry-mirrors": ["https://h45068lf.mirror.aliyuncs.com"]
    }
    ```

    或者直接执行

    ```sh
    tee /etc/docker/daemon.json <<-'EOF'
    {
      "registry-mirrors": ["https://registry.docker-cn.com"]
    }d
    EOF
    ```

  - 重启docker

    sudo systemctl daemon-reload
    sudo systemctl restart docker

##### 4.2 拉去镜像

```sh
docker search 关键字 #查询可以镜像 eg：docker search mysql
docker pull 镜像名:tag   #拉取镜像 eg：docker pull mysql:5.7
docker images  #查看所有本地镜像
docker rmi id #删除指定的本地镜像
```

##### 4.3 启动容器

这是一个测试

aaaaaaaaaaa

bbbbbbbbbb

ccccccccccccc

这是一个测试
