# 跨站请求伪造CSRF攻击

## 自解压文件csrf攻击

![image-20240707184024696](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240707184024696.png)

设置为自解压格式

![image-20240707184133874](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240707184133874.png)

自解压选项中设置解压后运行或者解压前运行

![image-20240707184417448](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240707184417448.png)

![image-20240707184439183](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240707184439183.png)

解压后打开百度页面 			//可以添加其他get请求网页完成其他操作

## discuz论坛csrf攻击实现拖库

### 部署discuz论坛项目

![image-20240707184858449](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240707184858449.png)

### 了解discuz项目

![image-20240707185141761](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240707185141761.png)

discuz管理中心可以进行数据备份

![image-20240707185318671](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240707185318671.png)

备份文件以sql命令的形式存储

### 实现csrf

![image-20240707185727563](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240707185727563.png)

使用burp抓包工具抓到提交数据备份请求的url

![image-20240707185843948](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240707185843948.png)

拿到url做一下修改

```
/dz/upload/uc_server/admin.php?m=db&a=operate&t=export&appid=1&backupdir=xxxx%26backupfilename%3Daaaa
```

然后使用创建的普通用户登录论坛发帖

![image-20240707190345610](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240707190345610.png)

图片地址可以是网络地址，比如

![image-20240707190719063](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240707190719063.png)

![image-20240707190701797](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240707190701797.png)

可以发现我们每次访问这个帖子都会解析我们传入的url

所以我们将上面构造好的url传入,高度宽度选择0

![image-20240707191031479](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240707191031479.png)

![image-20240707191114974](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240707191114974.png)

然后使用admin用户查看帖子，网站后台就会保存数据备份

![image-20240707192418286](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240707192418286.png)

然后我们浏览器访问数据备份的路径

![image-20240707192541011](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240707192541011.png) 

完成拖库



## scrf基于POST请求添加账号





## dvwa项目CSRF攻击

### low难度

![image-20240708132034397](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240708132034397.png)

使用burp抓取修改密码的包

![image-20240708132143961](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240708132143961.png)

发现是GET请求

使用burp工具生成CSRF poc

![image-20240708132356626](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240708132356626.png)

放在服务器中，访问

![image-20240708132650246](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240708132650246.png)

如果浏览器登陆着DVWA点击按钮将成功修改密码为 `liucm`

![image-20240708132816319](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240708132816319.png)

提示密码修改成功

### medium难度

![image-20240708134502277](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240708134502277.png)

查看源代码发现使用referer消息头进行防御

这样我们使用之前的方法是不行的

referer消息头指向跳转之前的url,与host消息头对比

![image-20240708135522501](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240708135522501.png)

如果我们是从其它站点执行POC则会对比失败导致修改密码不成功

HOST消息头内容为 `192.168.137.131` 指向的是DVWA服务器地址

referer中要包含HOST中的信息

修改poc名字使路径中含有 `192.168.137.131`

![image-20240708140549173](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240708140549173.png)

修改成功

// 内网访问中referer消息头中只包含请求来源的ip地址，CSRF攻击大多数处于公网环境中







### high难度

![image-20240708141039234](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240708141039234.png)



这个难度中，访问服务器时，服务器会随机生成一个token将它与响应包一起发送给客户端

![image-20240708141904323](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240708141904323.png)

每次请求返回的token都不一样

用户执行修改密码操作时会对比token，token相同时修改成功

我们的思路时修改密码时提前访问服务器获取token，携带token发送给服务器，使攻击成功

在burp中可以使用插件完成此功能

![image-20240708142441498](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240708142441498.png)

![image-20240708142542454](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240708142542454.png)

密码修改成功

![image-20240708142618047](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240708142618047.png)

每次执行poc插件中的token值都会发生改变





但是这样的攻击方式并不好用，因为攻击方很难抓到客户端发送的包，所以我们可以用js代码实现这一功能

受害者触发js脚本时我们重新访问页面获取token，然后修改密码

![image-20240708150029283](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240708150029283.png)

访问包含js脚本的html文件，完成攻击

















































