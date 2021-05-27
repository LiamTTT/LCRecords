# 数组

## 二分专题

> [704. 二分查找](https://leetcode-cn.com/problems/binary-search/)
>
> [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)
>
> [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
>
> [69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/)
>
> [367. 有效的完全平方数](https://leetcode-cn.com/problems/valid-perfect-square/)



---

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



---

#### [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

##### 思路

##### 题解

```cpp
// 半开半闭区间
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int lo=0; // 左闭
        int hi=nums.size(); // 右开
        int mi, ret;
        while(lo<hi) { // 不变量为不等式
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



---

#### [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

##### 思路

二分搜索老思想

麻烦点最后判断下范围

本做法无他新意

##### 代码

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        vector<int> ret = {-1, -1};
        if (nums.size()==0 || target<nums.front() || target>nums.back()) return ret;
        size_t lo = 0, hi = nums.size();
        size_t mi;
        while (lo < hi) {
            mi = lo + ((hi - lo) >> 1);
            if (target < nums[mi]) {
                hi = mi;
            }
            else if (target > nums[mi]) {
                lo = mi+1;
            }
            else {
                ret[0] = mi;
                ret[1] = mi;
                while(ret[0]>=0 && nums[ret[0]]==target) --ret[0];
                while(ret[1]<nums.size() && nums[ret[1]]==target) ++ret[1];
                ++ret[0];
                --ret[1];
                return ret;
            }
        }
        return ret;
    }
};

```



---

#### [69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/)

##### 思路

无它，只管二分，注意什么时候叫做找到了，是恰好等于或者略小于的时候

##### 代码

```cpp
class Solution {
public:
    int mySqrt(int x) {
        int lo=0, hi=x, mi=0, ret=0;  // 闭区间
        while(lo<=hi) { // 闭区间
            mi = lo + ((hi-lo)>>1);
            if ((long long)mi*mi <= x) {  // 注意大数
                ret = mi;
                lo = mi+1;  // 闭区间
            }
            else {
                hi = mi-1;  // 闭区间
            }
        }
        return ret;
    }
};
```



---

#### [367. 有效的完全平方数](https://leetcode-cn.com/problems/valid-perfect-square/)

##### 思路

找到了才输出，比69逻辑简单些

```cpp
class Solution {
public:
    bool isPerfectSquare(int num) {
        int lo=0, hi=num, mi=0, ret=0;  // 闭区间
        while(lo<=hi) { // 闭区间
            mi = lo + ((hi-lo)>>1);
            long long mi_ind = (long long)mi*mi;
            if (mi_ind < num) {  // 注意大数
                lo = mi+1;  // 闭区间
            }
            else if (mi_ind > num){
                hi = mi-1;  // 闭区间
            }
            else return true;
        }
        return false;
        }
};
```



---

## 移除元素专题（衍射双指针方法）

> [27. 移除元素](https://leetcode-cn.com/problems/remove-element/)
>
> [26. 删除有序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)
>
> [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)
>
> [844. 比较含退格的字符串](https://leetcode-cn.com/problems/backspace-string-compare/)
>
> [977. 有序数组的平方](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)



---

#### [27. 移除元素](https://leetcode-cn.com/problems/remove-element/)

##### 思路

- 双指针优
- 因为数组的性质，只需要将重复和不重复的交换就可以

##### 代码

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        vector<int>::iterator pos = nums.begin(), cur = nums.begin();
        for (;cur<nums.end(); ++cur){
            if (*cur==val) {
                continue;
            }
            swap(*cur, *pos); // 这里不用交换，直接用赋值号覆盖也可以
            ++pos;
        }
        return pos-nums.begin();
    }
};
```



##### 要点

- 一个是判断什么时候结束
- 一个是判断什么时候位置向前一步

---

#### [26. 删除有序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

##### 思路

##### 代码

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.size()==0) return 0;
        vector<int>::iterator slow=nums.begin(), fast=nums.begin();
        for (;fast<nums.end(); ++fast){
            if (*fast==*slow) {
                continue;
            }
            *++slow = *fast;  // 碰到不等就位置+1覆盖
        }
        return slow-nums.begin()+1;
    }
};
```



---

#### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

##### 思路

- 双指针
- 不能和删除元素一样覆盖了，得交换

##### 代码

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        vector<int>::iterator fast=nums.begin(), slow=nums.begin();
        for (; fast<nums.end(); ++fast){
            if (*fast==0) {
                continue;
            }
            swap(*slow++,*fast);  // 不能覆盖了，得交换
        }
    }
};
```



---

#### [844. 比较含退格的字符串](https://leetcode-cn.com/problems/backspace-string-compare/)

##### 思路

- 想着还原字符串之后再比较
- 双指针来还原
- 原地修改字符串

##### 代码

```cpp
class Solution {
public:
    bool backspaceCompare(string s, string t) {
        int sl = parse_text(s), tl = parse_text(t);
        return s.substr(0, sl)==t.substr(0, tl);
    }

    int parse_text(string& S) {
        string::iterator fast = S.begin(), slow = S.begin();
        for (; fast<S.end(); ++fast){
            if (*fast=='#') {
                slow = slow>S.begin() ? --slow : slow;  // 额外的判断光标是否在起始处
            }
            else {
                *slow++=*fast; // 这里采用的是覆盖的方式
            }
        }
        return slow-S.begin(); // 原地操作的话，时间是O(M+N),因为不需要额外开辟，空间也缩小为O(1)
    }
};
```



---

#### [977. 有序数组的平方](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)

##### 思路

- 数组是有序的
- 数组有负数
- 两头平方肯定比中间大，所以双指针比两头就行

##### 代码

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        vector<int> ret(nums.size());
        vector<int>::iterator head=nums.begin(), tail=nums.end()-1, cur=ret.end()-1; 
        while(head<=tail){
            int hsq = *head * *head, tsq = *tail * *tail;
            if (hsq>tsq) {
                *cur-- = hsq;
                ++head;
            }
            else {
                *cur-- = tsq;
                --tail;
            }
        }
        return ret;
    }
};
```



---

## 滑动窗口专题

> [209. 长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)
>
> [904. 水果成篮](https://leetcode-cn.com/problems/fruit-into-baskets/)
>
> [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)
>
> 滑动窗口基本复杂度都是O(n)



---

#### [209. 长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)

##### 思路

- 窗口内的和要满足条件
- **移动右边到满足条件**
- **满足条件后记录一次长度**
- **缩短窗口（移动左边**
- **循环到第二步**
- 遍历完后就是最终最短的结果

##### 代码

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int ret = INT32_MAX;
        int sum = 0;
        vector<int>::iterator  left = nums.begin(); // 窗口起点
        int sublength = 0;
        for (vector<int>::iterator right=nums.begin(); right<nums.end(); ++right) {
            sum += *right; // 扩大窗口
            while(sum >= target) { // 当总和大于目标值的时候进入循环
                sublength = right - left + 1;
                ret = sublength < ret ? sublength : ret;
                sum -= *left++;  // 缩小窗口
            }
        }
        // 结束遍历，留下最短的结果，不存在为零
        return ret == INT32_MAX ? 0 : ret;
    }
};
```



---

#### [904. 水果成篮](https://leetcode-cn.com/problems/fruit-into-baskets/)

##### 思路

- 滑动窗口
- 题目意思就是只包含两种不同元素最长子序列
- 主要是对于左边界的处理

##### 代码

```cpp
class Solution {
public:
    int totalFruit(vector<int>& tree) {
        // 条件使得这题变成求只包含两种不同元素的最长子序列
        int ret = 0;
        vector<int>::iterator left = tree.begin(); // 左边界
        int sublen = 0; // 临时长度
        set<int> types; // 记录已经装的类别
        for (auto right = tree.begin(); right<tree.end(); ++right) {
            types.insert(*right); // 右界滑动
            if (types.size()>2) { // 对于满足条件的进行左边界滑动
                left = right-1; // 从右往左搜寻左边界
                while(*left == *(left-1)) {
                    --left;
                };
                types.erase(*(left-1));
            }
            sublen = right - left + 1;
            ret = sublen>ret?sublen:ret; // 每扩张一步记录下长度
        }
        return ret;
    }
};
```



---

#### [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

##### 思路

- 思路还是滑动窗口
- 什么时候进行左边界滑动以及左边界滑动的的判断条件稍微复杂点
- 优化的空间还是有的

##### 代码

```cpp
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char, int> cnt, nums;
        // 建立字典，录入t中元素，建立计数器与计数对照
        for(char c: t) {
            cnt[c] = 0;
            if(nums.find(c)!=nums.end()) {
                ++nums[c];
            }
            else {
                nums[c] = 1;
            }
        }

        // 滑动窗口找寻子串
        string::iterator left = s.begin();
        string ret = "", substring = "";
        for (auto right = s.begin(); right<s.end(); ++right) {
            if (cnt.find(*right)!=cnt.end()) {
                ++cnt[*right];   
            }
            while(check(cnt, nums)) {
                substring = s.substr(left-s.begin(), right-left+1);
                ret = ret.size()==0 || substring.size()<ret.size() ? substring : ret;
                if (cnt.find(*left)!=cnt.end()) {
                    --cnt[*left];
                }
                ++left;
            }
        }

        return ret;

    }

    // 辅助检查
    bool check(unordered_map<char, int> &cnt, unordered_map<char, int> &nums){
        for (auto p : nums) {
            if (cnt[p.first] < p.second)
                return false;
        }
        return true;
    }
};
```



---

## 螺旋矩阵

>[59. 螺旋矩阵 II](https://leetcode-cn.com/problems/spiral-matrix-ii/)
>
>[54. 螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)
>
>
>
>不涉及到什么算法，就是模拟过程，但却十分考察对代码的掌控能力
>
>要坚持循环不变量原则



---

#### [59. 螺旋矩阵 II](https://leetcode-cn.com/problems/spiral-matrix-ii/)

##### 思路

##### 代码

```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> ret(n, vector<int>(n,0));
        int cnt = 1; // 控制值
        int i=0, j=0; // 控制位置
        int startx=0, starty=0; // 标定范围
        int offset = 1; // 控制偏置
        int loop = n/2; // 控制圈数
        while(loop --) {
            // 上行 左右
            for (j = starty; j < starty + n-offset; ++j) {
                ret[startx][j] = cnt++;
            }
            // 右列 上下
            for (i = startx;i < startx + n-offset; ++i) {
                ret[i][j] = cnt++;
            }
            // 下行 右左
            for (;j > startx; --j) {
                ret[i][j] = cnt++;
            }
            // 左列 下上
            for (;i > starty; --i) {
                ret[i][j] = cnt++;
            }
            // 轮完一圈，调整标定范围，调整offset，注意此处的距离调整2格才对
            ++startx;
            ++starty;
            offset += 2;
            
        }
        // 奇数时剩一个位置中间位置没有填，继续循环会出错，因此特殊处理
        if (n%2) {
            ret[n/2][n/2] = cnt;
        }
        return ret;
    }
};
```



---

#### [54. 螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)

##### 思路

- 这题用上下边界来做比较好理解
- 又给整晕了
- 主要是要处理奇数行或者列

##### 代码

```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> ret;
        if (matrix.empty()) return ret;
        // 根据上下边界来确定所处的圈圈
        int ub_x=0, ub_y=0;  // 设置上边界
        int bb_x=matrix[0].size()-1, bb_y=matrix.size()-1;  // 设置下边界
        int i,j;
        while(ub_x<=bb_x && ub_y<=bb_y) { // 循环不变量是上边界不大于下边界
            i=ub_y;
            j=ub_x;
            // 对于单行的要特殊处理
            if(ub_x==bb_x) {
                for(; i<=bb_y; ++i) {ret.push_back(matrix[i][j]);}
                break;
            }
            if(ub_y==bb_y) {
                for(; j<=bb_x; ++j) {ret.push_back(matrix[i][j]);}
                break;
            }
            // 对于普通情况正常遍历圈圈就行
            for(; j<bb_x; ++j) {ret.push_back(matrix[i][j]);}
            for(; i<bb_y; ++i) {ret.push_back(matrix[i][j]);}
            for(; j>ub_x; --j) {ret.push_back(matrix[i][j]);}
            for(; i>ub_y; --i) {ret.push_back(matrix[i][j]);}
            // 缩小圈圈
            ++ub_x;
            ++ub_y;
            --bb_x;
            --bb_y;
        }
        return ret;
    }
};
```



---

#### [剑指 Offer 29. 顺时针打印矩阵](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

##### 思路

同上

##### 代码

```cpp

