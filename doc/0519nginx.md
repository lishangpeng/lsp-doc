##### nginx 同一个地址区分根据不同的终端 返回不同的页面

```
location / {
	#如果终端是安卓或者ios转发到手机端的路径
	if ( $http_user_agent ~ "(iPhone)|(Android)"){
        rewrite ^/(.*)$ http://xxxx/phone/$1 break;
     }
     #PC端的路径
     rewrite ^/(.*)$  http://xxxx/pc/$1 break;
}

```

接下来可以根据/var/log/nginx下的error日志进行调配 配置

