最小的K个数
1.题目
输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4。

2.思路
最简单的方法就是先排序，然后在遍历输出最小的K个数，方法简单粗暴。
  
3.c++
```c++
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        vector<int> result;
        if(input.empty() || k > input.size()){
            return result;
        }
        sort(input.begin(), input.end());
        for(int i = 0; i < k; ++i){
            result.push_back(input[i]);
        }
        return result;
    }
};
```

3.1.优化
上述算法的时间复杂度为O(n^2)，速度慢。另一种O(nlogk)的算法是基于堆排序的，特别适合处理海量数据。

我们可以先创建一个大小为k的数据容器来存储最小的k个数字，接下来我们每次从输入的n个整数中的n个整数中读入一个数。如果容器中已有的数字少于k个，
则直接把这次读入的整数放入容器之中；如果容器已经有k个数字了，也就是容器满了，此时我们不能再插入新的数字而只能替换已有的数字。
找出这已有的k个数中的最大值，然后拿这次待插入的整数和最大值进行比较。如果待插入的值比当前已有的最大值小，则用这个数替换当前已有的最大值；
如果待插入的值比当前已有的最大值还要大，那么这个数不可能是最小的k个整数之一，于是我们可以抛弃这个整数。

因此当容器满了之后，我们要做3件事情：一是在k个整数中找到最大数；二是有可能在这个容器中删除最大数；三是有可能要插入一个新的数字。
如果用一个二叉树来实现这个数据容器，那么我们在O(logk)时间内实现这三步操作。因此对于n个输入数字而言，总的时间效率就是O(nlogk)。

3.1 c++
```c++
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        vector<int> result;
        int length = input.size();
        if(length <= 0 || k <= 0 || k > length){
            return result;
        }
        
        for(int i = 0; i < input.size(); i++){
            if(result.size() < k){
                result.push_back(input[i]);
            }
            else{
                for(int j = k / 2; j >= 0; j--){
                    HeadAdjust(result, j, k);
                }
                for(int j = k - 1; j > 0; j--){
                    swap(result[0], result[j]);
                    HeadAdjust(result, 0, j);
                }
                if(result[k-1] > input[i]){
                    result[k-1] = input[i];
                }
            }
        }
        return result;
    }
private:
    void HeadAdjust(vector<int> &input, int parent, int length){
        int temp = input[parent];
        int child = 2 * parent + 1;
        while(child < length){
            if(child + 1 < length && input[child] < input[child+1]){
                child++;
            }
            if(temp >= input[child]){
                break;
            }
            input[parent] = input[child];
            
            parent = child;
            child = 2 * parent + 1;
        }
        input[parent] = temp;
    }
};
```

3.2
对于上述代码，我们还可以进一步优化，不是每次循环都需要重新排序的，只有在更新了容器的数据之后，才需要重新排序。

进一步优化：

3.2c++
```c++
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        vector<int> result;
        int length = input.size();
        bool change = true;
        if(length <= 0 || k <= 0 || k > length){
            return result;
        }
        
        for(int i = 0; i < input.size(); i++){
            if(result.size() < k){
                result.push_back(input[i]);
            }
            else{
                if(change == true){
                    for(int j = k / 2; j >= 0; j--){
                        HeadAdjust(result, j, k);
                    }
                    for(int j = k - 1; j > 0; j--){
                        swap(result[0], result[j]);
                        HeadAdjust(result, 0, j);
                    }
                    change = false;
                }
                if(result[k-1] > input[i]){
                    result[k-1] = input[i];
                    change = true;
                }
            }
        }
        return result;
    }
private:
    void HeadAdjust(vector<int> &input, int parent, int length){
        int temp = input[parent];
        int child = 2 * parent + 1;
        while(child < length){
            if(child + 1 < length && input[child] < input[child+1]){
                child++;
            }
            if(temp >= input[child]){
                break;
            }
            input[parent] = input[child];
            
            parent = child;
            child = 2 * parent + 1;
        }
        input[parent] = temp;
    }
};
```

4.python
用数组的方法：
```python
class Solution:
    def GetLeastNumbers_Solution(self, tinput, k):
        if len(tinput) < k:
            return []
        tmp = sorted(tinput[:k])
        for each in tinput[k:]:
            index = k - 1
            flag = False
            while index >= 0 and tmp[index] > each:
                index -= 1
                flag = True
            if flag == True:
                tmp.insert(index+1, each)
                tmp.pop()
        return tmp
```

4.1
用堆排序的方法：
```python
class Solution:
    def HeadAdjust(self, input_list, parent, length):
        temp = input_list[parent]
        child = 2 * parent + 1
        while child < length:
            if child + 1 < length and input_list[child] < input_list[child+1]:
                child += 1
            if temp >= input_list[child]:
                break
            input_list[parent] = input_list[child]
            parent = child
            child = 2 * parent + 1
        input_list[parent] = temp
 
    def GetLeastNumbers_Solution(self, tinput, k):
        # write code here
        res = []
        length = len(tinput)
        change = True
        if length <= 0 or k <= 0 or k > length:
            return res
        res = tinput[:k]
 
        for i in range(k, length+1):
            if change == True:
                for j in range(0, k//2+1)[::-1]:
                    self.HeadAdjust(res, j, k)
                for j in range(1, k)[::-1]:
                    res[0], res[j] = res[j], res[0]
                    self.HeadAdjust(res, 0, j)
                chage = False
            if i != length and res[k-1] > tinput[i]:
                res[k-1] = tinput[i]
                chage = True
        return res
```
