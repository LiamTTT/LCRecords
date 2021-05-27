#### [1734. 解码异或后的排列](https://leetcode-cn.com/problems/decode-xored-permutation/)

> 2021.5.11

##### 思路

奇数 n

原数组 p 有 n 元素

编码数组 e 有 n-1 元素

**1.** 数组是前n排列 -> p_all = p[0]^p[1]^...^p[n-1] = 1^2^...^n

**2.** n 是奇数 + e[i]=p[i]+p[i+1] -> e[1]^e[3]...^e[n-2] = p[1]^p[2]^...^p[n] = p_others

**3.** 1.+2. -> p[0] = p_all^p_others

**4.** 剩下就是简单根据e和p[0]来确定其他p中元素了

##### 代码

```cpp
class Solution {
public:
    vector<int> decode(vector<int>& encoded) {
        // 关键信息
        // 1. perm是前n的排列 -> 可以知道所有perm元素的XOR结果
        // 2. n是个奇数 -> 辅助3结论
        // 3. e[i] = p[i]^p[i+1] -> 可以知道仅除某个元素之外（比如p[0]）所有元素的XOR结果
        // -> 可以求取被除开的那个元素
        // -> 知道一个元素之后就可以求取所有元素了
        int n = encoded.size()+1;
        int p_all = 0; // 求取所有perm元素XOR结果
        for(int i=1; i<=n; ++i){
            p_all ^= i;
        }
        int p_others = 0; // 求取除perm[0]外所有元素结果
        for(int i=1; i<n; i+=2){
            p_others ^= encoded[i];
        }
        vector<int> ret(n);
        ret[0] = p_all^p_others;
        for(int i=0; i<n-1; ++i){
            ret[i+1] = ret[i]^encoded[i];
        }
        return ret;
    }
};
```



---

#### [13. 罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)

> 2021.5.15

##### 思路

反向迭代

##### 代码

```cpp
class Solution {
public:
    int romanToInt(string s) {
        int ret=0;
        int pre=0;
        for(auto c=s.crbegin(); c!=s.crend(); ++c){  // 反向迭代
            int v = encode[*c];
            if (pre<=v) 
                ret += v;
            else 
                ret -= v;
            pre = v;
        }
        return ret;
    }
private:
    unordered_map<char, int> encode = {
        {'I',1},
        {'V',5},
        {'X',10},
        {'L',50},
        {'C',100},
        {'D',500},
        {'M',1000}
    };
};
```



---



