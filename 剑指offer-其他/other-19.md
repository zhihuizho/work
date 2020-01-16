顺时针打印矩阵
1.题目
输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字

2.思路
将结果存入vector数组，从左到右，再从上到下，再从右到左，最后从下到上遍历。


3.c++
```c++
class Solution {
public:
    vector<int> printMatrix(vector<vector<int> > matrix) {
        int rows = matrix.size();            //行数
        int cols = matrix[0].size();        //列数
        vector<int> result;
        
        if(rows == 0 && cols == 0){
            return result;
        }
        int left = 0, right = cols - 1, top = 0, bottom = rows - 1;
        
        while(left <= right && top <= bottom){
            //从左到右
            for(int i = left; i <= right; ++i){
                result.push_back(matrix[top][i]);
            }
            //从上到下
            for(int i = top + 1; i <= bottom; ++i){
                result.push_back(matrix[i][right]);
            }
            //从右到左
            if(top != bottom){
                for(int i = right - 1; i >= left; --i){
                    result.push_back(matrix[bottom][i]);
                }
            }
            //从下到上
            if(left != right){
                for(int i = bottom - 1; i > top; --i){
                    result.push_back(matrix[i][left]);
                }
            }
            left++, top++, right--, bottom--;
        }
        return result;
    }
};
```

4.python
```python
class Solution:
    # matrix类型为二维列表，需要返回列表
    def printMatrix(self, matrix):
        # write code here
        rows = len(matrix)
        cols = len(matrix[0])
        result = []
        if rows == 0 and cols == 0:
            return result
        left, right, top, buttom = 0, cols - 1, 0, rows - 1
        while left <= right and top <= buttom:
            for i in range(left, right+1):
                result.append(matrix[top][i])
            for i in range(top+1, buttom+1):
                result.append(matrix[i][right])
            if top != buttom:
                for i in range(left, right)[::-1]:
                    result.append(matrix[buttom][i])
            if left != right:
                for i in range(top+1, buttom)[::-1]:
                    result.append(matrix[i][left])
            left += 1
            top += 1
            right -= 1
            buttom -= 1
        return result
```

4.2
题目变型：

给定一个数字n，打印矩阵：

```python
def matrix(target):
    num = target * target
    left, right, top, bottom = 0, target-1, 0, target-1
    res = [ [0 for col in range(target)] for row in range(target)]
    each = 1
    while left <= right and top <= bottom and each <= num:
        for i in range(left, right+1):
            res[top][i] = each
            each += 1
        for i in range(top+1, bottom+1):
            res[i][right] = each
            each += 1
        if top != bottom:
            for i in range(left, right)[::-1]:
                res[bottom][i] = each
                each += 1
        if left != right and each <= num:
            for i in range(top+1, bottom)[::-1]:
                res[i][left] = each
                each += 1
        top += 1
        left += 1
        bottom -= 1
        right -= 1
    for i in range(len(res)):
        print("\t".join('%s' %id for id in res[i]))
 
if __name__ == '__main__':
    matrix(4)
```
