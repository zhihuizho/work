数值的整数次方
1.题目
给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

2.思路
当指数为负数的时候，可以先对指数求绝对值，然后算出次方的结果之后再取倒数。如果底数为0，则直接返回0。此时的次方在数学上是没有意义的。

除此之外，我们要注意：由于计算机表示小数（包括float和double型小数）都有误差，我们不能直接用等号（==）判断两个小数是否相等。
如果两个小数的差的绝对值很小，比如小于0.0000001，就可以认为它们相等。

在计算次方的时候，除了简单的遍历，我们可以使用如下公式进行计算，来减少计算量：


3.c++
```c++
class Solution {
public:
    double Power(double base, int exponent) {
        if(equal(base, 0.0)){
            return 0.0;
        }
        unsigned int absExponent = 0;
        if(exponent > 0){
            absExponent = (unsigned int)(exponent);
        }
        else{
            absExponent = (unsigned int)(-exponent);
        }
        double result = PowerWithUnsignedExponent(base, absExponent);
        if(exponent < 0){
            result = 1.0 / result;
        }
        return result;
    }
 
private:
    bool equal(double num1, double num2){
        if(num1 - num2 > -0.0000001 && (num1 - num2) < 0.0000001){
            return true;
        }
        else{
            return false;
        }
    }
    
    double PowerWithUnsignedExponent(double base, unsigned int exponent){
        if(exponent == 0){
            return 1;
        }
        if(exponent == 1){
            return base;
        }
        double result = PowerWithUnsignedExponent(base, exponent >> 1);
        result *= result;
        if(exponent & 0x1 == 1){
            result *= base;
        }
        return result;
    }
};
```

4.python
```python
class Solution:
    def Power(self, base, exponent):
        # write code here
        flag = 0
        result = 1
        if base == 0:
            return False
        if exponent < 0:
            flag = 1
        for i in range(abs(exponent)):
            result *= base
        if flag == 1:
            result = 1 / result
        return result
```
