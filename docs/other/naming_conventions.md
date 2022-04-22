## Rhapsody命名规范

## **命名格式**

格式:prefix_body_suffix

元素描述:

prefix_:前缀通常表示狂想曲对象的类型,尤其是组件(例如,tcp通讯点的tcp_).管理控制台对所有筛选器和通讯点使用相同的图标,并且不会在每个实例中指示其组件类型.

应与负责系统日常维护的用户一起概述和讨论组件类型前缀的约定.它们可能需要进行更改或添加,以保持良好的命名标准.

cp.-通讯点.

rt.-路线.

ws.-网络服务

_body:身体通常描述狂想曲物体的功能.

_suffix:如果需要,后缀提供补充信息.

### Engine Name-引擎名称

格式:engine_name-env

示例:

演示集成引擎-Alpha版

元素描述:

engine_name: 描述引擎的名称

env: 说明引擎所属环境,空为生产环境正式版;Alpha版:内部测试版;Beta版: 公测版;Release: 正式版;

### Lockers-工程名称

格式:function

示例:

complex conductor services

元素描述

function:储物柜的命名基于储物柜的要求(例如,按储物柜内接口的作用进行隔离,或按每个租户有一个文件夹的多机构租赁等分离要求进行划分)

### Folders-文件夹

格式: ##_type

示例:

01_get national identifiers

02_prepare myhr upload

1000_东华HIS(根据目的系统名称及代码也可)

元素描述

\##_:表示表示不同处理阶段的序列号的前缀.

type:文件夹可根据以下约定之一命名:

  他们集成的主要来源或目的地系统.

  路线执行的主要功能.

 

### Routes-路由

格式: rt.##_body_state

示例:

rt.01_adt validation_p

rt.01_empi sbr_or

rt.03_pas adt to lis_out

rt.03_terminology_in

元素描述

\##_:表示路线的两位数序列id的前缀.当配置在多个路由阶段中打破复杂的处理序列时,包括路由名称中的序列id可能有助于了解处理流程.

body:对于body来说,明确组件的目的是什么.

_state:表示路线状态的后缀:

  _p-处理.

  _or-编排.

  _out-出站.

  _in-入站.

所有方向规格应以狂想曲为中心:即入站意味着狂想曲接收消息,而出站意味着狂想曲正在发送消息.

### Communication Points-通讯点

格式: cp.type_function_mode

示例:

cp.tcp_pas_i

cp.email_abnormal_o

cp.ws_register doc_io

cp.ws_terminology_oi

cp.email_ascribe_e

cp.dir_reportable_bi

元素描述

cp.type_:例如,常用通讯点类型的前缀:

| **序号** | **通讯点类型前缀** | **通讯点类型**                 |
| -------- | ------------------ | ------------------------------ |
| 1        | cp.db_             | 数据库通讯点                   |
| 2        | cp.dbi_            | Database insertion通讯点       |
| 3        | cp.dir_            | 文件夹目录通讯点               |
| 4        | cp.email_          | 电子邮件客户端通讯点           |
| 5        | cp.tcps_           | tcp服务端通讯点                |
| 6        | cp.tcpc_           | tcp客户端通讯点                |
| 7        | cp.https_          | http网络服务通讯点             |
| 8        | cp.httpc_          | http网络通讯点客户端           |
| 9        | cp.dr_             | 动态路由通讯点                 |
| 10       | cp.wsh_            | Web service网络服务通讯点      |
| 11       | cp.wsc_            | Web service网络服务通讯点      |
| 12       | cp.sink_           | 消息回收站通讯点               |
| 13       | cp.exec_           | 外部可执行程序通讯点           |
| 14       | cp.rc_             | rhapsody连接器通讯点           |
| 15       | cp.timer_          | 定时器通讯点                   |
| 16       | cp.jsrest_         | JavaScript REST Client通讯点   |
| 17       | cp.jstcpc_         | JavaScript TCP Client通讯点    |
| 18       | cp.jstcps_         | JavaScript TCP Server通讯点    |
| 19       | cp.errredir_       | Error Message Redirector通讯点 |
| 20       | cp.ftp_            | (S)FTP Client通讯点            |
| 21       | cp.kafka_          | kafka通讯点                    |
| 22       | cp.amqp_           | AMQP通讯点                     |

 

function:描述系统及其目的的机构,例如:tcp_pas_adt.

_mode:识别消息流的后缀(与通讯点的模式相关联):

_i-输入

_o-输出

_io-先输入后输出

_oi-先输出后输入

_bi-双向

_e-出错

### Filters-过滤器

格式:type_function_route

示例:

js_process cda referra

元素描述:

type_:例如,前缀可识别过滤器的类型:

