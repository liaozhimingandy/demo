#### 容器部署Rhapsody集成引擎 :octicons-heart-fill-24:

##### 安装准备

| 文件         | 描述             |
| ------------ | ---------------- |
| docker.tar   | docker环境       |
| rhapsody.tar | rhapsody镜像文件 |
| xxxx.ohl     | license文件      |

前提有docker环境;centos7下安装docker请点击[此链接](https://docs.docker.com/engine/install/centos/)

##### 镜像下载

   ```yaml
   docker pull liaozhiming/rhapsody:6.7.210901_alpha
   ```

##### 容器启动

=== "docker命令"

```shell
docker run -d --name esb-rhapsody-dev -h rhapsody --restart=unless-stopped  -p 8444:8444 -p 8449:8449 -p 4031:4031 -p 3041:3041 -p 52053-52067:52053-52067 -e TZ="Asia/Shanghai" --add-host grpcservices.api.rhapsody.global:127.0.0.1 liaozhiming/rhapsody
```

=== "docker-compose命令"

```shell
# 容器启动命令
docker-compose up -d
# 容器移除命令
docker-compose down
```



##### docker-compose配置文件

??? ":material-file: docker-compose.yaml"

    ```yaml
    x-rhapsody-image: &rhapsody-image liaozhiming/rhapsody
    x-rhapsody-volumes: &rhapsody-volumes
      - data_rhapsody:/home/rhapsody/rhapsody/rhapsody-engine-6/rhapsody/data
      # ldap统一认证,根据需要开启
      # - ldap.properties:/home/rhapsody/rhapsody/rhapsody-engine-6/rhapsody/data/users/ldap.properties
    
    version: '3.8'
    services:
        rhapsody-server:
            labels:
              com.alsoapp.description: "rhapsody app"
              com.alsoapp.creator: "liaozhimingandy@qq.com"
              com.alsoapp.product: "Rhapsody"
              com.alsoapp.copyright: "©liaozhiming"   
            image: *rhapsody-image
            container_name: esb-rhapsody-pro
            restart: unless-stopped
            # 端口隐私设置,如果使用443,80等系统占用端口则会出现配置被拒绝,通讯点无法配置成功
            ports:
                - "8444:8444"
                - "8449:8449"
                - "3041:3041"
                - "4031:4031"
                - "52053-52067:52053-52067"
            volumes: *rhapsody-volumes
            hostname: rhapsody
            dns:
              - 8.8.8.8
              - 114.114.114.114
            extra_hosts:
              - grpcservices.api.rhapsody.global:127.0.0.1
              # ip改成现场实际
              - api.openldap.alsoapp.com:172.16.33.147
            environment:
              - TZ=Asia/Shanghai
    
    volumes:
      data_rhapsody:
        driver: local
    ```