# Rhapsody api操作

###### 请求头配置

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
  ###### GET示例

  ```
  GET https://localhost:8444/api/statistics/diskspace
  Accept: application/json
  Content-Type: application/json
  Authorization: Basic YWRtaW5pc3RyYXRvcjphZG1pbg==
  ```

  ###### POST示例

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

###### 开放对应的项目locker等权限

- ![增加locker范围](D:\BaiduNetdiskWorkspace\rhapsody\latest\imgs\image-20210324131512953.png)
- ![开放对应的其他权限范围](\imgs\image-20211011075430238.png)
- 
