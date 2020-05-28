

docker下安装单机模式的nacos



```
docker pull nacos/nacos-server
#运行命令
docker run --env MODE=standalone --name nacos -d -p 8848:8848 nacos/nacos-server
```





springmvc配置dubbo 注册中心为nacos

```java
<!-- Dubbo Nacos registry dependency -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>dubbo-registry-nacos</artifactId>
    <version>0.0.2</version>
</dependency>

<!-- https://mvnrepository.com/artifact/com.alibaba.nacos/nacos-client -->
<!-- 0.6以上的版本和我mvc的版本有报错，有一个Jar不存在 -->
<dependency>
    <groupId>com.alibaba.nacos</groupId>
    <artifactId>nacos-client</artifactId>
    <version>0.5.0</version>
</dependency>

<!-- Dubbo dependency -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>dubbo</artifactId>
    <version>2.6.5</version>
</dependency>

<!-- Alibaba Spring Context extension -->
<dependency>
    <groupId>com.alibaba.spring</groupId>
    <artifactId>spring-context-support</artifactId>
    <version>1.0.2</version>
</dependency>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
       http://dubbo.apache.org/schema/dubbo
       http://dubbo.apache.org/schema/dubbo/dubbo.xsd
       http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

  <!-- 这块是导入了配置信息,区分prod和dev中的nacos版本 -->
    <bean id="placeholderConfigurer" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
        <property name="order" value="3"/>
        <property name="ignoreUnresolvablePlaceholders" value="true"/>
        <property name="locations">
            <list>
                <value>classpath:profile/jdbc.properties</value>
            </list>
        </property>
    </bean>

    <!-- 提供方应用信息，用于计算依赖关系 -->
    <dubbo:application name="dubbo-provider-hty"/>

    <!-- 使用 Nacos 注册中心 -->
    <dubbo:registry address="${nacos}"/>

    <!-- 用dubbo协议在20880端口暴露服务 -->
    <dubbo:protocol name="dubbo" port="20880"/>

    <!-- 声明需要暴露的服务接口 -->
    <dubbo:service interface="com.onway.middle.app.AppOrderMiddle" ref="orderService"/>

    <!-- 和本地bean一样实现服务 -->
    <bean id="orderService" class="com.onway.middle.app.impl.AppOrderMiddleImpl"/>

</beans>

```

xml文件

启动nacos 就能在服务列表中看见服务了



配置一个springboot的消费者，因为我的另一个项目是springboot2.0以上的版本

```java
<!-- Dubbo Nacos registry dependency -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>dubbo-registry-nacos</artifactId>
    <version>0.0.2</version>
</dependency>

<!-- Keep latest Nacos client version -->
<dependency>
    <groupId>com.alibaba.nacos</groupId>
    <artifactId>nacos-client</artifactId>
    <version>0.5.0</version>
</dependency>

<!-- Dubbo dependency -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>dubbo</artifactId>
    <version>2.6.5</version>
</dependency>

<!-- Alibaba Spring Context extension -->
<dependency>
    <groupId>com.alibaba.spring</groupId>
    <artifactId>spring-context-support</artifactId>
    <version>1.0.2</version>
</dependency>
```

```java
package com.huitongyi.manage.appforweachat.conf;

import com.alibaba.dubbo.config.spring.context.annotation.EnableDubboConfig;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;

@EnableDubboConfig
@Configuration
@ConfigurationProperties(prefix = "dubbo")
public class DubboConfiguration {

}
```

配置类



```java
dubbo:
  application:
    id: htyWechat
    name: htyWechat
    qosEnable: true
    qosPort: 33333
    qosAcceptForeignIp: false
  registry:
    address: nacos://xxxxxx:8848
  protocol:
    name: dubbo
    port: 20110
```

配置文件

	qosEnable: true
	qosPort: 33333
	qosAcceptForeignIp: false
	是因为我启动了两个dubbo项目冲突了 默认是22222端口


启动类添加注解

```java
@EnableDubbo(scanBasePackages = "com.huitongyi.manage.appforweachat.service")
```

service注解

```
@Service 使用alibaba的
@Component
```

服务就注册上去了



```
@Reference 调用服务的注解
```