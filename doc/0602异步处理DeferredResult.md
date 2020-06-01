Callable 和 DeferredResult 很相似。

Callable适用于单个服务调用的时候，在主线程中可以开启一个副线程。

DeferredResult 适用于多个服务之间，A服务开启主线程，等待B服务的处理回调。



```java
@Component
public class DeferredResultHolder {
	
	private Map<String, DeferredResult<String>> map = new HashMap<String, DeferredResult<String>>();

	public Map<String, DeferredResult<String>> getMap() {
		return map;
	}

	public void setMap(Map<String, DeferredResult<String>> map) {
		this.map = map;
	}
	
}
```

这是一个存放DeferredResult 的类，可以用nacos、eruka、zookeeper暴露服务给其他服务使用



```java
	@RequestMapping("/order")
	public DeferredResult<String> order() throws Exception {
		logger.info("主线程开始");
		
		String orderNumber = RandomStringUtils.randomNumeric(8);
		
		DeferredResult<String> result = new DeferredResult<>();
		deferredResultHolder.getMap().put(orderNumber, result);
		
        mockQueue.setPlaceOrder(orderNumber);
        
		return result;
		
	}
```

这是A服务，开始之后使用MQ向B服务发送了一个消息，并且把DeferredResult对象和orderId绑定在一起存到Map中



B服务接受到消息，开始处理，处理完成之后

```java
deferredResultHolder.getMap().get(orderNumber).setResult("订单处理完成");
```

setResult()方法调用之后，A服务会返回结果

大致的流程就是 A服务接受到请求 ->  mq通知B服务处理 -> B服务接受到消息 ->  处理完毕setResult -> A服务返回结果



```java
public class WebConfig extends WebMvcConfigurerAdapter {
    
	@Override
	public void configureAsyncSupport(AsyncSupportConfigurer configurer) {
        //注册一个拦截器，在异步的方法中
		configurer.registerCallableInterceptors();
        //设置异步调用的超时时间
		configurer.setDefaultTimeout();
        //设置异步调用的线程池
		configurer.setTaskExecutor();
		super.configureAsyncSupport(configurer);
	}
    
}
```

