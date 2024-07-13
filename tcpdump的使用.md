# tcpdump的使用

应用场景：linux

## 过滤规则

### 基于IP地址进行过滤：host

```shell
tcpdump host ip
```



### 指定源 or 目的地址进行过滤：src or dst

```
tcpdump dst ip
```



### 基于网段进行过滤：net

```
tcpdump net ip网段
```



### 基于端口过滤：port

```
tcpdump tcp[udp] port 80
```

同时抓取两个或者多个端口的流量

```
tcpdump port 80 or port 22
tcpdump portrange 80-8080
```

### 基于协议过滤

```
tcpdump [协议]
```

应用层协议需要结合 `port`进行过滤

```
tcpdump port http
```

### 基于包大小进行过滤

```
tcpdump less 32				抓取小于32的流量报文
tcpdump greater 100			抓取大于100的流量报文
```



## 常用参数

### 指定网卡进行监听：-i

```
tcpdump -i eth0[any]
```

默认监听第一块网卡

参数any监听所有网卡

### 将捕获的流量保存到文件：-w

```
tcpdump -w test.pcap
```

保存后可以使用可视化软件分析（鲨鱼）

### 读取文件中的流量：-r

```
tcpdump -r test.pcap
```

### -n -nn -N

```
-n			不把ip转化成域名或主机名直接显示ip
-nn			不把协议与端口号转化成协议名
-N 			不打印host域名部分
```

### 与时间相关参数

```
-t			输出时不输出时间
-tt			输出时输出时间戳
-ttt		输出流量间的时间间隔
-tttt		输出时在前面添加日期输出
```

### -v -vv -vvv

```
-v			输出的更加详细
-vv			比-v更加详细
-vvv		比-vv更加详细
```

### 指定抓包数量: -c -C

```
tcpdump -c 20 		抓满20个包结束
tcpdump -C [文件大小默认1MB] -W最多写入几个文件 -w [文件名]
```

如果上一个文件写满了-C指定的大小则写入下一个文件

### 根据出入方向抓取流量：-Q

```
tcpdump -Q in		入
tcpdump -Q out		出
tcpdump -Q inout	入+出
```

### 指定捕获的长度：-s

单位是byte,如果超过了设定的大小就会被截断，打印行会出现[|proto]标识

```
tcpdump -s 0 			使用默认262144字节
```

### 读取流量内容：-A -X

```
tcpdump -A -r test.pcap			以ASCII格式打印所有分组并且读取此文件，可以结合grep过滤内容

tcpdump -X -r test.pcap			可以同时使用十六进制和ASCII打印	（跟鲨鱼差不多）
-X -A 不能同时使用
```

### 指定过滤规则文件：-F

```
tcpdump -F filename 			将过滤规则放在一个文件中方便以后使用，命令行上的规则会被忽略
```

-e：在输出行打印出数据链路层的头部信息（源和目的MAC地址以及VLAN tag信息）

-q: 简洁输出







































