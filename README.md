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

