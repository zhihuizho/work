1.题目
将一个字符串转换成一个整数，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0。

输入描述：
输入一个字符串,包括数字字母符号,可以为空

输出描述：
如果是合法的数值表达则返回该数字，否则返回0

2.思路
这道题要考虑全面，对异常值要做出处理。

对于这个题目，需要注意的要点有：

指针是否为空指针以及字符串是否为空字符串；
字符串对于正负号的处理；
输入值是否为合法值，即小于等于'9'，大于等于'0'；
int为32位，需要判断是否溢出；
使用错误标志，区分合法值0和非法值0。
代码中用两个函数来实现该功能，其中标志位g_nStatus用来表示是否为异常输出，minus标志位用来表示是否为负数。


3.c++
```c++
class Solution {
public:
    enum Status{kValid = 0, kInValid};
    int g_nStatus = kValid;
    int StrToInt(string str) {
        g_nStatus = kInValid;
        long long num = 0;
        const char* cstr = str.c_str();
        // 判断是否为指针和是否为空字符串
        if(cstr != NULL && *cstr != '\0'){
            // 正负号标志位，默认是加号
            bool minus = false;
            if(*cstr == '+'){
                cstr++;
            }
            else if(*cstr == '-'){
                minus = true;
                cstr++;
            }
            if(*cstr != '\0'){
                num = StrToIntCore(cstr, minus);
            }
        }
        return (int)num;
    }
private:
    long long StrToIntCore(const char* cstr, bool minus){
        long long num = 0;
        while(*cstr != '\0'){
            // 判断是否是非法值
            if(*cstr >= '0' && *cstr <= '9'){
                int flag = minus ? -1 : 1;
                num = num * 10 + flag * (*cstr - '0');
                // 判断是否溢出,32位
                if((!minus && num > 0x7fffffff) || (minus && num < (signed int)0x80000000)){
                    num = 0;
                    break;
                }
                cstr++;
            }
            else{
                num = 0;
                break;
            }
        }
        // 判断是否正常结束
        if(*cstr == '\0'){
            g_nStatus = kValid;
        }
        return num;
    }
};
```

4.python
```python
class Solution:
    def StrToInt(self, s):
        # write code here
        length = len(s)
        if length == 0:
            return 0
        else:
            minus = False
            flag = False
            if s[0] == '+':
                flag = True
            if s[0] == '-':
                flag = True
                minus = True
            begin = 0
            if flag:
                begin = 1
            num = 0
            minus = -1 if minus else 1
            for each in s[begin:]:
                if each >= '0' and each <= '9':
                    num = num * 10 + minus * (ord(each) - ord('0'))
                else:
                    num = 0
                    break
            return num
```
