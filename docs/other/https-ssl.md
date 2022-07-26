#### Rhapsody https SSL证书过期问题处理方案

Rhapsody自安装起,引擎会自动签发自签名证书,证书有效时间默认为5年,即当引擎运行5年后,使用IDE或控制台登录时会报SSL证书错误,此时需要处理的是在rhapsody安装目录下找到rhapsody删除以下文件后并{==**重启引擎**==};

1. ide.sha1.txt
2. managementConsole.sha1.txt
3. monitoring.keystore
4. soapApi.sha1.txt

!!! success "ssl证书信息存放位置"
     **服务端-windows端(需要重启引擎)**: 示例`D:\Rhapsody\Rhapsody Engine 6\rhapsody` <br>
     **服务端-centos端(需要重启引擎)**: 示例`/home/rhapsody/rhapsody/rhapsody-engine-6/rhapsody` <br>