| **序号** | **过滤器前缀**    | **过滤器名称**                       |
| -------- | ----------------- | ------------------------------------ |
| 1        | asyc_             | Asymmetric Cryptography过滤器        |
| 2        | base64_           | Base64 Encoding过滤器                |
| 3        | batch/debatch_    | Batch / De-batch过滤器               |
| 4        | chartset_         | Character Encoding Translator过滤器  |
| 5        | content_          | Content Population过滤器             |
| 6        | dbl_              | Database Lookup Filters过滤器        |
| 7        | dbe_              | Database Message Extraction过滤器    |
| 8        | codetrans_        | Generic Code Translation过滤器       |
| 9        | dimjpg_           | DICOM JPEG Converter Filters过滤器   |
| 10       | dimxml_           | DICOM XML Converter Filters过滤器    |
| 11       | dupmsg_           | Duplicate Message Detection过滤器    |
| 12       | eval_             | EDI Message Validation过滤器         |
| 13       | ebxml_            | ebXML Filters过滤器                  |
| 14       | errque_           | Error Queue过滤器                    |
| 15       | holdque_          | Hold Queue过滤器                     |
| 16       | exec_             | Execute Process过滤器                |
| 17       | fm_               | FreeMarker Mapper过滤器              |
| 18       | hl7_              | HL7 Filters过滤器                    |
| 19       | holdque_          | Hold Queue过滤器                     |
| 20       | js_               | JavaScript过滤器                     |
| 21       | map_              | Mapper过滤器                         |
| 22       | nop_              | No-operation过滤器                   |
| 23       | prg_              | RPG Filters过滤器                    |
| 24       | pro_              | Property Population过滤器            |
| 25       | smine_            | S/MIME过滤器                         |
| 26       | sch_              | Schematron过滤器                     |
| 27       | snapl_            | Snapshot Load Filters过滤器          |
| 28       | snaps_            | Snapshot Save Filters过滤器          |
| 29       | replace_          | Search and Replace过滤器             |
| 30       | x12_              | X12 Filters过滤器                    |
| 31       | xml_              | XML Filters过滤器                    |
| 32       | zip/unzip_        | Zip /Unzip过滤器                     |
| 33       | jsonpar_          | General JSON Parsing Filter过滤器    |
| 34       | fhirxml/fhirjson_ | FHIR XML / JSON过滤器                |
| 35       | edival_           | EDI Message Validation过滤器         |
| 36       | hl7ackgen_        | HL7 Acknowledgement Generation过滤器 |
| 37       | hl7detcharset_    | HL7 Detect Character Encoding过滤器  |
| 38       | hl7extcodetrans_  | HL7 External Code Translation过滤器  |
| 39       | hl7extcodeval_    | HL7 External Code Validation过滤器   |
| 40       | jp_               | General JSON Parsing Filter          |

 

function:描述过滤器功能的主体.

_route:用于连接路线的预订端的无操作过滤器的后缀:

_from_precedingroute-输入无操作过滤器及其前面的路由名称.

_to_nextroute-输出无操作过滤器,带有下一条路线的名称.

示例:

nop_to_rt past og2

nop_to_rt past og2

### FilterTests-过滤器测试

过滤器测试期间使用的过滤器测试和测试消息应保存一个有意义的名称和对测试的简明描述.这使得进行同行评审的用户能够使用保存的测试消息来审查和测试特定的使用案例.

示例:

名字描述

patient id found:when the patient id property is extracted and set from the database

patient id not found:when the patient id property is not set from the database

conditional/javascript connectors-条件/javascript脚写连接器

格式:condition

示例:

is referral message

is required msg type

is error condition adt only

元素描述

condition:应简洁地命名有条件连接器,以指示其用途.对于未命名的条件连接器,其实际状态显示在连接上以截断形式显示.

### MessageDefinitions-消息定义

映射定义文件

格式:source_to_target.mdf

示例:

cerner_adt_hl7v23_to_carefusion_adt_hl7v22.mdf

元素描述

source:指示正在映射的源和源格式的前缀.

target:表示映射目标和目标格式的后缀.

### EdiDefinitions-EDI定义

格式:system_hl7version_messagetypes.s3d

示例:

actpas_patientmerge_hl72.3.1_adt040.s3d

cerner_hl7v23_adt.s3d

元素描述

system_:指示源或目标系统的前缀.

hl7version:指示定义定义的hl7版本的机构.

_messagetypes:表示定义定义中的消息类型的后缀.

### MessageProperties-消息属性

格式:valuetype

示例:

sourcefacilitycode

patientregdb_patientid

元素描述

valuetype:应适当命名解决方案属性和瞬态属性(仅与填充和使用该属性的路线阶段集相关),以指示其存储的价值.在某些情况下,在路线处理过程中填充的属性可能需要前缀来帮助确定其目的和交付它们的解决方案.对于由发布路线填充并传递给任何需要下游处理的订阅者(通常这些属性应在配置文档中管理以支持新订阅者轻松识别和使用)的属性尤其如此.

