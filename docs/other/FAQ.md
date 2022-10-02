## FAQ-常见问题

## 访问 https 接口时经常会返回 证书不信任错误

```http
Unable to execute HTTP request: sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

###### 解决方法

- 通过浏览器下载对方证书

- 导入证书到 rhapsody

  ```python
  # 复制证书文件（langchao.cer）到 C:\Program Files\Rhapsody\Rhapsody Engine 6\jre\lib\security
  # 复制验证文件（SSLPoke.class）到 C:\Program Files\Rhapsody\Rhapsody Engine 6\jre\bin
  # cmd 管理员进入rhapsody服务安装目录 C:\Program Files\Rhapsody\Rhapsody Engine 6\jre\bin
  # 执行 keytool -import -alias langchao -keystore ../lib/security/cacerts -file ../lib/security/langchao.cer -trustcacerts
  
  # 验证 
  java SSLPoke 10.254.6.56 443 （接口服务器地址 端口）
  ```

  工具SSLPoke.class下载,请[点击此连接](/docs-note-rhapsody/assets/file/SSLPoke.class)

![操作截图](/docs-note-rhapsody/assets/images/image-20211011074403485.png)

## 访问https 提示ssl协议握手失败

原因是rhapsody支持的SSL协议有限,可能会部分不支持

###### 解决方法

- 把对方使用的ssl加密协议加到rhapsody的配置文件对于的ssl加密套件中

