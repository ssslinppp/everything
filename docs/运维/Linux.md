# Linux知识

## Linux启动流程
![启动流程](https://images2015.cnblogs.com/blog/99941/201606/99941-20160604163710258-1247432254.jpg)   

[Linux 开机引导和启动过程详解](https://linux.cn/article-8807-1.html)


![MBR](https://www.ibm.com/developerworks/cn/linux/l-linuxboot/fig2.gif)


- BIOS： 上点自检，会查找启动介质（DISK、USB等）
- MBR：主引导记录，包含`主引导加载程序(第一阶段)`：目的是查找并加载`次引导加载程序（第二阶段）`；
- GRUB： 安装在MBR的BootLoader中，加载内核，加载虚机文件系统(initramfs)，加载根文件系统的驱动程序；
- 内核引导： 根据GRUB中的配置，来加载内核
- Init阶段：内核主动调用`/sbin/init`进程，同时设置运行级别，3：多用户模式， 5：支持界面；
- Init执行`/etc/rc.d/rc.sysinit`： 准备基础环境
- Init执行`/etc/rc.d/rc.local`: 支持用户自定义脚本,用户个性化设置
- 启动阶段：