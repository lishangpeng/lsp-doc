dubbo官网上对dubbo注解方式的实现介绍的比较，比较多的是关于xml的配置。

我在这里写一下我的用法

```JAVA
@Reference(timeout = 3000,check = false,methods = {@Method(name = "cancelTradeOrder")})
private AppOrderMiddle appOrderMiddle;
```

这个注解 等于

```
<dubbo:reference interface="com.xxx.XxxService" check="false">
    <dubbo:method name="findXxx" timeout="3000" />
</dubbo:reference>
```

注入appOrderMiddle并且控制到方法级别，cancelTradeOrder这个方法，超时的时间为三秒，启动的时候没有提供者不抛出异常，避免两个服务相互依赖的时候启动报错

```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by Fernflower decompiler)
//

package org.apache.dubbo.config.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD, ElementType.METHOD, ElementType.ANNOTATION_TYPE})
public @interface Reference {
    Class<?> interfaceClass() default void.class;

    String interfaceName() default "";

    String version() default "";

    String group() default "";

    String url() default "";

    String client() default "";

    boolean generic() default false;

    boolean injvm() default true;

    boolean check() default true;

    boolean init() default false;

    boolean lazy() default false;

    boolean stubevent() default false;

    String reconnect() default "";

    boolean sticky() default false;

    String proxy() default "";

    String stub() default "";

    String cluster() default "";

    int connections() default 0;

    int callbacks() default 0;

    String onconnect() default "";

    String ondisconnect() default "";

    String owner() default "";

    String layer() default "";

    int retries() default 2;

    String loadbalance() default "";

    boolean async() default false;

    int actives() default 0;

    boolean sent() default false;

    String mock() default "";

    String validation() default "";

    int timeout() default 0;

    String cache() default "";

    String[] filter() default {};

    String[] listener() default {};

    String[] parameters() default {};

    String application() default "";

    String module() default "";

    String consumer() default "";

    String monitor() default "";

    String[] registry() default {};

    String protocol() default "";

    String tag() default "";

    Method[] methods() default {};

    String id() default "";
}

```

这个注解中还有很多参数

http://dubbo.apache.org/zh-cn/docs/dev/coding.html

可以在官网中找到对应的参数，不过官网上介绍的是schema的配置，但是名字差不多



协议相关的配置

http://dubbo.apache.org/zh-cn/docs/user/references/protocol/introduction.html



```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by Fernflower decompiler)
//

package org.apache.dubbo.config.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE})
@Inherited
public @interface Service {
    Class<?> interfaceClass() default void.class;

    String interfaceName() default "";

    String version() default "";

    String group() default "";

    String path() default "";

    boolean export() default true;

    String token() default "";

    boolean deprecated() default false;

    boolean dynamic() default true;

    String accesslog() default "";

    int executes() default 0;

    boolean register() default true;

    int weight() default 0;

    String document() default "";

    int delay() default 0;

    /** @deprecated */
    String local() default "";

    String stub() default "";

    String cluster() default "";

    String proxy() default "";

    int connections() default 0;

    int callbacks() default 1;

    String onconnect() default "";

    String ondisconnect() default "";

    String owner() default "";

    String layer() default "";

    int retries() default 2;

    String loadbalance() default "random";

    boolean async() default false;

    int actives() default 0;

    boolean sent() default false;

    String mock() default "";

    String validation() default "";

    int timeout() default 0;

    String cache() default "";

    String[] filter() default {};

    String[] listener() default {};

    String[] parameters() default {};

    String application() default "";

    String module() default "";

    String provider() default "";

    String[] protocol() default {};

    String monitor() default "";

    String[] registry() default {};

    String tag() default "";

    Method[] methods() default {};
}

```

注册服务的注解