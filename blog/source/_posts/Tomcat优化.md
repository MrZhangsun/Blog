---
title: Tomcat优化
date: 2017-01-31 09:00:25
tags: Tomcat
---

#### Tomcat优化

1. Tomcat的优化方案:

          1.配置支持HTTPS协议
          2.配置NIO
          3.配置Tomcat压缩静态文件
          4.配置最大线程数
          5.配置线程池

2. Tomcat配置HTTPS方法:

            1.生成安全证书, keytool -genkeypair -alias "tomcat" -keyalg "RSA" -keystore "f:\tomcat.keystore"  
            2.配置tomcat
               <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"  
               maxThreads="150" scheme="https" secure="true"  
               clientAuth="false" sslProtocol="TLS"   
               keystoreFile="D:\Tools\Web\ssl\tomcat.keystore"  
               keystorePass="tomcat"  
               ciphers="tomcat"/>  