#MyLeetCode

这是记录leetcode刷题的文档，采用git管理。

第一部分是数据结构基础。

# 1. 数据结构基础

## 1.1 数组

### 1.1.1 删除有序数组中的重复项

题目说明：

```c++
给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

 

说明:

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以「引用」方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
 
示例 1：

输入：nums = [1,1,2]
输出：2, nums = [1,2]
解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。
示例 2：

输入：nums = [0,0,1,1,1,2,2,3,3,4]
输出：5, nums = [0,1,2,3,4]
解释：函数应该返回新的长度 5 ， 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4 。不需要考虑数组中超出新长度后面的元素。
 

提示：

0 <= nums.length <= 3 * 104
-104 <= nums[i] <= 104
nums 已按升序排列


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：

本来只想到了笨办法（其实也没怎么想。。。） 就是遍历嘛，两个指针，第一个指向第一个元素，第二个指向后一个元素，遍历数组，如果第二个指针指向的元素与第一个指针指向的元素相等，那么删除这个元素，当第二个指针指向的元素不再和第一个指针指向的元素相等，那么将第一个指针指向下一个，第二个元素指针指向它的下一个，重复上述过程至完成。

但是删除元素貌似开销比较大。

在给出的题解中，大部分的大体思路是统一的：**剔除重复元素，就是留下不重复的**。将题目反过来，不做删除，而是把不同的元素挑选出来。

也是采用双指针的思路。快指针用于向后遍历，慢指针用于存储快指针寻找到的不同的元素。

代码：

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) {
            return 0;
        }
        int fast = 1, slow = 1;
        while (fast < n) {
            if (nums[fast] != nums[fast - 1]) {
                nums[slow] = nums[fast];
                ++slow;
            }
            ++fast;
        }
        return slow;
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/solution/shan-chu-pai-xu-shu-zu-zhong-de-zhong-fu-tudo/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

上面为官方题解。下面是我受官方思路启发写的解，和官方区别不大。

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
                int n = nums.size();
        if (n == 0) {
            return 0;
        }
        int fast = 1, slow = 0;
        while (fast < n){
            if (nums[slow] != nums[fast]){
                slow++;
                nums[slow] = nums[fast];
            }
            fast++;
        }
        return ++slow;
    }
};
```

### 1.1.2 移除元素

问题描述：

```c++
给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

 

说明:

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以「引用」方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

// nums 是以“引用”方式传递的。也就是说，不对实参作任何拷贝
int len = removeElement(nums, val);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
 

示例 1：

输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2]
解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。
示例 2：

输入：nums = [0,1,2,2,3,0,4,2], val = 2
输出：5, nums = [0,1,4,0,3]
解释：函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。注意这五个元素可为任意顺序。你不需要考虑数组中超出新长度后面的元素。
 

提示：

0 <= nums.length <= 100
0 <= nums[i] <= 50
0 <= val <= 100


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-element
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：

我的想法是采用双指针，快指针用来遍历，慢指针用来存储，当快指针指向非指定值时，将其存入慢指针，否则慢指针不动，快指针向后遍历。

官方的第一种方法就是这样：

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int n = nums.size();
        int left = 0;
        for (int right = 0; right < n; right++) {
            if (nums[right] != val) {
                nums[left] = nums[right];
                left++;
            }
        }
        return left;
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/remove-element/solution/yi-chu-yuan-su-by-leetcode-solution-svxi/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

官方还给出了优化：

>左指针指向第一个元素，右指针指向第最后一个元素。
>
>如果左指针 left指向的元素等于 val，此时将右指针 right 指向的元素复制到左指针 left的位置，然后右指针 right左移一位。如果赋值过来的元素恰好也等于 val可以继续把右指针 right指向的元素的值赋值过来（左指针 left 指向的等于 val 的元素的位置继续被覆盖），直到左指针指向的元素的值不等于 val 为止。
>
>左右指针重合，遍历完成。

代码如下：

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int left = 0, right = nums.size();
        while (left < right) {
            if (nums[left] == val) {
                nums[left] = nums[right - 1];
                right--;
            } else {
                left++;
            }
        }
        return left;
    }
};

```

总结：

官方给出的第二种方法很有参考意义。对于我这种没什么编程思想，常常理不清思路的人启发尤其大。

首先是对右指针等于val的处理。我的第一想法就是，若右指针恰好也等于val，那么就抛掉这个值，右指针左移，再判断是否等于val，不等于的话再把这个值赋给左指针。这样显然增加了逻辑的复杂度，不够清晰，容易出错。实际上是完全没有必要的，这个处理和左侧判断并赋值的功能其实有些重叠的。

像官方这样写其实是最简洁明了的。

所以，以后碰到这种情况，需要思考，究竟是否需要两边判断。

### 1.1.3 搜索插入位置

题目描述：

```c++
给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 O(log n) 的算法。

 

示例 1:

输入: nums = [1,3,5,6], target = 5
输出: 2
示例 2:

输入: nums = [1,3,5,6], target = 2
输出: 1
示例 3:

输入: nums = [1,3,5,6], target = 7
输出: 4
示例 4:

输入: nums = [1,3,5,6], target = 0
输出: 0
示例 5:

输入: nums = [1], target = 0
输出: 0


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/search-insert-position
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：

我想的话，无非就是遍历，优化的话就是用二分查找。二分法的时间复杂度就是$O(logN)$

代码如下：

```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int n = nums.size();
        int left = 0, right = n-1;
        int mid;
        while (left <= right)
        {
            mid = (right-left)/2 +left;
            if (nums[mid] < target ){
                left = mid+1;
            }
            else {
                right = mid-1;
            }
        }
        return left;
    }
};
```

总结：

题目是比较简单的，终于有一道题我的思路是正确的了。

**但是！！**

我还是没有写对，主要还是出现在边界条件上。

脑子里简单过一遍觉得自己会了，其实上手写了之后才发现根本不对。

 我一开始写的代码，left 和 right 的迭代如下：

```c++
left = mid;
right = mid;
```

事实证明，这样会造成不收敛，程序死循环。

以后不管多简单的问题，都要仔仔细细地写出来呀，自己水平还没高到有思路就能写对呢。

重新查看二分查找算法，参考资料[在这里](http://data.biancheng.net/view/122.html)



### 1.1.4 最大子序和

题目描述：

```c++
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

 

示例 1：

输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
示例 2：

输入：nums = [1]
输出：1
示例 3：

输入：nums = [0]
输出：0
示例 4：

输入：nums = [-1]
输出：-1
示例 5：

输入：nums = [-100000]
输出：-100000


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/maximum-subarray
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：

对于我这个菜鸡来说，能想到的唯一思路就是双指针暴力算法。

