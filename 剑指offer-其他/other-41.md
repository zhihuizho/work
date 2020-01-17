和为S的连续正数序列
1.
小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,
他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。

输出描述：

输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序。

2.思路
这道题还是蛮简单的。

设定两个指针，一个指向第一个数，一个指向最后一个数，在此之前需要设定第一个数和最后一个数的值，由于是正数序列，所以可以把第一个数设为1,
最后一个数为2（因为是要求是连续正数序列，最后不可能和第一个数重合）。下一步就是不断改变第一个数和最后一个数的值，
如果从第一个数到最后一个数的和刚好是要求的和，那么把所有的数都添加到一个序列中；如果大于要求的和，则说明从第一个数到最后一个数之间的范围太大，
因此减小范围，需要把第一个数的值加1，同时把当前和减去原来的第一个数的值；如果小于要求的和，说明范围太小，因此把最后一个数加1，
同时把当前的和加上改变之后的最后一个数的值。这样，不断修改第一个数和最后一个数的值，就能确定所有连续正数序列的和等于S的序列了。

注意：初中的求和公式应该记得吧，首项加尾项的和乘以个数除以2，即sum = (a + b) * n / 2。


3.c++
```c++
class Solution {
public:
    vector<vector<int> > FindContinuousSequence(int sum) {
        vector<vector<int> > result;
        // 高位指针和低位指针
        int phigh = 2, plow = 1;
        
        // 终止条件是phigh等于sum
        while(phigh > plow){
            // 当前和，使用求和公式s = (a+b) * n / 2
            int curSum = (plow + phigh) * (phigh - plow + 1) >> 1;
            if(curSum < sum){
                phigh++;
            }
            if(curSum == sum){
                vector<int> temp;
                for(int i = plow; i <= phigh; i++){
                    temp.push_back(i);
                }
                result.push_back(temp);
                plow++;
            }
            if(curSum > sum){
                plow++;
            }
        }
        return result;
    }
};
```

4.python
```python
class Solution:
    def FindContinuousSequence(self, tsum):
        # write code here
        result = []
        low, high = 1, 2
        while low < high:
            curSum = (low + high) * (high - low + 1) / 2
            if curSum == tsum:
                tmp = []
                for i in range(low, high+1):
                    tmp.append(i)
                result.append(tmp)
                low += 1
            elif curSum < tsum:
                high += 1
            else:
                low += 1
        return result
```
