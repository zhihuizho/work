1.题目
牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，
有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，
正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？

2.思路
观察字符串变化规律，你会发现这道题很简单。只需要对每个单词做翻转，然后再整体做翻转就得到了正确的结果。


3.c++
```c++
class Solution {
public:
    string ReverseSentence(string str) {
        string result = str;
        int length = result.size();
        if(length == 0){
            return "";
        }
        // 追加一个空格，作为反转标志位
        result += ' ';
        int mark = 0;
        // 根据空格，反转所有单词
        for(int i = 0; i < length + 1; i++){
            if(result[i] == ' '){
                Reverse(result, mark, i - 1);
                mark = i + 1;
            }
        }
        // 去掉添加的空格
        result = result.substr(0, length);
        // 整体反转
        Reverse(result, 0, length - 1);
        
        return result;
    }
private:
    void Reverse(string &str, int begin, int end){
        while(begin < end){
            swap(str[begin++], str[end--]);
        }
    }
};
```

4.python
```python
class Solution:
    def ReverseSentence(self, s):
        # write code here
        s_list = s.split(' ')
        return ' '.join(s_list[::-1])
```
