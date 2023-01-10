#### 系统要求

##### 硬件要求(按大型网站标准建设)

###### 假设: 

- 平均邮件大小10kB和平均邮件保留期28天
- 该网站每天收到超过3000000条消息
- 每条消息都需要从一种格式映射到另一种格式,并在将消息转发到另外10个系统之前执行简单的表查找
- 峰值负载约为持续速率的四倍

##### 最低硬件建议

- Windows® Server或Linux(推荐)
- 64位操作系统
- 8核16线程
- 16GB内存(RAM)和 8GB 堆内存(heap)
- 40GB磁盘空间用于操作系统和应用程序(RAID1)
- 用于数据存储(RAID10)的1TB磁盘空间(磁盘io延迟<4)

​	<a href="https://www.alsoapp.com/docs-rhapsody/6.9.1/en/hardware-requirements.html" target="_blank">点击此链接查看官方文档</a>