```



---



# 链表

## 虚拟头节点

> [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)
>
> 利用虚拟头节点进行链表的创建



---

#### [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

##### 思路

- 建立虚拟头节点，方便删除节点
- 注意内存的释放

##### 代码

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;  // 虚拟头指向当前头
        ListNode* cur = dummyHead;
        while(cur->next != nullptr) {
            if (cur->next->val == val) {
                ListNode* tmp = cur->next;
                cur->next = cur->next->next;
                delete tmp;
            }
            else {
                cur = cur->next;
            }
        }
        head = dummyHead->next;
        delete dummyHead;
        return head;
    }
};
```



---

#### [707. 设计链表](https://leetcode-cn.com/problems/design-linked-list/)

##### 思路

- 活用虚拟头节点

##### 代码

```cpp
class MyLinkedList {
public:

    /** Initialize your data structure here. */
    MyLinkedList() {
        _dummyHead = new ListNode(0);
        _size = 0;
    }
    
    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    int get(int index) {
        if (index>_size-1 || index<0) return -1;
        ListNode* cur = _dummyHead->next; // head;
        while(index--) {
            cur=cur->next;
        }
        return cur->val;
    }
    
    /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
    void addAtHead(int val) {
        ListNode* tmp = new ListNode(val);
        tmp->next = _dummyHead->next;
        _dummyHead->next = tmp;
        ++_size;
    }
    
    /** Append a node of value val to the last element of the linked list. */
    void addAtTail(int val) {
        ListNode* tmp = new ListNode(val);
        ListNode* cur = _dummyHead;
        while(cur->next!=nullptr){
            cur = cur->next;
        }
        cur->next = tmp;
        ++_size;
    }
    
    /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
    void addAtIndex(int index, int val) {
        if (index>_size) return;
        ListNode* tmp = new ListNode(val);
        ListNode* cur = _dummyHead;
        while(index--) {
            cur=cur->next;
        }
        // 此时是插入位置之前的元素
        tmp->next = cur->next;
        cur->next = tmp;
        ++_size;
    }
    
    /** Delete the index-th node in the linked list, if the index is valid. */
    void deleteAtIndex(int index) {
        // 删除链表元素那题一样
        if (_size<=0 || index<0 || index>=_size) return;
        ListNode* cur = _dummyHead;
        while(index--) {
            cur = cur->next;
        } // 此时为index之前的元素
        ListNode* tmp = cur->next;
        cur->next = cur->next->next;
        delete tmp;
        --_size;
    }
private:
    ListNode* _dummyHead;
    int _size;
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```



---



##### 思路

##### 代码

```cpp

```



---