暴力算法的代码如下(我自己没写，因为我虽然不会，但我确定肯定不是这种解法）：

```c++
class Solution
{
public:
    int maxSubArray(vector<int> &nums)
    {
        //类似寻找最大最小值的题目，初始值一定要定义成理论上的最小最大值
        int max = INT_MIN;
        int numsSize = int(nums.size());
        for (int i = 0; i < numsSize; i++)
        {
            int sum = 0;
            for (int j = i; j < numsSize; j++)
            {
                sum += nums[j];
                if (sum > max)
                {
                    max = sum;
                }
            }
        }

        return max;
    }
};

作者：pinku-2
链接：https://leetcode-cn.com/problems/maximum-subarray/solution/zui-da-zi-xu-he-cshi-xian-si-chong-jie-fa-bao-li-f/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



看解析，这道题应该用动态规划相关知识。嗯，我的第一道动态规划题（笑）。

动态规划思路解析如下：

![](https://raw.githubusercontent.com/Yetsang/PicBed/main/img/%E6%88%AA%E5%B1%8F2021-08-12%20%E4%B8%8B%E5%8D%886.02.53.png))

那么，对于我来说，最关键的思路就是第二段，求出第i个数结尾的连续子数组的最大和。后面的思路就水到渠成了。为什么不求第i个数开始的连续子数组的最大和，因为这样前面元素求出来的和后面元素无法直接利用，比较麻烦。

动态规划代码如下：

```c++
class Solution
{
public:
    int maxSubArray(vector<int> &nums)
    {
        //类似寻找最大最小值的题目，初始值一定要定义成理论上的最小最大值
        int result = INT_MIN;
        int numsSize = int(nums.size());
        //dp[i]表示nums中以nums[i]结尾的最大子序和
        vector<int> dp(numsSize);
        dp[0] = nums[0];
        result = dp[0];
        for (int i = 1; i < numsSize; i++)
        {
            dp[i] = max(dp[i - 1] + nums[i], nums[i]);
            result = max(result, dp[i]);
        }

        return result;
    }
};

作者：pinku-2
链接：https://leetcode-cn.com/problems/maximum-subarray/solution/zui-da-zi-xu-he-cshi-xian-si-chong-jie-fa-bao-li-f/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

这样的动态规划算法中，还隐藏着递归的思想。

例如，对于下图中数组：

![](https://github.com/Yetsang/PicBed/blob/main/img/截屏2021-08-12%20下午6.14.49.png?raw=true)

例如第三个元素`[-6]`, dp[1]已经给出了-6前元素能得到的最大值，到-6这里就只需要考虑：前面得到的最大值加上-6 和 -6本身究竟谁大，谁就是以-6结尾的子序列的最大和，也就是dp[2]的值。

之所以解释是因为开始看到这个解法的时候，总觉得人家没验证到所有条件，因为算法是从头逐个遍历过来的，那[3,-6]这样的不是从头开始的子序列是不是没考虑到呀？这肯定是错觉，因为前面已经解释了，dp[1]的值给出了-6前元素能得到的最大值，而这个值肯定是大于或等于3的。那么用这个最大值和-6相加 一定大于 3 和-6 相加。因此，非要说的话，这是一种“隐性”的考虑吧。

因为只用到了当前项的前一项的dp值，因此可以采用一个整型值代替dp数组，降低空间复杂度。

代码 如下：

```c++
class Solution
{
public:
    int maxSubArray(vector<int> &nums)
    {
        //类似寻找最大最小值的题目，初始值一定要定义成理论上的最小最大值
        int result = INT_MIN;
        int numsSize = int(nums.size());
        //因为只需要知道dp的前一项，我们用int代替一维数组
        int dp(nums[0]);
        result = dp;
        for (int i = 1; i < numsSize; i++)
        {
            dp = max(dp + nums[i], nums[i]);
            result = max(result, dp);
        }

        return result;
    }
};

作者：pinku-2
链接：https://leetcode-cn.com/problems/maximum-subarray/solution/zui-da-zi-xu-he-cshi-xian-si-chong-jie-fa-bao-li-f/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 1.1.5 加一

题目描述：

```c
给定一个由 整数 组成的 非空 数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

 

示例 1：

输入：digits = [1,2,3]
输出：[1,2,4]
解释：输入数组表示数字 123。
示例 2：

输入：digits = [4,3,2,1]
输出：[4,3,2,2]
解释：输入数组表示数字 4321。
示例 3：

输入：digits = [0]
输出：[1]
 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/plus-one
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

代码：

```c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        for(int i=digits.size()-1;i>=0;i--)
        {
            ++digits[i];

            if(digits[i]!=10)        //如果该位没有继续产生进位,则直接return 结果
                return digits;
            else
                digits[i]=0;        //进位
        }
        digits.insert(digits.begin(), 1);       //如果首位也产生进位,才会执行到这条语句，否则循环中已经return了
        return digits;
    }
};
```

### 1.1.6 合并两个有序数组

问题描述：

```c++
给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。

初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。你可以假设 nums1 的空间大小等于 m + n，这样它就有足够的空间保存来自 nums2 的元素。

 

示例 1：

输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
示例 2：

输入：nums1 = [1], m = 1, nums2 = [], n = 0
输出：[1]
 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/merge-sorted-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：

采用双指针，另建一个m+n的数组，指向两个数组的头部，比较，将较小的放入暂存数组。

代码如下：

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int p1 = 0, p2 = 0;
        int sorted[m + n];
        int cur;
        while (p1 < m || p2 < n) {
            if (p1 == m) {
                cur = nums2[p2++];
            } else if (p2 == n) {
                cur = nums1[p1++];
            } else if (nums1[p1] < nums2[p2]) {
                cur = nums1[p1++];
            } else {
                cur = nums2[p2++];
            }
            sorted[p1 + p2 - 1] = cur;
        }
        for (int i = 0; i != m + n; ++i) {
            nums1[i] = sorted[i];
        }
    }
};
```

看官方文件，还有逆向双指针的方法，不需要暂存数组。其基本思想是，将双指针指向两个数组有效元素的最后部分。项前迭代，将较大的元素放在数组1的末尾。

代码如下：

```c++

class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int p1 = m - 1, p2 = n - 1;
        int tail = m + n - 1;
        int cur;
        while (p1 >= 0 || p2 >= 0) {
            if (p1 == -1) {
                cur = nums2[p2--];
            } else if (p2 == -1) {
                cur = nums1[p1--];
            } else if (nums1[p1] > nums2[p2]) {
                cur = nums1[p1--];
            } else {
                cur = nums2[p2--];
            }
            nums1[tail--] = cur;
        }
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/merge-sorted-array/solution/he-bing-liang-ge-you-xu-shu-zu-by-leetco-rrb0/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## 1.2 字符串

### 1.2.1 罗马数字转整数

题目描述：

```c++
罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。

字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

 

示例 1:

输入: "III"
输出: 3
示例 2:

输入: "IV"
输出: 4
示例 3:

输入: "IX"
输出: 9
示例 4:

输入: "LVIII"
输出: 58
解释: L = 50, V= 5, III = 3.
示例 5:

输入: "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/roman-to-integer
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：

我的思路是就是按照各种情况，从头往下写，用if else 把所有情况都表示出来，但是这种方法太笨了。代码类似下面的：

```c
/**
 * @param {string} s
 * @return {number}
 */
var romanToInt = function(s) {
  const n = s.length;
  let i = 0;
  let res = 0;
  while(i < n) {
    if (s[i] === 'M') {
      res += 1000;
      i++;
    } else if (s[i] === 'C' && s[i+1] === 'M') {
      res += 900;
      i += 2;
    } else if (s[i] === 'D') {
      res += 500;
      i++;
    } else if (s[i] === 'C' && s[i+1] === 'D') {
      res += 400;
      i += 2;
    } else if (s[i] === 'C') {
      res += 100;
      i++;
    } else if (s[i] === 'X' && s[i+1] === 'C') {
      res += 90;
      i += 2;
    } else if (s[i] === 'L') {
      res += 50;
      i++;
    } else if (s[i] === 'X' && s[i+1] === 'L') {
      res += 40;
      i += 2;
    } else if (s[i] === 'X') {
      res += 10;
      i++;
    } else if (s[i] === 'I' && s[i+1] === 'X') {
      res += 9;
      i += 2;
    } else if (s[i] === 'V') {
      res += 5;
      i++;
    } else if (s[i] === 'I' && s[i+1] === 'V') {
      res += 4;
      i += 2;
    } else if (s[i] === 'I') {
      res += 1;
      i++;
    } 
  }
  return res;
};
```

题解中的思路更优秀。思路如下：

![](/Users/geyishan/Library/Mobile Documents/com~apple~CloudDocs/截屏2021-08-16 下午2.44.47.png)

官方代码如下：

```c++
class Solution {
private:
    unordered_map<char, int> symbolValues = {
        {'I', 1},
        {'V', 5},
        {'X', 10},
        {'L', 50},
        {'C', 100},
        {'D', 500},
        {'M', 1000},
    };

public:
    int romanToInt(string s) {
        int ans = 0;
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            int value = symbolValues[s[i]];
            if (i < n - 1 && value < symbolValues[s[i + 1]]) {
                ans -= value;
            } else {
                ans += value;
            }
        }
        return ans;
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/roman-to-integer/solution/luo-ma-shu-zi-zhuan-zheng-shu-by-leetcod-w55p/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

首先用哈希表([unordered_map](http://c.biancheng.net/view/7231.html))将字符对应的数值存储起来，然后根据规律写出循环，很快就能得到结果。我果然是个笨比。

### 1.2.2 最长公共前缀

题目描述：

```c++
编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

 

示例 1：

输入：strs = ["flower","flow","flight"]
输出："fl"
示例 2：

输入：strs = ["dog","racecar","car"]
输出：""
解释：输入不存在公共前缀。
 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-common-prefix
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：

就是对每个字符串，先从第一个字符开始，若相等，则继续扫描各个字符串的第二个。直到遇到不相等或字符串结束。

这也是题解中给出的解法之一，代码如下：

```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (!strs.size()) {
            return "";
        }
        int length = strs[0].size();
        int count = strs.size();
        for (int i = 0; i < length; ++i) {
            char c = strs[0][i];
            for (int j = 1; j < count; ++j) {
                if (i == strs[j].size() || strs[j][i] != c) {
                    return strs[0].substr(0, i);
                }
            }
        }
        return strs[0];
    }
};

```

本题目中，主要应用了 [vector](https://www.w3cschool.cn/cpp/cpp-i6da2pq0.html) 和 [string](http://c.biancheng.net/cpp/biancheng/view/3284.html) 两个数据结构。

其他解法：

包括横向扫描法和二分法等，后续再看。

### 1.2.3 有效括号

题目描述：

```c++
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
 

示例 1：

输入：s = "()"
输出：true
示例 2：

输入：s = "()[]{}"
输出：true
示例 3：

输入：s = "(]"
输出：false
示例 4：

输入：s = "([)]"
输出：false
示例 5：

输入：s = "{[]}"
输出：true
 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/valid-parentheses
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：

我的想法是利用栈，监测到是左括号，入栈，检测到是右括号，出栈。

具体来说就是从前向后遍历，左括号入栈，右括号和栈顶比较，匹配出栈，否则返回错误；遍历完栈非空，返回错误；或者遍历到右括号，栈空，返回错误；否则是正确的。

那么为了做好匹配工作，可以采用unordered_map存储，key为左括号，value为对应的右括号。

感动中国，第一次和官方思路一致。

```c++
class Solution {
public:
    bool isValid(string s) {
        int n = s.size();
        if (n % 2 == 1) {
            return false;
        }

        unordered_map<char, char> pairs = {
            {')', '('},
            {']', '['},
            {'}', '{'}
        };
        stack<char> stk;
        for (char ch: s) {
            if (pairs.count(ch)) { //如果是右括号
                if (stk.empty() || stk.top() != pairs[ch]) {
                    return false;
                }
                stk.pop();
            }
            else {
                stk.push(ch);
            }
        }
        return stk.empty();
    }
};

```

另外，**要在构思时就列出错误情况，以免遗漏**。

本题中主要用到的数据结构是[栈](http://c.biancheng.net/view/478.html)。

### 1.2.4 strstr

题目描述：

```c++
实现 strStr() 函数。

给你两个字符串 haystack 和 needle ，请你在 haystack 字符串中找出 needle 字符串出现的第一个位置（下标从 0 开始）。如果不存在，则返回  -1 。

 

说明：

当 needle 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 needle 是空字符串时我们应当返回 0 。这与 C 语言的 strstr() 以及 Java 的 indexOf() 定义相符。

 

示例 1：

输入：haystack = "hello", needle = "ll"
输出：2
示例 2：

输入：haystack = "aaaaa", needle = "bba"
输出：-1
示例 3：

输入：haystack = "", needle = ""
输出：0
 



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/implement-strstr
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：

暴力解法：

取needle的第一个字符，在haystack中寻找着一个字符，未找到，返回-1；找到后，从该位置取needle长度的子字符串和needle相比，相等则返回当前下标；否则继续寻找，遍历完成仍未找到，则返回-1。

官方给出的暴力解法和这个差不多，代码如下：

```c++
class Solution {
public:
    int strStr(string haystack, string needle) {
        int n = haystack.size(), m = needle.size();
        for (int i = 0; i + m <= n; i++) {
            bool flag = true;
            for (int j = 0; j < m; j++) {
                if (haystack[i + j] != needle[j]) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                return i;
            }
        }
        return -1;
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/implement-strstr/solution/shi-xian-strstr-by-leetcode-solution-ds6y/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

而真正意义上要讲的算法是KMP算法；

这个算法我觉得非常难，前后看了三篇教程才看懂：

1.[官方题解](https://leetcode-cn.com/problems/implement-strstr/solution/shi-xian-strstr-by-leetcode-solution-ds6y/)远离性讲得比较清楚，主要是从数学上讲的，比较难懂，但是简历了一些概念。

2.宫水三叶大佬的[这篇解释](https://leetcode-cn.com/problems/implement-strstr/solution/shua-chuan-lc-shuang-bai-po-su-jie-fa-km-tb86/)图画的很好，很清楚地解释了KMP算法和暴力算法过程的不同。但是next的实现部分我觉得讲的太少了。

3.[这篇文章](https://leetcode-cn.com/problems/implement-strstr/solution/tu-jie-kmp-zi-fu-chuan-pi-pei-wen-ti-by-c94sh/)算是对2的很好对补充，next的由来讲的很详细。

理解完成后，又翻到了[这位大佬的教程](https://leetcode-cn.com/problems/implement-strstr/solution/dai-ma-sui-xiang-lu-kmpsuan-fa-xiang-jie-mfbs/)，感觉更有体系一些。他的下面这张动图很好地解释了next数组的生成。



![KMP精讲3.gif](https://raw.githubusercontent.com/Yetsang/PicBed/main/img/1599638458-sHaHqX-KMP%E7%B2%BE%E8%AE%B23-20210816210006421.gif)

配合上图，[参考资料3](https://leetcode-cn.com/problems/implement-strstr/solution/tu-jie-kmp-zi-fu-chuan-pi-pei-wen-ti-by-c94sh/)中的下面这段代码是我认为最好理解的代码。代码如下：

```c++
void GetNext(vector<int>& next, const string& p){
    //i表示后缀尾，j表示前缀尾
    //同时j表示i之前的最长相等前后缀长度
    int i = 1, j = 0;
    next[0] = 0;
    for(;i<p.size(); ++i){
        while(j>0 && p[i] != p[j]){ 
            //当i和j不匹配时，j向前跳转
            //当j已经跳转到开头时，不再跳转，防止死循环
            j = next[j-1];
        }
        if(p[i] == p[j]) j++;
        next[i] = j;
    }
}

作者：horizon-29
链接：https://leetcode-cn.com/problems/implement-strstr/solution/tu-jie-kmp-zi-fu-chuan-pi-pei-wen-ti-by-c94sh/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

这位大佬写的匹配代码也是我认为最简单的了。

```c++
int KMP(const string& s, const string& p){
    vector<int> next(p.size(),0);
    GetNext(next,p);
    for(int i=0,j=0; i<s.size(); ++i){
        while(j > 0 && s[i] != p[j]){
            j = next[j-1];
        }
        if(s[i] == p[j]){
            j++;
        }
        if(j == p.size())   return i - p.size() + 1;
    }
    return -1;
}

作者：horizon-29
链接：https://leetcode-cn.com/problems/implement-strstr/solution/tu-jie-kmp-zi-fu-chuan-pi-pei-wen-ti-by-c94sh/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 1.2.5 excel 列表名字

题目描述：

```c++
给你一个整数 columnNumber ，返回它在 Excel 表中相对应的列名称。

例如：

A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
 

示例 1：

输入：columnNumber = 1
输出："A"
示例 2：

输入：columnNumber = 28
输出："AB"
示例 3：

输入：columnNumber = 701
输出："ZY"
示例 4：

输入：columnNumber = 2147483647
输出："FXSHRXW"
 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/excel-sheet-column-title
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：这道题我乍一看本来以为非常简单的26进制，但事实证明我想简单了。这个数值表示是1～26而不是0～25.

这就造成如果像求解二进制一样求解的话，如26，除以26，余数为0，没有对应的字母。因此需要想办法对应起来。

给出的办法是：输入数字减1，在对26求余，得到的余数加上A即使当前的字母。如26-1=25，25%26=25，A+25=Z;

最后一步的加法是string 和 字符串的性质。

代码如下：

```c++
class Solution {
public:
    string convertToTitle(int columnNumber) {
        string ans;
        while (columnNumber > 0) {
            --columnNumber;
            ans += columnNumber % 26 + 'A';
            columnNumber /= 26;
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/excel-sheet-column-title/solution/excelbiao-lie-ming-cheng-by-leetcode-sol-hgj4/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 1.2.6 无重复的最长子串

题目描述：

```c++
给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。

 

示例 1:

输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
示例 2:

输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
示例 3:

输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
示例 4:

输入: s = ""
输出: 0
 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-substring-without-repeating-characters
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：采用滑动窗口法。这个我以前做过，有印象。

具体为：采用set结构存储非重复字符。取left指向0位置。遍历字符串，若遇到set中存在的字符，将set中的s[left]删掉。left++，这一阶段的子字符串长度为i-left+1。从头遍历到尾，得到的最大长度即为所求。

**注意：本题目中的移除s[left]的操作，是在只求长度的情况下才成立的。** 比如题目要求中的第三个例子，遍历到pww时，存在重复元素。移除s[left]移除的是p，留下的是ww。这样本阶段最长子字符串的长度不变，但是如果要返回最长子字符串的话就不对了。

我自己的代码就不贴了，太难看，参考代码如下：

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        // 哈希集合，记录每个字符是否出现过
        unordered_set<char> occ;
        int n = s.size();
        // 右指针，初始值为 -1，相当于我们在字符串的左边界的左侧，还没有开始移动
        int rk = -1, ans = 0;
        // 枚举左指针的位置，初始值隐性地表示为 -1
        for (int i = 0; i < n; ++i) {
            if (i != 0) {
                // 左指针向右移动一格，移除一个字符
                occ.erase(s[i - 1]);
            }
            while (rk + 1 < n && !occ.count(s[rk + 1])) {
                // 不断地移动右指针
                occ.insert(s[rk + 1]);
                ++rk;
            }
            // 第 i 到 rk 个字符是一个极长的无重复字符子串
            ans = max(ans, rk - i + 1);
        }
        return ans;
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/solution/wu-zhong-fu-zi-fu-de-zui-chang-zi-chuan-by-leetc-2/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 1.2.7 最长回文子字符串

题目描述：

```c++
示例 1：

输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
示例 2：

输入：s = "cbbd"
输出："bb"
示例 3：

输入：s = "a"
输出："a"
示例 4：

输入：s = "ac"
输出："a"
 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-palindromic-substring
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：

主要思路有两种：

第一种为动态规划方法。

![截屏2021-08-17 上午11.38.42](https://raw.githubusercontent.com/Yetsang/PicBed/main/img/%E6%88%AA%E5%B1%8F2021-08-17%20%E4%B8%8A%E5%8D%8811.38.42.png)

上图显示算法的代码如下：

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        if (n < 2) {
            return s;
        }

        int maxLen = 1;
        int begin = 0;
        // dp[i][j] 表示 s[i..j] 是否是回文串
        vector<vector<int>> dp(n, vector<int>(n));
        // 初始化：所有长度为 1 的子串都是回文串
        for (int i = 0; i < n; i++) {
            dp[i][i] = true;
        }
        // 递推开始
        // 先枚举子串长度
        for (int L = 2; L <= n; L++) {
            // 枚举左边界，左边界的上限设置可以宽松一些
            for (int i = 0; i < n; i++) {
                // 由 L 和 i 可以确定右边界，即 j - i + 1 = L 得
                int j = L + i - 1;
                // 如果右边界越界，就可以退出当前循环
                if (j >= n) {
                    break;
                }

                if (s[i] != s[j]) {
                    dp[i][j] = false;
                } else {
                    if (j - i < 3) {
                        dp[i][j] = true; //只有两个元素，两元素相等则为回文
                    } else {
                        dp[i][j] = dp[i + 1][j - 1]; //超过两元素，取决于它“里面”一层的字符串是否是回文
                    }
                }

                // 只要 dp[i][L] == true 成立，就表示子串 s[i..L] 是回文，此时记录回文长度和起始位置
                if (dp[i][j] && j - i + 1 > maxLen) {
                    maxLen = j - i + 1;
                    begin = i;
                }
            }
        }
        return s.substr(begin, maxLen);
    }
};
```

第二种方法为采用中间扩展法。就是从中间向两端扩展，直到两端不等，扩展停止。

代码如下：

```c++
class Solution {
public:
    pair<int, int> expandAroundCenter(const string& s, int left, int right) {
        while (left >= 0 && right < s.size() && s[left] == s[right]) {
            --left;
            ++right;
        }
        return {left + 1, right - 1};
    }

    string longestPalindrome(string s) {
        int start = 0, end = 0;
        for (int i = 0; i < s.size(); ++i) {
            auto [left1, right1] = expandAroundCenter(s, i, i);
            auto [left2, right2] = expandAroundCenter(s, i, i + 1);
            if (right1 - left1 > end - start) {
                start = left1;
                end = right1;
            }
            if (right2 - left2 > end - start) {
                start = left2;
                end = right2;
            }
        }
        return s.substr(start, end - start + 1);
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/longest-palindromic-substring/solution/zui-chang-hui-wen-zi-chuan-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

**注意 有两种扩展起源**

### 1.2.8 整数转罗马数字

题目描述：

```c++
罗马数字包含以下七种字符： I， V， X， L，C，D 和 M。

字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
给你一个整数，将其转为罗马数字。

 

示例 1:

输入: num = 3
输出: "III"
示例 2:

输入: num = 4
输出: "IV"
示例 3:

输入: num = 9
输出: "IX"
示例 4:

输入: num = 58
输出: "LVIII"
解释: L = 50, V = 5, III = 3.
示例 5:

输入: num = 1994
输出: "MCMXCIV"
解释: M = 1000, CM = 900, XC = 90, IV = 4.
 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/integer-to-roman
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：

我的思路是：按位转化。

通过除法和取余相结合，取出每一位。对于千位来说，只能是0123，那么就可以利用for循环，有几个千位就加几个M，对于其他的位（拿个位举例），首先看是不是4或9，然后再看是否大于等于5，小于5，（不可能为4），则用for循环，加I，大于等于5，则对5取余，写成V+余数个I。

这样的做法思路简单，但是ifelse 情况比较多，容易写乱。

官方给出的参考程序很好地解决了这个问题,且执行效率快得多。

```c++
const string thousands[] = {"", "M", "MM", "MMM"};
const string hundreds[]  = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
const string tens[]      = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
const string ones[]      = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};

class Solution {
public:
    string intToRoman(int num) {
        return thousands[num / 1000] + hundreds[num % 1000 / 100] + tens[num % 100 / 10] + ones[num % 10];
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/integer-to-roman/solution/zheng-shu-zhuan-luo-ma-shu-zi-by-leetcod-75rs/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

官方题解中的另一种方法其实也是我的思路的优化版，将复杂的if else 判断用简单的循环写出。代码如下：

```c++
const pair<int, string> valueSymbols[] = {
    {1000, "M"},
    {900,  "CM"},
    {500,  "D"},
    {400,  "CD"},
    {100,  "C"},
    {90,   "XC"},
    {50,   "L"},
    {40,   "XL"},
    {10,   "X"},
    {9,    "IX"},
    {5,    "V"},
    {4,    "IV"},
    {1,    "I"},
};

class Solution {
public:
    string intToRoman(int num) {
        string roman;
        for (const auto &[value, symbol] : valueSymbols) {
            while (num >= value) {
                num -= value;
                roman += symbol;
            }
            if (num == 0) {
                break;
            }
        }
        return roman;
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/integer-to-roman/solution/zheng-shu-zhuan-luo-ma-shu-zi-by-leetcod-75rs/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

# 2. 算法



## 2.1 动态规划

### 2.1.1 背包问题

本部分内容参考[代码随想录的github项目](https://github.com/youngyangyang04/leetcode-master)内容进行学习，这里记录心得。

在背包问题中，先介绍了二维数组的写法，又介绍了一维数组的写法。

两者原理类似，最大的不同在于，遍历时，二维数组可以从前到后遍历，而一位数组必须要从后往前遍历。

理解如下：

无论是一维数组还是二维数组，当前遍历都需要用到**上一次遍历**的数据。

在二维数组中，上一次遍历的数据存储在本次遍历计算的**上一行**，无论本次计算结果如何，都不会影响上一次计算的结果，本次遍历中 每一个元素使用的上一次遍历的数据，都是实实在在的上一次遍历的数据。因此二维数组下，正向遍历和反向遍历都不会有影响。

在一维数组中，上一次遍历的数据和本次需要计算的数据存储在同一位置。如果从前向后遍历，新计算的结果就会将上次遍历的结果覆盖；而后面的计算还需要前面元素的**上次遍历结果**，但此时取到的数据已经是**此次遍历**的数据了。对于本题目来说，若使用一维数组存储，那么当前元素的值只与当前元素前面值有关，后面发生变化对当前元素不会有影响。因此，从后往前遍历，可以保证每个遍历到的元素计算需要的**上一次遍历结果**都是**未经更改的上一次遍历结果**。

另外，在二维数组写法中 ，代码中进行了判断，如果背包重量小于物品重量，直接复制上次遍历值。而在一维数组中，直接讲循环停止条件设置为背包重量小于物品重量就停止。这样前面的元素依旧保留上次遍历的结果，省去了复制操作的开销。

两种写法的代码如下：

二维数组方法：

```c++
void test_2_wei_bag_problem1() {
    vector<int> weight = {1, 3, 4};
    vector<int> value = {15, 20, 30};
    int bagWeight = 4;

    // 二维数组
    vector<vector<int>> dp(weight.size() + 1, vector<int>(bagWeight + 1, 0));

    // 初始化
    for (int j = weight[0]; j <= bagWeight; j++) {
        dp[0][j] = value[0];
    }

    // weight数组的大小 就是物品个数
    for(int i = 1; i < weight.size(); i++) { // 遍历物品
        for(int j = 0; j <= bagWeight; j++) { // 遍历背包容量
            if (j < weight[i]) dp[i][j] = dp[i - 1][j];
            else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);

        }
    }

    cout << dp[weight.size() - 1][bagWeight] << endl;
}

int main() {
    test_2_wei_bag_problem1();
}
```

一维数组方法：

```c++
void test_1_wei_bag_problem() {
    vector<int> weight = {1, 3, 4};
    vector<int> value = {15, 20, 30};
    int bagWeight = 4;

    // 初始化
    vector<int> dp(bagWeight + 1, 0);
    for(int i = 0; i < weight.size(); i++) { // 遍历物品
        for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
        }
    }
    cout << dp[bagWeight] << endl;
}

int main() {
    test_1_wei_bag_problem();
}


```

### 2.1.2 等和数组问题（01背包变体）

题目描述：

```c
给你一个 只包含正整数 的 非空 数组 nums 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

 

示例 1：

输入：nums = [1,5,11,5]
输出：true
解释：数组可以分割成 [1, 5, 5] 和 [11] 。
示例 2：

输入：nums = [1,2,3,5]
输出：false
解释：数组不能分割成两个元素和相等的子集。
 

提示：

1 <= nums.length <= 200
1 <= nums[i] <= 100


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/partition-equal-subset-sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

代码：

```c++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
    //转化为动态规划背包问题实现
    //背包容量为sum/2；
    //待放入物品就是集合中的各个元素；
    //物品的重量和价值都是元素的数值；
    //如果最终存在价值为sum/2的情况，则返回true，否则返回false

    //创建dp数组，dp[j]表示容量为j时，背包内物品最大值
    //根据提示，sum最大为20000，因此背包容量为10001即可。（包括0）
    vector<int> dp (10001,0);

    //转移方程推导：
    //对于待放入元素nums[i]，两种情况：
    //1. 重量大于背包容量，放不进去，dp[j] 维持原值不变；
    //2. 重量小于背包容量，两种选择：
    //  （1）不放进去，dp[j]维持原值
    //  （2）放进去，并将剩余空间价值最大化 dp[j] = dp[j-nums[i]]+nums[i];
    //两种情况取较大值。

    //初始化
    //全都初始化为0即可：如果如果题目给的价值都是正整数那么非0下标都初始化为0就可以了，如果题目给的价值有负数，那么非0下标就要初始化为负无穷。

    //确定遍历顺序：
    //j从后向前

    //计算
    int bagweight=0;
    for(int i = 0; i<nums.size();i++){
        bagweight += nums[i];
    }
    if (bagweight%2 == 1) return false;

    bagweight = bagweight/2;   //背包容量

    for (int i=0; i<nums.size(); i++){
        for (int j = bagweight; j >= nums[i]; j--){
            dp[j] = max(dp[j],dp[j-nums[i]]+nums[i]);
        }
    }
    if (dp[bagweight] == bagweight) return true;
    else return false;

    }
};
```

总结：

边界很重要，遍历的时候一定要写对边界。

`dp[bagweight] == bagweight`是本题的另一个关键点。因为在本题目中，重量和价值是一样的，所以如果存在和为sum/2（bagweight）的情况，即价值为bagweight，那么重量也一定是bagweight，即`dp[bagweight] == bagweight`是唯一可能存在的满足等和数组分割的情况。因此不需要在循环中判断`dp[j] == bagweight`是否存在，而只需要在迭代结束后进行一次判断即可。

### 2.1.3 最后一块石头的重量（类似等和数组）

题目描述：

```c
有一堆石头，用整数数组 stones 表示。其中 stones[i] 表示第 i 块石头的重量。

每一回合，从中选出任意两块石头，然后将它们一起粉碎。假设石头的重量分别为 x 和 y，且 x <= y。那么粉碎的可能结果如下：

如果 x == y，那么两块石头都会被完全粉碎；
如果 x != y，那么重量为 x 的石头将会完全粉碎，而重量为 y 的石头新重量为 y-x。
最后，最多只会剩下一块 石头。返回此石头 最小的可能重量 。如果没有石头剩下，就返回 0。

 

示例 1：

输入：stones = [2,7,4,1,8,1]
输出：1
解释：
组合 2 和 4，得到 2，所以数组转化为 [2,7,1,8,1]，
组合 7 和 8，得到 1，所以数组转化为 [2,1,1,1]，
组合 2 和 1，得到 1，所以数组转化为 [1,1,1]，
组合 1 和 1，得到 0，所以数组转化为 [1]，这就是最优值。
示例 2：

输入：stones = [31,26,33,21,40]
输出：5
示例 3：

输入：stones = [1,2]
输出：1
 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/last-stone-weight-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

代码如下：

```c++
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
    //实质就是将石头分成重量尽可能小的两堆，类似于等和子数组
    //因此可以转化为背包问题，石头的重量和value都是stone[i]。
    //创建dp数组，长度待优化，目前为所需最大长度：3000/2 = 1500；
    //vector<int> dp (1501,0);

    int sum = 0;
    for (int i = 0; i < stones.size(); i++ ){
        sum += stones[i];
    }

    int bagweight = sum/2; //偶数整除，奇数向下取整

    vector<int> dp (bagweight+1,0);

    for (int i = 0; i < stones.size(); i++ ){
        for (int j = bagweight; j >= stones[i]; j--){
            dp[j] = max(dp[j],dp[j-stones[i]]+stones[i]);
        }
    }

    //上述循环求出的dp[bagweight]就是容量为bagweight的背包能装下的最大重量。
    //剩余重量为sum - dp[bagweight];
    //因为bagweight为向下取整，所以sum - dp[bagweight]始终大于或等于dp[bagweight]；
    return sum - dp[bagweight]-dp[bagweight];
    }
};
```

总结：感觉实现并不难，难的是思路分析，如何把这个问题转化成动态规划问题。以及动态规划算法最终得到的结果该如何转化为题目要求的结果。

### 2.1.4 目标和

题目描述：

```c
给你一个整数数组 nums 和一个整数 target 。

向数组中的每个整数前添加 '+' 或 '-' ，然后串联起所有整数，可以构造一个 表达式 ：

例如，nums = [2, 1] ，可以在 2 之前添加 '+' ，在 1 之前添加 '-' ，然后串联起来得到表达式 "+2-1" 。
返回可以通过上述方法构造的、运算结果等于 target 的不同 表达式 的数目。

 

示例 1：

输入：nums = [1,1,1,1,1], target = 3
输出：5
解释：一共有 5 种方法让最终目标和为 3 。
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
示例 2：

输入：nums = [1], target = 1
输出：1
 

提示：

1 <= nums.length <= 20
0 <= nums[i] <= 1000
0 <= sum(nums[i]) <= 1000
-1000 <= target <= 1000


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/target-sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

代码如下：

```c++
#include <iostream>
#include <vector>

using namespace std;

int findTargetSumWays(vector<int>& nums, int target) {
    //转化为动态规划问题：
    /*
    思路分析：设数组和为sum，最后组合式中，正数和为pos，负数和为sum-pos;
    因此： pos-(sum-pos) = S
    即：   pos = (S+sum)/2;
    从而，pos 的值也确定了，问题转化为从数组中选择元素，使其和为pos，求共有多少种方法；
    这种求和问题类似前面做过的等和数组和石头分堆问题，但又有所不同；前两个问题都是求背包最大价值（容量），而本问题求共多少种方法；
    因此本问题是一个组合问题。
    nums[i]为第i个元素的重量及价值。
    */

    int sum = 0;
    for (int i = 0; i < nums.size(); i++){
        sum += nums[i];
    }

    int bagweight; //背包容量

    if (target > sum) return 0; //这种情况下，pos和大于sum，显然不成立；
    if ((target + sum)%2 == 1) return 0;   //pos必须为正整数，否则不成立;

    bagweight = (target + sum) / 2;

    if(bagweight < 0) return 0;  //这种情况没有考虑到，一直不通过

    //创建dp，dp[j]表示填满容量为j的背包，共有多少种方法。
    vector<int> dp (bagweight + 1, 0);

    //转移方程：
    /*
    对于待放入元素nums[i],首先应满足其重量小于背包容量；本问题中，求解的是将包填满的方法数量，因此在这里应该只考虑填满的情况
    例如，背包容量为5，待放入元素为3，那么此种情况下，能填满的方法数为dp[5-3],当前总的填满容量5的包的方法就是原来的每次对i迭
    代的方法数之和，即dp[j] += dp[j-nums[i]];
    */

    //初始化
    dp[0] = 1; //特别注意，若是初始化为0，那么所有元素都永远是零。

    for (int i = 0; i < nums.size(); i++){
        for (int j = bagweight; j >= nums[i]; j--){
            dp[j] += dp[j-nums[i]];
        }
    }
    cout<<dp[bagweight]<<endl;

    return dp[bagweight];

}
int main() {
    vector <int> test (1,100);
    findTargetSumWays(test,-200);

}

```

### 2.1.5 一和零

题目描述：

```c
给你一个二进制字符串数组 strs 和两个整数 m 和 n 。

请你找出并返回 strs 的最大子集的大小，该子集中 最多 有 m 个 0 和 n 个 1 。

如果 x 的所有元素也是 y 的元素，集合 x 是集合 y 的 子集 。

 

示例 1：

输入：strs = ["10", "0001", "111001", "1", "0"], m = 5, n = 3
输出：4
解释：最多有 5 个 0 和 3 个 1 的最大子集是 {"10","0001","1","0"} ，因此答案是 4 。
其他满足题意但较小的子集包括 {"0001","1"} 和 {"10","1","0"} 。{"111001"} 不满足题意，因为它含 4 个 1 ，大于 n 的值 3 。
示例 2：

输入：strs = ["10", "0", "1"], m = 1, n = 1
输出：2
解释：最大的子集是 {"0", "1"} ，所以答案是 2 。
 

提示：

1 <= strs.length <= 600
1 <= strs[i].length <= 100
strs[i] 仅由 '0' 和 '1' 组成
1 <= m, n <= 100


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ones-and-zeroes
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

**分析：这道题乍一看是多重背包问题，但是实际上可以化为01背包问题**

代码如下：

```c++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
    
    //动态规划 01背包问题
    //物品为字符串，背包为二维背包，m个0和n个1.

    //dp[i][j]表示含有i个0和j个1的最大子集大小
    //*注意* 这里的二维数组和前面做过的可以简化为一维数组的是不一样的。
    vector<vector<int>> dp(m + 1 , vector<int> (n + 1 , 0));

    //转移公式：
    /*
    对于当前待放入背包字符串，其1的个数为oneNum，0的个数为zeroNum。
    其dp值计算如下：
    1.首先待放入背包字符串应小于背包容量，否则放不进去，dp维持原状。
    2.待放入字符串满足容量要求，有两种情况，放入或者不放入；
      不放入：dp[i][j] 维持上次迭代数据；
      放入：  背包容量减掉当前字符串“重量”，并使剩余空间价值最大化，即剩余空间最大子集个数，
             则根据dp定义：dp[i][j] = dp[i-oneNum][j-zeroNum] + 1 
      上述两种情况取较大值。
    */
    //初始化：dp 初始化为0即可，而对于组合问题，需要考虑为0是否合适

    //遍历顺序： 先遍历物品，再遍历背包，背包遍历从后向前

    for (string str : strs) { // 遍历物品
            int oneNum = 0, zeroNum = 0;
            for (char c : str) {
                if (c == '0') zeroNum++;
                else oneNum++;
            }
            for (int i = m; i >= zeroNum; i--) { // 遍历背包容量且从后向前遍历！
                for (int j = n; j >= oneNum; j--) {
                    dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);
                }
            }
        }
    return dp[m][n];
    }
};
```

### 2.1.6 完全背包问题

完全背包和01背包的唯一区别就是每种物品有**无数**多个，可以放入背包**多次**。

具体到代码实现中，和01背包一样，dp数组可以用一维或二维数组实现。但是细节上有不同：

* 遍历顺序问题：

  对于01背包：若是使用二维数组表示，可以从前向后遍历。**但是若采用一维数组遍历，则必须从后向前遍历**。可以从两个方向来理解。

  1.从防止重复放入物品角度，如果采用正向遍历，**则会将已经放入背包中的物品重复放入**。

  例如: 

  对于物品0,重量`weight[0]`,价值为`value[0]`，遍历背包容量，若正向遍历，则有：

  ```c++
  //d[j] = max(dp[j] , d[j-weight[0]] + value[0]) 只考虑后半部分计算
  
  d[1] = d[1-weight[0]] + value[0]; //= 15
  //设value[0] = 15, weight[0] = 1, 初始d[j] = 0;
  
  d[2] = d[2-weight[0]] + value[0]; //= 30
  /* 
  针对本次背包容量循环，待放入物品只有物品0；且在本次循环前，背包为空；
  那么正确结果应该是：
  若背包容量小于物品0重量，则背包价值为0;
  若背包容量大于物品0重量，则背包价值为value[0]，且不管容量多大，都为value[0],因为物品0只有一个;
  很显然，代码出现问题。
  问题在于，计算d[2]时，调用了d[1]的计算结果。d[2]的计算公式的意义是：当前物品，也就是物品0放入背包，并要将剩余空间价值最大化；
  可是剩余空间也就是dp[1]价值最大化的情况就是放入物品0，所以dp[2]的计算相当于将物品0放入了两次。这与01背包的定义不符。
  */
  ```

  2.除了从原理上理解，还可以单纯从代码角度理解。

  一维数组的写法是从二维数组的写法简化而来。手动复原二维数组的遍历过程即可发现，对于本次循环计算`dp[i][j]`，等式右侧所用到的数据都来自本次循环对应数组元素的**上一行**，也就是**上次循环的结果**。化简成一维数组的写法，**就是将上次计算的结果拷贝到本次计算的位置，然后取用并更新**。根据转移公式，`d[j]`的计算需要用到`j`之前的结果，且结果需要为**上一次**循环的结果，因此，如果从前到后遍历，计算到`d[j]`时，需要用到的**前面元素的上一次计算结果已经不见了，取而代之的是本次循环的结果！**这样计算出来的`d[j]`自然就是错的。又因为`d[j]`的计算只需要用到`j`之前的结果，`j`之后元素如何变化对于计算`d[j]`	没有影响，因此，从后向前遍历才能得到正确结果。

  对于完全背包：一个物品可以放入多次，因此**要正向遍历**。

* 循环顺序问题：

  对于01背包问题的一维数组实现，循环顺序**只能是先遍历物品，再遍历背包容量**；而对于01背包的二维数组实现和完全背包则**先遍历哪个都可以**。

  对于01背包的的一维数组实现来说，若先遍历背包容量，再遍历物品，那么**每个背包容量情况下，只会放置最多一件物品**。

  将循环顺序颠倒后代码如下:

  ```c++
  int i = 0;
  for(int j = bagweight; j >= weight[i]; j--){
    for (i = 0; i < weight.size(); i++){
      dp[j] = max (dp[j] , dp[j - weight[i]] + value[i] );
    }
  }
  
  /*
  d[j]初始化全为0，在j的一个循环中，对于i的全部循环，dp[j - weight[i]] == 0 恒成立。
  完成i的整个循环，实际上只是选择了value[i]最大的，放入d[j]中，因此每个背包容量最多只放入一件物品。
  */
  ```

  上述情况的出现是由于01背包的的一维数组实现的遍历顺序导致的。从后向前遍历，前面的元素还都是零，因此只能得到物品本身value。

  而01背包的的二维数组实现和完全背包实现时，是从前到后遍历的，因此不用担心循环顺序问题，只要保证计算到`d[j]`时，需要用到的前面的元素已经计算好了即可。

  **值得注意的是：对于纯完全背包问题，其for循环的先后循环是可以颠倒的！若题目变化，则需要具体考虑！**

### 2.1.7零钱II

问题描述：

```c++
给你一个整数数组 coins 表示不同面额的硬币，另给一个整数 amount 表示总金额。

请你计算并返回可以凑成总金额的硬币组合数。如果任何硬币组合都无法凑出总金额，返回 0 。

假设每一种面额的硬币有无限个。 

题目数据保证结果符合 32 位带符号整数。

 

示例 1：

输入：amount = 5, coins = [1, 2, 5]
输出：4
解释：有四种方式可以凑成总金额：
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
示例 2：

输入：amount = 3, coins = [2]
输出：0
解释：只用面额 2 的硬币不能凑成总金额 3 。
示例 3：

输入：amount = 10, coins = [10] 
输出：1
 

提示：

1 <= coins.length <= 300
1 <= coins[i] <= 5000
coins 中的所有值 互不相同
0 <= amount <= 5000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/coin-change-2
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

代码：

```c++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
    //完全背包问题
    //背包容量为金额amount，物品为硬币，物品重量和物品价值都是硬币金额
    //dp[j]表示 凑出j金额的硬币组合为d[j]个
    vector<int> dp (amount+1 , 0);

    //转移公式
    /*对于待放入coins[i]，还是老思路：首先，要满足背包容量j>coins[i]；
                                  因为是填满问题，而不是价值最大化问题，因此，在这里满足条件的coins[i]
                                  是一定要放进去的，因为不放进去的情况在coins[0]~coins[i-1]的时候已经
                                  考虑到了。
      又因为是组合问题，因此dp[j] += dp[j-coins[i]];
    */

    //初始化
    //组合问题，dp[0] = 1,否则dp[j]一直为0；
    dp[0] = 1;

    //遍历顺序
    /*
    纯正的完全背包问题，先遍历背包或先遍历物品都是可以的。
    对于本题目来说，求的是组合数，必须先遍历物品。
    */
    for (int i = 0; i < coins.size(); i++){
        for (int j = coins[i]; j <= amount; j++){
            dp[j] += dp[j-coins[i]];
        }
    }
    return dp[amount];
    }
};
```

总结：

这道题看似简单，实则很有代表性。最难的部分就是遍历顺序这部分。

本题必须先遍历物品，再遍历背包，求得的才是**组合**数，否则就是**排列数**。

这里理解比较困难，需要配合手动迭代图解释。

对于先遍历物品，再遍历背包，遍历图如下：





<img src="https://raw.githubusercontent.com/Yetsang/PicBed/main/img/%E5%9B%BE%E5%83%8F-9551010.jpeg" alt="图像" style="zoom:25%;" />

按行从左到右遍历，过程符合人的思维。首先是只有一种硬币的情况下，也就是第一行，对于1元硬币来说，无论总额是多少，都只有1种组合方式；再到第二行，在第一种硬币的基础上，再加一种硬币，即2元。然后按照转移公式递推下去。

对于先遍历背包，再遍历物品，遍历图如下：

<img src="https://raw.githubusercontent.com/Yetsang/PicBed/main/img/%E5%9B%BE%E5%83%8F%202-9551249.jpeg" alt="图像 2" style="zoom: 25%;" />

在`j = 3`处，两者发生了变化。对于图2来说，`i = 0`时`d[j] = 2`是根据转移公式计算出来的，这里就错了。

至于为什么说下面这种方法计算的是排列数，暂时还没想明白。



