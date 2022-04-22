Dockerfile

```dockerfile
# 使用官方镜像CentOS作为运行的基础镜像;centos7会出现已知问题,故采用centos8作为基础镜像;
FROM centos:centos8

# 维护者信息
LABEL maintainer="liaozhiming<liaozhimingandy@qq.com>"

ENV TZ Asia/Shanghai
# 设置语言包
ENV LANG C.UTF-8

# 更新软件安装依赖软件java,如果不安装java则会提示没有显示屏幕
RUN yum update -y && yum install java-1.8.0-openjdk -y && yum clean all && rm -rf /var/cache/yum/* && useradd rhapsody && echo "rhapsody:rhapsody@1993" | chpasswd && chgrp rhapsody /var/lock/ && chmod g+w /var/lock/

USER rhapsody

# 设置工作目录为 /home/rhapsody
WORKDIR /home/rhapsody

# 将当前目录内容复制到位于的容器中 /home/rhapsody
# 切记新建rhapsody文件夹,将需要构建的内容放置rhapsody文件夹内;
# 请注意rhapsody.properties文件内data的路径问题;需要证书文件也放入的情况,请自行添加mv操作同时需要注意证书记录了主机名等信息,若检测到变化则证书失效;
COPY docker .
RUN ./rhapsody-6_7_0-linux-x64.sh -q && rm -rf rhapsody-6_7_0-linux-x64.sh 

USER root
RUN mv wrapper-local.conf /home/rhapsody/rhapsody/rhapsody-engine-6/bin/ && mv zh_CN.locale.csv /home/rhapsody/rhapsody/rhapsody-engine-6/rhapsody/locale/ && mv rhapsody.properties /home/rhapsody/rhapsody/rhapsody-engine-6/rhapsody/rhapsody.properties

USER rhapsody
# 容器启动时启动应用程序
CMD ["sh","-c","/home/rhapsody/rhapsody/rhapsody-engine-6/bin/rhapsody.sh console"]

# docker build --squash -t rhapsody:6.5.0.210527_release . 可以进一步减少安装包遗留在镜像内,此功能需要docker设置中配置
# 如果需要将rhapsody的授权证书也放入镜像封装,请注意rhapsody证书注册时会记录主机名进行检验,所以务必将容器主机名称设置为rhapsody;
# rhapsody二次封装请先手工停止rhapsody服务后进行封装
```

