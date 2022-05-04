#### 容器部署Rhapsody集成引擎 :octicons-heart-fill-24:

##### 安装准备

| 文件                          | 描述             |
| ----------------------------- | ---------------- |
| docker.tar                    | docker环境       |
| rhapsody_6.7.210901_alpha.tar | rhapsody镜像文件 |
| xxxx.ohl                      | license文件      |

前提有docker环境;centos7下安装docker请点击[此链接](docker_setup_centos7.md)

##### 镜像下载

   ```yaml
   docker pull liaozhiming/rhapsody:6.7.210901_alpha
   ```

##### 容器启动

=== "docker命令"

     ```dockerfile
     docker run -d --name esb-rhapsody-dev -h rhapsody --restart=always  -p 8444:8444 -p 8449:8449 -p 4031:4031 -p 3041:3041 -p 52053-52067:52053-52067 -e TZ="Asia/Shanghai" --add-host grpcservices.api.rhapsody.global:127.0.0.1 liaozhiming/rhapsody:6.7.0.20220503_release
     ```

=== "docker-compose命令"

    ```shell
     容器启动命令
    docker-compose up -d
     容器移除命令
    docker-compose down
    ```



##### docker-compose配置文件

??? ":material-file: docker-compose.yaml"
    ```yaml
     # 设置变量
         x-rhapsody-image: &rhapsody-image liaozhiming/rhapsody:6.7.0.220319_beta
         x-rhapsody-version: &version 6.7.0.220319_beta
         x-rhapsody-volumes: &rhapsody-volumes
           # 数据挂载在宿主机目录
           # 以下使用docker数据卷形式保存数据
           - data_rhapsody:/home/rhapsody/rhapsody/rhapsody-engine-6/rhapsody/data
           # 宿主机路径挂载到容器内部路径
           - /d/KHdata:/opt/dicom
    ```

         version: '3.8'
         # 语法参考官网:https://docs.docker.com/compose/compose-file/
         services:
             rhapsody-server:
                 labels:
                   com.alsoapp.description: "rhapsody app"
                   com.alsoapp.creator: "liaozhimingandy@qq.com"
                   com.alsoapp.product: "Rhapsody"
                   com.alsoapp.version: *version
                   com.alsoapp.copyright: "©liaozhiming"
    
                 image: *rhapsody-image
                 container_name: esb-rhapsody-dev
                 # 生产环境中请使用自动重启; always
                 restart: unless-stopped
                 # 端口隐私设置,如果使用443,80等系统占用端口则会出现配置被拒绝,通讯点无法配置成功
                 ports:
                     - "8444:8444"
                     - "8449:8449"
                     - "3041:3041"
                     - "4031:4031"
                     - "52053-52067:52053-52067"
                 networks:
                   - esb
    
                 # 因采用挂载方式启动容器,无法将数据进行封装进容器,生产环境中,不建议采用数据挂载;
                 # 1.二次封装的镜像使用挂载的话,需要先不挂载启动容器,拷贝挂载的数据至宿主机,然后使用挂载方式启动容器,最后需要赋予文件下所有文件可执行权限;
                 # docker cp cda-rhapsody-beta:/home/rhapsody/rhapsody/rhapsody-engine-6/rhapsody/data/.  /home/su01/data/data_cda
                 # chmod 777 -R data_cda
                 # 2.如果是使用docker数据卷,需要还原数据到数据卷,先借助其它中间容器,如:docker run -it -v data_rhapsody:/var/data/data centos:centos8.4.2105 /bin/bash;
                 # 进入容器执行:chmod 777 -R data
                 volumes: *rhapsody-volumes
                 # 资源限制
                 # deploy:
                 #   resources:
                 #     limits:
                 #       memory: 7G
                 #     reservations:
                 #       memory: 128M
    
                 hostname: rhapsody
                 # DNS配置;
                 dns:
                   - 8.8.8.8
                   - 114.114.114.114
                 # 添加域名ip对应到hosts文件;
                 extra_hosts:
                   - grpcservices.api.rhapsody.global:127.0.0.1
                 environment:
                   - TZ=Asia/Shanghai
                 # 伪终端,防止自动退出;
                 stdin_open: true
                 tty: true
                 # 特权模式,使用完毕后建议关闭,特权模式下方可真正拥有root权限;
                 privileged: false
    
         networks:
           esb:
             driver: bridge
    
         volumes:
           data_rhapsody:
             # 判断是否外部已有,若无则报错,建议关闭
             # external: true
             name: data_rhapsody
         # 由于做了资源限制, 并且没有使用swarm, 所以要加上--compatibility参数, 不然会报错
         # docker-compose --compatibility -f gateway.yml up -d
    ```





