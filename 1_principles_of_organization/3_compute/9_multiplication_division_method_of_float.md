## 浮点数的乘除法运算

>### 对于任意 x = S<sub>x</sub> * r<sup>j<sub>x</sub></sup>，y = S<sub>y</sub> * r<sup>j<sub>y</sub></sup>

* 乘法：阶码相加，尾数求积
    * x * y = ( S<sub>x</sub> * S<sub>y</sub> ) * r<sup> ( j<sub>x</sub> + j<sub>y</sub> ) </sup>
* 除法：阶码相减，尾数求商
    * x / y = ( S<sub>x</sub> / S<sub>y</sub> ) * r<sup> ( j<sub>x</sub> - j<sub>y</sub> ) </sup>
* 过程
    * 解码运算 → 尾数运算 → 尾数规格化 → 舍入 → 溢出判断
* 例子
    * x = 0.11010011 * 2<sup>1101</sup>，y = 0.11101110 * 2<sup>0001</sup>，假设阶码四位，尾数八位，求 x * y
        * x * y = ( 0.11010011 * 0.11101110 ) * 2<sup>1101 + 0001</sup> = 0.11000100（保留八位）* 2<sup>1110</sup>
        * 后续进行尾数规格化 → 舍入 → 溢出判断，与浮点数加减法一致，此处略。
 