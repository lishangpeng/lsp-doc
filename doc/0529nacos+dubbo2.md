```java
<!-- https://mvnrepository.com/artifact/org.apache.dubbo/dubbo-registry-nacos -->
<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo-registry-nacos</artifactId>
    <version>2.7.3</version>
</dependency>


<!-- Keep latest Nacos client version -->
<!-- https://mvnrepository.com/artifact/com.alibaba.nacos/nacos-client -->
<dependency>
    <groupId>com.alibaba.nacos</groupId>
    <artifactId>nacos-client</artifactId>
    <version>1.1.1</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.apache.dubbo/dubbo -->
<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo</artifactId>
    <version>2.7.3</version>
</dependency>


<!-- Alibaba Spring Context extension -->
<dependency>
    <groupId>com.alibaba.spring</groupId>
    <artifactId>spring-context-support</artifactId>
    <version>1.0.2</version>
</dependency>
```

升级了版本 之前遇到com.alibaba 的dubbo 2.6.5 和 nacos-client 0.5.0以上的版本会有 缺少jar包的报错

现在采用了org.apache 开源的dubbo 比较稳定的2.7.3的版本 目前比较新的是2.7.7的版本



遇到关于序列化的问题，服务方的异常无法被捕捉，只能捕捉到一个未定义的异常

解决方案

```java
dubbo:
  application:
    id: htyWechat
    name: htyWechat
    qosEnable: true
    qosPort: 33333
    qosAcceptForeignIp: false
  registry:
    address: nacos://xxxxxxx:8848
  protocol:
    name: dubbo
    port: 20110
    serialization: hessian2
#指定了序列化的方式

```

引入了同个包名的异常。



就这，浪费了我几个小时的时间....文档上的都比较老，github上也没有建议运用哪些版本，我就只是想用nacos当一下注册中心呀...需要引入这个多maven，版本不对还会各种报错...害



