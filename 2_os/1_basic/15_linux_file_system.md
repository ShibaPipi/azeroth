## Linux 的文件系统

>### 文件系统概览
* FAT
    * File Allocation Table
    * FAT 16、Fat 32 等，是微软 DOS/Windows 使用的文件系统
    * 使用一张表保存盘块的信息
* NTFS
    * New Technology File System
    * WindowsNT 环境的文件系统
    * 对 FAT 进行了改进，取代了旧的文件系统
* EXT 2/3/4
    * Extended File System，扩展文件系统
    * Linux 的文件系统
    * 数字表示第几代

>### EXT 文件系统
* EXT 文件系统

    | Boot Sector |
    | :---: |
    | Block Group 1 | 
    | Block Group 2 | 
    | Block Group 3 | 

    * Boot Sector：启动扇区，安装开机管理程序
    * Block Group：块组，存储数据的实际位置
        
        | SuperBlock |
        | :---: |
        | 文件系统描述 | 
        | Inode bitmap | 
        | Block bitmap | 
        | Inode table | 
        |  |
        |  |
        |  |
        | Data Block | 
        |  |
        |  |
        |  |
        
        * Inode Table
            * 存放文件 Inode 的地方
            * 每一个文件（目录）都有一个 Inode
            * 是每一个文件（目录）的 `索引节点`
        * Inode
            * Inode
                * 文件类型
                    * 套接字、普通文件、目录文件、符号链接、设备文件、FIFO 文件
                * 文件权限
                * 文件物理地址
                * 文件长度
                * 文件连接计数
                * 文件存取时间
                    * 文件的创建时间、最后查看时间、最后修改时间
                * 索引编号
                    * 每个文件的唯一编号
                * 文件状态
                    * 打开、关闭
                * 访问计数
                    * 当前访问这个文件的所有进程
                * 链接指针
            * 文件名不是存放在 Inode 节点上的，而是存放在目录的 Inode 节点，目的是列出目录文件（此操作较为频繁，文件多时较为耗时）的时候无需加载文件的 Inode
        * Inode bitmap
            * Inode 位示图
            * 记录已分配的 Inode 的未分配的 Inode（当文件系统在初始化，Inode 的事务就已固定）
            
                | index 表 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | ... |
                | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
                | - | 0 | 0 | 0 | 1 | 1 | 0 | 0 | 0 | ... |
    
            * 用 0 或 1 标记 Inode节点是否已经分配
        * Data Block
            * Data Block 是存放文件内容的地方
            * 每个 block 都有唯一的编号
            * 文件的 block 记录在文件的 Inode 上
        * Block bitmap
            * 功能与 Inode bitmap 类似
            * 记录 Data Block 的使用情况
        * SuperBlock
            * 记录整个文件系统相关信息
            * 记录 Block 和 Inode 的使用情况
            * 文件系统的时间信息、控制信息等
* Linux 相关操作
    * df -T 查看系统挂载的磁盘信息
        * Used 使用信息
        * Available 可用信息
        * Use% Mounted on 使用的比例
    * dumpe2fs 查看文件的 Inode 信息
        * Inode count：Inode 数量
        * Block count：Data Block 数量
        * Free blocks：未使用的 Block 数量
        * Free Inode：未使用的 Inode 数量
        * Inode size：每个 Inode 的大小
        * Block size：每个 Block 的大小
        * Group 0
            * Block bitmap at：Block bitmap 具体地址
            * Inode bitmap at：Inode bitmap 具体地址
            * Free blocks：还没使用的 block
            * Free inodes：还没使用的 Inode
    * stat 查看文件具体信息
        * Blocks：block 数量
        * Inode：Inode 节点编号（文件在系统中的唯一标识）
        * Access：最后访问时间
        * Modify：最后修改时间（文件内容已修改）
        * Change：最后元数据修改时间（例如文件权限）
