需要写一个过滤器 实现OncePerRequestFilter

重写doFilterInternal方法

```java
	@Override
	protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain chain) throws ServletException, IOException {
		//获取手机验证或者网页验证
        
        try{
           //进行校验
           
        }catch(ValidateCodeException exception){
            //失败
            authenticationFailureHandler.onAuthenticationFailure(request, response, exception);
            return;
        }
        chain.doFilter(request, response);
    }
```

自定义异常ValidateCodeException继承AuthenticationException 

```java
public class ValidateCodeException extends AuthenticationException {


	private static final long serialVersionUID = -7285211528095468156L;

	public ValidateCodeException(String msg) {
		super(msg);
	}

}
```

自定义失败处理 继承SimpleUrlAuthenticationFailureHandler

```java
	@Override
	public void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response,
			AuthenticationException exception) throws IOException, ServletException {
		
		logger.info("登录失败");
        //判断是跳转到页面 还是 返回json
		if (LoginResponseType.JSON.equals(securityProperties.getBrowser().getLoginType())) {
			response.setStatus(HttpStatus.INTERNAL_SERVER_ERROR.value());
			response.setContentType("application/json;charset=UTF-8");
			response.getWriter().write(objectMapper.writeValueAsString(new SimpleResponse(exception.getMessage())));
		}else{
			super.onAuthenticationFailure(request, response, exception);
		}
	}
```

