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

填写OK后 点击注册申请

生成Github登录密钥
-----------------
![image](https://user-images.githubusercontent.com/94978556/149084777-16456710-700e-48ac-9146-8fbb6aa547dd.png)

Client ID  记录下来

Client secrets  点击旁边生成按钮  然后记录下来

部署哪吒面板
---------------------
脚本如下：
```
curl -L https://raw.githubusercontent.com/naiba/nezha/master/script/install.sh -o nezha.sh && chmod +x nezha.sh && ./nezha.sh
```

**选择1开始安装面板**（安装好后部署宝塔面板）**

宝塔面板搭建
---------------------------
脚本如下：
```
wget -O install.sh http://download.yu.al/install/install-ubuntu_6.0.sh && sudo bash install.sh
```
**我这里使用的是一个破解版的宝塔  需要正版自己去官网找安装脚本**

**安装好后登录宝塔面板（登录账号什么的后期自己改 我就不多讲了）**

**登录好后安装插件  只需要安装一个Nginx**

**我安装的版本是Nginx 1.18.0**

宝塔添加站点
--------
**域名填写你解析的带小云朵那个  然后直接提交**

![image](https://user-images.githubusercontent.com/94978556/149086619-17bac46e-03fc-4ee7-bfdf-6ac5c7a808d7.png)

**点击域名使用宝塔SSL证书申请  选择Let's Encrypt然后文件认证  选择你的域名申请就好了**

![image](https://user-images.githubusercontent.com/94978556/149086948-184aeb30-c321-493d-aa9a-bcb6bdbd2ffd.png)

**然后创建一个反向代理  名字尽量英文（不然会出错）目标URL填写带小云朵那个域名  然后提交

![image](https://user-images.githubusercontent.com/94978556/149087855-f679739b-a551-4534-aadf-42c8ed511a10.png)

配置反代文件
------------------
**点击配置文件  删除原来所有的文件  粘贴以下代码保存**

![image](https://user-images.githubusercontent.com/94978556/149088007-32977ee9-c813-4913-87a0-a246cc39dc16.png)
```
location /
{
    proxy_pass http://127.0.0.1:8008;
    proxy_set_header Host $host;
}
location /ws
{
    proxy_pass http://127.0.0.1:8008;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
    proxy_set_header Host $host;
}
location /terminal
{
    proxy_pass http://127.0.0.1:8008;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
    proxy_set_header Host $host;
}
```
访问站点设置机器
------------------------
**用你带小云朵的域名直接访问  点击授权登录**

![image](https://user-images.githubusercontent.com/94978556/149088730-0f5f71e2-72a6-4be6-aeec-49d383bc104f.png)

**登录好后进后台管理**

![image](https://user-images.githubusercontent.com/94978556/149088982-735e80e4-dfba-4afd-8848-c76a922f804f.png)

**进入后台添加主机  名称随你  分组填你VPS地区  然后直接添加就好了**

![image](https://user-images.githubusercontent.com/94978556/149089285-b2d205b0-c429-4c08-a558-cec4abbc19c5.png)

**添加好后  复制密钥**

![image](https://user-images.githubusercontent.com/94978556/149089709-fafde35c-a238-4f88-b4c6-4b753b139b9a.png)

被监控机的安装
---------------------
脚本如下：
```
curl -L https://raw.githubusercontent.com/naiba/nezha/master/script/install.sh -o nezha.sh && chmod +x nezha.sh && ./nezha.sh
```
**选择8安装监控Agent**

![image](https://user-images.githubusercontent.com/94978556/149090110-f757282a-8c50-4614-803e-7d2beb52687f.png)

**现在填入你没有开启小云朵的域名**

![image](https://user-images.githubusercontent.com/94978556/149090320-6fa03fec-3403-498c-b3e3-3950291e3955.png)

**回车键后 面板RPC端口回车默认**

**Agent 密钥 填写 哪吒面板生成的密钥**

![image](https://user-images.githubusercontent.com/94978556/149090777-2d09ac52-4d43-4ccd-b0ed-3c0e171a8ae5.png)

**然后返回前端应该就能看到你的VPS状态了**

![image](https://user-images.githubusercontent.com/94978556/149091339-fc44c018-5cf2-43ff-b096-b7d979b9b321.png)

**其他VPS添加同理**

TG机器人离线通知
-------------------
**先申请一个机器人 @Botfather 记住密钥**

**然后获取自己TG的数字ID  @userinfobot 记住数字**

**进入面板机后台，报警——先添加通知方式 如下 记得修改我备注的**

```
https://api.telegram.org/bot你的机器人密钥/sendMessage?chat_id=你的数字ID&text=#NEZHA#
```
**添加报警规则  备注的记得修改成你想要几秒**
```
[{"Type":"offline","Duration":多少秒随你}]
```
至此整个搭建结束
---------------------------------------
