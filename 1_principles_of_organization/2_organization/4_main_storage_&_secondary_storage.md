## 计算机的主存储器与辅助存储器

>### 问题
* 为什么计算机断电，内存数据丢失？
* 为什么计算机断电，磁盘数据不丢失？

>### 主存储器 —— 内存
* RAM（随机存取存储器：Random Access Memory）
* RAM 通过 `电容` 存储数据，必须隔一段时间刷新一次
* 如果断电，电容内部的电子会丢失，一段时间后将丢失所有数据
* 组成（图片请自行 Google）
    * 半导体存储体
    * 驱动器
    * 译码器
    * 读写电路
    * 控制电路
* 连接方式
    * 译码器 → 驱动器 → 半导体存储体 ←→ 读写电路 ← 控制电路
* 与 CPU 的通信方式
    * 内存读写电路 ←-- 数据总线 --→ CPU 主存数据寄存器（MDR）
    * 内存译码器 ←-- 地址总线 --→ CPU 主存地址寄存器（MAR）
* 系统与内存支持
    * `32 位系统`：寻址范围最多为 2<sup>32</sup> = 4 * 2<sup>30</sup> = 4 `GB`，因此加更多的内存也是没有用的，它的地址总线只有 32 位
    * `32 位系统`：寻址范围最多为 2<sup>64</sup> = 2<sup>34</sup> * 2<sup>30</sup> = 2<sup>34</sup> `GB`

>### 辅助存储器 —— 磁盘
* 物理结构
    * 组成
        * 光滑的盘片，磁材料
        * 悬臂（磁头）
    * 磁盘的立体结构
        * 磁道 `t`
        * 扇区 `s`
        * 柱面 `c`
        * 盘片
        * 读写磁头
        * 传轴
        * 机械臂杆
    * 表面是可磁化的硬磁材料
    * 移动磁头径向运动读取磁道信息
* 调度算法
    * 假设磁头现在在磁道 4，磁头方向在外，现要求依次读取磁道 1, 4, 2, 3, 1, 5
        * 先来先服务算法（简单粗暴）
        * 按顺序访问进程的磁道读写要求
        * 读取方法：1 → 4 → 2 → 3 → 1 → 5
    * 最短寻道时间优先
        * 与磁头的当前位置有关
        * 优先访问离磁头最近的磁道
        * 读取方法（磁道 3，5 和 4 的距离相同，假设先读取磁道 5）：4 → 5 → 3 → 2 → 1 → 1
    * 扫描算法（电梯算法）
        * 每次只往一个方向移动
        * 到达一个方向需要服务的尽头，再反向移动
        * 读取方法（由于磁头方向在外）：4 → 3 → 2 → 1 → 1 → 5
    * 循环扫描算法
        * 只能往一个方向读取
        * 读取方法（假设磁头由外往内读取）：4 → 5 → 1 → 1 → 2 → 3
