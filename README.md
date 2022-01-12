# 哪吒探针VPS监控搭建
脚本来源：https://github.com/naiba/nezha

准备工作
-------------
准备一个域名

注册一个Github的账户

准备一个VPS作为面板机器，并搭建好宝塔面板

解析域名
-------------------
建议把域名托管到cloudflare方便操作

一个开小云朵 作用：用于之后日常访问

一个不开小云朵  作用：小鸡和面板机的数据传输

注册Github
------------------
注册好后创建一个OAuth Apps

https://github.com/settings/developers 然后点击New OAuth App按钮

Github OAuth Apps填写方法
------------------------
Application name 这个填写名字 具体随你

Homepage URL
```
https://你开启小云朵的域名
```
Authorization callback URL
```
https://你开启小云朵的域名/oauth2/callback
```

填写OK后 点击
