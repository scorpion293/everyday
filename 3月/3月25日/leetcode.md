# leetcode第一题：两数之和
## 题目介绍
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 的那 两个 整数，并返回它们的数组下标。
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。
你可以按任意顺序返回答案。

示例 1：
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
## 算法思路、代码、心得
### 暴力法
直接双重循环，对于数组中的每一个数x，都来找在这个数组中有没有对应的target-x
```
    vector<int> twoSum(vector<int>& nums, int target) {
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                if (nums[i] + nums[j] == target) {
                    return {i, j};
                }
            }
        }
        return {};
    }
```
vector是c++里面stl库里的一个容器，大概就相当于一个可变数组，貌似leetcode后面都用vector，这里记一下它的用法  
(1)头文件#include<vector>  
  
(2)创建vector对象，vector<int> vec;  
  
(3)尾部插入数字：vec.push_back(a);  

(4)使用下标访问元素，cout<<vec[0]<<endl;记住下标是从0开始的。 

(5)使用迭代器访问元素  

  (6)插入元素：    vec.insert(vec.begin()+i,a);在第i个元素后面插入a;

(7)删除元素：    vec.erase(vec.begin()+2);删除第3个元素

(8)向量大小:vec.size();

(9)清空:vec.clear()　　
  

不过这种算法的时间复杂度比较高，提交上去也只击败了百分之十几的人，之后看了下题解，发现还可以用哈希表来做

### 哈希表法
这种方法呢就是先创一个map，把数组里的数全存到map中，并赋予他们一个对应的key值，然后依次查找每个数对应的另一个值

```
vector<int> twoSum(vector<int>& nums, int target) {
        map<int,int> a;
        vector<int> b(0,0);
        for(int i=0;i<nums.size();i++)
            a.insert(pair<int,int>(nums[i],i));
        for(int i=0;i<nums.size();i++)
        {
            if(a.count(target-nums[i])>0&&(a[target-nums[i]]!=i))
            {
                b[0]=i;
                b[1]=a[target-nums[i]];
                break;
            }
        }
        return b;
    };
```
  stl库里有map这个容器，一般都用它实现哈希表，注意一下map的基本用法
  
  如创建对象map<int,int> a，插入数值a.insert(<int,int>pair(nums[i],i)),验证表内是否有值a.count(target-nums[i])>0
  
  不过这个算法提交之后复杂度比暴力算法还高一些，估计应该是创建哈希表耗时太长，所以想到了以下算法
  
  在进行迭代并将元素插入到表中的同时，我们还会回过头来检查表中是否已经存在当前元素所对应的目标元素。如果它存在，那我们已经找到了对应解，并立即将其返回。这样就相当于只进行了一遍哈希表操作
  
  代码如下
```
vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> hashtable;
        hashtable[nums[0]]=0;
        for (int i = 1; i < nums.size(); ++i) {
            auto it = hashtable.find(target - nums[i]);
            if (it != hashtable.end()) {
                return {it->second, i};
            }
            hashtable[nums[i]]=i;
        }
        return {};
    }
```

