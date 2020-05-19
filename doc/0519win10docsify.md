##### win10搭建docsify

​	win10 禁止脚本运行解决方案：

​	![image-20200519232849381](C:\Users\12149\AppData\Roaming\Typora\typora-user-images\image-20200519232849381.png)

set-ExecutionPolicy RemoteSigned

get-ExecutionPolicy



```
1.npm i docsify-cli -g  下载插件
2.创建一个文件 进入
3.docsify init ./ 初始化
4.docsify serve ./ 启动服务
```



_sidebar.md 侧边栏md文件

_coverpage.md 首页界面

index.html 中编写代码



https://docsify.js.org/ 文档地址