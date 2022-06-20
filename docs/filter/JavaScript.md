#### JavaScript操作

[参考资料](https://blog.csdn.net/qq_42842786/article/details/107641415)

!!! warning "JavaScript过滤器不应处理大批量json数据,否则导致引擎GC Pause"

##### 常用

```javascript
js解析消息体节点请尽量使用input[0].getField(),不使用next.getField()
next一般用于设置属性,设置字段;防止内部死循环;
             
// 计算代码执行时间
var dt_start = new Date().valueOf();
var dt_end = new Date().valueOf();
log.info(dt_end-dt_start)
// 动态路由目标动态设置
next.setProperty("router:Destination", "@router-"+_json.event.eventCode);
// 或者使用过滤器Property Population设置属性
next.setProperty("rhapsody:consumerOperation", "对方web service接口方法名");
// 禁用消息超时
next.setProperty("rhapsody:TimeToLive", null)

//移除属性
next.setProperty("data", null);

//生成uuid
generateUuid()
```

##### XML操作

```javascript
// 遍历xml节点示例
var data = new XML(input[0].xml);

// 此处不能从根节点开始遍历
//去除不是出院的就诊信息
var data = new XML(input[0].xml);
var tmp_data = new XML(<AdmInfoList></AdmInfoList>);
for(var i = 0; i < data.AdmInfo.length(); i++){
	if(data.AdmInfo[i].AdmStatusCode == 'D'){
		tmp_data.appendChild(data.AdmInfo[i]);
		}
	}
next.setText(tmp_data, 'UTF8');

var data =  input[0].xml;
data.DML_TYPE = 'D'; // 也适用于json消息字段设置
或者
next.setField('/CACHE/DML_TYPE', 'D');  //也适用于HL7 v2版本消息字段设置

// 若不指定item索引值,则返回所以item的节点用,分割
var patient_id = next.getField('//controlActProcess/subject/procedureRequest/componentOf1/encounter/subject/patient/id/item[@root="2.16.156.10011.2.5.1.4"]/@extension')
//设置xpath节点数据
next.setField('//observationRequest/componentOf1/encounter/id/item[@root="2.16.156.10011.2.5.1.8"]/@extension', next.getProperty('visit_times'))
```

##### JSON操作

```javascript
// 文本转json对象
var data = JSON.parse(input[0].text);
// json转文本
next.text = JSON.stringify(data);
// 删除json节点
//删除门诊就诊节点信息
delete data.queryAck.ENCOUNTER_OUTPATIENTS;

//如果消息体< 500k则解析,否则不解析
if(input[0].body.length < 512000){
}

// 遍历json节点
var content = JSON.parse(next.text);

var data = content.query.PATHOLOGY_RESULT
if(data != null && data.length > 0){
for(var i = 0; i <data.length; i++){
    var name_en = data[i].DATA_ELEMENT_EN_NAME;
    var value = data[i].DATA_ELEMENT_VALUE;

    next.setProperty(name_en, value);
    }
}

//json节点判断存在及过滤
if(data.hasOwnProperty('status_code') && data.status_code != 200){
	throw "返回内容异常";
	}
```
##### 引用js库

```javascript
var com_alsoapp_esb_utils = require("com_alsoapp_esb_utils");
var tmp_url = next.getProperty('http:request-url');
var data = com_alsoapp_esb_utils.get_url_key(tmp_url);
```

##### 正则表达式

```javascript
// 更多的正则表达式匹配示例请参考regexp.md文件
var tmp_data = input[0].text;
var reg = new RegExp("http://17.{1,128}.pdf");
//如果匹配到则执行一下语句
if(reg.test(input[0].text)){
	var tmp_url = reg.exec(input[0].text);
    //或使用str.match(reg)
	next.setProperty("tmp_url", tmp_url);
	var tmp_data = input[0].text.replace(reg, 'url因为有特殊字符而被替换');
	}
var data = "<xml><![CDATA["+ tmp_data +"]]></xml>";
next.setText(data ,"UTF8");

// 匹配DIAGNOSIS_ID和01_1之间任意内容
DIAGNOSIS_ID(.|\n)*?01_1

// 除去换行符号和空格;不适合>50k的文本处理                                                     
var data = input[0].text.replace(/\r\n/g,"").replace(/\s+/g,"");
next.text = data;
// 全部替换,如果不加/g,则替换第一个
replace(/-/g, '')

// xml特殊符号去除
// 存数据库之前
value.replace(/\&/g,"&amp;").
replace(/\"/g,"&quot;").
replace(/\'/g,"&#39;").
replace(/\</g,"&lt;").
replace(/\>/g,"&gt;")

#json.parse解析之前
var data = input[0].text.
replace(/</g, "&lt;").
replace(/>/g, "&gt;").
replace(/\'/g, "&#39;").
replace(/("")+/g, "&quot;").
replace(/\r/g, "\\r").
replace(/\n/g, "\\n").
replace(/\t/g, "\\t").
replace(/\\/g, "\\\\").
replace(/ /g, "&nbsp;");
next.text = '<xml>'+data+'</xml>';
```
##### 自定义索引

```javascript
/*
为输入属性编制索引
*/
next.indexInputProperty("hip_id");

/*
为输出属性编制索引
*/
next.indexOutputProperty("hip_id");
/*
删除指定属性的所有索引
*/
removeIndexingForProperty(string propertyName)
```

##### Base64操作

```javascript
//编码
encodeBase64(base64encodedString)
//解码
decodeBase64(value[, encoding])
```

##### JSON简单转xml

###### 方式一

```javascript
var tmp_data = JSON.parse(next.text);
// 构造xml数据
var data = <xml>
	<code>200</code>
	<msg>消息接收成功!</msg>
	<gmt_rcv>{new Date(parseInt(next.getProperty('InputTime')))}</gmt_rcv>
	<gmt_created>{new Date().toString()}</gmt_created>
</xml>;
next.setText(data, 'UTF8');
```

###### 方式二

```javascript
//通过getErrors()方法获取错误路由返回的异常数据
var body = input[0].getErrors()

//循环解析具体的异常信息
var errxml = new XML(<err></err>);
for(var i=0; i<body.length; i++){
    var errdata = "<err"+i+">"+body[i]+"</err+"+i+">";
    errxml.appendChild(errdata);
}

//将异常信息输出
next.setText("<message>"+errxml+"<message>")
```

##### xml转json

```python
// 动态xml节点转json
var tmp_data = new XML(input[0].xml);
var content = tmp_data.children();
var data = {};
for(var i = 0; i < content.length(); i++){
	//log.info(t[i]);
	//log.info(content[i].name().toString()+':'+content[i].text());
	var str_data = content[i].name().toString()+':'+content[i].text()
	var arr = str_data.split(":");  
	data[arr[0].toLowerCase()] = arr[1];  
	};

next.text = JSON.stringify(data);
```



##### 查找表操作

```javascript
// 查找表使用
if(tmp_dig_code != null && tmp_dig_code.length > 0){
	var data = lookup("mapping_icd_10",{"value_dh": tmp_dig_code});
	var dig_code = data.value;
	var dig_name = data.comment;
	}

//获取环境变量
var env = lookup("configs", {"key": "env"});
next.setProperty("env", env.value);
```

##### 随机数生成

```javascript
//需要产生个人信息注册的消息厂商列表
var list_sender = ['H0001', 'H0002'];
var sender = list_sender[Math.floor(Math.random()*list_sender.length)];
```

##### 其他操作

```python
// 日期格式化转换
var data = dateChangeFormat('2012/01/02', 'YYYY/MM/dd', 'dd/MM/YYYY');
	
// 一个属性添加多个值
next.addPropertyValue('router:Destination', '@hl7v3')
	
// 设置消息体内容
next.text = ''
或: next.setText('', 'UTF8');
	
getBodySize() //获取body消息体大小
getFieldAsList(string fieldPath) // 得到字段为重复的数组
getRepeatCount(string fieldPath) // 得到字段重复次数
getPropertyNames() // 得到所有属性内容
getErrors() // 得到错误信息

// 迭代消息属性遍历 
var properties = readOnlyMessage.getPropertyAsList("PropertyList");

    // Loop through the properties, 
    var iterator = properties.iterator();
    while (iterator.hasNext()) {
        var oneProperty = iterator.next();
        log.info("One property is: " + oneProperty);
    }

// 获取rhapsody全局变量
getVariable(variableName[, decryptVariable])

// javascript计数器使用
getCounter(counterName)
setCounter(counterName, int)
incCounter(counterName[, increment]) //新增后返回计数器
                                                                              
//将两个或多个字符的文本组合起来，返回一个新的字符串                                               if(body.lastIndexOf('}') == -1){
body = body.concat('}');
}
//返回字符串中一个子串最后一处出现的索引（从右到左搜索），如果没有匹配项，返回 -1 
body.lastIndexOf('}')                                                                     
```

##### JavaScript计数器

```javascript
// 递增全局计数器以获取新的唯一ID
var myUniqueID = incCounter("MyUID", 1);

// 将新ID设置为消息的属性
next.setProperty("UniqueID", myUniqueID);
```

##### 异常处理

```javascript
try {

  //运行代码

} catch(err) {

   log.info(err)
   log.info(err.message);
   next.addError(err.message);

}finally{    //可选
	
	}

// 获取js抛出的异常
next.getErrors()
```
##### 常用自定义函数

###### 解析URL参数

```javascript
function get_url_key(url) {
  // 解析url携带的参数信息
  var params = {};
  try{
    var urls = url.split("?");           
    var arr = urls[1].split("&");              
    for (var i = 0, l = arr.length; i < l; i++) {
      var a = arr[i].split("=");              
      params[a[0]] = a[1];                  
    }
                                           
  }catch(e){}
  return params;
}
var tmp_url = next.getProperty('http:request-url');
var data = get_url_key(tmp_url);
next.setProperty("patient_id", data["patient_id"]);
next.setProperty("source_system_code", data["source_system_code"]);
```

###### 当前日期时间自定义格式

```javascript
var get_datettime_format = function (obj_date, fmt) {
  var dateTime=obj_date;
  var o = {
      "M+": dateTime.getMonth() + 1, //月份 
      "d+": dateTime.getDate(), //日 
      "H+": dateTime.getHours(), //小时 
      "m+": dateTime.getMinutes(), //分 
      "s+": dateTime.getSeconds(), //秒 
      "q+": Math.floor((dateTime.getMonth() + 3) / 3), //季度 
      "S": dateTime.getMilliseconds() //毫秒 
    };
  if (/(y+)/.test(fmt)) {
 	 fmt = fmt.replace(RegExp.$1, (dateTime.getFullYear() + "").substr(4 - 	RegExp.$1.length));
  }
  for (var k in o){
	  if (new RegExp("(" + k + ")").test(fmt)) 
	  {
	  	fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
	  }
  }
  return fmt;
}

//eg:
log.info(get_datettime_format(new Date(), "yyyy-MM-dd HH:mm:ss.S"));
//eg2:
log.info(get_datettime_format(new Date(parseInt(next.getProperty("InputTime"))), "yyyy-MM-dd HH:mm:ss.S"));
```

