由于dubbo采用了docker中得ip 

我采用得方法是使用宿主机得网络，可能会出现端口得冲突，需要修改端口

网上还有修改host 我没试过

在dockerFile得环境中写上docker得注册地址，我试了 没用

```docker
docker build -t hty-wechat .
&& docker run
-p 8100:8100 -p 20110:20110
--name hty-wechat
-v /home/appforwechat/:/home/appforwechat/
--cap-add SYS_PTRACE
--net host
hty-wechat 
```

这是我发布项目得命令 主要是在后面加上了--net host

亲测可用，如果你不好使 就看下两个服务器得dubbo端口是否是通的