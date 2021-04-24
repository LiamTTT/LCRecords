# 数组

#### [704. 二分查找](https://leetcode-cn.com/problems/binary-search/)

##### 思路

如题名，二 分 查 找

容易绕晕的的就是边界怎么确定，简单来讲就是：闭区间缩一，开区间不变。

具体表现就是:

[lo, hi) -> lo=mid+1, hi=mid

[lo, hi] -> lo=mid+1, hi=mid-1

左边往右边缩，右边往左边缩。

循环条件怎么控制？简单来讲就是无效了再跳出。

[lo, hi] lo<=hi 都是有效的，lo<hi也是有效的但是有一个元素没考虑到。

[lo, hi) 只有lo<hi有效，lo=hi 是无效的（lo=hi的同时lo<hi，这河里妈）。

##### 题解

```cpp
// 左闭右开区间
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int lo=0;
        int hi=nums.size();
        int mi;
        while(lo < hi){
            mi = lo + ((hi-lo) >> 1);
            if (target < nums[mi]) {
                hi = mi;
            }
            else if (target > nums[mi]) {
                lo = mi+1;
            }
            else 
                return mi;
        }
        return -1;
    }
};
```

```cpp
// 闭区间
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int lo=0;
        int hi=nums.size()-1;
        int mi;
        while(lo <= hi){
            mi = lo + ((hi-lo) >> 1);
            if (target < nums[mi]) {
                hi = mi-1;
            }
            else if (target > nums[mi]) {
                lo = mi+1;
            }
            else 
                return mi;
        }
        return -1;
    }
};
```

```cpp
// 递归
class Solution {
public:
    int rec(vector<int>& nums, int target, int lo, int hi) {
        // 递归三步
        // 终止条件
        // 子问题处理
        // 忘了
        if (lo > hi ) return -1;
        int mi = lo + ((hi-lo)>>1);
        if (target < nums[mi]) return rec(nums, target, lo, mi-1); // left
        else if (target > nums[mi]) return rec(nums, target, mi+1, hi); // right
        else return mi;
    }

    int search(vector<int>& nums, int target) {
        return rec(nums, target, 0, nums.size()-1); // 闭区间，开区间同上
    }
};
```

##### 要点

- 区间的确定

##### 拓展

[35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)



---

#### [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

##### 思路

##### 题解

```cpp
// 半开半闭区间
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int lo=0;
        int hi=nums.size();
        int mi, ret;
        while(lo<hi) {
            mi = lo + ((hi-lo)>>1);
            if (target < nums[mi]) {
                hi = mi;
            }
            else if (target > nums[mi]) {
                lo = mi+1;
            }
            else {
                return mi;
            };
        }
        return lo;  // lo=hi的时候始终是其插入点
    }
};
```

```cpp
// 闭区间
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int lo=0;
        int hi=nums.size()-1; // 闭区间
        int mi, ret;
        while(lo<=hi) {
            mi = lo + ((hi-lo)>>1);
            if (target < nums[mi]) {
                hi = mi-1;
            }
            else if (target > nums[mi]) {
                lo = mi+1;
            }
            else {
                return mi;
            };
        }
        return lo;  
        // 闭区间，跳出循环时hi<lo，跳出循环前一步一定lo=hi，
        // 根据target的值，如果大于lo=hi所指，则往当前索引+1插入，否则往当前索引插入
        // 而根据判断，大于时执行lo=mi+1，往lo插入就行；小于时lo不变而hi=mid+1，此时还是往lo插入，所以返回lo就行
    }
};
```



##### 要点





