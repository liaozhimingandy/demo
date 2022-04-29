### 模板引擎-freemarker

[参考文档](https://www.cnblogs.com/iqian/p/5106580.html)

#### 基础部分

```python
# 注释格式
<#-- 此处是注释 -->

# 定义一个变量
<#assign answer=42/>

# 定义函数
# 解析xml里的json数据,使用以下语句处理json,可以避免xml转json中节点含有null的报错问题;若操作的是xml,则可跳过此步骤
<#function parseJSON str>
  <#-- null is not a keyword in FTL -->
  <#local null = 'null'> ;
  <#return str?eval>
</#function>
<#assign tmp_data>
${inputXML.xml}
</#assign>

<#-- 使用函数 -->
<#assign data=parseJSON(tmp_data) />

# 为空时则使用默认值
${msg!""}或${msg!}

# ?? 判断变量是否存在或对象的属性是否为null

#删除连续多个变量之间的换行符; 重要--可以去除多余空白,节约空间
<#t> // 去掉左右空白和回车换行 
<#lt>// 去掉左边空白和回车换行
<#rt>// 去掉右边空白和回车换行 
<#nt>// 取消上面的效果

#如何输出${xxx} 这样的字符串 
<#noparse>${ccc}</#noparse>   
```

#### 字符串操作

```python
# 字符串连接操作- +
eg:${str1 + str2}
# 获取字符串的长度- ?length
eg:${str1?length}
# 取第一次出现的索引- ?index_of()
eg:${str1?index_of('e')}
# 取最后一次出现的索引- ?last_index_of()
eg:${str1?last_index_of('e')}
# 字符串截取- ?substring()
eg:${str1?substring(0,2)}
# 字符串替换- ?replace()
eg:${str1?replace('o','dddd')}
# 字符串分割- ?split()
eg:<#list str1?split("_") as item>${item}</#list>
# 去掉前后的空格- ?trim
eg:${str1?length}-${str1?trim?length}
# 判断字符串是否包含指定的子字符串- ?contains()
eg:${str1?contains('llo')?c}
# 判断是否以给定的子字符串开头- ?starts_with()
eg:${str1?starts_with("Hel")?c}
# 判断是否以给定的子字符串结尾- ?ends_with()
eg:${str1?ends_with("oe")?c}
# 将字符串全部转换为大写- ?upper_case
eg:${str1?upper_case}
# 将字符串全部转换为小写- ?lower_case
eg:${str1?lower_case}
# 将字符串的首字母变大写- ?cap_first
eg:${str2?cap_first}
<#-- 从索引为3位置开始截取，到索引为5位置结束不包括5 5-3-->
${"asddfdsd"?substring(3,5)}
<#--字符串的第一个字符大写-->
${"abcd"?cap_first}
<#--字符串的第一个字符小写-->
${"abcd"?uncap_first}
<#--字符串的每个字母的第一个字符大写-->
${"abcd say "?capitalize}
<#--判断 字符串的最后一个字符是什么，返回的是布尔值 需要特殊处理-->
${"abcd"?ends_with("a")?c}
<#--判断 字符串的第一个字符是什么，返回的是布尔值 需要特殊处理-->
${"abcd"?starts_with("a")?c}
<#--判断 指定字符第一次出现的索引位置-->
${"abcaad"?index_of("a")}
<#--判断 指定字符最后一次出现的索引位置-->
${"abcda"?last_index_of("a")}
<#--指定字符串的长度5 若字符串的长度<5 则向字符串的左侧插入指定的字符串，默认插入空格-->
${"abcd"?left_pad(5,"1x")}
<#--指定字符串的长度5 若字符串的长度<5 则向字符串的右侧插入指定的字符串，默认插入空格-->
${"abcd"?right_pad(5,"1x")}
<#--字符串去除字符串两端的空格-->
${"   v abcd cc "?trim}
<#--以单词的 形式 分割字符串-->
<#list " we are chinese you no diao"?word_list as word>
    ${word}
</#list><#if word_has_next>,</#if>
?将特殊html标记进行转换,如<转换成<
?matches：是否匹配 一个正则
```

#### 日期相关操作

```python
# 获取当前时间
${.now?string('yyyyMMdd HHmmss')?replace(' ', 'T')}
${.now?string('yyyyMMddHHmmss')}
# 获取当前时间,精确到毫秒
${.now?string('yyyy-MM-dd HH:mm:ss.SSS')}
#当前日期+-天数;eg:当前时间后15天;
${(.now?long+1296000000)?number_to_datetime?string('yyyyMMddHHmmss')}
# eg2:
${((inputXML.Response.GetLabOrderListReturn.OrderLists.OrderList[0].OrderSttDat?date('yyyy-MM-dd'))?long+1296000000)?number_to_datetime?string('yyyyMMddHHmmss')}
# 字符串转日期时间
${data.creationTime?replace('T', ' ')?datetime('yyyyMMdd HHmmss')?string("yyyy-MM-dd HH:mm:ss")!}
# 字符串日期时间类型转换
${data.EnterDateTime?replace('-', '')?replace(' ', 'T')?replace(':', '')}
```

#### 数字处理

```python
# freemarker 中预订义了三种数字格式：number,currency（货币）和percent(百分比)其中number为默认的数字格式转换
eg：   
<#assign tempNum=20>
${tempNum}      
${tempNum?string.number} # 结果为20  
${tempNum?string.currency} # 结果为￥20.00  
${tempNum?string.percent} # 结果为2,000% 

#数字格式化插值可采用#{expr;format}形式来格式化数字,其中format可以是:
mX:小数部分最小X位
MX:小数部分最大X位
如下面的例子:
<#assign x=2.582/>
<#assign y=4/>
#{x; M2} <#-- 输出2.58 -->
#{y; M2} <#-- 输出4 -->
#{x; m2} <#-- 输出2.6 -->
#{y; m2} <#-- 输出4.0 -->
#{x; m1M2} <#-- 输出2.58 -->
#{x; m1M2} <#-- 输出4.0 -->


${num?number}  字符串转数字
```

#### 数值格式化

```java
<#assign num = 1234567.8>

${num?string('0.00')}
输出为：1234567.80
如果小数点后不足两位，用 0 代替

${num?string('#.##')}
输出为：1234567.8
如果小数点后多余两位，就只保留两位，否则输出实际值

${num?string(',###.00')}
输出为：1,234,567.80
整数部分每三位用 , 分割，并且保证小数点后保留两位，不足用 0 代替

${num?string(',###.##')}
输出为：1,234,567.8
整数部分每三位用 , 分割，并且小数点后多余两位就只保留两位，不足两位就取实际位数，可以不不包含小数点

<#assign num = 12.3>

${num?string('000.00')}
输出为：012.30
整数部分如果不足三位（000），前面用0补齐，否则取实际的整数位

${num?string('###.00')}
等价于
${num?string('#.00')}
输出为：12.30
整数取实际的位数

<#assign num = 12.345>

${num?string('#.##')}
输出为：12.35
```

#### 解析xml节点

```python
/*
消息体结构如下:
<message>
<cda>
<tid>test</tid>
</cda>
</message>
*/
取tid节点值
${inputXML.message.cda.tid!}
# 遍历xml节点
<#list inputXML.message.section as data>
${data.PatientID!}
</#list>
```

#### 解析xml里json数据

```python
# 1.在javascript中
var data = "<xml><![CDATA["+input[0].text+"]]></xml>";
next.setText(data ,"UTF8");

# 2.freemarker中使用函数解析json
<#assign data=parseJSON(tmp_data) />

# 取xml第1个节点
${inputXML.message.SourceAndScheduleInfo[0].SCHEDULE_DOCTOR_ID!}

# 排序后遍历json节点内容
<#list data.EXAM_APPLY?sort_by("DATA_ELEMENT_ID") as data>
			<#if data.DATA_ELEMENT_EN_NAME == 'LAB_APPLY_NO'>
			<LabNo>${data.DATA_ELEMENT_VALUE!}</LabNo>
			</#if>
</#list>

//判断数组存在且不为空
<#if d.LAB_RESULT_ASTS?? && (d.LAB_RESULT_ASTS?size > 0)>
</#if>
```

#### 使用查找表

```javascript
${LookupTables.get('mapping_use_drg').lookup('value_dh', data.UsageCode, 'value')!'默认值'}
// 使用默认配置文件中的值
${input.getProperty('ORG_CODE')!LookupTables.get("config").lookup("value", "org_code", "comment")}
// 是由xml xpath路径节点动态转换
${LookupTables.get('mapping_icd_10').lookup('value', tmp_data.DiagnoseCode, 'value_dh')!''}
```

#### 使用消息属性值

```python
# 获取消息的属性,默认值
${input.getProperty("msg_id")!'-1'}
```

#### 逻辑判断

```python
# 三元运算符
${(tmp.EMPI_ID == '')?string('0', tmp.EMPI_ID)}
# ?? 判断左侧的变量是否丢失
<#if !data.message.PATIENT?? && !data.message.PATIENT_BASE??>
	<#else>
		<SEARCH_CODE null="yes"/>
		<SEARCH_NAME null="yes"/>
		<PK_PATIENT null="yes"/>
		<EMPI_ID null="yes"/>
		<PATIENT_NAME null="yes"/>
</#if>
# 示例2
#?reverse使用同sort相同。reverse还可以同sort_by一起使用
<#list data.ORDER_OUTPAT?sort_by("DATA_ELEMENT_ID") as data>
		<#if data.DATA_ELEMENT_EN_NAME == 'PATIENT_ID'>
		<PatientId>${data.DATA_ELEMENT_VALUE!}</PatientId>
        <#break> 
		</#if>
		<#if data.DATA_ELEMENT_EN_NAME == 'ENCOUNTER_ID'>
		<IDNo>${data.DATA_ELEMENT_VALUE!}</IDNo>
		</#if>
</#list>

#case 用法
//字符串用法
<#switch being.size>  
  <#case "small">  
          This will be processed if it is small  
          <#break>  
  <#case "medium">  
          This will be processed if it is medium  
          <#break>  
  <#case "large">  
          This will be processed if it is large  
          <#break>  
  <#default>  
          This will be processed if it is neither  
</#switch>

//数字用法
<#switch x>  
  <#case 1>  
         1  
  <#case 2>  
         2  
  <#default>  
         d  
</#switch>

# if-else逻辑判断写法
<#if x == 1>
  x is 1
<#elseif x == 2>
  x is 4
<#else>
  x is not 1 nor 2 nor 3 nor 4
</#if>
```

#### 数组操作

```javascript
<#--声明一个序列，包含若干个元素--> 
<#assign order_status_is_not_neeed = ['U', 'D', 'C']> 
<#--使用seq_contains判断序列中的元素是否存在-->
eg1:
order_status_is_not_neeed?seq_contains(data.StatusCode)
eg2:
"U": ${x?seq_contains("U")?string("yes", "no")}
//输出 U:yes
//?size: 得到序列、数组的元素个数
//遍历序列
<#list status as order_status_is_not_neeed> 
    ${status}<br> 
</#list>　　
```

#### 带参数的宏

```javascript
宏是和某个变量关联的模板片断，以便在模板中通过用户定义指令使用该变量
宏的参数是局部变量，只能在宏定义中有效 
2.1：带一个参数的宏 
定义宏
<#macro greet person> 
    <font size="+2">Hello ${person}!</font> 
</#macro> 
调用宏   
<@greet person="test"/>  　　
2.2：带多个参数的宏 
<#macro greet person color> 
    <font size="+2" color="${color}">Hello ${person}!</font> 
</#macro> 
<@greet person="test" color="blue"/> 
   
2.3： 带参数缺省值的宏 
<#macro greet person color="blue"> 
    <font size="+2" color="${color}">Hello ${person}!</font> 
</#macro> 

<@greet person="test" color="yellow"/>  <br> 
<@greet person="test"/>
```

#### 正则表达式

```python
#字符串正则替换示例
${inputXML.PatOrdList.ClinicDisease?replace('\n', '<br>', 'r')?replace('"', '”')}
https://freemarker.sourceforge.io/docs/ref_builtins_string.html#ref_builtin_string_flags
```

