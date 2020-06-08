![image-20200607232439652](..\doc\SpringSecurityResource.assets\image-20200607232439652.png)

认证的处理流程



![image-20200607233443115](..\doc\SpringSecurityResource.assets\image-20200607233443115.png)

认证结果如何在多个请求中共享



![image-20200607233831154](..\doc\SpringSecurityResource.assets\image-20200607233831154.png)

黄色过滤器的作用：进来时有检查Session有认证信息就放到当前的线程中，出来的时候检查线程中有认证信息就放到session中



![image-20200607234313161](..\doc\SpringSecurityResource.assets\image-20200607234313161.png)