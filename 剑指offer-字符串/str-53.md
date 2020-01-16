题目
请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 
但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

1、思路
这道题还是比较简单的。表示数值的字符串遵循如下模式：

[sign]integral-digits[.[fractional-digits]][e|E[sign]exponential-digits]

其中，('['和']'之间的为可有可无的部分)。

在数值之前可能有一个表示正负的'+'或者'-'。接下来是若干个0到9的数位表示数值的整数部分（在某些小数里可能没有数值的整数部分）。
如果数值是一个小数，那么在小数后面可能会有若干个0到9的数位表示数值的小数部分。如果数值用科学记数法表示，接下来是一个'e'或者'E'，
以及紧跟着的一个整数（可以有正负号）表示指数。

判断一个字符串是否符合上述模式时，首先看第一个字符是不是正负号。如果是，在字符串上移动一个字符，继续扫描剩余的字符串中0到9的数位。
如果是一个小数，则将遇到小数点。另外，如果是用科学记数法表示的数值，在整数或者小数的后面还有可能遇到'e'或者'E'。


3.c++
```c++
class Solution {
public:
    // 数字的格式可以用A[.[B]][e|EC]或者.B[e|EC]表示，
    // 其中A和C都是整数（可以有正负号，也可以没有）
    // 而B是一个无符号整数
    bool isNumeric(char* string)
    {
        // 非法输入处理
        if(string == NULL || *string == '\0'){
            return false;
        }
        // 正负号判断
        if(*string == '+' || *string == '-'){
            ++string;
        }
        bool numeric = true;
        scanDigits(&string);
        if(*string != '\0'){
            // 小数判断
            if(*string == '.'){
                ++string;
                scanDigits(&string);
                if(*string == 'e' || *string == 'E'){
                    numeric = isExponential(&string);
                }
            }
            // 整数判断
            else if(*string == 'e' || *string == 'E'){
                numeric = isExponential(&string);
            }
            else{
                numeric = false;
            }
        }
        return numeric && *string == '\0';
    }
private:
    // 扫描数字，对于合法数字，直接跳过
    void scanDigits(char** string){
        while(**string != '\0' && **string >= '0' && **string <= '9'){
            ++(*string);
        }
    }
    // 用来潘达un科学计数法表示的数值的结尾部分是否合法
    bool isExponential(char** string){
        ++(*string);
        if(**string == '+' || **string == '-'){
            ++(*string);
        }
        if(**string == '\0'){
            return false;
        }
        scanDigits(string);
        // 判断是否结尾，如果没有结尾，说明还有其他非法字符串
        return (**string == '\0') ? true : false;
    }
};
```
