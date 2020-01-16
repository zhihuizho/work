1.题目:包含min函数的栈
定义栈的数据结构，请在类型中实现一个能够得到栈最小元素的min函数。

2.思路
使用两个stack，一个为数据栈，另一个为辅助栈。数据栈用于存储所有数据，辅助栈用于存储最小值。

举个例子：

入栈的时候：首先往空的数据栈里压入数字3，显然现在3是最小值，我们也把最小值压入辅助栈。接下来往数据栈里压入数字4。
由于4大于之前的最小值，因此我们只要入数据栈，不压入辅助栈。

出栈的时候：当数据栈和辅助栈的栈顶元素相同的时候，辅助栈的栈顶元素出栈。否则，数据栈的栈顶元素出栈。

获得栈顶元素的时候：直接返回数据栈的栈顶元素。

栈最小元素：直接返回辅助栈的栈顶元素。


3.c++
```c++
class Solution {
public:
    void push(int value) {
        Data.push(value);
        if(Min.empty()){
            Min.push(value);
        }
        if(Min.top() > value){
            Min.push(value);
        }
    }
    void pop() {
        if(Data.top() == Min.top()){
            Min.pop();
        }
        Data.pop();
    }
    int top() {
        return Data.top();
    }
    int min() {
        return Min.top();
    }
private:
    stack<int> Data;            //数据栈
    stack<int> Min;                //最小栈
};
```

4.python
```python
class Solution:
    def __init__(self):
        self.Data = []
        self.Min = []
    def push(self, node):
        # write code here
        self.Data.append(node)
        if self.Min:
            if self.Min[-1] > node:
                self.Min.append(node)
            else:
                self.Min.append(self.Min[-1])   #保证Min栈的栈顶一直指向最小的数字，即使出现重复输入也不会出错
        else:
            self.Min.append(node)
    def pop(self):
        # write code here
        if self.Data == []:
            return None
        self.Min.pop()
        return self.Data.pop()
    def top(self):
        # write code here
        if self.Data == []:
            return None
        return self.Data[-1]
    def min(self):
        # write code here
        if self.Min == []:
            return None
        return self.Min[-1]
```
