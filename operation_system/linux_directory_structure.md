#### bin(Essential User Binaries)

包含了一些必需的二进制程序，例如bash shell

#### boot(Static Boot Files)

包含了引导系统所需要的文件，例如，GRUB引导的文件以及linux内核

#### cdrom(Historical Mount Point for CD-ROMs)

非FHS标准，如Ubuntu中可以看到，挂载的CD-ROMs会放到这个目录下。在FHS标准中是放在media目录下

#### dev(Device Files)

linux把所有设备看作文件，如，/dev/sda就代表第一个SATA驱动。/dev/null是一个特殊的设备，所有的输入都会被丢弃

#### etc(Configuration Files)

包含系统级配置文件，用户级配置文件在各用户的home目录下

#### home(Home Folders)

用户目录

#### lib(Esstential Shared Libraries)

包含必要的共享库文件

#### lost+found(Recovered Files)

包含用于系统恢复的文件

#### media(Removable Media)

包含可移除的设备，如，cd

#### mnt(Temporary Mount Points)

临时挂载点

#### opt(Optional Packages

可选包，通常用于专用的不遵守标准文件系统结构的软件，如，一个专有程序在安装的时候把它的文件放到/opt/application中

#### proc(Kernel & Process Files

包含指定的代表系统和进程信息的文件

#### root(Root Home Deirectory)

root用户的用户目录

#### run(Application State Files)

用于应用保存临时文件，例如，socket, processID。tmp下可能会被删除

#### sbin(System Administration Binaries)

包含必要的用于root用户进行系统管理的二进制文件

#### selinux(SELinux Virtual File System)

如果系统用SELinux作安全防御，这个目录给SELinux使用

#### srv(Service Data)

包含“系统提供的用于服务的数据”，如果有一个Apache HTTP 服务来提供Web服务，通常会把网站文件放到这个目录下

#### tmp(Temporary Files)

保存临时文件，在重启系统或者被一些清理程序执行的时候被删除

#### usr(User Binaries & Read-Only Data)

用于保存用户级的应用和文件，如，不是必须的应用被保存在/usr/bin，而不是在/bin下；非必需的系统管理程序保存在/usr/sbin，而不是在/sbin下。

#### var(Variable Data Files))

和usr相对的保存可写的程序文件，如，日志文件

## 引用

1. [HowToGeek: The Linux Directory Structure, Explained](https://www.howtogeek.com/117435/htg-explains-the-linux-directory-structure-explained/)