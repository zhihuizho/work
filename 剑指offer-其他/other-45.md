扑克牌顺子
1.题目
LL今天心情特别好,因为他去买了一副扑克牌,发现里面居然有2个大王,2个小王(一副牌原本是54张😊)...他随机从中抽出了5张牌,想测测自己的手气,
看看能不能抽到顺子,如果抽到的话,他决定去买体育彩票,嘿嘿！！“红心A,黑桃3,小王,大王,方片5”,“Oh My God!”不是顺子.....LL不高兴了,
他想了想,决定大\小 王可以看成任何数字,并且A看作1,J为11,Q为12,K为13。上面的5张牌就可以变成“1,2,3,4,5”(大小王分别看作2和4),“So Lucky!”。
LL决定去买体育彩票啦。 现在,要求你使用这幅牌模拟上面的过程,然后告诉我们LL的运气如何。为了方便起见,你可以认为大小王是0。

2.思路
这题说了一堆，提取主要信息，我们不难整理出，满足如下条件才可以认为是顺子：

输入数据个数为5；
输入数据都在0-13之间；
没有相同的数字；
最大值与最小值的差值不大于5。
PS：大小王可以当成任意数。

这里可以使用一个技巧，即利用一个flag记录每个数字出现的次数。具体实现直接看代码吧，代码有详细注释。


3.c++
```c++
class Solution {
public:
    bool IsContinuous( vector<int> numbers ) {
        if(numbers.size() < 5){
            return false;
        }
        int max = -1, min = 14;
        int flag = 0;
        for(int i = 0; i < numbers.size(); i++){
            int curNum = numbers[i];
            if(curNum < 0 || curNum > 13){
                return false;
            }
            // 大小王，可以模拟任意数
            if(curNum == 0){
                continue;
            }
            
            // 如果数字出现了一次
            if((flag >> curNum) & 1 == 1){
                return false;
            }
            
            // 按位保存数字出现次数，比如0110表示，0出现0次，1出现1次，2出现1次，3出现0次。
            flag |= 1 << curNum;
            
            // 更新最小值
            if(curNum < min){
                min = curNum;
            }
            // 更新最大值
            if(curNum > max){
                max = curNum;
            }
            // 超过范围一定不是顺子
            if(max - min >= 5){
                return false;
            }
        }
        return true;
    }
};
```

4.python
```python
class Solution:
    def IsContinuous(self, numbers):
        # write code here
        if len(numbers) < 5:
            return False
        max_num = -1
        min_num = 14
        flag = 0
        for number in numbers:
            if number < 0 or number > 13:
                return False
            if number == 0:
                continue
            if (flag >> number) & 1 == 1:
                return False
            flag |= 1 << number
            if number < min_num:
                min_num = number
            if number > max_num:
                max_num = number
            if max_num - min_num >= 5:
                return False
        return True
```
