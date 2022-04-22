# Centos7安装Docker

## docker版本列表

[请点击此链接获得最新docker版本](https://download.docker.com/linux/static/stable/x86_64/)

1. ## 在线命令执行脚本自动安装Docker(需要外网环境)-方式一

   ```http
   curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
   ```
   
2. ## docker安装包下载-方式二

   ```python
   # 离线下载:https://download.docker.com/linux/static/stable/x86_64/
   # 在线下载(推荐):
   curl -L https://download.docker.com/linux/static/stable/x86_64/docker-20.10.7.tgz > docker-20.10.7.tgz
   ```

3. ## docker-compose软件包下载

   ```python
   # 离线下载地址:https://github.com/docker/compose/releases
   # 在线下载(推荐): 
   curl -L https://get.daocloud.io/docker/compose/releases/download/1.29.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
   # 赋予执行权限: 
   chmod +x /usr/local/bin/docker-compose
   ```

4. ## docker安装

   ```python
   #1.下载好安装包或使用xshell将已下载好的docker安装包及docker-compose安装包复制到centos7服务器上
   #2.解压docker文件
   tar -zxvf docker-20.10.7.tgz
   #3.将解压后的docker文件拷贝到/usr/bin/
   mv docker/* /usr/bin/ && rm -rf docker
   #4.创建docker服务文件;并且复制对应的内容到下面的文件内;
   vi /etc/systemd/system/docker.service
   #5.增加可执行权限
   chmod 777 /etc/systemd/system/docker.service
   #6.重新加载配置文件
   systemctl daemon-reload
   #7.启动docker服务;注意需要root权限启动才能正常,否则engine服务会启动不了;
   systemctl start docker
   #8.设置开机自启,根据需要选择
   systemctl enable docker.service
   #9.根据需要设置镜像仓库
   vi /etc/docker/daemon.json
   #添加以下内容;可以设置镜像源;需要重启
   {"registry-mirrors": ["https://sxfj1832.mirror.aliyuncs.com","https://gcr.azk8s.cn", "https://docker.mirrors.ustc.edu.cn", "http://hub-mirror.c.163.com", "https://registry.docker-cn.com"]}
   #10.清空回收站
   rm -rf /root/.local/share/Trash/files
   # centos8下需要先关闭linux内核,否则报错
   setenforce 0
   ```

   docker.service配置文件

   ```python
   # 复制以下内容到docker.service文件;
   [Unit]
   Description=Docker Application Container Engine
   Documentation=https://docs.docker.com
   After=network-online.target firewalld.service
   Wants=network-online.target
   
   [Service]
   Type=notify
   # the default is not to use systemd for cgroups because the delegate issues still
   # exists and systemd currently does not support the cgroup feature set required
   # for containers run by docker,下面execstart不要添加与后面daemon.json冲突的内容,否则导致docker无法启动;
   ExecStart=/usr/bin/dockerd --selinux-enabled=false
   ExecReload=/bin/kill -s HUP $MAINPID
   # Having non-zero Limit*s causes performance problems due to accounting overhead
   # in the kernel. We recommend using cgroups to do container-local accounting.
   LimitNOFILE=infinity
   LimitNPROC=infinity
   LimitCORE=infinity
   # Uncomment TasksMax if your systemd version supports it.
   # Only systemd 226 and above support this version.
   #TasksMax=infinity
   TimeoutStartSec=0
   # set delegate yes so that systemd does not reset the cgroups of docker containers
   Delegate=yes
   # kill only the docker process, not all processes in the cgroup
   KillMode=process
   # restart the docker process if it exits prematurely
   Restart=on-failure
   StartLimitBurst=3
   StartLimitInterval=60s
   
   [Install]
   WantedBy=multi-user.target
   ```

5. ## 将用户加入docker组

   ```python
   # 非root用户使用docker，可以将用户加入docker组，避免每次执行都需要加上sudo语句。
   # 将$USER改为当前用户名称
   # 注：执行后需要注销退出重新登录生效。
   sudo groupadd docker
   sudo usermod -aG docker $USER
   sudo service docker restart 
   ```
   
6. ## 设置docker0默认网段,避免与院内ip网段冲突(如果院内网段为172段)

   ```python
   #route 查看网段信息
   #vim /etc/docker/daemon.json（这里没有这个文件的话，自行创建）
   {
       "bip":"192.168.0.1/24"
   }
   #重启docker 
   systemctl restart docker
   ```

7. ## 部署registry简易镜像仓库(建议使用)

   ```python
   #1.其它docker配置镜像源
   vi /etc/docker/daemon.json
   #2.添加以下内容;
   echo '{ "insecure-registries":["172.16.33.140:5000"] }' > /etc/docker/daemon.json
   #3.加载配置文件
   systemctl daemon-reload
   #4.重启docker生效
   systemctl restart docker
   #docker配置镜像源也是解决如下的问题的方法:
   # Error response from daemon: Get https://172.16.33.140:5000/v2/: http: server gave HTTP response to HTTPS client
   ```

   registry镜像常用命令

   ```python
   # 镜像拉取
   docker pull registry
   # 运行容器
   docker run -d -p 5000:5000 --name registry registry:latest
   #获取所有类别
   http://localhost:5000/v2/_catalog
   #获取镜像所有标签
   http://localhost:5000/v2/rhapsody/tags/list
   #打标签
   docker tag rhapsody:v6.5.0.210315_beta localhost:5000/rhapsody:v6.5.0.210315_beta
   #推送镜像到仓库
   docker push localhost:5000/rhapsody:v6.5.0.210315_beta
   
   # 非本机镜像拉取
   docker pull 172.16.33.140:5000/rhapsody:v6.5.0.210315_beta
   # 非本机镜像推送
   docker push 172.16.33.140:5000/rhapsody:v6.5.0.210315_beta
   ```