### RhapsodyVariables-狂想曲变量

格式:externalsystem_componenttype_function

示例:

actpas_ws_userid

smt_outputdoctype

cdr_db_password

ris_tcp_port

元素描述

externalsystem_:表示变量与外部系统相关的前缀.

componenttype:表示狂想曲变量用于配置的通讯点或过滤器类型的机构

示例:

常用通讯点

| **序号** | **通讯点类型前缀** | **通讯点类型**                       |
| -------- | ------------------ | ------------------------------------ |
| 1        | db_                | 数据库通讯点                         |
| 2        | dbi_               | Database insertion通讯点             |
| 3        | dir_               | 文件夹目录通讯点                     |
| 4        | email_             | 电子邮件客户端通讯点                 |
| 5        | tcps_              | tcp服务端通讯点                      |
| 6        | tcpc_              | tcp客户端通讯点                      |
| 7        | https_             | http网络服务通讯点                   |
| 8        | httpc_             | http网络通讯点客户端                 |
| 9        | dr_                | 动态路由通讯点                       |
| 10       | wsh_               | Web service网络服务通讯点            |
| 11       | wsc_               | Web service网络服务通讯点            |
| 12       | sink_              | 消息回收站通讯点                     |
| 13       | exec_              | 外部可执行程序通讯点                 |
| 14       | rc_                | rhapsody连接器通讯点                 |
| 15       | timer_             | 定时器通讯点                         |
| 16       | jsrest_            | JavaScript REST Client通讯点         |
| 17       | jstcpc_            | JavaScript TCP Client通讯点          |
| 18       | jstcps_            | JavaScript TCP Server通讯点          |
| 19       | errredir_          | Error Message Redirector通讯点       |
| 20       | ftp_               | (S)FTP Client通讯点                  |
| 21       | kafka_             | kafka通讯点                          |
| 22       | amqp_              | AMQP通讯点                           |
| 23       | asyc_              | Asymmetric Cryptography过滤器        |
| 24       | base64_            | Base64 Encoding过滤器                |
| 25       | batch/debatch_     | Batch / De-batch过滤器               |
| 26       | chartset_          | Character Encoding Translator过滤器  |
| 27       | content_           | Content Population过滤器             |
| 28       | dbl_               | Database Lookup Filters过滤器        |
| 29       | dbe_               | Database Message Extraction过滤器    |
| 30       | codetrans_         | Generic Code Translation过滤器       |
| 31       | dimjpg_            | DICOM JPEG Converter Filters过滤器   |
| 32       | dimxml_            | DICOM XML Converter Filters过滤器    |
| 33       | dupmsg_            | Duplicate Message Detection过滤器    |
| 34       | eval_              | EDI Message Validation过滤器         |
| 35       | ebxml_             | ebXML Filters过滤器                  |
| 36       | errque_            | Error Queue过滤器                    |
| 37       | holdque_           | Hold Queue过滤器                     |
| 38       | exec_              | Execute Process过滤器                |
| 39       | fm_                | FreeMarker Mapper过滤器              |
| 40       | hl7_               | HL7 Filters过滤器                    |
| 41       | holdque_           | Hold Queue过滤器                     |
| 42       | js_                | JavaScript过滤器                     |
| 43       | map_               | Mapper过滤器                         |
| 44       | nop_               | No-operation过滤器                   |
| 45       | prg_               | RPG Filters过滤器                    |
| 46       | pro_               | Property Population过滤器            |
| 47       | smine_             | S/MIME过滤器                         |
| 48       | sch_               | Schematron过滤器                     |
| 49       | snapl_             | Snapshot Load Filters过滤器          |
| 50       | snaps_             | Snapshot Save Filters过滤器          |
| 51       | replace_           | Search and Replace过滤器             |
| 52       | x12_               | X12 Filters过滤器                    |
| 53       | xml_               | XML Filters过滤器                    |
| 54       | zip/unzip_         | Zip /Unzip过滤器                     |
| 55       | jsonpar_           | General JSON Parsing Filter过滤器    |
| 56       | fhirxml/fhirjson_  | FHIR XML / JSON过滤器                |
| 57       | edival_            | EDI Message Validation过滤器         |
| 58       | hl7ackgen_         | HL7 Acknowledgement Generation过滤器 |
| 59       | hl7detcharset_     | HL7 Detect Character Encoding过滤器  |
| 60       | hl7extcodetrans_   | HL7 External Code Translation过滤器  |
| 61       | hl7extcodeval_     | HL7 External Code Validation过滤器   |

_function:表示该变量用作替代物的后缀(例如组件的配置属性).

[参考规范](https://www.alsoapp.com/docs-v1-rhapsody/Rhapsody-Best-Practices_133161788.html)

