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
