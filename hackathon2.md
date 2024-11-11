# hackathon2

首先使用nmap扫描目标

![image-20240801181421471](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240801181421471.png)

21端口开放了ftp服务

80端口开放了http服务

ftp可以匿名登录 `anonymous`

![image-20240801181710805](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240801181710805.png)

其中有两个文件，word.dir是一个密码字典

但是不知道用户名

查看http服务

![image-20240801181920323](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240801181920323.png)

这样的

进行目录扫描

![image-20240801182016982](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240801182016982.png)

扫描出了 `happy`路径

访问

![image-20240801182129868](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240801182129868.png)

源代码里有用户名

使用 `hydra` 进行密码爆破

`hydra -l hackathonll -P word.dir -vV -f ssh://192.168.137.133:7223`

爆破出密码为![image-20240801182358739](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240801182358739.png)

ssh登录

` ssh hackathonll@192.168.137.133 -p 7223`

尝试提权

发现可以以root权限运行vim

![image-20240801182541351](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240801182541351.png)

使用vim进行提权

![image-20240801182648251](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240801182648251.png)

提权成功

