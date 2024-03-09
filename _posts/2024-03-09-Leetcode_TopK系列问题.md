---

layout:   post

title:   TopK系列笔试题

subtitle:  Heap的妙用

date:    2024-03-09

author:   BY LinShengfeng

header-img: img/post-bg-ios9-web.jpg

catalog: true

tags:

- 算法

- leetcode

---

# TopK系列笔试题

[前人栽树，后人乘凉，来自前人的归纳](https://blog.csdn.net/weixin_46114074/article/details/129802909)

## 题1 求K个最小值

设计一个算法，找出数组中最小的k个数。以任意顺序返回这k个数均可。

### 思路

最简单粗暴的方法是由小到大排序然后取前K个，时间复杂度为 O(nlogn)，考虑用Heap（Priority Queue）来解决: 维护一个存放K个最小数的 Heap（用最大堆，每次比较只与最大的元素比即可），向后遍历数组，遇到比Heap顶还小的元素X，就把顶部元素pop掉，然后把X插入进Heap。遍历的复杂度为 O(n)，每次Heap维护的复杂度为 O(logK)，故时间复杂度为 O(n log K)，相比于O(n log n)，已经轻松了一些。

### 代码

```c++
class Solution {
public:
    vector<int> smallestK(vector<int>& arr, int k) {
        priority_queue<int> pq;
        for (int i = 0; i < k; i++) pq.push(arr[i]);
        if (k > 0) { // 防止出现 k = 0的时候下面代码出现nullptr的错
            for (int i = k; i < arr.size(); i++) {
                if (arr[i] < pq.top()) {
                    pq.pop();
                    pq.push(arr[i]);
                }
            }
        }
        vector<int> ans;
        for (int i = 0; i < k; i++) {
            ans.push_back(pq.top());
            pq.pop();
        }
        return ans;
    }
};
```

### 拓展 求最大的K个数

有个改动点：创建最小堆，如此一来用 top() 方法直接可以获取前面元素的下界，如果当前元素X比这个下界大，那么就替换掉。

难点在于创建最小堆的语法，完整代码如下:

```c++
class Solution {
public:
// 自定义比较器，用于最小堆
struct MinHeapComparator {
    bool operator()(const int& lhs, const int& rhs) const {
        return lhs > rhs; // 使得优先队列变成最小堆
    }
};
    vector<int> smallestK(vector<int>& arr, int k) {
        priority_queue<int, vector<int>, MinHeapComparator> pq;
        for (int i = 0; i < k; i++) pq.push(arr[i]);
        if (k > 0) {
            for (int i = k; i < arr.size(); i++) {
                if (arr[i] > pq.top()) { // 这里符号有改动
                    pq.pop();
                    pq.push(arr[i]);
                }
            }
        }
        vector<int> ans;
        for (int i = 0; i < k; i++) {
            ans.push_back(pq.top());
            pq.pop();
        }
        return ans;
    }
};
```

