#### Rhapsody api操作

##### 请求头配置

- Accept:application/json

- Content-Type: application/json

- Authorization:Basic emhpbWluZzp6aGltaW5n

  说明:需要使用拥有restful权限的用户方可进行调用,默认administrator组没有该权限,默认用户administrator有最高权限

  ```python
  # 使用base64进行加密,emhpbWluZzp6aGltaW5n 代表zhiming:zhiming
  Authorization: Basic emhpbWluZzp6aGltaW5n
  # 如果使用POST需要添加X-CSRF-Token,可以通过get请求时得response header得到
  X-CSRF-Token: bH+ZcY9oB1+ZmfZDggWJ03Gex+tENImbqqTEbjQ92YH3g7teW+umHi8bLX4VQQMy32L1e5S0Y32A0DWz/y1fyQ==
  ```
  GET示例

  ```
  GET https://localhost:8444/api/statistics/diskspace
  Accept: application/json
  Content-Type: application/json
  Authorization: Basic YWRtaW5pc3RyYXRvcjphZG1pbg==
  ```

  POST示例

  ```
  POST https://localhost:8444/api/statistics/diskspace
  Accept: application/json
  Content-Type: application/json
  Authorization: Basic YWRtaW5pc3RyYXRvcjphZG1pbg==
  X-CSRF-Token: bH+ZcY9oB1+ZmfZDggWJ03Gex+tENImbqqTEbjQ92YH3g7teW+umHi8bLX4VQQMy32L1e5S0Y32A0DWz/y1fyQ==
  
  {
    "startTime":"2015-08-17T16:01:00",
    "endTime":"2015-08-17T17:01:00",
    "samplingResolution":"PT10M"
  }
  ```

#### 示例

##### 同步配置

###### 下载需要同步的配置

```http
GET https://172.16.33.174:8444/admin/config
Accept: application/json
Accept: application/vnd.orchestral.rhapsody.6_2_2+zip
Authorization: Basic YWRtaW5pc3RyYXRvcjphZG1pbg==
```

![下载需要同步的配置](/docs-note-rhapsody/assets/images/微信截图_20220513174942.png){ align=left }

###### 更新配置

获取**X-CSRF-Token**,通过下面的请求得到响应中的**X-CSRF-Token**

```http
GET https://172.16.33.188:8444/api/info
Accept: application/json
Authorization: Basic YWRtaW5pc3RyYXRvcjphZG1pbg==
```

![获取X-CSRF-Token](/docs-note-rhapsody/assets/images/微信截图_20220513175622.png)

提交配置文件

```http
POST https://172.16.33.188:8444/admin/config?commitComment='更新内容'
Content-Type: application/zip
Content-Type: application/vnd.orchestral.rhapsody.6_2_2+zip
X-CSRF-Token: NlSbnWZAzHDSUrZEmlY5xDoI+fRP49h+RCxx6mdSZFRGEuHRthY/Pl7mCUw2kyF0hy3VCXWEoOIntrBR1VJBNg==
Authorization: Basic YWRtaW5pc3RyYXRvcjphZG1pbg==

# 附件zip二进制文件
```

![提交配置文件](/docs-note-rhapsody/assets/images/微信截图_20220513175925.png)

###### 开放对应的权限

```js
# 需要以下权限;建议使用administrator的角色权限,该权限赋予所有权限
Log in to Rhapsody IDE.
Make changes with IDE.
Clear configurations.
Load configurations.
Load configuration REST API.
```

