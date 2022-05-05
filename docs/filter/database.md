#### **数据库操作**

##### 解析输入消息

| 字段                  | 含义/示例                                                    |
| --------------------- | ------------------------------------------------------------ |
| @id                   | @获得上一个数据集字段数据或者数据库关键列或者存储过程select的值 |
| EDI消息中的字段使用@  | @PID.PatientName[0].FamilyName.Surname                       |
| XML消息中的字段使用`  | &#96;/*/queryByParameter/patient/id/value/item[1]/@extension&#96; |
| 列自上一个结果使用@   | @PatientName                                                 |
| 存储过程输出参数使用# | #newPatientId                                                |
| **$MessageContent**   | 消息主体                                                     |
| $$tbl_name            | 动态表名/列名;需要开启允许动态查询的配置(**Options**)        |

!!! warning "关键列只针对数据库通信点生效,对于过滤器不起作用"

!!! warning "dbe数据库过滤器不应处理查询大批量数据,否则导致引擎GC Pause"

???+ danger highlight blink "除以下支持外,其它需要自行下载jdbc驱动"
	- [x] **SQL Server**<br>
    - [x] **Oracle**

!!! example  "自定义驱动程序存放位置"
     **服务端-windows端(需要重启引擎)**: 示例`C:\Program Files\Rhapsody\Rhapsody Engine 6\rhapsody\data\lib` <br>
     **服务端-centos端(需要重启引擎)**: 示例`/home/rhapsody/rhapsody/rhapsody-engine-6/rhapsody/data/lib` <br>
     **IDE端**: 示例 C:\Program Files (x86)\Rhapsody\Rhapsody IDE 6\Rhapsody IDE\DatabaseStructureReader


#### Access数据库

```python
# 驱动名称
Access_JDBC30.jar
# java驱动类名 driver class name
com.hxtt.sql.access.AccessDriver
# jdbc url;请注意,如果是ide则为ide能访问的地址,如果是服务端则填服务端的地址
# windows下连接方法
jdbc:access:/D:/KHdata/Database/Upload.mdb
# centos下连接方法示例
jdbc:access:////opt/dicom/Upload.mdb
```

#### MongoDB数据库

```python
# 驱动名称
mongodb_unityjdbc_full.jar
# java驱动类名 driver class name
mongodb.jdbc.MongoDriver
# 连接方法示例
jdbc:mongo://10.88.10.72:27077/esb_msg
```

#### Cache数据库

```python
# 驱动名称
cachejdbc.jar
# java驱动类名 driver class name
com.intersys.jdbc.CacheDriver
# 连接方法示例
jdbc:Cache://10.88.10.72:27077/esb_msg
```

#### MySQL数据库

```python
# 驱动名称
mysql-connector-java-5.1.44-bin.jar
# java驱动类名 driver class name
com.mysql.jdbc.Driver
# 连接方法示例
jdbc:mysql://DatabaseServer/PatientRecords
```

