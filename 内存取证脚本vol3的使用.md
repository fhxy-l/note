# 内存取证脚本vol的使用

格式

`python vol.py -f [文件名] [使用插件]`

插件windows.info		（列出系统基本信息）



![Screenshot_2024-04-25_07_43_17](C:\Users\刘春明\Desktop\Screenshot_2024-04-25_07_43_17.png)

windows.pstree		(查看进程列表)

![image-20240425194810660](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240425194810660.png)

windows.cmdline			(进程命令行参数)

![image-20240425194922205](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240425194922205.png)

常用插件

```
    layerwriter：列出内存镜像platform信息
    linux.bash：从内存中恢复bash命令历史记录
    linux.check_afinfo：验证网络协议的操作功能指针
    linux.check_syscall：检查系统调用表中的挂钩
    linux.elfs：列出所有进程的所有内存映射ELF文件
    linux.lsmod：列出加载的内核模块
    linux.lsof：列出所有进程的所有内存映射
    linux.malfind：列出可能包含注入代码的进程内存范围
    linux.proc：列出所有进程的所有内存映射
    linux.pslist：列出linux内存映像中存在的进程
    linux.pstree：列出进程树
    mac.bash：从内存中恢复bash命令历史记录
    mac.check_syscall：检查系统调用表中的挂钩
    mac.check_sysctl：检查sysctl处理程序的挂钩
    mac.check_trap_table：检查trap表中的挂钩
    mac.ifconfig：列出网卡信息
    mac.lsmod：列出加载的内核模块
    mac.lsof：列出所有进程的所有内存映射
    mac.malfind：列出可能包含注入代码的进程内存范围
    mac.netstat：列出所有进程的所有网络连接
    mac.psaux：恢复程序命令行参数
    mac.pslist：列出linux内存映像中存在的进程
    mac.pstree：列出进程树
    mac.tasks：列出Mac内存映像中存在的进程
    windows.info：显示正在分析的内存样本的OS和内核详细信息
    windows.callbacks：列出内核回调和通知例程
    windows.cmdline：列出进程命令行参数
    windows.dlldump：将进程内存范围DLL转储
    windows.dlllist：列出Windows内存映像中已加载的dll模块
    windows.driverirp：在Windows内存映像中列出驱动程序的IRP
    windows.driverscan：扫描Windows内存映像中存在的驱动程序
    windows.filescan：扫描Windows内存映像中存在的文件对象
    windows.handles：列出进程打开的句柄
    windows.malfind：列出可能包含注入代码的进程内存范围
    windows.moddump：转储内核模块
    windows.modscan：扫描Windows内存映像中存在的模块
    windows.mutantscan：扫描Windows内存映像中存在的互斥锁
    windows.pslist：列出Windows内存映像中存在的进程
    windows.psscan：扫描Windows内存映像中存在的进程
    windows.pstree：列出进程树
    windows.procdump：转储处理可执行映像
    windows.registry.certificates：列出注册表中存储的证书
    windows.registry.hivelist：列出内存映像中存在的注册表配置单元
    windows.registry.hivescan：扫描Windows内存映像中存在的注册表配置单元
    windows.registry.printkey：在配置单元或特定键值下列出注册表项
    windows.registry.userassist：打印用户助手注册表项和信息
    windows.ssdt：列出系统调用表
    windows.strings：读取字符串命令的输出，并指示每个字符串属于哪个进程
    windows.svcscan：扫描Windows服务
    windows.symlinkscan：扫描Windows内存映像中存在的链接
```

例题

帕鲁杯应急响应27题

27.请对办公区留存的镜像取证并指出内存疑似恶意进程

直接列出进程windows.pslist发现.hack.ex

![image-20240425195927028](C:\Users\刘春明\AppData\Roaming\Typora\typora-user-images\image-20240425195927028.png)

判断为恶意程序