## Linux 文件基本操作

>### Linux 目录
* Linux `一切皆文件`
    * `根目录`
        * `/bin`
            * 存放可执行的二进制文件（绿色）
            * 常用的命令都存放在此目录，如 `mv`，`ls`
        * `/etc`
            * 存放系统管理、配置文件（.conf）
        * `/home`
            * 每个用户文件的根目录，是用户主目录的基点
        * `/usr`
            * 系统应用目录
            * `/usr/local`
                * 管理员安装软件的目录
            * `/usr/bin`
                * 安装软件的二进制文件
            * `/usr/etc`
                * 用户安装软件的配置目录
            * `/usr/lib`，`/usr/include` 
                * 用户安装软件的文件和动态库
        * `/opt`
            * 额外安装的可选应用程序包所放置的位置
        * `/root`
            * 超级用户（系统管理员）的主目录
        * `/sbin`
            * 存放二进制可执行文件，`只有 root 可以访问`
        * `/proc`（process）
            * 虚拟文件系统目录，是系统内存的映射
            * 可以通过此目录访问系统中所有信息，包括 `CPU` 信息（cpuinfo），内存信息（meminfo）
            * 其中以数字命名的目录代表进程的目录
        * `/dev`
            * 设备目录，存放 `linux` 的设备文件，包括终端，键盘输入，鼠标，输出设备（黄色）
        * `/boot`
            * 系统引导时使用的文件
        * `/mnt`
            * 系统管理员安装临时文件系统的安装点，系统提供这个目录的目的是 `让用户临时挂在其他的文件系统`
        * `/lib`
            * 存放系统启动和运行时所需要的共享库及内核模块
        * `/var`
            * 用于存放运行时需要改变数据的文件
* 相对路径
    * 从 `根目录` 开始的路径
* 绝对路径
    * 相对于 `当前的操作目录` 的路径
    * `..` 代表返回上一级目录

>### Linux 文件常用操作
* （目录 / 文件）创建、删除、读取、写入
    * `touch` 创建文件
    * `vim` 查看，编辑文件，如果文件不存在则自动创建并进入编辑页面
    * `cat` 查看文件内容并打印到终端
    * `rm` 删除文件
    * `mkdir` 创建目录
    * `rm -r` 递归删除目录中的内容

>### Linux 文件类型
* 套接字
    * `s`
* 普通文件
    * `-`
* 目录文件
    * `d`
* 符号链接
    * `l`
* 设备文件
    * `c` 字符设备文件
    * `b` 块设备文件
* FIFO 文件
    * `p`
