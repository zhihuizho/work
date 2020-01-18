求1+2+3+…+n
题目
求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

1、思路
没什么好说的，这是一道超级无敌送分题，使用递归即可。


3.c++
```c++
class Solution {
public:
    int Sum_Solution(int n) {
        int ans = n;
        // &&就是逻辑与，逻辑与有个短路特点，前面为假，后面不计算。即递归终止条件
        ans && (ans += Sum_Solution(n - 1));
        return ans;
    }
};
```

4.python
```python
class Solution:
    def Sum_Solution(self, n):
        # write code here
        ans = n
        if ans:
            ans += self.Sum_Solution(n-1)
        return ans
```
