对于restful 接口进行校验

​	

```
/**
 * 处理所有接口数据验证异常
 */
@ExceptionHandler(MethodArgumentNotValidException.class)
public ResponseEntity<ApiError> handleMethodArgumentNotValidException(MethodArgumentNotValidException e){
    // 打印堆栈信息
    log.error(ThrowableUtil.getStackTrace(e));
    String[] str = Objects.requireNonNull(e.getBindingResult().getAllErrors().get(0).getCodes())[1].split("\\.");
    String message = e.getBindingResult().getAllErrors().get(0).getDefaultMessage();
    String msg = "不能为空";
    if(msg.equals(message)){
        message = str[1] + ":" + message;
    }
    return buildResponseEntity(ApiError.error(message));
}
```



使用@Valid注解 model中的相关校验可以查询文档

自定义校验注解的方法：

​	

```
@Target({ElementType.METHOD, ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
//java的一个校验注解
@Constraint(validatedBy = MyConstraintValidator.class)
public @interface MyConstraint {
	
	String message();

	Class<?>[] groups() default { };

	Class<? extends Payload>[] payload() default { };

}
```



```
public class MyConstraintValidator implements ConstraintValidator<MyConstraint, Object> {

	@Autowired
	//随便顶一个类
	private HelloService helloService;
	
	@Override
	public void initialize(MyConstraint constraintAnnotation) {
		System.out.println("my validator init");
	}

	@Override
	public boolean isValid(Object value, ConstraintValidatorContext context) {
		//自定义校验方式
		//true代表校验成功 flase代表校验失败
		helloService.greeting("tom");
		System.out.println(value);
		return false;
	}

}
```

