---
title: LeetCode Hot 100 Python版 个人学习笔记
date: 2026-06-09 12:00:00
categories: Study
tags:
  - LeetCode
  - Python
  - 算法
  - 面试
toc: true
---

## 一、数组与哈希表
 
### 1. 两数之和 (Two Sum)
 
**题目描述：** 
给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** `target` 的那 **两个** 整数，并返回它们的数组下标。 
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。 
你可以按任意顺序返回答案。 
**示例：** 
 
```python
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```
 
**解题思路：** 
使用哈希表（字典）存储已遍历的元素及其索引。对于每个元素，计算目标值与当前元素的差值，检查该差值是否已存在于哈希表中。如果存在，则找到了两个数；否则，将当前元素及其索引存入哈希表。 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是数组的长度。每个元素最多被访问一次。
- 空间复杂度：O(n)，用于存储哈希表中的元素。
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：两数之和
# 思路：使用哈希表（字典）存储已遍历的元素及其索引。对于每个元素，计算目标值与当前元素的差值，检查该差值是否已存在于哈希表中。如果存在，则找到了两个数；否则，将当前元素及其索引存入哈希表。
def twoSum(nums, target):  # 定义函数 twoSum，接收参数: nums、target
    num_map = {}  # num_map: 空字典
    for i, num in enumerate(nums):  # 遍历: i, num 依次取 enumerate(nums) 中的每个值
        complement = target - num  # complement: 赋值/计算
        if complement in num_map:  # 判断: complement in num_map
            return [num_map[complement], i]  # 返回列表结果
        num_map[num] = i  # num_map[num]: 赋值/计算
    return []  # 返回 [] (标志/空值)
```
 
### 15. 三数之和 (3Sum)
 
**题目描述：** 
给你一个整数数组 `nums`，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k`，同时还满足 `nums[i] + nums[j] + nums[k] == 0`。请你返回所有和为 `0` 且不重复的三元组。 
**注意：** 答案中不可以包含重复的三元组。 
**示例：** 
 
```python
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```
 
**解题思路：** 
- 首先对数组进行排序
- 固定第一个数，使用双指针在剩余部分寻找另外两个数
- 使用双指针法：左指针指向固定数的下一个位置，右指针指向数组末尾
- 根据三数之和与 0 的关系移动指针
- 注意跳过重复元素以避免重复解
 
**复杂度分析：** 
- 时间复杂度：O(n²)，其中 n 是数组的长度。排序 O(n log n) + 双指针遍历 O(n²)
- 空间复杂度：O(1)，除了存储答案的空间外，只需要常数的额外空间
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：三数之和
# 思路：首先对数组进行排序固定第一个数，使用双指针在剩余部分寻找另外两个数使用双指针法：左指针指向固定数的下一个位置，右指针指向数组末尾根据三数之和与 0 的关系移动指针注意跳过重复元素以避免重复解
def threeSum(nums):  # 定义函数 threeSum，接收参数: nums
    nums.sort()  # 原地排序
    res = []  # res: 空列表
    n = len(nums)  # n: 获取长度

    for i in range(n - 2):  # 遍历: i 依次取 range(n - 2) 中的每个值
        # 跳过重复元素
        if i > 0 and nums[i] == nums[i - 1]:  # 判断: i > 0 and nums[i] == nums[i - 1]
            continue  # 跳过本轮迭代

        left, right = i + 1, n - 1  # 交换/多重赋值
        while left < right:  # 当 left < right 时循环
            total = nums[i] + nums[left] + nums[right]  # total: 赋值/计算
            if total < 0:  # 判断: total < 0
                left += 1  # left 自反赋值+=
            elif total > 0:  # 否则如果
                right -= 1  # right 自反赋值-=
            else:  # 否则 (以上条件都不满足时执行)
                res.append([nums[i], nums[left], nums[right]])  # 追加到列表末尾
                # 跳过重复元素
                while left < right and nums[left] == nums[left + 1]:  # 当 left < right and nums[left] == 时循环
                    left += 1  # left 自反赋值+=
                while left < right and nums[right] == nums[right - 1]:  # 当 left < right and nums[right] = 时循环
                    right -= 1  # right 自反赋值-=
                left += 1  # left 自反赋值+=
                right -= 1  # right 自反赋值-=

    return res  # 返回计算结果
```
 
### 11. 盛最多水的容器 (Container With Most Water)
 
**题目描述：** 
给定一个长度为 `n` 的整数数组 `height`。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])`。 
找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。 
返回容器可以储存的最大水量。 
**示例：** 
 
```python
输入：height = [1,8,6,2,5,4,8,3,7]
输出：49
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```
 
**解题思路：** 
使用双指针法： 
- 初始化左右指针分别指向数组的两端
- 计算当前容器的容量：`min(height[left], height[right]) * (right - left)`
- 更新最大容量
- 移动较短的那一侧指针（因为移动较长的一侧不会增加容量）
- 重复直到左右指针相遇
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是数组的长度。双指针最多遍历整个数组一次
- 空间复杂度：O(1)，只需要常数的额外空间
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：盛最多水的容器
# 思路：使用双指针法：；初始化左右指针分别指向数组的两端计算当前容器的容量：min(height[left], height[right]) * (right - left)更新最大容量移动较短的那一侧指针（因为移动较长的一侧不会增加容量）重复直到左右指针相遇
def maxArea(height):  # 定义函数 maxArea，接收参数: height
    left, right = 0, len(height) - 1  # 交换/多重赋值
    max_area = 0  # max_area: 计数器/下标初始化为0

    while left < right:  # 当 left < right 时循环
        h = min(height[left], height[right])  # h: 赋值/计算
        width = right - left  # width: 赋值/计算
        max_area = max(max_area, h * width)  # max_area: 乘积计算

        if height[left] < height[right]:  # 判断: height[left] < height[right]
            left += 1  # left 自反赋值+=
        else:  # 否则 (以上条件都不满足时执行)
            right -= 1  # right 自反赋值-=

    return max_area  # 返回计算结果
```
 
### 3. 无重复字符的最长子串 (Longest Substring Without Repeating Characters)
 
**题目描述：** 
给定一个字符串 `s`，请你找出其中不含有重复字符的 **最长子串** 的长度。 
**示例：** 
 
```python
输入: s = "abcabcbb"
输出: 3
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```
 
**解题思路：** 
使用滑动窗口（双指针）+ 哈希集合： 
- 使用两个指针表示滑动窗口的左右边界
- 使用哈希集合记录窗口内的字符
- 右指针不断向右移动，如果遇到重复字符，移动左指针直到窗口内无重复字符
- 在移动过程中更新最大长度
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是字符串的长度。每个字符最多被访问两次（左指针和右指针各一次）
- 空间复杂度：O(min(n, m))，其中 m 是字符集的大小。哈希集合最多存储 min(n, m) 个字符
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：无重复字符的最长子串
# 思路：使用滑动窗口（双指针）+ 哈希集合：；使用两个指针表示滑动窗口的左右边界使用哈希集合记录窗口内的字符右指针不断向右移动，如果遇到重复字符，移动左指针直到窗口内无重复字符在移动过程中更新最大长度
def lengthOfLongestSubstring(s):  # 定义函数 lengthOfLongestSubstring，接收参数: s
    char_set = set()  # char_set: 空集合
    left = 0  # left: 计数器/下标初始化为0
    max_length = 0  # max_length: 计数器/下标初始化为0

    for right in range(len(s)):  # 遍历: right 依次取 range(len(s)) 中的每个值
        # 如果遇到重复字符，移动左指针
        while s[right] in char_set:  # 当 s[right] in char_set 时循环
            char_set.remove(s[left])  # 移除指定元素
            left += 1  # left 自反赋值+=

        char_set.add(s[right])  # 向集合添加元素
        max_length = max(max_length, right - left + 1)  # max_length: 赋值/计算

    return max_length  # 返回计算结果
```
 
### 5. 最长回文子串 (Longest Palindromic Substring)
 
**题目描述：** 
给你一个字符串 `s`，找到 `s` 中最长的回文子串。 
如果字符串的反序与原始字符串相同，则该字符串称为回文字符串。 
**示例：** 
 
```python
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
```
 
**解题思路：** 
使用中心扩展法： 
- 回文串可能是奇数长度（以某个字符为中心）或偶数长度（以两个字符的中间为中心）
- 对于每个可能的中心位置，向两边扩展，找到最长的回文子串
- 比较所有找到的回文子串，返回最长的
 
**复杂度分析：** 
- 时间复杂度：O(n²)，其中 n 是字符串的长度。对于每个中心位置，最多需要扩展 O(n) 次
- 空间复杂度：O(1)，只需要常数的额外空间
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：最长回文子串
# 思路：使用中心扩展法：；回文串可能是奇数长度（以某个字符为中心）或偶数长度（以两个字符的中间为中心）对于每个可能的中心位置，向两边扩展，找到最长的回文子串比较所有找到的回文子串，返回最长的
def longestPalindrome(s):  # 定义函数 longestPalindrome，接收参数: s
    def expandAroundCenter(left, right):  # 定义函数 expandAroundCenter，接收参数: left、right
        while left >= 0 and right < len(s) and s[left] == s[right]:  # 当 left >= 0 and right < len(s) a 时循环
            left -= 1  # left 自反赋值-=
            right += 1  # right 自反赋值+=
        return right - left - 1  # 返回计算结果

    start = end = 0  # start: 赋值/计算

    for i in range(len(s)):  # 遍历: i 依次取 range(len(s)) 中的每个值
        # 奇数长度的回文串
        len1 = expandAroundCenter(i, i)  # len1: 赋值/计算
        # 偶数长度的回文串
        len2 = expandAroundCenter(i, i + 1)  # len2: 赋值/计算
        max_len = max(len1, len2)  # max_len: 赋值/计算

        if max_len > end - start:  # 判断: max_len > end - start
            start = i - (max_len - 1) // 2  # start: 赋值/计算
            end = i + max_len // 2  # end: 赋值/计算

    return s[start:end + 1]  # 返回列表结果
```
 
### 88. 合并两个有序数组 (Merge Sorted Array)
 
**题目描述：** 
给你两个按 **非递减顺序** 排列的整数数组 `nums1` 和 `nums2`，另有两个整数 `m` 和 `n`，分别表示 `nums1` 和 `nums2` 中元素的数目。 
请你 **合并** `nums2` 到 `nums1` 中，使合并后的数组同样按 **非递减顺序** 排列。 
**注意：** 最终，合并后数组不应由函数返回，而是存储在数组 `nums1` 中。为了应对这种情况，`nums1` 的初始长度为 `m + n`，其中前 `m` 个元素表示应合并的元素，后 `n` 个元素为 `0`，应忽略。`nums2` 的长度为 `n`。 
**示例：** 
 
```python
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
解释：需要合并 [1,2,3] 和 [2,5,6] 。
合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。
```
 
**解题思路：** 
使用双指针从后往前合并： 
- 因为 `nums1` 后面有足够的空间，所以从后往前填充
- 使用三个指针：`i` 指向 `nums1` 的有效元素末尾，`j` 指向 `nums2` 的末尾，`k` 指向合并后的数组末尾
- 比较 `nums1[i]` 和 `nums2[j]`，将较大的元素放到 `nums1[k]`
- 如果 `nums2` 还有剩余元素，需要全部复制到 `nums1` 的前面
 
**复杂度分析：** 
- 时间复杂度：O(m + n)，其中 m 和 n 分别是两个数组的长度
- 空间复杂度：O(1)，只需要常数的额外空间
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：合并两个有序数组
# 思路：使用双指针从后往前合并：；因为 nums1 后面有足够的空间，所以从后往前填充使用三个指针：i 指向 nums1 的有效元素末尾，j 指向 nums2 的末尾，k 指向合并后的数组末尾比较 nums1[i] 和 nums2[j]，将较大的元素放到 nums1[k]如果 nums2 还有剩余元素，需
def merge(nums1, m, nums2, n):  # 定义函数 merge，接收参数: nums1、m、nums2、n
    i, j, k = m - 1, n - 1, m + n - 1  # 交换/多重赋值

    while i >= 0 and j >= 0:  # 当 i >= 0 and j >= 0 时循环
        if nums1[i] > nums2[j]:  # 判断: nums1[i] > nums2[j]
            nums1[k] = nums1[i]  # nums1[k]: 赋值/计算
            i -= 1  # i 自反赋值-=
        else:  # 否则 (以上条件都不满足时执行)
            nums1[k] = nums2[j]  # nums1[k]: 赋值/计算
            j -= 1  # j 自反赋值-=
        k -= 1  # k 自反赋值-=

    # 如果 nums2 还有剩余元素
    while j >= 0:  # 当 j >= 0 时循环
        nums1[k] = nums2[j]  # nums1[k]: 赋值/计算
        j -= 1  # j 自反赋值-=
        k -= 1  # k 自反赋值-=
```
 
### 283. 移动零 (Move Zeroes)
 
**题目描述：** 
给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。 
**注意** 必须在不复制数组的情况下原地对数组进行操作。 
**示例：** 
 
```python
输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]
```
 
**解题思路：** 
使用双指针： 
- 使用一个指针 `left` 指向当前应该放置非零元素的位置
- 遍历数组，遇到非零元素就将其放到 `left` 位置，然后 `left++`
- 遍历结束后，将 `left` 之后的所有位置置为 0
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是数组的长度
- 空间复杂度：O(1)，只需要常数的额外空间
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：移动零
# 思路：使用双指针：；使用一个指针 left 指向当前应该放置非零元素的位置遍历数组，遇到非零元素就将其放到 left 位置，然后 left++遍历结束后，将 left 之后的所有位置置为 0
def moveZeroes(nums):  # 定义函数 moveZeroes，接收参数: nums
    left = 0  # left: 计数器/下标初始化为0

    # 将所有非零元素移到前面
    for right in range(len(nums)):  # 遍历: right 依次取 range(len(nums)) 中的每个值
        if nums[right] != 0:  # 判断: nums[right] != 0
            nums[left], nums[right] = nums[right], nums[left]  # 交换/多重赋值
            left += 1  # left 自反赋值+=
```
 
### 448. 找到所有数组中消失的数字 (Find All Numbers Disappeared in an Array)
 
**题目描述：** 
给你一个含 `n` 个整数的数组 `nums`，其中 `nums[i]` 在区间 `[1, n]` 内。请你找出所有在 `[1, n]` 范围内但没有出现在 `nums` 中的数字，并以数组的形式返回结果。 
**示例：** 
 
```python
输入：nums = [4,3,2,7,8,2,3,1]
输出：[5,6]
```
 
**解题思路：** 
利用数组本身作为哈希表： 
- 对于每个数字 `nums[i]`，将 `nums[abs(nums[i]) - 1]` 标记为负数
- 如果某个位置的数字已经是负数，说明该数字出现过
- 最后遍历数组，如果某个位置的数字是正数，说明该位置对应的数字没有出现过
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是数组的长度
- 空间复杂度：O(1)，除了返回数组外，只需要常数的额外空间
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：找到所有数组中消失的数字
# 思路：利用数组本身作为哈希表：；对于每个数字 nums[i]，将 nums[abs(nums[i]) - 1] 标记为负数如果某个位置的数字已经是负数，说明该数字出现过最后遍历数组，如果某个位置的数字是正数，说明该位置对应的数字没有出现过
def findDisappearedNumbers(nums):  # 定义函数 findDisappearedNumbers，接收参数: nums
    # 标记出现过的数字
    for num in nums:  # 遍历: num 依次取 nums 中的每个值
        index = abs(num) - 1  # index: 赋值/计算
        if nums[index] > 0:  # 判断: nums[index] > 0
            nums[index] = -nums[index]  # nums[index]: 赋值/计算

    # 找出未出现的数字
    result = []  # result: 空列表
    for i in range(len(nums)):  # 遍历: i 依次取 range(len(nums)) 中的每个值
        if nums[i] > 0:  # 判断: nums[i] > 0
            result.append(i + 1)  # 追加到列表末尾

    return result  # 返回计算结果
```
 
### 238. 除自身以外数组的乘积 (Product of Array Except Self)
 
**题目描述：** 
给你一个整数数组 `nums`，返回 **数组 answer**，其中 `answer[i]` 等于 `nums` 中除 `nums[i]` 之外其余各元素的乘积。 
题目数据 **保证** 数组 `nums` 之中任意元素的全部前缀元素和后缀的乘积都在 **32 位整数** 范围内。 
请 **不要使用除法**，且在 `O(n)` 时间复杂度内完成此题。 
**示例：** 
 
```python
输入: nums = [1,2,3,4]
输出: [24,12,8,6]
```
 
**解题思路：** 
使用左右乘积列表： 
- 创建两个数组：`left` 存储每个元素左侧所有元素的乘积，`right` 存储每个元素右侧所有元素的乘积
- 对于位置 `i`，`answer[i] = left[i] * right[i]`
- 可以优化空间复杂度：先计算 `left` 数组，然后从右往左遍历，用一个变量存储右侧乘积
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是数组的长度
- 空间复杂度：O(1)，除了返回数组外，只需要常数的额外空间（优化后）
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：除自身以外数组的乘积
# 思路：使用左右乘积列表：
def productExceptSelf(nums):  # 定义函数 productExceptSelf，接收参数: nums
    n = len(nums)  # n: 获取长度
    answer = [1] * n  # answer: 乘积计算

    # 计算左侧乘积
    for i in range(1, n):  # 遍历: i 依次取 range(1, n) 中的每个值
        answer[i] = answer[i - 1] * nums[i - 1]  # answer[i]: 乘积计算

    # 计算右侧乘积并同时更新答案
    right = 1  # right: 初始化为1
    for i in range(n - 1, -1, -1):  # 遍历: i 依次取 range(n - 1, -1, -1) 中的每个值
        answer[i] *= right  # answer[i] 自反赋值*=
        right *= nums[i]  # right 自反赋值*=

    return answer  # 返回计算结果
```
 
### 48. 旋转图像 (Rotate Image)
 
**题目描述：** 
给定一个 `n × n` 的二维矩阵 `matrix` 表示一个图像。请你将图像顺时针旋转 90 度。 
你必须在 **原地** 旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要** 使用另一个矩阵来旋转图像。 
**示例：** 
**解题思路：** 
方法一：先转置再翻转每一行 
方法二：直接旋转 
**复杂度分析：** 
**Python 解答（含详细注释）：** 
 
```python
# 目的：旋转图像
# 思路：方法1：转置 + 翻转；先转置矩阵（行列互换）再翻转每一行；方法2：四角交换
def rotate(matrix):  # 定义函数 rotate，接收参数: matrix
    n = len(matrix)  # n: 获取长度

    # 转置
    for i in range(n):  # 遍历: i 依次取 range(n) 中的每个值
        for j in range(i, n):  # 遍历: j 依次取 range(i, n) 中的每个值
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]  # 交换/多重赋值

    # 翻转每一行
    for i in range(n):  # 遍历: i 依次取 range(n) 中的每个值
        matrix[i].reverse()  # 原地反转列表
```
 
### 31. 下一个排列 (Next Permutation)
 
**题目描述：** 
整数数组的一个 **排列** 就是将其所有成员以序列或线性顺序排列。 
- 例如，`arr = [1,2,3]`，以下这些都可以视作 `arr` 的排列：`[1,2,3]`、`[1,3,2]`、`[3,1,2]`、`[2,3,1]`。
 
整数数组的 **下一个排列** 是指其整数的下一个字典序更大的排列。更正式地，如果数组的所有排列根据其字典顺序从小到大排列在一个容器中，那么数组的 **下一个排列** 就是在这个有序容器中排在它后面的那个排列。如果不存在下一个更大的排列，那么这个数组必须重排为字典序最小的排列（即，其元素按升序排列）。 
- 例如，`arr = [1,2,3]` 的下一个排列是 `[1,3,2]`。
- 类似地，`arr = [2,3,1]` 的下一个排列是 `[3,1,2]`。
- 而 `arr = [3,2,1]` 的下一个排列是 `[1,2,3]`（因为此排列没有字典序更大的下一个排列）。
 
给你一个整数数组 `nums`，找出 `nums` 的下一个排列。 
必须 **原地** 修改，只允许使用额外常数空间。 
**示例：** 
 
```python
输入：nums = [1,2,3]
输出：[1,3,2]
```
 
**解题思路：** 
- 从右向左找到第一个降序的位置 `i`（即 `nums[i] < nums[i+1]`）
- 如果找不到，说明整个数组是降序的，直接反转整个数组
- 从右向左找到第一个大于 `nums[i]` 的位置 `j`
- 交换 `nums[i]` 和 `nums[j]`
- 反转 `i+1` 到末尾的部分
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：下一个排列
# 思路：从右向左找到第一个降序的位置 i（即 nums[i] ）如果找不到，说明整个数组是降序的，直接反转整个数组从右向左找到第一个大于 nums[i] 的位置 j交换 nums[i] 和 nums[j]反转 i+1 到末尾的部分
def nextPermutation(nums):  # 定义函数 nextPermutation，接收参数: nums
    n = len(nums)  # n: 获取长度
    i = n - 2  # i: 赋值/计算

    while i >= 0 and nums[i] >= nums[i + 1]:  # 当 i >= 0 and nums[i] >= nums[i + 时循环
        i -= 1  # i 自反赋值-=

    if i >= 0:  # 判断: i >= 0
        j = n - 1  # j: 赋值/计算
        while j > i and nums[j] <= nums[i]:  # 当 j > i and nums[j] <= nums[i] 时循环
            j -= 1  # j 自反赋值-=
        nums[i], nums[j] = nums[j], nums[i]  # 交换/多重赋值

    left, right = i + 1, n - 1  # 交换/多重赋值
    while left < right:  # 当 left < right 时循环
        nums[left], nums[right] = nums[right], nums[left]  # 交换/多重赋值
        left += 1  # left 自反赋值+=
        right -= 1  # right 自反赋值-=
```
 
### 33. 搜索旋转排序数组 (Search in Rotated Sorted Array)
 
**题目描述：** 
整数数组 `nums` 按升序排列，数组中的值 **互不相同**。 
在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 <= k < nums.length`）上进行了 **旋转**，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 **从 0 开始** 计数）。例如，`[0,1,2,4,5,6,7]` 在下标 `3` 处经旋转后可能变为 `[4,5,6,7,0,1,2]`。 
给你 **旋转后** 的数组 `nums` 和一个整数 `target`，如果 `nums` 中存在这个目标值 `target`，则返回它的下标，否则返回 `-1`。 
**示例：** 
 
```python
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
```
 
**解题思路：** 
二分查找： 
- 虽然数组被旋转，但至少有一半是有序的
- 判断 `nums[mid]` 和 `nums[left]` 的关系，确定哪一半是有序的
- 如果左半部分有序，判断 `target` 是否在左半部分
- 如果右半部分有序，判断 `target` 是否在右半部分
- 根据判断结果缩小搜索范围
 
**复杂度分析：** 
- 时间复杂度：O(log n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：搜索旋转排序数组
# 思路：二分查找：；虽然数组被旋转，但至少有一半是有序的判断 nums[mid] 和 nums[left] 的关系，确定哪一半是有序的如果左半部分有序，判断 target 是否在左半部分如果右半部分有序，判断 target 是否在右半部分根据判断结果缩小搜索范围
def search(nums, target):  # 定义函数 search，接收参数: nums、target
    left, right = 0, len(nums) - 1  # 交换/多重赋值

    while left <= right:  # 当 left <= right 时循环
        mid = (left + right) // 2  # mid: 赋值/计算

        if nums[mid] == target:  # 判断: nums[mid] == target
            return mid  # 返回计算结果

        if nums[left] <= nums[mid]:  # 判断: nums[left] <= nums[mid]
            if nums[left] <= target < nums[mid]:  # 判断: nums[left] <= target < nums[mid]
                right = mid - 1  # right: 赋值/计算
            else:  # 否则 (以上条件都不满足时执行)
                left = mid + 1  # left: 赋值/计算
        else:  # 否则 (以上条件都不满足时执行)
            if nums[mid] < target <= nums[right]:  # 判断: nums[mid] < target <= nums[right]
                left = mid + 1  # left: 赋值/计算
            else:  # 否则 (以上条件都不满足时执行)
                right = mid - 1  # right: 赋值/计算

    return -1  # 返回 -1 (标志/空值)
```
 
### 34. 在排序数组中查找元素的第一个和最后一个位置 (Find First and Last Position of Element in Sorted Array)
 
**题目描述：** 
给你一个按照非递减顺序排列的整数数组 `nums`，和一个目标值 `target`。请你找出给定目标值在数组中的开始位置和结束位置。 
如果数组中不存在目标值 `target`，返回 `[-1, -1]`。 
你必须设计并实现时间复杂度为 `O(log n)` 的算法解决此问题。 
**示例：** 
 
```python
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```
 
**解题思路：** 
二分查找： 
- 使用两次二分查找
- 第一次查找第一个大于等于 `target` 的位置
- 第二次查找第一个大于 `target` 的位置
- 如果找到，返回 `[第一个位置, 第二个位置 - 1]`
 
**复杂度分析：** 
- 时间复杂度：O(log n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：在排序数组中查找元素的第一个和最后一个位置
# 思路：二分查找：；使用两次二分查找第一次查找第一个大于等于 target 的位置第二次查找第一个大于 target 的位置如果找到，返回 [第一个位置, 第二个位置 - 1]
def searchRange(nums, target):  # 定义函数 searchRange，接收参数: nums、target
    def findFirst(nums, target):  # 定义函数 findFirst，接收参数: nums、target
        left, right = 0, len(nums)  # 交换/多重赋值
        while left < right:  # 当 left < right 时循环
            mid = (left + right) // 2  # mid: 赋值/计算
            if nums[mid] < target:  # 判断: nums[mid] < target
                left = mid + 1  # left: 赋值/计算
            else:  # 否则 (以上条件都不满足时执行)
                right = mid  # right: 赋值/计算
        return left if left < len(nums) and nums[left] == target else -1  # 返回列表结果

    def findLast(nums, target):  # 定义函数 findLast，接收参数: nums、target
        left, right = 0, len(nums)  # 交换/多重赋值
        while left < right:  # 当 left < right 时循环
            mid = (left + right) // 2  # mid: 赋值/计算
            if nums[mid] <= target:  # 判断: nums[mid] <= target
                left = mid + 1  # left: 赋值/计算
            else:  # 否则 (以上条件都不满足时执行)
                right = mid  # right: 赋值/计算
        return left - 1 if left > 0 and nums[left - 1] == target else -1  # 返回列表结果

    first = findFirst(nums, target)  # first: 赋值/计算
    if first == -1:  # 判断: first == -1
        return [-1, -1]  # 返回列表结果
    last = findLast(nums, target)  # last: 赋值/计算
    return [first, last]  # 返回列表结果
```
 
### 75. 颜色分类 (Sort Colors)
 
**题目描述：** 
给定一个包含红色、白色和蓝色、共 `n` 个元素的数组 `nums`，**原地**对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。 
我们使用整数 `0`、`1` 和 `2` 分别表示红色、白色和蓝色。 
必须在不使用库内置的 sort 函数的情况下解决这个问题。 
**示例：** 
 
```python
输入：nums = [2,0,2,1,1,0]
输出：[0,0,1,1,2,2]
```
 
**解题思路：** 
荷兰国旗问题，使用三指针： 
- 使用三个指针：`left`（指向0的右边界）、`curr`（当前遍历位置）、`right`（指向2的左边界）
- 当 `nums[curr] == 0` 时，与 `nums[left]` 交换，`left++`，`curr++`
- 当 `nums[curr] == 1` 时，`curr++`
- 当 `nums[curr] == 2` 时，与 `nums[right]` 交换，`right--`（注意不移动 `curr`，因为交换来的元素还未检查）
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：颜色分类
# 思路：荷兰国旗问题，使用三指针：；使用三个指针：left（指向0的右边界）、curr（当前遍历位置）、right（指向2的左边界）当 nums[curr] == 0 时，与 nums[left] 交换，left++，curr++当 nums[curr] == 1 时，curr++当 nums[curr]
def sortColors(nums):  # 定义函数 sortColors，接收参数: nums
    left = curr = 0  # left: 赋值/计算
    right = len(nums) - 1  # right: 获取长度

    while curr <= right:  # 当 curr <= right 时循环
        if nums[curr] == 0:  # 判断: nums[curr] == 0
            nums[left], nums[curr] = nums[curr], nums[left]  # 交换/多重赋值
            left += 1  # left 自反赋值+=
            curr += 1  # curr 自反赋值+=
        elif nums[curr] == 1:  # 否则如果
            curr += 1  # curr 自反赋值+=
        else:  # 否则 (以上条件都不满足时执行)
            nums[curr], nums[right] = nums[right], nums[curr]  # 交换/多重赋值
            right -= 1  # right 自反赋值-=
```
 
## 二、链表
 
### 206. 反转链表 (Reverse Linked List)
 
**题目描述：** 
给你单链表的头节点 `head`，请你反转链表，并返回反转后的链表。 
**示例：** 
 
```python
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```
 
**解题思路：** 
使用迭代法： 
- 使用三个指针：`prev`（前一个节点）、`curr`（当前节点）、`next`（下一个节点）
- 遍历链表，将当前节点的 `next` 指向前一个节点
- 然后移动三个指针继续遍历
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是链表的长度
- 空间复杂度：O(1)，只需要常数的额外空间
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：反转链表
# 思路：使用迭代法：；使用三个指针：prev（前一个节点）、curr（当前节点）、next（下一个节点）遍历链表，将当前节点的 next 指向前一个节点然后移动三个指针继续遍历
class ListNode:  # 定义类 ListNode
    def __init__(self, val=0, next=None):  # 定义函数 __init__，接收参数: self、val、next
        self.val = val  # self.val: 赋值/计算
        self.next = next  # self.next: 赋值/计算
def reverseList(head):  # 定义函数 reverseList，接收参数: head
    prev = None  # prev: 空值None
    curr = head  # curr: 赋值/计算

    while curr:  # 当 curr 时循环
        next_temp = curr.next  # next_temp: 赋值/计算
        curr.next = prev  # curr.next: 赋值/计算
        prev = curr  # prev: 赋值/计算
        curr = next_temp  # curr: 赋值/计算

    return prev  # 返回计算结果
```
 
### 21. 合并两个有序链表 (Merge Two Sorted Lists)
 
**题目描述：** 
将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
**示例：** 
 
```python
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```
 
**解题思路：** 
使用双指针（迭代法）： 
- 创建一个虚拟头节点 `dummy`，用于简化边界处理
- 使用指针 `curr` 指向当前合并后的链表的末尾
- 比较两个链表的当前节点，将较小的节点连接到 `curr` 后面
- 移动指针继续比较，直到其中一个链表为空
- 将剩余的链表连接到结果链表的末尾
 
**复杂度分析：** 
- 时间复杂度：O(n + m)，其中 n 和 m 分别是两个链表的长度
- 空间复杂度：O(1)，只需要常数的额外空间
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：合并两个有序链表
# 思路：使用双指针（迭代法）：；创建一个虚拟头节点 dummy，用于简化边界处理使用指针 curr 指向当前合并后的链表的末尾比较两个链表的当前节点，将较小的节点连接到 curr 后面移动指针继续比较，直到其中一个链表为空将剩余的链表连接到结果链表的末尾
def mergeTwoLists(l1, l2):  # 定义函数 mergeTwoLists，接收参数: l1、l2
    dummy = ListNode(0)  # dummy: 赋值/计算
    curr = dummy  # curr: 赋值/计算

    while l1 and l2:  # 当 l1 and l2 时循环
        if l1.val <= l2.val:  # 判断: l1.val <= l2.val
            curr.next = l1  # curr.next: 赋值/计算
            l1 = l1.next  # l1: 赋值/计算
        else:  # 否则 (以上条件都不满足时执行)
            curr.next = l2  # curr.next: 赋值/计算
            l2 = l2.next  # l2: 赋值/计算
        curr = curr.next  # curr: 赋值/计算

    curr.next = l1 if l1 else l2  # curr.next: 赋值/计算

    return dummy.next  # 返回计算结果
```
 
### 141. 环形链表 (Linked List Cycle)
 
**题目描述：** 
给你一个链表的头节点 `head`，判断链表中是否有环。 
如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。注意：`pos` 不作为参数进行传递。仅仅是为了标识链表的实际情况。 
如果链表中存在环，则返回 `true`。否则，返回 `false`。 
**示例：** 
 
```python
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```
 
**解题思路：** 
使用快慢指针（Floyd 判圈算法）： 
- 使用两个指针：`slow` 每次移动一步，`fast` 每次移动两步
- 如果链表中存在环，快慢指针最终会相遇
- 如果快指针到达链表末尾（`nullptr`），说明没有环
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是链表中节点的数量
- 空间复杂度：O(1)，只需要常数的额外空间
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：环形链表
# 思路：使用快慢指针（Floyd 判圈算法）：；使用两个指针：slow 每次移动一步，fast 每次移动两步如果链表中存在环，快慢指针最终会相遇如果快指针到达链表末尾（nullptr），说明没有环
def hasCycle(head):  # 定义函数 hasCycle，接收参数: head
    if not head or not head.next:  # 判断: not head or not head.next
        return False  # 返回 False

    slow = head  # slow: 赋值/计算
    fast = head.next  # fast: 赋值/计算

    while slow != fast:  # 当 slow != fast 时循环
        if not fast or not fast.next:  # 判断: not fast or not fast.next
            return False  # 返回 False
        slow = slow.next  # slow: 赋值/计算
        fast = fast.next.next  # fast: 赋值/计算

    return True  # 返回 True
```
 
### 142. 环形链表 II (Linked List Cycle II)
 
**题目描述：** 
给定一个链表的头节点 `head`，返回链表开始入环的第一个节点。如果链表无环，则返回 `null`。 
如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 `pos` 是 `-1`，则在该链表中没有环。 
**示例：** 
 
```python
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```
 
**解题思路：** 
使用快慢指针找到相遇点，然后找到环的入口： 
- 使用快慢指针找到相遇点
- 设从起点到环入口的距离为 `a`，从环入口到相遇点的距离为 `b`，从相遇点到环入口的距离为 `c`
- 当快慢指针相遇时，慢指针走了 `a + b`，快指针走了 `a + b + n(b + c)`
- 由于快指针速度是慢指针的 2 倍：`2(a + b) = a + b + n(b + c)`，化简得 `a = (n-1)(b+c) + c`
- 这意味着从起点到环入口的距离等于从相遇点到环入口的距离（加上整数倍的环长）
- 因此，将一个指针重置到起点，两个指针同时移动，相遇点就是环的入口
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是链表中节点的数量
- 空间复杂度：O(1)，只需要常数的额外空间
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：环形链表 II
# 思路：使用快慢指针找到相遇点，然后找到环的入口：；使用快慢指针找到相遇点设从起点到环入口的距离为 a，从环入口到相遇点的距离为 b，从相遇点到环入口的距离为 c当快慢指针相遇时，慢指针走了 a + b，快指针走了 a + b + n(b + c)由于快指针速度是慢指针的 2 倍：2(a + b) = a
def detectCycle(head):  # 定义函数 detectCycle，接收参数: head
    if not head or not head.next:  # 判断: not head or not head.next
        return None  # 返回 None

    slow = fast = head  # slow: 赋值/计算

    # 找到相遇点
    while fast and fast.next:  # 当 fast and fast.next 时循环
        slow = slow.next  # slow: 赋值/计算
        fast = fast.next.next  # fast: 赋值/计算
        if slow == fast:  # 判断: slow == fast
            break  # 跳出当前循环
    else:  # 否则 (以上条件都不满足时执行)
        return None  # 没有环  # 返回计算结果

    # 找到环的入口
    slow = head  # slow: 赋值/计算
    while slow != fast:  # 当 slow != fast 时循环
        slow = slow.next  # slow: 赋值/计算
        fast = fast.next  # fast: 赋值/计算

    return slow  # 返回计算结果
```
 
### 160. 相交链表 (Intersection of Two Linked Lists)
 
**题目描述：** 
给你两个单链表的头节点 `headA` 和 `headB`，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 `null`。 
**示例：** 
 
```python
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
输出：Intersected at '8'
解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
```
 
**解题思路：** 
使用双指针法： 
- 创建两个指针 `pA` 和 `pB`，分别指向两个链表的头节点
- 同时移动两个指针，当其中一个指针到达链表末尾时，将其重置到另一个链表的头节点
- 如果两个链表相交，两个指针最终会在相交节点相遇
- 如果两个链表不相交，两个指针最终都会到达 `nullptr`
 
**复杂度分析：** 
- 时间复杂度：O(m + n)，其中 m 和 n 分别是两个链表的长度
- 空间复杂度：O(1)，只需要常数的额外空间
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：相交链表
# 思路：使用双指针法：；创建两个指针 pA 和 pB，分别指向两个链表的头节点同时移动两个指针，当其中一个指针到达链表末尾时，将其重置到另一个链表的头节点如果两个链表相交，两个指针最终会在相交节点相遇如果两个链表不相交，两个指针最终都会到达 nullptr
def getIntersectionNode(headA, headB):  # 定义函数 getIntersectionNode，接收参数: headA、headB
    if not headA or not headB:  # 判断: not headA or not headB
        return None  # 返回 None

    pA, pB = headA, headB  # 交换/多重赋值

    while pA != pB:  # 当 pA != pB 时循环
        pA = pA.next if pA else headB  # pA: 赋值/计算
        pB = pB.next if pB else headA  # pB: 赋值/计算

    return pA  # 返回计算结果
```
 
### 19. 删除链表的倒数第 N 个结点 (Remove Nth Node From End of List)
 
**题目描述：** 
给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。 
**示例：** 
 
```python
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```
 
**解题思路：** 
使用双指针法： 
- 创建一个虚拟头节点 `dummy`，简化边界处理
- 使用两个指针 `first` 和 `second`，`first` 先移动 `n + 1` 步
- 然后同时移动 `first` 和 `second`，直到 `first` 到达链表末尾
- 此时 `second` 指向倒数第 `n + 1` 个节点，删除其下一个节点即可
 
**复杂度分析：** 
- 时间复杂度：O(L)，其中 L 是链表的长度
- 空间复杂度：O(1)，只需要常数的额外空间
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：删除链表的倒数第 N 个结点
# 思路：使用双指针法：；创建一个虚拟头节点 dummy，简化边界处理使用两个指针 first 和 second，first 先移动 n + 1 步然后同时移动 first 和 second，直到 first 到达链表末尾此时 second 指向倒数第 n + 1 个节点，删除其下一个节点即可
def removeNthFromEnd(head, n):  # 定义函数 removeNthFromEnd，接收参数: head、n
    dummy = ListNode(0)  # dummy: 赋值/计算
    dummy.next = head  # dummy.next: 赋值/计算

    first = dummy  # first: 赋值/计算
    second = dummy  # second: 赋值/计算

    # first 先移动 n + 1 步
    for _ in range(n + 1):  # 遍历: _ 依次取 range(n + 1) 中的每个值
        first = first.next  # first: 赋值/计算

    # 同时移动 first 和 second
    while first:  # 当 first 时循环
        first = first.next  # first: 赋值/计算
        second = second.next  # second: 赋值/计算

    # 删除倒数第 n 个节点
    second.next = second.next.next  # second.next: 赋值/计算

    return dummy.next  # 返回计算结果
```
 
### 2. 两数相加 (Add Two Numbers)
 
**题目描述：** 
给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 **一位** 数字。 
请你将两个数相加，并以相同形式返回一个表示和的链表。 
你可以假设除了数字 0 之外，这两个数都不会以 0 开头。 
**示例：** 
 
```python
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
```
 
**解题思路：** 
模拟加法过程： 
- 同时遍历两个链表，逐位相加
- 使用变量 `carry` 记录进位
- 对于每一位，计算 `sum = l1.val + l2.val + carry`
- 新节点的值为 `sum % 10`，进位为 `sum // 10`
- 如果其中一个链表遍历完了，继续处理另一个链表
- 最后如果还有进位，需要添加一个新节点
 
**复杂度分析：** 
- 时间复杂度：O(max(m, n))，其中 m 和 n 分别是两个链表的长度
- 空间复杂度：O(max(m, n))，新链表的长度最多为 max(m, n) + 1
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：两数相加
# 思路：模拟加法过程：；同时遍历两个链表，逐位相加使用变量 carry 记录进位对于每一位，计算 sum = l1.val + l2.val + carry新节点的值为 sum % 10，进位为 sum // 10如果其中一个链表遍历完了，继续处理另一个链表最后如果还有进位，需要添加一个新节点
def addTwoNumbers(l1, l2):  # 定义函数 addTwoNumbers，接收参数: l1、l2
    dummy = ListNode(0)  # dummy: 赋值/计算
    curr = dummy  # curr: 赋值/计算
    carry = 0  # carry: 计数器/下标初始化为0

    while l1 or l2 or carry:  # 当 l1 or l2 or carry 时循环
        val1 = l1.val if l1 else 0  # val1: 赋值/计算
        val2 = l2.val if l2 else 0  # val2: 赋值/计算

        total = val1 + val2 + carry  # total: 赋值/计算
        carry = total // 10  # carry: 赋值/计算
        curr.next = ListNode(total % 10)  # curr.next: 赋值/计算
        curr = curr.next  # curr: 赋值/计算

        l1 = l1.next if l1 else None  # l1: 赋值/计算
        l2 = l2.next if l2 else None  # l2: 赋值/计算

    return dummy.next  # 返回计算结果
```
 
### 23. 合并 K 个升序链表 (Merge k Sorted Lists)
 
**题目描述：** 
给你一个链表数组，每个链表都已经按升序排列。 
请你将所有链表合并到一个升序链表中，返回合并后的链表。 
**示例：** 
 
```python
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```
 
**解题思路：** 
方法一：分治法 
- 将 k 个链表两两合并，直到只剩下一个链表
- 使用分治的思想，递归地合并链表
 
方法二：优先队列（最小堆） 
- 将所有链表的头节点放入优先队列
- 每次取出最小的节点，连接到结果链表
- 如果该节点还有下一个节点，将其放入优先队列
- 重复直到优先队列为空
 
**复杂度分析：** 
- 时间复杂度：O(n log k)，其中 n 是所有链表中节点的总数，k 是链表的数量
- 空间复杂度：O(1)（分治法）或 O(k)（优先队列）
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：合并 K 个升序链表
# 思路：方法一：分治法；将 k 个链表两两合并，直到只剩下一个链表使用分治的思想，递归地合并链表；方法二：优先队列（最小堆）
def mergeKLists(lists):  # 定义函数 mergeKLists，接收参数: lists
    if not lists:  # 判断: not lists
        return None  # 返回 None
    if len(lists) == 1:  # 判断: len(lists) == 1
        return lists[0]  # 返回列表结果

    mid = len(lists) // 2  # mid: 获取长度
    left = mergeKLists(lists[:mid])  # left: 赋值/计算
    right = mergeKLists(lists[mid:])  # right: 赋值/计算

    return mergeTwoLists(left, right)  # 返回计算结果
def mergeTwoLists(l1, l2):  # 定义函数 mergeTwoLists，接收参数: l1、l2
    dummy = ListNode(0)  # dummy: 赋值/计算
    curr = dummy  # curr: 赋值/计算

    while l1 and l2:  # 当 l1 and l2 时循环
        if l1.val <= l2.val:  # 判断: l1.val <= l2.val
            curr.next = l1  # curr.next: 赋值/计算
            l1 = l1.next  # l1: 赋值/计算
        else:  # 否则 (以上条件都不满足时执行)
            curr.next = l2  # curr.next: 赋值/计算
            l2 = l2.next  # l2: 赋值/计算
        curr = curr.next  # curr: 赋值/计算

    curr.next = l1 if l1 else l2  # curr.next: 赋值/计算
    return dummy.next  # 返回计算结果
```
 
## 三、字符串
 
### 20. 有效的括号 (Valid Parentheses)
 
**题目描述：** 
给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s`，判断字符串是否有效。 
有效字符串需满足： 
- 左括号必须用相同类型的右括号闭合。
- 左括号必须以正确的顺序闭合。
- 每个右括号都有一个对应的相同类型的左括号。
 
**示例：** 
 
```python
输入：s = "()[]{}"
输出：true
```
 
**解题思路：** 
使用栈： 
- 遍历字符串，遇到左括号就入栈
- 遇到右括号时，检查栈顶是否是对应的左括号
- 如果是，弹出栈顶；如果不是，返回 false
- 最后检查栈是否为空
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是字符串的长度
- 空间复杂度：O(n)，最坏情况下栈中存储所有左括号
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：有效的括号
# 思路：使用栈：；遇到左括号，入栈遇到右括号，检查栈顶是否匹配如果匹配，出栈；否则返回 false最后检查栈是否为空
def isValid(s):  # 定义函数 isValid，接收参数: s
    stack = []  # stack: 空列表
    mapping = {"hl-string">')': "hl-string">'(', "hl-string">'}': "hl-string">'{', "hl-string">']': "hl-string">'['}  # mapping: 赋值/计算

    for char in s:  # 遍历: char 依次取 s 中的每个值
        if char in mapping:  # 判断: char in mapping
            if not stack or stack.pop() != mapping[char]:  # 判断: not stack or stack.pop() != mapping
                return False  # 返回 False
        else:  # 否则 (以上条件都不满足时执行)
            stack.append(char)  # 追加到列表末尾

    return not stack  # 返回计算结果
```
 
### 49. 字母异位词分组 (Group Anagrams)
 
**题目描述：** 
给你一个字符串数组，请你将 **字母异位词** 组合在一起。可以按任意顺序返回结果列表。 
**字母异位词** 是由重新排列源单词的字母得到的一个新单词，所有源单词中的字母通常恰好只用一次。 
**示例：** 
 
```python
输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
输出: [["bat"],["nat","tan"],["ate","eat","tea"]]
```
 
**解题思路：** 
使用哈希表： 
- 对于每个字符串，将其排序后的结果作为键
- 将具有相同键的字符串分组
- 返回所有分组
 
**复杂度分析：** 
- 时间复杂度：O(nk log k)，其中 n 是字符串数组的长度，k 是字符串的最大长度
- 空间复杂度：O(nk)，用于存储哈希表和结果
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：字母异位词分组
# 思路：使用哈希表：；对于每个字符串，将其排序后的结果作为键将具有相同键的字符串分组返回所有分组
def groupAnagrams(strs):  # 定义函数 groupAnagrams，接收参数: strs
    from collections import defaultdict  # 导入模块

    groups = defaultdict(list)  # groups: 赋值/计算

    for s in strs:  # 遍历: s 依次取 strs 中的每个值
        key = "hl-string">''.join(sorted(s))  # key: 赋值/计算
        groups[key].append(s)  # 追加到列表末尾

    return list(groups.values())  # 返回计算结果
```
 
## 四、动态规划
 
### 70. 爬楼梯 (Climbing Stairs)
 
**题目描述：** 
假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。 
每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶？ 
**示例：** 
 
```python
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶
```
 
**解题思路：** 
动态规划： 
- 定义 `dp[i]` 为到达第 i 阶的方法数
- 状态转移方程：`dp[i] = dp[i-1] + dp[i-2]`
- 初始条件：`dp[1] = 1, dp[2] = 2`
- 可以优化空间复杂度，只使用两个变量
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是楼梯的阶数
- 空间复杂度：O(1)，只需要常数的额外空间（优化后）
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：爬楼梯
# 思路：动态规划：
def climbStairs(n):  # 定义函数 climbStairs，接收参数: n
    if n <= 2:  # 判断: n <= 2
        return n  # 返回计算结果

    a, b = 1, 2  # 交换/多重赋值
    for i in range(3, n + 1):  # 遍历: i 依次取 range(3, n + 1) 中的每个值
        a, b = b, a + b  # 交换/多重赋值

    return b  # 返回计算结果
```
 
### 121. 买卖股票的最佳时机 (Best Time to Buy and Sell Stock)
 
**题目描述：** 
给定一个数组 `prices`，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。 
你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。 
返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0`。 
**示例：** 
 
```python
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```
 
**解题思路：** 
一次遍历： 
- 维护两个变量：`min_price`（到目前为止的最低价格）和 `max_profit`（最大利润）
- 遍历数组，对于每一天：
- 更新最低价格：`min_price = min(min_price, prices[i])`
- 更新最大利润：`max_profit = max(max_profit, prices[i] - min_price)`
 
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是数组的长度
- 空间复杂度：O(1)，只需要常数的额外空间
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：买卖股票的最佳时机
# 思路：一次遍历：；维护一个变量记录最低价格遍历数组，计算当前价格与最低价格的差值更新最大利润
def maxProfit(prices):  # 定义函数 maxProfit，接收参数: prices
    min_price = float("hl-string">'inf')  # min_price: 赋值/计算
    max_profit = 0  # max_profit: 计数器/下标初始化为0

    for price in prices:  # 遍历: price 依次取 prices 中的每个值
        min_price = min(min_price, price)  # min_price: 赋值/计算
        max_profit = max(max_profit, price - min_price)  # max_profit: 赋值/计算

    return max_profit  # 返回计算结果
```
 
### 53. 最大子数组和 (Maximum Subarray)
 
**题目描述：** 
给你一个整数数组 `nums`，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。 
**子数组** 是数组中的一个连续部分。 
**示例：** 
 
```python
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```
 
**解题思路：** 
动态规划（Kadane 算法）： 
- 定义 `dp[i]` 为以第 i 个元素结尾的最大子数组和
- 状态转移方程：`dp[i] = max(nums[i], dp[i-1] + nums[i])`
- 可以优化空间复杂度，只使用一个变量
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是数组的长度
- 空间复杂度：O(1)，只需要常数的额外空间（优化后）
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：最大子数组和
# 思路：动态规划（Kadane 算法）：
def maxSubArray(nums):  # 定义函数 maxSubArray，接收参数: nums
    max_sum = current_sum = nums[0]  # max_sum: 赋值/计算

    for i in range(1, len(nums)):  # 遍历: i 依次取 range(1, len(nums)) 中的每个值
        current_sum = max(nums[i], current_sum + nums[i])  # current_sum: 赋值/计算
        max_sum = max(max_sum, current_sum)  # max_sum: 赋值/计算

    return max_sum  # 返回计算结果
```
 
### 198. 打家劫舍 (House Robber)
 
**题目描述：** 
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。 
给定一个代表每个房屋存放金额的非负整数数组，计算你 **不触动警报装置的情况下**，一夜之内能够偷窃到的最高金额。 
**示例：** 
 
```python
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```
 
**解题思路：** 
动态规划： 
- 定义 `dp[i]` 为偷窃前 i 间房屋能获得的最大金额
- 状态转移方程：`dp[i] = max(dp[i-1], dp[i-2] + nums[i])`
- 不偷第 i 间：`dp[i-1]`
- 偷第 i 间：`dp[i-2] + nums[i]`
 
- 可以优化空间复杂度，只使用两个变量
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是数组的长度
- 空间复杂度：O(1)，只需要常数的额外空间（优化后）
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：打家劫舍
# 思路：动态规划：
def rob(nums):  # 定义函数 rob，接收参数: nums
    if not nums:  # 判断: not nums
        return 0  # 返回 0 (标志/空值)
    if len(nums) == 1:  # 判断: len(nums) == 1
        return nums[0]  # 返回列表结果

    prev2 = nums[0]  # prev2: 赋值/计算
    prev1 = max(nums[0], nums[1])  # prev1: 赋值/计算

    for i in range(2, len(nums)):  # 遍历: i 依次取 range(2, len(nums)) 中的每个值
        current = max(prev1, prev2 + nums[i])  # current: 赋值/计算
        prev2 = prev1  # prev2: 赋值/计算
        prev1 = current  # prev1: 赋值/计算

    return prev1  # 返回计算结果
```
 
### 279. 完全平方数 (Perfect Squares)
 
**题目描述：** 
给你一个整数 `n`，返回 **和为 n 的完全平方数的最少数量**。 
**完全平方数** 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，`1`、`4`、`9` 和 `16` 都是完全平方数，而 `3` 和 `11` 不是。 
**示例：** 
 
```python
输入：n = 12
输出：3
解释：12 = 4 + 4 + 4
```
 
**解题思路：** 
动态规划： 
- 定义 `dp[i]` 为组成数字 i 的完全平方数的最少数量
- 状态转移方程：`dp[i] = min(dp[i], dp[i - j*j] + 1)`，其中 `j*j <= i`
- 初始条件：`dp[0] = 0`
 
**复杂度分析：** 
- 时间复杂度：O(n√n)，其中 n 是给定的整数
- 空间复杂度：O(n)，用于存储 dp 数组
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：完全平方数
# 思路：动态规划：；定义 dp[i] 为组成数字 i 的完全平方数的最少数量状态转移方程：dp[i] = min(dp[i], dp[i - j*j] + 1)，其中 j*j 初始条件：dp[0] = 0
def numSquares(n):  # 定义函数 numSquares，接收参数: n
    dp = [float("hl-string">'inf')] * (n + 1)  # dp: 乘积计算
    dp[0] = 0  # dp[0]: 计数器/下标初始化为0

    for i in range(1, n + 1):  # 遍历: i 依次取 range(1, n + 1) 中的每个值
        j = 1  # j: 初始化为1
        while j * j <= i:  # 当 j * j <= i 时循环
            dp[i] = min(dp[i], dp[i - j * j] + 1)  # dp[i]: 乘积计算
            j += 1  # j 自反赋值+=

    return dp[n]  # 返回列表结果
```
 
### 139. 单词拆分 (Word Break)
 
**题目描述：** 
给你一个字符串 `s` 和一个字符串列表 `wordDict` 作为字典。如果可以利用字典中出现的单词拼接出 `s` 则返回 `true`。 
**注意：** 不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。 
**示例：** 
 
```python
输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以由 "leet" 和 "code" 拼接成。
```
 
**解题思路：** 
动态规划： 
- 定义 `dp[i]` 表示字符串 `s` 的前 i 个字符是否可以被字典中的单词拼接
- 状态转移方程：`dp[i] = dp[j] && s[j:i] in wordDict`，其中 `0 <= j < i`
- 初始条件：`dp[0] = true`（空字符串可以被拼接）
 
**复杂度分析：** 
- 时间复杂度：O(n²)，其中 n 是字符串的长度
- 空间复杂度：O(n)，用于存储 dp 数组
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：单词拆分
# 思路：动态规划：；定义 dp[i] 表示字符串 s 的前 i 个字符是否可以被字典中的单词拼接状态转移方程：dp[i] = dp[j] && s[j:i] in wordDict，其中 0 初始条件：dp[0] = true（空字符串可以被拼接）
def wordBreak(s, wordDict):  # 定义函数 wordBreak，接收参数: s、wordDict
    word_set = set(wordDict)  # word_set: 赋值/计算
    n = len(s)  # n: 获取长度
    dp = [False] * (n + 1)  # dp: 乘积计算
    dp[0] = True  # dp[0]: 布尔值True

    for i in range(1, n + 1):  # 遍历: i 依次取 range(1, n + 1) 中的每个值
        for j in range(i):  # 遍历: j 依次取 range(i) 中的每个值
            if dp[j] and s[j:i] in word_set:  # 判断: dp[j] and s[j:i] in word_set
                dp[i] = True  # dp[i]: 布尔值True
                break  # 跳出当前循环

    return dp[n]  # 返回列表结果
```
 
### 300. 最长递增子序列 (Longest Increasing Subsequence)
 
**题目描述：** 
给你一个整数数组 `nums`，找到其中最长严格递增子序列的长度。 
**子序列** 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，`[3,6,2,7]` 是数组 `[0,3,1,6,2,2,7]` 的子序列。 
**示例：** 
 
```python
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```
 
**解题思路：** 
动态规划 + 二分查找： 
- 
方法一：动态规划 
- 定义 `dp[i]` 为以 `nums[i]` 结尾的最长递增子序列的长度
- 状态转移方程：`dp[i] = max(dp[j]) + 1`，其中 `0 <= j < i` 且 `nums[j] < nums[i]`
- 时间复杂度：O(n²)
 
- 
方法二：贪心 + 二分查找（优化） 
- 维护一个数组 `tails`，其中 `tails[i]` 表示长度为 i+1 的递增子序列的最小末尾元素
- 使用二分查找找到第一个大于等于当前元素的位置
- 时间复杂度：O(n log n)
 
 
**复杂度分析：** 
- 时间复杂度：O(n log n)（优化方法），O(n²)（基础方法）
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：最长递增子序列
# 思路：动态规划 + 二分查找：；使用数组 dp 存储长度为 i+1 的递增子序列的最小末尾元素对于每个元素，使用二分查找找到应该插入的位置如果元素大于所有已有元素，追加到数组末尾
def lengthOfLIS(nums):  # 定义函数 lengthOfLIS，接收参数: nums
    dp = []  # dp: 空列表

    for num in nums:  # 遍历: num 依次取 nums 中的每个值
        left, right = 0, len(dp)  # 交换/多重赋值
        while left < right:  # 当 left < right 时循环
            mid = (left + right) // 2  # mid: 赋值/计算
            if dp[mid] < num:  # 判断: dp[mid] < num
                left = mid + 1  # left: 赋值/计算
            else:  # 否则 (以上条件都不满足时执行)
                right = mid  # right: 赋值/计算

        if left == len(dp):  # 判断: left == len(dp)
            dp.append(num)  # 追加到列表末尾
        else:  # 否则 (以上条件都不满足时执行)
            dp[left] = num  # dp[left]: 赋值/计算

    return len(dp)  # 返回计算结果
```
 
### 322. 零钱兑换 (Coin Change)
 
**题目描述：** 
给你一个整数数组 `coins`，表示不同面额的硬币；以及一个整数 `amount`，表示总金额。 
计算并返回可以凑成总金额所需的 **最少的硬币个数**。如果没有任何一种硬币组合能组成总金额，返回 `-1`。 
你可以认为每种硬币的数量是无限的。 
**示例：** 
 
```python
输入：coins = [1, 2, 5], amount = 11
输出：3
解释：11 = 5 + 5 + 1
```
 
**解题思路：** 
动态规划： 
- 定义 `dp[i]` 为凑成金额 i 所需的最少硬币数
- 状态转移方程：`dp[i] = min(dp[i], dp[i - coin] + 1)`，其中 `coin` 是硬币面额
- 初始条件：`dp[0] = 0`，其他为 `inf`
 
**复杂度分析：** 
- 时间复杂度：O(S × n)，其中 S 是金额，n 是硬币种类数
- 空间复杂度：O(S)，用于存储 dp 数组
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：零钱兑换
# 思路：动态规划：；定义 dp[i] 为凑成金额 i 所需的最少硬币数状态转移方程：dp[i] = min(dp[i], dp[i - coin] + 1)，其中 coin 是硬币面额初始条件：dp[0] = 0，其他为 inf
def coinChange(coins, amount):  # 定义函数 coinChange，接收参数: coins、amount
    dp = [float("hl-string">'inf')] * (amount + 1)  # dp: 乘积计算
    dp[0] = 0  # dp[0]: 计数器/下标初始化为0

    for i in range(1, amount + 1):  # 遍历: i 依次取 range(1, amount + 1) 中的每个值
        for coin in coins:  # 遍历: coin 依次取 coins 中的每个值
            if i >= coin:  # 判断: i >= coin
                dp[i] = min(dp[i], dp[i - coin] + 1)  # dp[i]: 赋值/计算

    return dp[amount] if dp[amount] != float("hl-string">'inf') else -1  # 返回列表结果
```
 
### 72. 编辑距离 (Edit Distance)
 
**题目描述：** 
给你两个单词 `word1` 和 `word2`，请返回将 `word1` 转换成 `word2` 所使用的最少操作数。 
你可以对一个单词进行如下三种操作： 
- 插入一个字符
- 删除一个字符
- 替换一个字符
 
**示例：** 
 
```python
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
```
 
**解题思路：** 
动态规划： 
- 定义 `dp[i][j]` 为将 `word1` 的前 i 个字符转换为 `word2` 的前 j 个字符所需的最少操作数
- 状态转移方程：
- 如果 `word1[i-1] == word2[j-1]`：`dp[i][j] = dp[i-1][j-1]`
- 否则：`dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1`
- `dp[i-1][j]`：删除 word1[i-1]
- `dp[i][j-1]`：在 word1 中插入 word2[j-1]
- `dp[i-1][j-1]`：替换 word1[i-1] 为 word2[j-1]
 
 
 
**复杂度分析：** 
- 时间复杂度：O(m × n)，其中 m 和 n 分别是两个字符串的长度
- 空间复杂度：O(m × n)，可以优化到 O(min(m, n))
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：编辑距离
# 思路：动态规划：；dp[i][j] 表示 word1 的前 i 个字符转换为 word2 的前 j 个字符的最少操作数如果 word1[i-1] == word2[j-1]，dp[i][j] = dp[i-1][j-1]否则，dp[i][j] = min(dp[i-1][j], dp[i][j-1], 
def minDistance(word1, word2):  # 定义函数 minDistance，接收参数: word1、word2
    m, n = len(word1), len(word2)  # 交换/多重赋值
    dp = [[0] * (n + 1) for _ in range(m + 1)]  # dp: 列表推导式

    for i in range(m + 1):  # 遍历: i 依次取 range(m + 1) 中的每个值
        dp[i][0] = i
    for j in range(n + 1):  # 遍历: j 依次取 range(n + 1) 中的每个值
        dp[0][j] = j

    for i in range(1, m + 1):  # 遍历: i 依次取 range(1, m + 1) 中的每个值
        for j in range(1, n + 1):  # 遍历: j 依次取 range(1, n + 1) 中的每个值
            if word1[i-1] == word2[j-1]:  # 判断: word1[i-1] == word2[j-1]
                dp[i][j] = dp[i-1][j-1]
            else:  # 否则 (以上条件都不满足时执行)
                dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1

    return dp[m][n]  # 返回列表结果
```
 
### 1143. 最长公共子序列 (Longest Common Subsequence)
 
**题目描述：** 
给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长 **公共子序列** 的长度。如果不存在 **公共子序列**，返回 `0`。 
一个字符串的 **子序列** 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。 
**示例：** 
 
```python
输入：text1 = "abcde", text2 = "ace"
输出：3
解释：最长公共子序列是 "ace" ，它的长度为 3 。
```
 
**解题思路：** 
动态规划： 
- 定义 `dp[i][j]` 为 `text1` 的前 i 个字符和 `text2` 的前 j 个字符的最长公共子序列长度
- 状态转移方程：
- 如果 `text1[i-1] == text2[j-1]`：`dp[i][j] = dp[i-1][j-1] + 1`
- 否则：`dp[i][j] = max(dp[i-1][j], dp[i][j-1])`
 
 
**复杂度分析：** 
- 时间复杂度：O(m × n)，其中 m 和 n 分别是两个字符串的长度
- 空间复杂度：O(m × n)，可以优化到 O(min(m, n))
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：最长公共子序列
# 思路：动态规划：；定义 dp[i][j] 为 text1 的前 i 个字符和 text2 的前 j 个字符的最长公共子序列长度状态转移方程：如果 text1[i-1] == text2[j-1]：dp[i][j] = dp[i-1][j-1] + 1否则：dp[i][j] = max(dp[i-1][j
def longestCommonSubsequence(text1, text2):  # 定义函数 longestCommonSubsequence，接收参数: text1、text2
    m, n = len(text1), len(text2)  # 交换/多重赋值
    dp = [[0] * (n + 1) for _ in range(m + 1)]  # dp: 列表推导式

    for i in range(1, m + 1):  # 遍历: i 依次取 range(1, m + 1) 中的每个值
        for j in range(1, n + 1):  # 遍历: j 依次取 range(1, n + 1) 中的每个值
            if text1[i-1] == text2[j-1]:  # 判断: text1[i-1] == text2[j-1]
                dp[i][j] = dp[i-1][j-1] + 1
            else:  # 否则 (以上条件都不满足时执行)
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])

    return dp[m][n]  # 返回列表结果
```
 
## 五、二叉树
 
### 94. 二叉树的中序遍历 (Binary Tree Inorder Traversal)
 
**题目描述：** 
给定一个二叉树的根节点 `root`，返回它的 **中序** 遍历。 
**示例：** 
 
```python
输入：root = [1,null,2,3]
输出：[1,3,2]
```
 
**解题思路：** 
方法1：递归 
- 先遍历左子树
- 访问根节点
- 再遍历右子树
 
方法2：迭代（使用栈） 
- 使用栈模拟递归过程
- 先一直向左遍历，将节点入栈
- 当左子树遍历完后，弹出栈顶节点并访问
- 转向右子树
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是二叉树的节点数
- 空间复杂度：O(h)，其中 h 是二叉树的高度
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：二叉树的中序遍历
# 思路：方法1：递归；先遍历左子树访问根节点再遍历右子树；方法2：迭代（使用栈）
def inorderTraversal(root):  # 定义函数 inorderTraversal，接收参数: root
    result = []  # result: 空列表

    def inorder(node):  # 定义函数 inorder，接收参数: node
        if node:  # 判断: node
            inorder(node.left)
            result.append(node.val)  # 追加到列表末尾
            inorder(node.right)

    inorder(root)
    return result  # 返回计算结果
# 迭代方法
def inorderTraversal(root):  # 定义函数 inorderTraversal，接收参数: root
    result = []  # result: 空列表
    stack = []  # stack: 空列表
    curr = root  # curr: 赋值/计算

    while curr or stack:  # 当 curr or stack 时循环
        while curr:  # 当 curr 时循环
            stack.append(curr)  # 追加到列表末尾
            curr = curr.left  # curr: 赋值/计算
        curr = stack.pop()  # curr: 赋值/计算
        result.append(curr.val)  # 追加到列表末尾
        curr = curr.right  # curr: 赋值/计算

    return result  # 返回计算结果
```
 
### 104. 二叉树的最大深度 (Maximum Depth of Binary Tree)
 
**题目描述：** 
给定一个二叉树，找出其最大深度。 
二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。 
**说明：** 叶子节点是指没有子节点的节点。 
**示例：** 
 
```python
给定二叉树 [3,9,20,null,null,15,7]，
    3
   / \
  9  20
    /  \
   15   7
返回它的最大深度 3 。
```
 
**解题思路：** 
递归法： 
- 如果根节点为空，返回 0
- 否则，返回 `max(左子树深度, 右子树深度) + 1`
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是二叉树的节点数
- 空间复杂度：O(h)，其中 h 是二叉树的高度（递归栈的深度）
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：二叉树的最大深度
# 思路：递归法：；如果根节点为空，返回 0否则，返回 max(左子树深度, 右子树深度) + 1
class TreeNode:  # 定义类 TreeNode
    def __init__(self, val=0, left=None, right=None):  # 定义函数 __init__，接收参数: self、val、left、right
        self.val = val  # self.val: 赋值/计算
        self.left = left  # self.left: 赋值/计算
        self.right = right  # self.right: 赋值/计算
def maxDepth(root):  # 定义函数 maxDepth，接收参数: root
    if not root:  # 判断: not root
        return 0  # 返回 0 (标志/空值)
    return max(maxDepth(root.left), maxDepth(root.right)) + 1  # 返回计算结果
```
 
### 101. 对称二叉树 (Symmetric Tree)
 
**题目描述：** 
给你一个二叉树的根节点 `root`，检查它是否轴对称。 
**示例：** 
 
```python
输入：root = [1,2,2,3,4,4,3]
输出：true
```
 
**解题思路：** 
递归法： 
- 定义辅助函数 `isMirror(left, right)` 判断两个子树是否镜像对称
- 两个子树镜像对称的条件：
- 两个根节点的值相等
- 左子树的左子树与右子树的右子树镜像对称
- 左子树的右子树与右子树的左子树镜像对称
 
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是二叉树的节点数
- 空间复杂度：O(h)，其中 h 是二叉树的高度
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：对称二叉树
# 思路：递归法：；定义辅助函数 isMirror(left, right) 判断两个子树是否镜像对称两个子树镜像对称的条件：两个根节点的值相等左子树的左子树与右子树的右子树镜像对称左子树的右子树与右子树的左子树镜像对称
def isSymmetric(root):  # 定义函数 isSymmetric，接收参数: root
    def isMirror(left, right):  # 定义函数 isMirror，接收参数: left、right
        if not left and not right:  # 判断: not left and not right
            return True  # 返回 True
        if not left or not right:  # 判断: not left or not right
            return False  # 返回 False
        return (left.val == right.val and  # 返回计算结果
                isMirror(left.left, right.right) and
                isMirror(left.right, right.left))

    if not root:  # 判断: not root
        return True  # 返回 True
    return isMirror(root.left, root.right)  # 返回计算结果
```
 
### 102. 二叉树的层序遍历 (Binary Tree Level Order Traversal)
 
**题目描述：** 
给你二叉树的根节点 `root`，返回其节点值的 **层序遍历**。（即逐层地，从左到右访问所有节点）。 
**示例：** 
 
```python
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[9,20],[15,7]]
```
 
**解题思路：** 
使用队列（BFS）： 
- 将根节点入队
- 当队列不为空时：
- 记录当前层的节点数
- 遍历当前层的所有节点，将它们的值加入结果，并将它们的子节点入队
- 将当前层的结果加入最终结果
 
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是二叉树的节点数
- 空间复杂度：O(n)，用于存储队列
 
**Python 解答（含详细注释）：** 
 
```python
from collections import deque  # 导入模块
# 目的：二叉树的层序遍历
# 思路：使用队列（BFS）：；将根节点入队当队列不为空时：记录当前层的节点数遍历当前层的所有节点，将它们的值加入结果，并将它们的子节点入队将当前层的结果加入最终结果
def levelOrder(root):  # 定义函数 levelOrder，接收参数: root
    if not root:  # 判断: not root
        return []  # 返回 [] (标志/空值)

    result = []  # result: 空列表
    queue = deque([root])  # queue: 赋值/计算

    while queue:  # 当 queue 时循环
        level_size = len(queue)  # level_size: 获取长度
        level = []  # level: 空列表

        for _ in range(level_size):  # 遍历: _ 依次取 range(level_size) 中的每个值
            node = queue.popleft()  # node: 赋值/计算
            level.append(node.val)  # 追加到列表末尾

            if node.left:  # 判断: node.left
                queue.append(node.left)  # 追加到列表末尾
            if node.right:  # 判断: node.right
                queue.append(node.right)  # 追加到列表末尾

        result.append(level)  # 追加到列表末尾

    return result  # 返回计算结果
```
 
### 108. 将有序数组转换为二叉搜索树 (Convert Sorted Array to Binary Search Tree)
 
**题目描述：** 
给你一个整数数组 `nums`，其中元素已经按 **升序** 排列，请你将其转换为一棵 **高度平衡** 二叉搜索树。 
高度平衡 二叉树是一棵满足「每个节点的左右两个子树的高度差的绝对值不超过 1」的二叉树。 
**示例：** 
 
```python
输入：nums = [-10,-3,0,5,9]
输出：[0,-3,9,-10,null,5]
解释：[0,-10,5,null,-3,null,9] 也将被视为正确答案：
```
 
**解题思路：** 
递归法： 
- 选择数组中间的元素作为根节点
- 递归构建左子树（左半部分）和右子树（右半部分）
- 这样可以保证树的高度平衡
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是数组的长度
- 空间复杂度：O(log n)，递归栈的深度
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：将有序数组转换为二叉搜索树
# 思路：递归法：；选择数组中间的元素作为根节点递归构建左子树（左半部分）和右子树（右半部分）这样可以保证树的高度平衡
def sortedArrayToBST(nums):  # 定义函数 sortedArrayToBST，接收参数: nums
    def build(left, right):  # 定义函数 build，接收参数: left、right
        if left > right:  # 判断: left > right
            return None  # 返回 None

        mid = (left + right) // 2  # mid: 赋值/计算
        root = TreeNode(nums[mid])  # root: 赋值/计算
        root.left = build(left, mid - 1)  # root.left: 赋值/计算
        root.right = build(mid + 1, right)  # root.right: 赋值/计算
        return root  # 返回计算结果

    return build(0, len(nums) - 1)  # 返回计算结果
```
 
### 98. 验证二叉搜索树 (Validate Binary Search Tree)
 
**题目描述：** 
给你一个二叉树的根节点 `root`，判断其是否是一个有效的二叉搜索树。 
**有效** 二叉搜索树定义如下： 
- 节点的左子树只包含 **小于** 当前节点的数。
- 节点的右子树只包含 **大于** 当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。
 
**示例：** 
 
```python
输入：root = [2,1,3]
输出：true
```
 
**解题思路：** 
中序遍历法： 
- 二叉搜索树的中序遍历结果是严格递增的
- 进行中序遍历，检查当前节点的值是否大于前一个节点的值
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是二叉树的节点数
- 空间复杂度：O(h)，其中 h 是二叉树的高度
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：验证二叉搜索树
# 思路：中序遍历法：；二叉搜索树的中序遍历结果是严格递增的进行中序遍历，检查当前节点的值是否大于前一个节点的值
def isValidBST(root):  # 定义函数 isValidBST，接收参数: root
    prev = None  # prev: 空值None

    def inorder(node):  # 定义函数 inorder，接收参数: node
        nonlocal prev
        if not node:  # 判断: not node
            return True  # 返回 True

        if not inorder(node.left):  # 判断: not inorder(node.left)
            return False  # 返回 False

        if prev is not None and node.val <= prev:  # 判断: prev is not None and node.val <= pr
            return False  # 返回 False
        prev = node.val  # prev: 赋值/计算

        return inorder(node.right)  # 返回计算结果

    return inorder(root)  # 返回计算结果
```
 
### 236. 二叉树的最近公共祖先 (Lowest Common Ancestor of a Binary Tree)
 
**题目描述：** 
给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。 
**最近公共祖先** 的定义为：”对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。” 
**示例：** 
 
```python
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出：3
解释：节点 5 和节点 1 的最近公共祖先是节点 3 。
```
 
**解题思路：** 
递归法： 
- 情况1:当前节点为空或等于 p 或 q，返回当前节点
- 情况2:左右节点都找到了，返回root
- 情况3:左/右节点找到了，返回递归那一边的结果
- 情况4:左右子树都没有，返回空节点
- 本质上是后序遍历，查询顺序-左右根
- 时空复杂度o(n)
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是二叉树的节点数
- 空间复杂度：O(h)，其中 h 是二叉树的高度
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：二叉树的最近公共祖先
# 思路：递归法：；如果当前节点为空或等于 p 或 q，返回当前节点递归查找左子树和右子树如果左右子树都找到了节点，说明当前节点是最近公共祖先如果只有一边找到了，返回那一边的结果
def lowestCommonAncestor(self,root:'TreeNode', p:'TreeNode', q:'TreeNode')-> 'TreeNode': # 定义函数 lowestCommonAncestor，接收参数: root、p、q
    if root is None or root is p or root is q:  
        return root  # 返回计算结果

    left = lowestCommonAncestor(root.left, p, q)  # left: 赋值/计算
    right = lowestCommonAncestor(root.right, p, q)  # right: 赋值/计算

    if left and right:  
        return root  
    return left if left else right  # 包含了左右都没有的情况
```
 
### 124. 二叉树中的最大路径和 (Binary Tree Maximum Path Sum)
 
**题目描述：** 
路径 被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。同一个节点在一条路径序列中 **至多出现一次**。该路径 **至少包含一个** 节点，且不一定经过根节点。 
**路径和** 是路径中各节点值的总和。 
给你一个二叉树的根节点 `root`，返回其 **最大路径和**。 
**示例：** 
 
```python
输入：root = [-10,9,20,null,null,15,7]
输出：42
解释：最优路径是 15 -> 20 -> 7 ，路径和为 15 + 20 + 7 = 42
```

<img src="/img/leetcode-124-max-path-sum.png" alt="二叉树最大路径和示意图" width="50%">
 
**解题思路：** 
- 其实感觉有点像一笔画问题 不走回头路 不反复经过一个节点
递归法： 
- 定义辅助函数 `maxGain(node)` 返回以 node 为起点的最大路径和
- 对于每个节点，计算：
- 经过该节点的最大路径和 = `node.val + left_gain + right_gain`
- 返回给父节点的最大增益 = `node.val + max(left_gain, right_gain)`
 
- 在递归过程中更新全局最大路径和
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是二叉树的节点数
- 空间复杂度：O(h)，其中 h 是二叉树的高度
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：二叉树中的最大路径和
# 思路：递归法：；定义辅助函数 maxGain(node) 返回以 node 为起点的最大路径和对于每个节点，计算：经过该节点的最大路径和 = node.val + left_gain + right_gain返回给父节点的最大增益 = node.val + max(left_gain, right_ga
def maxPathSum(root):  # 定义函数 maxPathSum，接收参数: root
    max_sum = float("hl-string">'-inf')  # max_sum: 赋值/计算

    def maxGain(node):  # 定义函数 maxGain，接收参数: node
        nonlocal max_sum
        if not node:  # 判断: not node
            return 0  # 返回 0 (标志/空值)

        left_gain = max(maxGain(node.left), 0)  # left_gain: 赋值/计算
        right_gain = max(maxGain(node.right), 0)  # right_gain: 赋值/计算

        current_path = node.val + left_gain + right_gain  # current_path: 赋值/计算
        max_sum = max(max_sum, current_path)  # max_sum: 赋值/计算

        return node.val + max(left_gain, right_gain)  # 返回计算结果

    maxGain(root)
    return max_sum  # 返回计算结果
```
 
### 105. 从前序与中序遍历序列构造二叉树 (Construct Binary Tree from Preorder and Inorder Traversal)
 
**题目描述：** 
给定两个整数数组 `preorder` 和 `inorder`，其中 `preorder` 是二叉树的**先序遍历**，`inorder` 是同一棵树的**中序遍历**，请构造二叉树并返回其根节点。 
**示例：** 
 
```python
输入：preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
输出：[3,9,20,null,null,15,7]
```
 
**解题思路：** 
递归构造： 
- 前序遍历的第一个元素是根节点
- 在中序遍历中找到根节点的位置，左边是左子树，右边是右子树
- 递归构造左子树和右子树
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是节点数
- 空间复杂度：O(n)，用于存储哈希表
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：从前序与中序遍历序列构造二叉树
# 思路：递归构造：；前序遍历的第一个元素是根节点在中序遍历中找到根节点的位置，左边是左子树，右边是右子树递归构造左子树和右子树
def buildTree(preorder, inorder):  # 定义函数 buildTree，接收参数: preorder、inorder
    if not preorder or not inorder:  # 判断: not preorder or not inorder
        return None  # 返回 None

    inorder_map = {val: idx for idx, val in enumerate(inorder)}  # inorder_map: 赋值/计算
    pre_idx = 0  # pre_idx: 计数器/下标初始化为0

    def build(left, right):  # 定义函数 build，接收参数: left、right
        nonlocal pre_idx
        if left > right:  # 判断: left > right
            return None  # 返回 None

        root_val = preorder[pre_idx]  # root_val: 赋值/计算
        root = TreeNode(root_val)  # root: 赋值/计算
        pre_idx += 1  # pre_idx 自反赋值+=

        root_index = inorder_map[root_val]  # root_index: 赋值/计算
        root.left = build(left, root_index - 1)  # root.left: 赋值/计算
        root.right = build(root_index + 1, right)  # root.right: 赋值/计算

        return root  # 返回计算结果

    return build(0, len(inorder) - 1)  # 返回计算结果
```
 
### 103. 二叉树的锯齿形层序遍历 (Binary Tree Zigzag Level Order Traversal)
 
**题目描述：** 
给你二叉树的根节点 `root`，返回其节点值的 **锯齿形层序遍历**。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。 
**示例：** 
 
```python
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[20,9],[15,7]]
```
 
**解题思路：** 
层序遍历 + 方向标志： 
- 使用队列进行层序遍历
- 使用一个标志位控制每层的遍历方向
- 奇数层从左到右，偶数层从右到左（或相反）
 
**复杂度分析：** 
- 时间复杂度：O(n)，其中 n 是节点数
- 空间复杂度：O(n)，队列的空间
 
**Python 解答（含详细注释）：** 
 
```python
from collections import deque  # 导入模块
# 目的：二叉树的锯齿形层序遍历
# 思路：层序遍历 + 方向标志：；使用队列进行层序遍历使用一个标志位控制每层的遍历方向奇数层从左到右，偶数层从右到左（或相反）
def zigzagLevelOrder(root):  # 定义函数 zigzagLevelOrder，接收参数: root
    if not root:  # 判断: not root
        return []  # 返回 [] (标志/空值)

    result = []  # result: 空列表
    queue = deque([root])  # queue: 赋值/计算
    left_to_right = True  # left_to_right: 布尔值True

    while queue:  # 当 queue 时循环
        level_size = len(queue)  # level_size: 获取长度
        level = []  # level: 空列表

        for _ in range(level_size):  # 遍历: _ 依次取 range(level_size) 中的每个值
            node = queue.popleft()  # node: 赋值/计算
            level.append(node.val)  # 追加到列表末尾

            if node.left:  # 判断: node.left
                queue.append(node.left)  # 追加到列表末尾
            if node.right:  # 判断: node.right
                queue.append(node.right)  # 追加到列表末尾

        if not left_to_right:  # 判断: not left_to_right
            level.reverse()  # 原地反转列表
        result.append(level)  # 追加到列表末尾
        left_to_right = not left_to_right  # left_to_right: 赋值/计算

    return result  # 返回计算结果
```
 
### 199. 二叉树的右视图 (Binary Tree Right Side View)
 
**题目描述：** 
给定一个二叉树的 **根节点** `root`，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。 
**示例：** 
 
```python
输入：root = [1,2,3,null,5,null,4]
输出：[1,3,4]
```
 
**解题思路：** 
层序遍历： 
- 使用层序遍历，记录每层最后一个节点
- 或者使用DFS，优先访问右子树，记录每层第一个访问到的节点
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(h)，其中 h 是树的高度
 
**Python 解答（含详细注释）：** 
 
```python
from collections import deque  # 导入模块
# 目的：二叉树的右视图
# 思路：层序遍历：；使用层序遍历，记录每层最后一个节点或者使用DFS，优先访问右子树，记录每层第一个访问到的节点
def rightSideView(root):  # 定义函数 rightSideView，接收参数: root
    if not root:  # 判断: not root
        return []  # 返回 [] (标志/空值)

    result = []  # result: 空列表
    queue = deque([root])  # queue: 赋值/计算

    while queue:  # 当 queue 时循环
        level_size = len(queue)  # level_size: 获取长度
        for i in range(level_size):  # 遍历: i 依次取 range(level_size) 中的每个值
            node = queue.popleft()  # node: 赋值/计算
            if i == level_size - 1:  # 判断: i == level_size - 1
                result.append(node.val)  # 追加到列表末尾

            if node.left:  # 判断: node.left
                queue.append(node.left)  # 追加到列表末尾
            if node.right:  # 判断: node.right
                queue.append(node.right)  # 追加到列表末尾

    return result  # 返回计算结果
```
 
### 297. 二叉树的序列化与反序列化 (Serialize and Deserialize Binary Tree)
 
**题目描述：** 
序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。 
请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。 
**示例：** 
 
```python
输入：root = [1,2,3,null,null,4,5]
输出：[1,2,3,null,null,4,5]
```
 
**解题思路：** 
前序遍历序列化： 
- 序列化：使用前序遍历，空节点用特殊字符（如”None”）表示
- 反序列化：按照前序遍历的顺序重建二叉树
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：二叉树的序列化与反序列化
# 思路：前序遍历序列化：；序列化：使用前序遍历，空节点用特殊字符（如”None”）表示反序列化：按照前序遍历的顺序重建二叉树
class Codec:  # 定义类 Codec
    def serialize(self, root):  # 定义函数 serialize，接收参数: self、root
        def rserialize(node):  # 定义函数 rserialize，接收参数: node
            if not node:  # 判断: not node
                return "None,"  # 返回计算结果
            return str(node.val) + "," + rserialize(node.left) + rserialize(node.right)  # 返回计算结果
        return rserialize(root)  # 返回计算结果

    def deserialize(self, data):  # 定义函数 deserialize，接收参数: self、data
        def rdeserialize(data_list):  # 定义函数 rdeserialize，接收参数: data_list
            if data_list[0] == "None":  # 判断: data_list[0] == "None"
                data_list.pop(0)  # 弹出末尾元素
                return None  # 返回 None

            root = TreeNode(int(data_list[0]))  # root: 赋值/计算
            data_list.pop(0)  # 弹出末尾元素
            root.left = rdeserialize(data_list)  # root.left: 赋值/计算
            root.right = rdeserialize(data_list)  # root.right: 赋值/计算
            return root  # 返回计算结果

        data_list = data.split("hl-string">',')  # data_list: 赋值/计算
        return rdeserialize(data_list)  # 返回计算结果
```
 
### 230. 二叉搜索树中第K小的元素 (Kth Smallest Element in a BST)
 
**题目描述：** 
给定一个二叉搜索树的根节点 `root`，和一个整数 `k`，请你设计一个算法查找其中第 `k` 个最小元素（从 1 开始计数）。 
**示例：** 
 
```python
输入：root = [3,1,4,null,2], k = 1
输出：1
```
 
**解题思路：** 
中序遍历： 
- 二叉搜索树的中序遍历结果是严格递增的
- 进行中序遍历，找到第 k 个元素
 
**复杂度分析：** 
- 时间复杂度：O(H + k)，其中 H 是树的高度
- 空间复杂度：O(H)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：二叉搜索树中第K小的元素
# 思路：中序遍历：；二叉搜索树的中序遍历结果是严格递增的进行中序遍历，找到第 k 个元素
def kthSmallest(root, k):  # 定义函数 kthSmallest，接收参数: root、k
    stack = []  # stack: 空列表
    curr = root  # curr: 赋值/计算

    while curr or stack:  # 当 curr or stack 时循环
        while curr:  # 当 curr 时循环
            stack.append(curr)  # 追加到列表末尾
            curr = curr.left  # curr: 赋值/计算

        curr = stack.pop()  # curr: 赋值/计算
        k -= 1  # k 自反赋值-=
        if k == 0:  # 判断: k == 0
            return curr.val  # 返回计算结果
        curr = curr.right  # curr: 赋值/计算
```
 
### 113. 路径总和 II (Path Sum II)
 
**题目描述：** 
给你二叉树的根节点 `root` 和一个整数目标和 `targetSum`，找出所有 **从根节点到叶子节点** 路径总和等于给定目标和的路径。 
**示例：** 
 
```python
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：[[5,4,11,2],[5,8,4,5]]
```
 
**解题思路：** 
DFS + 回溯： 
- 使用深度优先搜索遍历所有路径
- 维护当前路径和路径和
- 当到达叶子节点且路径和等于目标值时，将路径加入结果
- 回溯时撤销选择
 
**复杂度分析：** 
- 时间复杂度：O(n²)，最坏情况下需要复制路径
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：路径总和 II
# 思路：DFS + 回溯：；使用深度优先搜索遍历所有路径维护当前路径和路径和当到达叶子节点且路径和等于目标值时，将路径加入结果回溯时撤销选择
def pathSum(root, targetSum):  # 定义函数 pathSum，接收参数: root、targetSum
    result = []  # result: 空列表
    path = []  # path: 空列表

    def dfs(node, target):  # 定义函数 dfs，接收参数: node、target
        if not node:  # 判断: not node
            return

        path.append(node.val)  # 追加到列表末尾
        target -= node.val  # target 自反赋值-=

        if not node.left and not node.right and target == 0:  # 判断: not node.left and not node.right an
            result.append(path[:])  # 追加到列表末尾

        dfs(node.left, target)
        dfs(node.right, target)

        path.pop()  # 弹出末尾元素

    dfs(root, targetSum)
    return result  # 返回计算结果
```
 
### 116. 填充每个节点的下一个右侧节点指针 (Populating Next Right Pointers in Each Node)
 
**题目描述：** 
给定一个 **完美二叉树**，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下： 
填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL`。 
初始状态下，所有 next 指针都被设置为 `NULL`。 
**示例：** 
 
```python
输入：root = [1,2,3,4,5,6,7]
输出：[1,#,2,3,#,4,5,6,7,#]
```
 
**解题思路：** 
层序遍历： 
- 使用层序遍历，连接同一层的节点
- 或者使用已建立的 next 指针，逐层连接下一层
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)（不使用队列的情况下）
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：填充每个节点的下一个右侧节点指针
# 思路：层序遍历：；使用层序遍历，连接同一层的节点或者使用已建立的 next 指针，逐层连接下一层
def connect(root):  # 定义函数 connect，接收参数: root
    if not root:  # 判断: not root
        return root  # 返回计算结果

    leftmost = root  # leftmost: 赋值/计算

    while leftmost.left:  # 当 leftmost.left 时循环
        head = leftmost  # head: 赋值/计算
        while head:  # 当 head 时循环
            head.left.next = head.right  # head.left.next: 赋值/计算
            if head.next:  # 判断: head.next
                head.right.next = head.next.left  # head.right.next: 赋值/计算
            head = head.next  # head: 赋值/计算
        leftmost = leftmost.left  # leftmost: 赋值/计算

    return root  # 返回计算结果
```
 
### 257. 二叉树的所有路径 (Binary Tree Paths)
 
**题目描述：** 
给你一个二叉树的根节点 `root`，按 **任意顺序**，返回所有从根节点到叶子节点的路径。 
**叶子节点** 是指没有子节点的节点。 
**示例：** 
 
```python
输入：root = [1,2,3,null,5]
输出：["1->2->5","1->3"]
```
 
**解题思路：** 
DFS + 回溯： 
- 使用深度优先搜索遍历所有路径
- 当到达叶子节点时，将路径加入结果
- 回溯时撤销选择
 
**复杂度分析：** 
- 时间复杂度：O(n²)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：二叉树的所有路径
# 思路：DFS + 回溯：；使用深度优先搜索遍历所有路径当到达叶子节点时，将路径加入结果回溯时撤销选择
def binaryTreePaths(root):  # 定义函数 binaryTreePaths，接收参数: root
    result = []  # result: 空列表

    def dfs(node, path):  # 定义函数 dfs，接收参数: node、path
        if not node:  # 判断: not node
            return

        path += str(node.val)  # path 自反赋值+=

        if not node.left and not node.right:  # 判断: not node.left and not node.right
            result.append(path)  # 追加到列表末尾
        else:  # 否则 (以上条件都不满足时执行)
            path += "->"  # path 自反赋值+=
            dfs(node.left, path)
            dfs(node.right, path)

    dfs(root, "")
    return result  # 返回计算结果
```
 
### 111. 二叉树的最小深度 (Minimum Depth of Binary Tree)
 
**题目描述：** 
给定一个二叉树，找出其最小深度。 
最小深度是从根节点到最近叶子节点的最短路径上的节点数量。 
**说明：** 叶子节点是指没有子节点的节点。 
**示例：** 
 
```python
输入：root = [3,9,20,null,null,15,7]
输出：2
```
 
**解题思路：** 
DFS 或 BFS： 
- DFS：递归计算左右子树的最小深度
- BFS：层序遍历，找到第一个叶子节点
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(h)，其中 h 是树的高度
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：二叉树的最小深度
# 思路：DFS 或 BFS：；DFS：递归计算左右子树的最小深度BFS：层序遍历，找到第一个叶子节点
def minDepth(root):  # 定义函数 minDepth，接收参数: root
    if not root:  # 判断: not root
        return 0  # 返回 0 (标志/空值)

    if not root.left and not root.right:  # 判断: not root.left and not root.right
        return 1  # 返回 1 (标志/空值)

    if not root.left:  # 判断: not root.left
        return 1 + minDepth(root.right)  # 返回计算结果
    if not root.right:  # 判断: not root.right
        return 1 + minDepth(root.left)  # 返回计算结果

    return 1 + min(minDepth(root.left), minDepth(root.right))  # 返回计算结果
```
 
### 112. 路径总和 (Path Sum)
 
**题目描述：** 
给你二叉树的根节点 `root` 和一个表示目标和的整数 `targetSum`。判断该树中是否存在 **根节点到叶子节点** 的路径，这条路径上所有节点值相加等于目标和 `targetSum`。如果存在，返回 `true`；否则，返回 `false`。 
**示例：** 
 
```python
输入：root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
输出：true
```
 
**解题思路：** 
DFS： 
- 递归遍历，维护当前路径和
- 当到达叶子节点时，检查路径和是否等于目标值
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(h)，其中 h 是树的高度
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：路径总和
# 思路：DFS：；递归遍历，维护当前路径和当到达叶子节点时，检查路径和是否等于目标值
def hasPathSum(root, targetSum):  # 定义函数 hasPathSum，接收参数: root、targetSum
    if not root:  # 判断: not root
        return False  # 返回 False

    if not root.left and not root.right:  # 判断: not root.left and not root.right
        return root.val == targetSum  # 返回计算结果

    return hasPathSum(root.left, targetSum - root.val) or \  # 返回计算结果
           hasPathSum(root.right, targetSum - root.val)
```
 
### 543. 二叉树的直径 (Diameter of Binary Tree)
 
**题目描述：** 
给你一棵二叉树的根节点，返回该树的 **直径**。 
二叉树的 **直径** 是指树中任意两个节点之间最长路径的 **长度**。这条路径可能经过也可能不经过根节点 `root`。 
两节点之间路径的 **长度** 由它们之间边数表示。 
**示例：** 
 
```python
输入：root = [1,2,3,4,5]
输出：3
解释：3，取路径 [4,2,1,3] 或 [5,2,1,3] 的长度。
```
 
**解题思路：** 
DFS： 
- 对于每个节点，计算经过该节点的最长路径
- 路径长度 = 左子树最大深度 + 右子树最大深度
- 在递归过程中更新全局最大直径
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(h)，其中 h 是树的高度
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：二叉树的直径
# 思路：DFS：；对于每个节点，计算经过该节点的最长路径路径长度 = 左子树最大深度 + 右子树最大深度在递归过程中更新全局最大直径
def diameterOfBinaryTree(root):  # 定义函数 diameterOfBinaryTree，接收参数: root
    max_diameter = 0  # max_diameter: 计数器/下标初始化为0

    def maxDepth(node):  # 定义函数 maxDepth，接收参数: node
        nonlocal max_diameter
        if not node:  # 判断: not node
            return 0  # 返回 0 (标志/空值)

        left_depth = maxDepth(node.left)  # left_depth: 赋值/计算
        right_depth = maxDepth(node.right)  # right_depth: 赋值/计算

        max_diameter = max(max_diameter, left_depth + right_depth)  # max_diameter: 赋值/计算

        return 1 + max(left_depth, right_depth)  # 返回计算结果

    maxDepth(root)
    return max_diameter  # 返回计算结果
```
 
### 637. 二叉树的层平均值 (Average of Levels in Binary Tree)
 
**题目描述：** 
给定一个非空二叉树的根节点 `root`，以数组的形式返回每一层节点的平均值。与实际答案相差 `10^-5` 以内的答案可以被接受。 
**示例：** 
 
```python
输入：root = [3,9,20,null,null,15,7]
输出：[3.00000,14.50000,11.00000]
解释：第 0 层的平均值为 3,第 1 层的平均值为 14.5,第 2 层的平均值为 11 。
因此返回 [3, 14.5, 11] 。
```
 
**解题思路：** 
层序遍历： 
- 使用队列进行层序遍历
- 计算每层节点的平均值
- 将平均值加入结果数组
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
from collections import deque  # 导入模块
# 目的：二叉树的层平均值
# 思路：层序遍历：；使用队列进行层序遍历计算每层节点的平均值将平均值加入结果数组
def averageOfLevels(root):  # 定义函数 averageOfLevels，接收参数: root
    if not root:  # 判断: not root
        return []  # 返回 [] (标志/空值)

    result = []  # result: 空列表
    queue = deque([root])  # queue: 赋值/计算

    while queue:  # 当 queue 时循环
        level_size = len(queue)  # level_size: 获取长度
        level_sum = 0  # level_sum: 计数器/下标初始化为0

        for _ in range(level_size):  # 遍历: _ 依次取 range(level_size) 中的每个值
            node = queue.popleft()  # node: 赋值/计算
            level_sum += node.val  # level_sum 自反赋值+=

            if node.left:  # 判断: node.left
                queue.append(node.left)  # 追加到列表末尾
            if node.right:  # 判断: node.right
                queue.append(node.right)  # 追加到列表末尾

        result.append(level_sum / level_size)  # 追加到列表末尾

    return result  # 返回计算结果
```
 
### 993. 二叉树的堂兄弟节点 (Cousins in Binary Tree)
 
**题目描述：** 
在二叉树中，根节点位于深度 `0`，每个深度为 `k` 的节点的子节点位于深度 `k+1`。 
如果二叉树的两个节点深度相同，但 **父节点不同**，则它们是一对**堂兄弟节点**。 
我们给出了具有唯一值的二叉树的根节点 `root`，以及树中两个不同节点的值 `x` 和 `y`。 
只有与值 `x` 和 `y` 对应的节点是堂兄弟节点时，才返回 `true`。否则，返回 `false`。 
**示例：** 
 
```python
输入：root = [1,2,3,4], x = 4, y = 3
输出：false
```
 
**解题思路：** 
BFS 或 DFS： 
- 找到 x 和 y 的深度和父节点
- 如果深度相同且父节点不同，返回 true
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：二叉树的堂兄弟节点
# 思路：BFS 或 DFS：；找到 x 和 y 的深度和父节点如果深度相同且父节点不同，返回 true
def isCousins(root, x, y):  # 定义函数 isCousins，接收参数: root、x、y
    x_info = y_info = None  # x_info: 赋值/计算

    def dfs(node, parent, depth):  # 定义函数 dfs，接收参数: node、parent、depth
        nonlocal x_info, y_info
        if not node:  # 判断: not node
            return

        if node.val == x:  # 判断: node.val == x
            x_info = (parent, depth)  # x_info: 赋值/计算
        if node.val == y:  # 判断: node.val == y
            y_info = (parent, depth)  # y_info: 赋值/计算

        dfs(node.left, node, depth + 1)
        dfs(node.right, node, depth + 1)

    dfs(root, None, 0)

    if x_info and y_info:  # 判断: x_info and y_info
        return x_info[1] == y_info[1] and x_info[0] != y_info[0]  # 返回列表结果
    return False  # 返回 False
```
 
### 563. 二叉树的坡度 (Binary Tree Tilt)
 
**题目描述：** 
给你一个二叉树的根节点 `root`，计算并返回 **整个树** 的坡度。 
一个树的 **节点的坡度** 定义即为，该节点左子树的节点之和和右子树节点之和的 **差的绝对值**。如果没有左子树的话，左子树的节点之和为 0；没有右子树的话也是一样。空节点的坡度是 0。 
**整个树** 的坡度就是其所有节点的坡度之和。 
**示例：** 
 
```python
输入：root = [1,2,3]
输出：1
解释：
节点 2 的坡度：|0-0| = 0（没有子节点）
节点 3 的坡度：|0-0| = 0（没有子节点）
节点 1 的坡度：|2-3| = 1（左子树就是左子节点，所以和是 2；右子树就是右子节点，所以和是 3）
坡度总和：0 + 0 + 1 = 1
```
 
**解题思路：** 
DFS： 
- 对于每个节点，计算左右子树的节点和
- 坡度 = |左子树和 - 右子树和|
- 累加所有节点的坡度
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(h)，其中 h 是树的高度
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：二叉树的坡度
# 思路：DFS：；对于每个节点，计算左右子树的节点和坡度 = |左子树和 - 右子树和|累加所有节点的坡度
def findTilt(root):  # 定义函数 findTilt，接收参数: root
    total_tilt = 0  # total_tilt: 计数器/下标初始化为0

    def sumTree(node):  # 定义函数 sumTree，接收参数: node
        nonlocal total_tilt
        if not node:  # 判断: not node
            return 0  # 返回 0 (标志/空值)

        left_sum = sumTree(node.left)  # left_sum: 赋值/计算
        right_sum = sumTree(node.right)  # right_sum: 赋值/计算

        tilt = abs(left_sum - right_sum)  # tilt: 赋值/计算
        total_tilt += tilt  # total_tilt 自反赋值+=

        return node.val + left_sum + right_sum  # 返回计算结果

    sumTree(root)
    return total_tilt  # 返回计算结果
```
 
### 662. 二叉树的最大宽度 (Maximum Width of Binary Tree)
 
**题目描述：** 
给你一棵二叉树的根节点 `root`，返回树的 **最大宽度**。 
树的 **最大宽度** 是所有层中最大的 **宽度**。 
每一层的 **宽度** 被定义为该层最左和最右的非空节点（两端点间的 `null` 节点也计入长度）之间的长度。这个二叉树与满二叉树（full binary tree）结构相同，但一些节点为空。 
每一层的宽度被定义为两个端点（该层最左和最右的非空节点）之间的长度。 
**示例：** 
 
```python
输入：root = [1,3,2,5,3,null,9]
输出：4
解释：最大宽度出现在树的第 3 层，宽度为 4 (5,3,null,9)。
```
 
**解题思路：** 
层序遍历 + 位置编号： 
- 给每个节点编号，根节点为 1，左子节点为 2i，右子节点为 2i+1
- 每层的宽度 = 最右节点编号 - 最左节点编号 + 1
- 使用层序遍历，记录每层最左和最右节点的编号
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
from collections import deque  # 导入模块
# 目的：二叉树的最大宽度
# 思路：层序遍历 + 位置编号：；给每个节点编号，根节点为 1，左子节点为 2i，右子节点为 2i+1每层的宽度 = 最右节点编号 - 最左节点编号 + 1使用层序遍历，记录每层最左和最右节点的编号
def widthOfBinaryTree(root):  # 定义函数 widthOfBinaryTree，接收参数: root
    if not root:  # 判断: not root
        return 0  # 返回 0 (标志/空值)

    max_width = 0  # max_width: 计数器/下标初始化为0
    queue = deque([(root, 0)])  # queue: 赋值/计算

    while queue:  # 当 queue 时循环
        level_size = len(queue)  # level_size: 获取长度
        leftmost = queue[0][1]  # leftmost: 赋值/计算

        for _ in range(level_size):  # 遍历: _ 依次取 range(level_size) 中的每个值
            node, pos = queue.popleft()

            if node.left:  # 判断: node.left
                queue.append((node.left, 2 * pos))  # 追加到列表末尾
            if node.right:  # 判断: node.right
                queue.append((node.right, 2 * pos + 1))  # 追加到列表末尾

        if level_size > 0:  # 判断: level_size > 0
            rightmost = queue[-1][1] if queue else leftmost  # rightmost: 赋值/计算
            max_width = max(max_width, rightmost - leftmost + 1)  # max_width: 赋值/计算

    return max_width  # 返回计算结果
```
 
### 987. 二叉树的垂直遍历 (Vertical Order Traversal of a Binary Tree)
 
**题目描述：** 
给你二叉树的根结点 `root`，请你设计算法计算二叉树的 **垂序遍历** 序列。 
对位于 `(row, col)` 的每个结点而言，其左右子结点分别位于 `(row + 1, col - 1)` 和 `(row + 1, col + 1)`。树的根结点位于 `(0, 0)`。 
二叉树的 **垂序遍历** 从最左边的列开始直到最右边的列结束，按列索引每一列上的所有结点，形成一个按出现位置从上到下排序的有序列表。如果同行同列上有多个结点，则按结点的值从小到大进行排序。 
返回二叉树的 **垂序遍历** 序列。 
**示例：** 
 
```python
输入：root = [3,9,20,null,null,15,7]
输出：[[9],[3,15],[20],[7]]
```
 
**解题思路：** 
DFS + 排序： 
- 使用 DFS 遍历树，记录每个节点的 (row, col, val)
- 按照 col 分组，相同 col 的节点按 row 排序，相同 row 和 col 的按 val 排序
- 返回结果
 
**复杂度分析：** 
- 时间复杂度：O(n log n)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
from collections import defaultdict  # 导入模块
# 目的：二叉树的垂直遍历
# 思路：DFS + 排序：；使用 DFS 遍历树，记录每个节点的 (row, col, val)按照 col 分组，相同 col 的节点按 row 排序，相同 row 和 col 的按 val 排序返回结果
def verticalTraversal(root):  # 定义函数 verticalTraversal，接收参数: root
    nodes = []  # nodes: 空列表

    def dfs(node, row, col):  # 定义函数 dfs，接收参数: node、row、col
        if not node:  # 判断: not node
            return

        nodes.append((col, row, node.val))  # 追加到列表末尾
        dfs(node.left, row + 1, col - 1)
        dfs(node.right, row + 1, col + 1)

    dfs(root, 0, 0)
    nodes.sort()  # 原地排序

    result = []  # result: 空列表
    current_col = None  # current_col: 空值None
    for col, row, val in nodes:  # 遍历: col, row, val 依次取 nodes 中的每个值
        if col != current_col:  # 判断: col != current_col
            result.append([])  # 追加到列表末尾
            current_col = col  # current_col: 赋值/计算
        result[-1].append(val)  # 追加到列表末尾

    return result  # 返回计算结果
```
 
## 二、回溯算法
 
### 17. 电话号码的字母组合 (Letter Combinations of a Phone Number)
 
**题目描述：** 
给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。答案可以按 **任意顺序** 返回。 
给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。 
**示例：** 
 
```python
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```
 
**解题思路：** 
回溯算法： 
- 建立数字到字母的映射表
- 使用回溯法，对于每个数字，尝试所有可能的字母
- 当处理完所有数字时，将当前组合加入结果
- 回溯时撤销选择
 
**复杂度分析：** 
- 时间复杂度：O(4^m × m)，其中 m 是输入数字的个数，4 是每个数字最多对应的字母数
- 空间复杂度：O(m)，递归栈深度为 m
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：电话号码的字母组合
# 思路：回溯算法：；建立数字到字母的映射表使用回溯法，对于每个数字，尝试所有可能的字母当处理完所有数字时，将当前组合加入结果回溯时撤销选择
def letterCombinations(digits):  # 定义函数 letterCombinations，接收参数: digits
    if not digits:  # 判断: not digits
        return []  # 返回 [] (标志/空值)

    mapping = {  # mapping: 赋值/计算
        "hl-string">'2': "hl-string">'abc', "hl-string">'3': "hl-string">'def', "hl-string">'4': "hl-string">'ghi', "hl-string">'5': "hl-string">'jkl',
        "hl-string">'6': "hl-string">'mno', "hl-string">'7': "hl-string">'pqrs', "hl-string">'8': "hl-string">'tuv', "hl-string">'9': "hl-string">'wxyz'
    }

    result = []  # result: 空列表

    def backtrack(index, path):  # 定义函数 backtrack，接收参数: index、path
        if index == len(digits):  # 判断: index == len(digits)
            result.append(path)  # 追加到列表末尾
            return

        for letter in mapping[digits[index]]:  # 遍历: letter 依次取 mapping[digits[index]] 中的每个值
            backtrack(index + 1, path + letter)

    backtrack(0, "hl-string">'')
    return result  # 返回计算结果
```
 
### 22. 括号生成 (Generate Parentheses)
 
**题目描述：** 
数字 `n` 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。 
**示例：** 
 
```python
输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]
```
 
**解题思路：** 
回溯算法： 
- 使用两个计数器 `left` 和 `right` 记录已使用的左右括号数量
- 只有当 `left < n` 时，可以添加左括号
- 只有当 `right < left` 时，可以添加右括号
- 当 `left == right == n` 时，找到一个有效组合
 
**复杂度分析：** 
- 时间复杂度：O(4^n / √n)，卡特兰数的复杂度
- 空间复杂度：O(n)，递归栈深度为 n
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：括号生成
# 思路：回溯算法：；使用两个计数器 left 和 right 记录已使用的左右括号数量只有当 left  时，可以添加左括号只有当 right  时，可以添加右括号当 left == right == n 时，找到一个有效组合
def generateParenthesis(n):  # 定义函数 generateParenthesis，接收参数: n
    result = []  # result: 空列表

    def backtrack(path, left, right):  # 定义函数 backtrack，接收参数: path、left、right
        if len(path) == 2 * n:  # 判断: len(path) == 2 * n
            result.append(path)  # 追加到列表末尾
            return

        if left < n:  # 判断: left < n
            backtrack(path + "hl-string">'(', left + 1, right)
        if right < left:  # 判断: right < left
            backtrack(path + "hl-string">')', left, right + 1)

    backtrack("hl-string">'', 0, 0)
    return result  # 返回计算结果
```
 
### 46. 全排列 (Permutations)
 
**题目描述：** 
给定一个不含重复数字的数组 `nums`，返回其 **所有可能的全排列**。你可以 **按任意顺序** 返回答案。 
**示例：** 
 
```python
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```
 
**解题思路：** 
回溯算法： 
- 使用回溯法，维护一个当前排列 `path`
- 对于每个位置，尝试所有未使用的数字
- 选择一个数字后，递归处理下一个位置
- 回溯时撤销选择，尝试其他可能性
 
**复杂度分析：** 
- 时间复杂度：O(n × n!)，共有 n! 个排列，每个排列需要 O(n) 时间复制
- 空间复杂度：O(n)，递归栈深度为 n
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：全排列
# 思路：回溯算法：；使用回溯法，维护一个当前排列 path对于每个位置，尝试所有未使用的数字选择一个数字后，递归处理下一个位置回溯时撤销选择，尝试其他可能性
def permute(nums):  # 定义函数 permute，接收参数: nums
    result = []  # result: 空列表

    def backtrack(path, used):  # 定义函数 backtrack，接收参数: path、used
        if len(path) == len(nums):  # 判断: len(path) == len(nums)
            result.append(path[:])  # 追加到列表末尾
            return

        for i in range(len(nums)):  # 遍历: i 依次取 range(len(nums)) 中的每个值
            if not used[i]:  # 判断: not used[i]
                used[i] = True  # used[i]: 布尔值True
                path.append(nums[i])  # 追加到列表末尾
                backtrack(path, used)
                path.pop()  # 弹出末尾元素
                used[i] = False  # used[i]: 布尔值False

    backtrack([], [False] * len(nums))
    return result  # 返回计算结果
```
 
### 78. 子集 (Subsets)
 
**题目描述：** 
给你一个整数数组 `nums`，数组中的元素 **互不相同**。返回该数组所有可能的子集（幂集）。 
解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。 
**示例：** 
 
```python
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```
 
**解题思路：** 
回溯算法： 
- 对于每个元素，有两种选择：包含或不包含
- 从第一个元素开始，逐个决定是否加入当前子集
- 当处理完所有元素时，将当前子集加入结果
 
**复杂度分析：** 
- 时间复杂度：O(n × 2^n)，共有 2^n 个子集，每个子集需要 O(n) 时间复制
- 空间复杂度：O(n)，递归栈深度为 n
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：子集
# 思路：回溯算法：；对于每个元素，有两种选择：包含或不包含从第一个元素开始，逐个决定是否加入当前子集当处理完所有元素时，将当前子集加入结果
def subsets(nums):  # 定义函数 subsets，接收参数: nums
    result = []  # result: 空列表

    def backtrack(start, path):  # 定义函数 backtrack，接收参数: start、path
        result.append(path[:])  # 追加到列表末尾

        for i in range(start, len(nums)):  # 遍历: i 依次取 range(start, len(nums)) 中的每个值
            path.append(nums[i])  # 追加到列表末尾
            backtrack(i + 1, path)
            path.pop()  # 弹出末尾元素

    backtrack(0, [])
    return result  # 返回计算结果
```
 
### 90. 子集 II (Subsets II)
 
**题目描述：** 
给你一个整数数组 `nums`，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。 
解集 **不能** 包含重复的子集。返回的解集中，子集可以按 **任意顺序** 排列。 
**示例：** 
 
```python
输入：nums = [1,2,2]
输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]
```
 
**解题思路：** 
回溯算法 + 去重： 
- 先对数组排序，便于去重
- 使用回溯法生成所有子集
- 对于重复元素，跳过同一层级的重复选择
- 只选择第一个重复元素，跳过后续重复元素
 
**复杂度分析：** 
- 时间复杂度：O(n × 2^n)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：子集 II
# 思路：回溯算法 + 去重：；先对数组排序，便于去重使用回溯法生成所有子集对于重复元素，跳过同一层级的重复选择只选择第一个重复元素，跳过后续重复元素
def subsetsWithDup(nums):  # 定义函数 subsetsWithDup，接收参数: nums
    result = []  # result: 空列表
    nums.sort()  # 原地排序

    def backtrack(start, path):  # 定义函数 backtrack，接收参数: start、path
        result.append(path[:])  # 追加到列表末尾

        for i in range(start, len(nums)):  # 遍历: i 依次取 range(start, len(nums)) 中的每个值
            if i > start and nums[i] == nums[i-1]:  # 判断: i > start and nums[i] == nums[i-1]
                continue  # 跳过本轮迭代
            path.append(nums[i])  # 追加到列表末尾
            backtrack(i + 1, path)
            path.pop()  # 弹出末尾元素

    backtrack(0, [])
    return result  # 返回计算结果
```
 
### 39. 组合总和 (Combination Sum)
 
**题目描述：** 
给你一个 **无重复元素** 的整数数组 `candidates` 和一个目标整数 `target`，找出 `candidates` 中可以使数字和为目标数 `target` 的 **所有** 不同组合，并以列表形式返回。你可以按 **任意顺序** 返回这些组合。 
`candidates` 中的 **同一个** 数字可以 **无限制重复被选取**。如果至少一个数字的被选数量不同，则两种组合是不同的。 
**示例：** 
 
```python
输入：candidates = [2,3,6,7], target = 7
输出：[[2,2,3],[7]]
解释：
2 和 3 可以形成一组候选，2 + 2 + 3 = 7。注意 2 可以使用多次。
7 也是一个候选，7 = 7。
仅有这两种组合。
```
 
**解题思路：** 
回溯算法： 
- 对数组排序，便于剪枝
- 从第一个元素开始，尝试所有可能的组合
- 如果当前和等于 target，加入结果
- 如果当前和小于 target，继续递归
- 剪枝：如果当前和大于 target，直接返回
 
**复杂度分析：** 
- 时间复杂度：O(S)，其中 S 是所有可行解的长度之和
- 空间复杂度：O(target)，递归栈深度最多为 target
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：组合总和
# 思路：回溯算法：；对数组排序，便于剪枝从第一个元素开始，尝试所有可能的组合如果当前和等于 target，加入结果如果当前和小于 target，继续递归剪枝：如果当前和大于 target，直接返回
def combinationSum(candidates, target):  # 定义函数 combinationSum，接收参数: candidates、target
    result = []  # result: 空列表
    candidates.sort()  # 原地排序

    def backtrack(start, path, current_sum):  # 定义函数 backtrack，接收参数: start、path、current_sum
        if current_sum == target:  # 判断: current_sum == target
            result.append(path[:])  # 追加到列表末尾
            return

        for i in range(start, len(candidates)):  # 遍历: i 依次取 range(start, len(candidates)) 中的每个值
            if current_sum + candidates[i] > target:  # 判断: current_sum + candidates[i] > targe
                break  # 跳出当前循环
            path.append(candidates[i])  # 追加到列表末尾
            backtrack(i, path, current_sum + candidates[i])
            path.pop()  # 弹出末尾元素

    backtrack(0, [], 0)
    return result  # 返回计算结果
```
 
### 79. 单词搜索 (Word Search)
 
**题目描述：** 
给定一个 `m x n` 二维字符网格 `board` 和一个字符串单词 `word`。如果 `word` 存在于网格中，返回 `true`；否则，返回 `false`。 
单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中”相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。 
**示例：** 
 
```python
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```
 
**解题思路：** 
回溯 + DFS： 
- 遍历网格，找到与单词首字母匹配的位置
- 从该位置开始，使用 DFS 搜索
- 对于每个位置，检查上下左右四个方向
- 使用 visited 数组标记已访问的位置
- 如果找到完整路径，返回 true
 
**复杂度分析：** 
- 时间复杂度：O(m × n × 4^L)，其中 L 是单词长度
- 空间复杂度：O(L)，递归栈深度为 L
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：单词搜索
# 思路：回溯 + DFS：；遍历网格，找到与单词首字母匹配的位置从该位置开始，使用 DFS 搜索对于每个位置，检查上下左右四个方向使用 visited 数组标记已访问的位置如果找到完整路径，返回 true
def exist(board, word):  # 定义函数 exist，接收参数: board、word
    m, n = len(board), len(board[0])  # 交换/多重赋值
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]  # directions: 赋值/计算

    def dfs(i, j, index):  # 定义函数 dfs，接收参数: i、j、index
        if index == len(word):  # 判断: index == len(word)
            return True  # 返回 True

        if i < 0 or i >= m or j < 0 or j >= n or board[i][j] != word[index]:  # 判断: i < 0 or i >= m or j < 0 or j >= n 
            return False  # 返回 False

        temp = board[i][j]  # temp: 赋值/计算
        board[i][j] = "hl-string">'#'

        for dx, dy in directions:  # 遍历: dx, dy 依次取 directions 中的每个值
            if dfs(i + dx, j + dy, index + 1):  # 判断: dfs(i + dx, j + dy, index + 1)
                return True  # 返回 True

        board[i][j] = temp
        return False  # 返回 False

    for i in range(m):  # 遍历: i 依次取 range(m) 中的每个值
        for j in range(n):  # 遍历: j 依次取 range(n) 中的每个值
            if dfs(i, j, 0):  # 判断: dfs(i, j, 0)
                return True  # 返回 True

    return False  # 返回 False
```
 
### 131. 分割回文串 (Palindrome Partitioning)
 
**题目描述：** 
给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是 **回文串**。返回 `s` 所有可能的分割方案。 
**示例：** 
 
```python
输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]
```
 
**解题思路：** 
回溯算法： 
- 使用回溯法，尝试所有可能的分割方式
- 对于每个位置，检查从当前位置到字符串末尾的所有子串
- 如果子串是回文，加入当前路径，继续递归
- 回溯时撤销选择
 
**复杂度分析：** 
- 时间复杂度：O(N × 2^N)，最坏情况下所有子串都是回文
- 空间复杂度：O(N)，递归栈深度为 N
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：分割回文串
# 思路：回溯算法：；使用回溯法，尝试所有可能的分割方式对于每个位置，检查从当前位置到字符串末尾的所有子串如果子串是回文，加入当前路径，继续递归回溯时撤销选择
def partition(s):  # 定义函数 partition，接收参数: s
    result = []  # result: 空列表

    def is_palindrome(left, right):  # 定义函数 is_palindrome，接收参数: left、right
        while left < right:  # 当 left < right 时循环
            if s[left] != s[right]:  # 判断: s[left] != s[right]
                return False  # 返回 False
            left += 1  # left 自反赋值+=
            right -= 1  # right 自反赋值-=
        return True  # 返回 True

    def backtrack(start, path):  # 定义函数 backtrack，接收参数: start、path
        if start == len(s):  # 判断: start == len(s)
            result.append(path[:])  # 追加到列表末尾
            return

        for i in range(start, len(s)):  # 遍历: i 依次取 range(start, len(s)) 中的每个值
            if is_palindrome(start, i):  # 判断: is_palindrome(start, i)
                path.append(s[start:i+1])  # 追加到列表末尾
                backtrack(i + 1, path)
                path.pop()  # 弹出末尾元素

    backtrack(0, [])
    return result  # 返回计算结果
```
 
## 三、贪心算法
 
### 55. 跳跃游戏 (Jump Game)
 
**题目描述：** 
给定一个非负整数数组 `nums`，你最初位于数组的 **第一个下标**。数组中的每个元素代表你在该位置可以跳跃的最大长度。 
判断你是否能够到达最后一个下标。 
**示例：** 
 
```python
输入：nums = [2,3,1,1,4]
输出：true
解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。
```
 
**解题思路：** 
贪心算法： 
- 维护一个变量 `max_reach`，表示当前能到达的最远位置
- 遍历数组，更新 `max_reach = max(max_reach, i + nums[i])`
- 如果 `max_reach >= len(nums) - 1`，说明可以到达最后一个位置
- 如果在某个位置 `i > max_reach`，说明无法继续前进
 
**复杂度分析：** 
- 时间复杂度：O(n)，遍历数组一次
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：跳跃游戏
# 思路：贪心算法：；维护一个变量 max_reach，表示当前能到达的最远位置遍历数组，更新 max_reach = max(max_reach, i + nums[i])如果 max_reach >= len(nums) - 1，说明可以到达最后一个位置如果在某个位置 i > max_reach，说明无
def canJump(nums):  # 定义函数 canJump，接收参数: nums
    max_reach = 0  # max_reach: 计数器/下标初始化为0
    for i in range(len(nums)):  # 遍历: i 依次取 range(len(nums)) 中的每个值
        if i > max_reach:  # 判断: i > max_reach
            return False  # 返回 False
        max_reach = max(max_reach, i + nums[i])  # max_reach: 赋值/计算
        if max_reach >= len(nums) - 1:  # 判断: max_reach >= len(nums) - 1
            return True  # 返回 True
    return True  # 返回 True
```
 
### 45. 跳跃游戏 II (Jump Game II)
 
**题目描述：** 
给你一个非负整数数组 `nums`，你最初位于数组的第一个位置。数组中的每个元素代表你在该位置可以跳跃的最大长度。 
你的目标是使用最少的跳跃次数到达数组的最后一个位置。假设你总是可以到达数组的最后一个位置。 
**示例：** 
 
```python
输入：nums = [2,3,1,1,4]
输出：2
解释：跳到最后一个位置的最小跳跃数是 2。从下标为 0 跳到下标为 1 跳 1 步，然后跳 3 步到达数组的最后一个位置。
```
 
**解题思路：** 
贪心算法： 
- 维护 `end` 表示当前跳跃能到达的最远位置
- 维护 `max_pos` 表示在 `[start, end]` 范围内能到达的最远位置
- 当到达 `end` 时，跳跃次数加1，更新 `end = max_pos`
- 继续遍历直到到达最后一个位置
 
**复杂度分析：** 
- 时间复杂度：O(n)，遍历数组一次
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：跳跃游戏 II
# 思路：贪心算法：；维护 end 表示当前跳跃能到达的最远位置维护 max_pos 表示在 [start, end] 范围内能到达的最远位置当到达 end 时，跳跃次数加1，更新 end = max_pos继续遍历直到到达最后一个位置
def jump(nums):  # 定义函数 jump，接收参数: nums
    jumps = 0  # jumps: 计数器/下标初始化为0
    end = 0  # end: 计数器/下标初始化为0
    max_pos = 0  # max_pos: 计数器/下标初始化为0

    for i in range(len(nums) - 1):  # 遍历: i 依次取 range(len(nums) - 1) 中的每个值
        max_pos = max(max_pos, i + nums[i])  # max_pos: 赋值/计算
        if i == end:  # 判断: i == end
            jumps += 1  # jumps 自反赋值+=
            end = max_pos  # end: 赋值/计算
    return jumps  # 返回计算结果
```
 
### 56. 合并区间 (Merge Intervals)
 
**题目描述：** 
以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]`。请你合并所有重叠的区间，并返回一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间。 
**示例：** 
 
```python
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6]。
```
 
**解题思路：** 
排序 + 贪心： 
- 按区间的起始位置排序
- 遍历区间，如果当前区间与结果中最后一个区间重叠，则合并
- 否则，将当前区间加入结果
 
**复杂度分析：** 
- 时间复杂度：O(n log n)，排序的时间复杂度
- 空间复杂度：O(1)，不考虑结果存储的空间
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：合并区间
# 思路：排序 + 贪心：；按区间的起始位置排序遍历区间，如果当前区间与结果中最后一个区间重叠，则合并否则，将当前区间加入结果
def merge(intervals):  # 定义函数 merge，接收参数: intervals
    intervals.sort(key=lambda x: x[0])  # 原地排序
    result = [intervals[0]]  # result: 赋值/计算

    for interval in intervals[1:]:  # 遍历: interval 依次取 intervals[1 中的每个值
        if interval[0] <= result[-1][1]:  # 判断: interval[0] <= result[-1][1]
            result[-1][1] = max(result[-1][1], interval[1])
        else:  # 否则 (以上条件都不满足时执行)
            result.append(interval)  # 追加到列表末尾

    return result  # 返回计算结果
```
 
### 57. 插入区间 (Insert Interval)
 
**题目描述：** 
给你一个 **无重叠的**，按照区间起始端点排序的区间列表 `intervals`，其中 `intervals[i] = [starti, endi]` 表示第 `i` 个区间的开始和结束，并且 `intervals` 按照 `starti` 升序排列。同样给定一个区间 `newInterval = [start, end]` 表示另一个区间的开始和结束。 
在 `intervals` 中插入区间 `newInterval`，使得 `intervals` 仍然按照区间起始端点排序，且区间之间不重叠（如果有必要的话，可以合并区间）。 
返回插入后的 `intervals`。 
**示例：** 
 
```python
输入：intervals = [[1,3],[6,9]], newInterval = [2,5]
输出：[[1,5],[6,9]]
```
 
**解题思路：** 
- 找到所有与新区间重叠的区间
- 合并这些区间
- 将合并后的区间插入到正确位置
 
**复杂度分析：** 
- 时间复杂度：O(n)，遍历数组一次
- 空间复杂度：O(1)，不考虑结果存储的空间
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：插入区间
# 思路：找到所有与新区间重叠的区间合并这些区间将合并后的区间插入到正确位置
def insert(intervals, newInterval):  # 定义函数 insert，接收参数: intervals、newInterval
    result = []  # result: 空列表
    i = 0  # i: 计数器/下标初始化为0

    while i < len(intervals) and intervals[i][1] < newInterval[0]:  # 当 i < len(intervals) and interva 时循环
        result.append(intervals[i])  # 追加到列表末尾
        i += 1  # i 自反赋值+=

    while i < len(intervals) and intervals[i][0] <= newInterval[1]:  # 当 i < len(intervals) and interva 时循环
        newInterval[0] = min(newInterval[0], intervals[i][0])  # newInterval[0]: 赋值/计算
        newInterval[1] = max(newInterval[1], intervals[i][1])  # newInterval[1]: 赋值/计算
        i += 1  # i 自反赋值+=

    result.append(newInterval)  # 追加到列表末尾

    while i < len(intervals):  # 当 i < len(intervals) 时循环
        result.append(intervals[i])  # 追加到列表末尾
        i += 1  # i 自反赋值+=

    return result  # 返回计算结果
```
 
### 54. 螺旋矩阵 (Spiral Matrix)
 
**题目描述：** 
给你一个 `m` 行 `n` 列的矩阵 `matrix`，请按照 **顺时针螺旋顺序**，返回矩阵中的所有元素。 
**示例：** 
 
```python
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```
 
**解题思路：** 
模拟法： 
- 定义四个边界：top, bottom, left, right
- 按照右、下、左、上的顺序遍历
- 每完成一个方向，更新对应的边界
- 当边界相遇时停止
 
**复杂度分析：** 
- 时间复杂度：O(m × n)，需要遍历所有元素
- 空间复杂度：O(1)，不考虑结果存储的空间
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：螺旋矩阵
# 思路：模拟法：；定义四个边界：top, bottom, left, right按照右、下、左、上的顺序遍历每完成一个方向，更新对应的边界当边界相遇时停止
def spiralOrder(matrix):  # 定义函数 spiralOrder，接收参数: matrix
    if not matrix:  # 判断: not matrix
        return []  # 返回 [] (标志/空值)

    result = []  # result: 空列表
    top, bottom = 0, len(matrix) - 1  # 交换/多重赋值
    left, right = 0, len(matrix[0]) - 1  # 交换/多重赋值

    while top <= bottom and left <= right:  # 当 top <= bottom and left <= righ 时循环
        for j in range(left, right + 1):  # 遍历: j 依次取 range(left, right + 1) 中的每个值
            result.append(matrix[top][j])  # 追加到列表末尾
        top += 1  # top 自反赋值+=

        for i in range(top, bottom + 1):  # 遍历: i 依次取 range(top, bottom + 1) 中的每个值
            result.append(matrix[i][right])  # 追加到列表末尾
        right -= 1  # right 自反赋值-=

        if top <= bottom:  # 判断: top <= bottom
            for j in range(right, left - 1, -1):  # 遍历: j 依次取 range(right, left - 1, -1) 中的每个值
                result.append(matrix[bottom][j])  # 追加到列表末尾
            bottom -= 1  # bottom 自反赋值-=

        if left <= right:  # 判断: left <= right
            for i in range(bottom, top - 1, -1):  # 遍历: i 依次取 range(bottom, top - 1, -1) 中的每个值
                result.append(matrix[i][left])  # 追加到列表末尾
            left += 1  # left 自反赋值+=

    return result  # 返回计算结果
```
 
### 48. 旋转图像 (Rotate Image)
 
**题目描述：** 
给定一个 `n × n` 的二维矩阵 `matrix` 表示一个图像。请你将图像顺时针旋转 90 度。 
你必须在 **原地** 旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要** 使用另一个矩阵来旋转图像。 
**示例：** 
 
```python
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[[7,4,1],[8,5,2],[9,6,3]]
```
 
**解题思路：** 
方法1：转置 + 翻转 
- 先转置矩阵（行列互换）
- 再翻转每一行
 
方法2：四角交换 
- 对于每个位置 (i, j)，找到旋转后的位置
- 一次交换四个位置的值
 
**复杂度分析：** 
- 时间复杂度：O(n²)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：旋转图像
# 思路：方法1：转置 + 翻转；先转置矩阵（行列互换）再翻转每一行；方法2：四角交换
def rotate(matrix):  # 定义函数 rotate，接收参数: matrix
    n = len(matrix)  # n: 获取长度

    # 转置
    for i in range(n):  # 遍历: i 依次取 range(n) 中的每个值
        for j in range(i, n):  # 遍历: j 依次取 range(i, n) 中的每个值
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]  # 交换/多重赋值

    # 翻转每一行
    for i in range(n):  # 遍历: i 依次取 range(n) 中的每个值
        matrix[i].reverse()  # 原地反转列表
```
 
### 240. 搜索二维矩阵 II (Search a 2D Matrix II)
 
**题目描述：** 
编写一个高效的算法来搜索 `m x n` 矩阵 `matrix` 中的一个目标值 `target`。该矩阵具有以下特性： 
- 每行的元素从左到右升序排列。
- 每列的元素从上到下升序排列。
 
**示例：** 
 
```python
输入：matrix = [[1,4,7,11],[2,5,8,12],[3,6,9,16],[10,13,14,17]], target = 5
输出：true
```
 
**解题思路：** 
从右上角开始搜索： 
- 如果当前元素等于 target，返回 true
- 如果当前元素大于 target，向左移动（排除当前列）
- 如果当前元素小于 target，向下移动（排除当前行）
 
**复杂度分析：** 
- 时间复杂度：O(m + n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：搜索二维矩阵 II
# 思路：从右上角开始搜索：；如果当前元素等于 target，返回 true如果当前元素大于 target，向左移动（排除当前列）如果当前元素小于 target，向下移动（排除当前行）
def searchMatrix(matrix, target):  # 定义函数 searchMatrix，接收参数: matrix、target
    if not matrix:  # 判断: not matrix
        return False  # 返回 False

    m, n = len(matrix), len(matrix[0])  # 交换/多重赋值
    i, j = 0, n - 1  # 交换/多重赋值

    while i < m and j >= 0:  # 当 i < m and j >= 0 时循环
        if matrix[i][j] == target:  # 判断: matrix[i][j] == target
            return True  # 返回 True
        elif matrix[i][j] > target:  # 否则如果
            j -= 1  # j 自反赋值-=
        else:  # 否则 (以上条件都不满足时执行)
            i += 1  # i 自反赋值+=

    return False  # 返回 False
```
 
### 128. 最长连续序列 (Longest Consecutive Sequence)
 
**题目描述：** 
给定一个未排序的整数数组 `nums`，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。 
请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。 
**示例：** 
 
```python
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```
 
**解题思路：** 
哈希集合： 
- 将所有数字存入哈希集合
- 对于每个数字，如果它是连续序列的起点（即 num-1 不在集合中），则从该数字开始计算连续序列的长度
- 更新最长连续序列的长度
 
**复杂度分析：** 
- 时间复杂度：O(n)，每个数字最多被访问两次
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：最长连续序列
# 思路：哈希集合：；将所有数字存入哈希集合对于每个数字，如果它是连续序列的起点（即 num-1 不在集合中），则从该数字开始计算连续序列的长度更新最长连续序列的长度
def longestConsecutive(nums):  # 定义函数 longestConsecutive，接收参数: nums
    if not nums:  # 判断: not nums
        return 0  # 返回 0 (标志/空值)

    num_set = set(nums)  # num_set: 赋值/计算
    longest = 0  # longest: 计数器/下标初始化为0

    for num in num_set:  # 遍历: num 依次取 num_set 中的每个值
        if num - 1 not in num_set:  # 判断: num - 1 not in num_set
            current_num = num  # current_num: 赋值/计算
            current_length = 1  # current_length: 初始化为1

            while current_num + 1 in num_set:  # 当 current_num + 1 in num_set 时循环
                current_num += 1  # current_num 自反赋值+=
                current_length += 1  # current_length 自反赋值+=

            longest = max(longest, current_length)  # longest: 赋值/计算

    return longest  # 返回计算结果
```
 
## 四、图论
 
### 200. 岛屿数量 (Number of Islands)
 
**题目描述：** 
给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。 
岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。 
此外，你可以假设该网格的四条边均被水包围。 
**示例：** 
 
```python
输入：grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
输出：1
```
 
**解题思路：** 
DFS 或 BFS： 
- 遍历网格，遇到 ‘1’ 时，岛屿数量加1
- 使用 DFS 或 BFS 将相邻的所有 ‘1’ 标记为 ‘0’（沉岛）
- 继续遍历直到所有位置都被访问
 
**复杂度分析：** 
- 时间复杂度：O(m × n)
- 空间复杂度：O(m × n)，递归栈深度
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：岛屿数量
# 思路：DFS 或 BFS：；遍历网格，遇到 ‘1’ 时，岛屿数量加1使用 DFS 或 BFS 将相邻的所有 ‘1’ 标记为 ‘0’（沉岛）继续遍历直到所有位置都被访问
def numIslands(grid):  # 定义函数 numIslands，接收参数: grid
    if not grid:  # 判断: not grid
        return 0  # 返回 0 (标志/空值)

    m, n = len(grid), len(grid[0])  # 交换/多重赋值
    count = 0  # count: 计数器/下标初始化为0

    def dfs(i, j):  # 定义函数 dfs，接收参数: i、j
        if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] != "hl-string">'1':  # 判断: i < 0 or i >= m or j < 0 or j >= n 
            return
        grid[i][j] = "hl-string">'0'
        dfs(i + 1, j)
        dfs(i - 1, j)
        dfs(i, j + 1)
        dfs(i, j - 1)

    for i in range(m):  # 遍历: i 依次取 range(m) 中的每个值
        for j in range(n):  # 遍历: j 依次取 range(n) 中的每个值
            if grid[i][j] == "hl-string">'1':  # 判断: grid[i][j] == '1'
                count += 1  # count 自反赋值+=
                dfs(i, j)

    return count  # 返回计算结果
```
 
### 207. 课程表 (Course Schedule)
 
**题目描述：** 
你这个学期必须选修 `numCourses` 门课程，记为 `0` 到 `numCourses - 1`。 
在选修某些课程之前需要一些先修课程。先修课程按数组 `prerequisites` 给出，其中 `prerequisites[i] = [ai, bi]`，表示如果要学习课程 `ai` 则 **必须** 先学习课程 `bi`。 
例如，先修课程对 `[0, 1]` 表示：想要学习课程 `0`，你需要先完成课程 `1`。 
请你判断是否可能完成所有课程的学习？如果可以，返回 `true`；否则，返回 `false`。 
**示例：** 
 
```python
输入：numCourses = 2, prerequisites = [[1,0]]
输出：true
解释：总共有 2 门课程。学习课程 1 之前，你需要完成课程 0。这是可能的。
```
 
**解题思路：** 
拓扑排序（检测环）： 
- 构建有向图，计算每个节点的入度
- 使用队列存储所有入度为 0 的节点
- 每次从队列中取出一个节点，将其相邻节点的入度减1
- 如果相邻节点的入度变为0，将其加入队列
- 如果所有节点都被访问，说明没有环，返回 true
 
**复杂度分析：** 
- 时间复杂度：O(V + E)，V 是节点数，E 是边数
- 空间复杂度：O(V + E)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：课程表
# 思路：拓扑排序（检测环）：；构建有向图，计算每个节点的入度使用队列存储所有入度为 0 的节点每次从队列中取出一个节点，将其相邻节点的入度减1如果相邻节点的入度变为0，将其加入队列如果所有节点都被访问，说明没有环，返回 true
def canFinish(numCourses, prerequisites):  # 定义函数 canFinish，接收参数: numCourses、prerequisites
    graph = [[] for _ in range(numCourses)]  # graph: 列表推导式
    indegree = [0] * numCourses  # indegree: 乘积计算

    for course, prereq in prerequisites:  # 遍历: course, prereq 依次取 prerequisites 中的每个值
        graph[prereq].append(course)  # 追加到列表末尾
        indegree[course] += 1  # indegree[course] 自反赋值+=

    queue = [i for i in range(numCourses) if indegree[i] == 0]
    count = 0  # count: 计数器/下标初始化为0

    while queue:  # 当 queue 时循环
        node = queue.pop(0)  # node: 赋值/计算
        count += 1  # count 自反赋值+=
        for neighbor in graph[node]:  # 遍历: neighbor 依次取 graph[node] 中的每个值
            indegree[neighbor] -= 1  # indegree[neighbor] 自反赋值-=
            if indegree[neighbor] == 0:  # 判断: indegree[neighbor] == 0
                queue.append(neighbor)  # 追加到列表末尾

    return count == numCourses  # 返回计算结果
```
 
### 133. 克隆图 (Clone Graph)
 
**题目描述：** 
给你无向 **连通** 图中一个节点的引用，请你返回该图的 **深拷贝**（克隆）。 
图中的每个节点都包含它的值 `val`（`int`）和其邻居的列表（`List[Node]`）。 
**示例：** 
 
```python
输入：adjList = [[2,4],[1,3],[2,4],[1,3]]
输出：[[2,4],[1,3],[2,4],[1,3]]
```
 
**解题思路：** 
DFS + 哈希表： 
- 使用哈希表存储已克隆的节点
- 对于每个节点，如果已克隆，直接返回
- 否则，创建新节点，递归克隆所有邻居
 
**复杂度分析：** 
- 时间复杂度：O(V + E)
- 空间复杂度：O(V)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：克隆图
# 思路：DFS + 哈希表：；使用哈希表存储已克隆的节点对于每个节点，如果已克隆，直接返回否则，创建新节点，递归克隆所有邻居
def cloneGraph(node):  # 定义函数 cloneGraph，接收参数: node
    if not node:  # 判断: not node
        return None  # 返回 None

    visited = {}  # visited: 空字典

    def dfs(original):  # 定义函数 dfs，接收参数: original
        if original in visited:  # 判断: original in visited
            return visited[original]  # 返回列表结果

        clone = Node(original.val)  # clone: 赋值/计算
        visited[original] = clone  # visited[original]: 赋值/计算

        for neighbor in original.neighbors:  # 遍历: neighbor 依次取 original.neighbors 中的每个值
            clone.neighbors.append(dfs(neighbor))  # 追加到列表末尾

        return clone  # 返回计算结果

    return dfs(node)  # 返回计算结果
```
 
### 127. 单词接龙 (Word Ladder)
 
**题目描述：** 
字典 `wordList` 中从单词 `beginWord` 和 `endWord` 的 **转换序列** 是一个按下述规格形成的序列： 
- 序列中第一个单词是 `beginWord`。
- 序列中最后一个单词是 `endWord`。
- 每次转换只能改变一个字母。
- 转换过程中的中间单词必须是字典 `wordList` 中的单词。
 
给你两个单词 `beginWord` 和 `endWord` 和一个字典 `wordList`，找到从 `beginWord` 到 `endWord` 的 **最短转换序列** 中的 **单词数目**。如果不存在这样的转换序列，返回 `0`。 
**示例：** 
 
```python
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
输出：5
解释：一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog", 返回它的长度 5。
```
 
**解题思路：** 
BFS： 
- 将 beginWord 加入队列
- 对于队列中的每个单词，尝试改变每个位置的字符
- 如果改变后的单词在 wordList 中且未被访问，加入队列
- 使用 BFS 找到最短路径
 
**复杂度分析：** 
- 时间复杂度：O(M × N)，M 是单词长度，N 是字典大小
- 空间复杂度：O(N)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：单词接龙
# 思路：BFS：；将 beginWord 加入队列对于队列中的每个单词，尝试改变每个位置的字符如果改变后的单词在 wordList 中且未被访问，加入队列使用 BFS 找到最短路径
def ladderLength(beginWord, endWord, wordList):  # 定义函数 ladderLength，接收参数: beginWord、endWord、wordList
    wordSet = set(wordList)  # wordSet: 赋值/计算
    if endWord not in wordSet:  # 判断: endWord not in wordSet
        return 0  # 返回 0 (标志/空值)

    queue = [(beginWord, 1)]  # queue: 赋值/计算
    visited = {beginWord}  # visited: 赋值/计算

    while queue:  # 当 queue 时循环
        word, length = queue.pop(0)  # 弹出末尾元素

        if word == endWord:  # 判断: word == endWord
            return length  # 返回计算结果

        for i in range(len(word)):  # 遍历: i 依次取 range(len(word)) 中的每个值
            for c in "hl-string">'abcdefghijklmnopqrstuvwxyz':  # 遍历: c 依次取 'abcdefghijklmnopqrstuvwxyz' 中的每个值
                new_word = word[:i] + c + word[i+1:]  # new_word: 赋值/计算
                if new_word in wordSet and new_word not in visited:  # 判断: new_word in wordSet and new_word no
                    visited.add(new_word)  # 向集合添加元素
                    queue.append((new_word, length + 1))  # 追加到列表末尾

    return 0  # 返回 0 (标志/空值)
```
 
## 五、堆与优先队列
 
### 215. 数组中的第K个最大元素 (Kth Largest Element in an Array)
 
**题目描述：** 
给定整数数组 `nums` 和整数 `k`，请返回数组中第 `k` 个最大的元素。 
请注意，你需要找的是数组排序后的第 `k` 个最大的元素，而不是第 `k` 个不同的元素。 
**示例：** 
 
```python
输入：[3,2,1,5,6,4], k = 2
输出：5
```
 
**解题思路：** 
方法1：快速选择（类似快排）
方法2：最小堆（维护大小为 k 的最小堆） 
**复杂度分析：** 
- 时间复杂度：O(n log k)（堆方法）或 O(n)（快速选择，平均情况）
- 空间复杂度：O(k)（堆方法）或 O(1)（快速选择）
 
**Python 解答（含详细注释）：** 
 
```python
import heapq  # 导入模块
# 目的：数组中的第K个最大元素
# 思路：方法1：快速选择（类似快排）方法2：最小堆（维护大小为 k 的最小堆）
def findKthLargest(nums, k):  # 定义函数 findKthLargest，接收参数: nums、k
    heap = []  # heap: 空列表
    for num in nums:  # 遍历: num 依次取 nums 中的每个值
        heapq.heappush(heap, num)
        if len(heap) > k:  # 判断: len(heap) > k
            heapq.heappop(heap)
    return heap[0]  # 返回列表结果
```
 
### 347. 前 K 个高频元素 (Top K Frequent Elements)
 
**题目描述：** 
给你一个整数数组 `nums` 和一个整数 `k`，请你返回其中出现频率前 `k` 高的元素。你可以按 **任意顺序** 返回答案。 
**示例：** 
 
```python
输入：nums = [1,1,1,2,2,3], k = 2
输出：[1,2]
```
 
**解题思路：** 
- 统计每个元素的频率
- 使用最小堆维护频率最高的 k 个元素
- 返回堆中的元素
 
**复杂度分析：** 
- 时间复杂度：O(n log k)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
import heapq  # 导入模块
from collections import Counter  # 导入模块
# 目的：前 K 个高频元素
# 思路：统计每个元素的频率使用最小堆维护频率最高的 k 个元素返回堆中的元素
def topKFrequent(nums, k):  # 定义函数 topKFrequent，接收参数: nums、k
    count = Counter(nums)  # count: 赋值/计算
    heap = []  # heap: 空列表

    for num, freq in count.items():  # 遍历: num, freq 依次取 count.items() 中的每个值
        heapq.heappush(heap, (freq, num))
        if len(heap) > k:  # 判断: len(heap) > k
            heapq.heappop(heap)

    return [num for freq, num in heap]  # 返回列表结果
```
 
### 23. 合并K个升序链表 (Merge k Sorted Lists)
 
**题目描述：** 
给你一个链表数组，每个链表都已经按升序排列。请你将所有链表合并到一个升序链表中，返回合并后的链表。 
**示例：** 
 
```python
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
```
 
**解题思路：** 
优先队列（最小堆）： 
- 将所有链表的头节点加入最小堆
- 每次取出堆顶节点，加入结果链表
- 如果该节点还有下一个节点，将下一个节点加入堆
- 重复直到堆为空
 
**复杂度分析：** 
- 时间复杂度：O(n log k)，n 是总节点数，k 是链表数
- 空间复杂度：O(k)
 
**Python 解答（含详细注释）：** 
 
```python
import heapq  # 导入模块
# 目的：合并K个升序链表
# 思路：优先队列（最小堆）：；将所有链表的头节点加入最小堆每次取出堆顶节点，加入结果链表如果该节点还有下一个节点，将下一个节点加入堆重复直到堆为空
def mergeKLists(lists):  # 定义函数 mergeKLists，接收参数: lists
    heap = []  # heap: 空列表
    for i, node in enumerate(lists):  # 遍历: i, node 依次取 enumerate(lists) 中的每个值
        if node:  # 判断: node
            heapq.heappush(heap, (node.val, i, node))

    dummy = ListNode(0)  # dummy: 赋值/计算
    current = dummy  # current: 赋值/计算

    while heap:  # 当 heap 时循环
        val, idx, node = heapq.heappop(heap)
        current.next = node  # current.next: 赋值/计算
        current = current.next  # current: 赋值/计算
        if node.next:  # 判断: node.next
            heapq.heappush(heap, (node.next.val, idx, node.next))

    return dummy.next  # 返回计算结果
```
 
## 六、栈与队列
 
### 20. 有效的括号 (Valid Parentheses)
 
**题目描述：** 
给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s`，判断字符串是否有效。 
有效字符串需满足： 
- 左括号必须用相同类型的右括号闭合。
- 左括号必须以正确的顺序闭合。
- 每个右括号都有一个对应的相同类型的左括号。
 
**示例：** 
 
```python
输入：s = "()[]{}"
输出：true
```
 
**解题思路：** 
使用栈： 
- 遇到左括号，入栈
- 遇到右括号，检查栈顶是否匹配
- 如果匹配，出栈；否则返回 false
- 最后检查栈是否为空
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：有效的括号
# 思路：使用栈：；遇到左括号，入栈遇到右括号，检查栈顶是否匹配如果匹配，出栈；否则返回 false最后检查栈是否为空
def isValid(s):  # 定义函数 isValid，接收参数: s
    stack = []  # stack: 空列表
    mapping = {"hl-string">')': "hl-string">'(', "hl-string">'}': "hl-string">'{', "hl-string">']': "hl-string">'['}  # mapping: 赋值/计算

    for char in s:  # 遍历: char 依次取 s 中的每个值
        if char in mapping:  # 判断: char in mapping
            if not stack or stack.pop() != mapping[char]:  # 判断: not stack or stack.pop() != mapping
                return False  # 返回 False
        else:  # 否则 (以上条件都不满足时执行)
            stack.append(char)  # 追加到列表末尾

    return not stack  # 返回计算结果
```
 
### 155. 最小栈 (Min Stack)
 
**题目描述：** 
设计一个支持 `push`，`pop`，`top` 操作，并能在常数时间内检索到最小元素的栈。 
实现 `MinStack` 类： 
- `MinStack()` 初始化堆栈对象。
- `void push(int val)` 将元素val推入堆栈。
- `void pop()` 删除堆栈顶部的元素。
- `int top()` 获取堆栈顶部的元素。
- `int getMin()` 获取堆栈中的最小元素。
 
**示例：** 
 
```python
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]
输出：
[null,null,null,null,-3,null,0,-2]
```
 
**解题思路：** 
使用辅助栈存储最小值： 
- 主栈存储所有元素
- 辅助栈存储每个状态下的最小值
- 每次 push 时，同时更新最小值栈
- 每次 pop 时，同时弹出最小值栈
 
**复杂度分析：** 
- 时间复杂度：O(1)，所有操作都是常数时间
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：最小栈
# 思路：使用辅助栈存储最小值：；主栈存储所有元素辅助栈存储每个状态下的最小值每次 push 时，同时更新最小值栈每次 pop 时，同时弹出最小值栈
class MinStack:  # 定义类 MinStack
    def __init__(self):  # 定义函数 __init__，接收参数: self
        self.stack = []  # self.stack: 空列表
        self.min_stack = []  # self.min_stack: 空列表

    def push(self, val):  # 定义函数 push，接收参数: self、val
        self.stack.append(val)  # 追加到列表末尾
        if not self.min_stack or val <= self.min_stack[-1]:  # 判断: not self.min_stack or val <= self.m
            self.min_stack.append(val)  # 追加到列表末尾

    def pop(self):  # 定义函数 pop，接收参数: self
        if self.stack.pop() == self.min_stack[-1]:  # 判断: self.stack.pop() == self.min_stack[
            self.min_stack.pop()  # 弹出末尾元素

    def top(self):  # 定义函数 top，接收参数: self
        return self.stack[-1]  # 返回列表结果

    def getMin(self):  # 定义函数 getMin，接收参数: self
        return self.min_stack[-1]  # 返回列表结果
```
 
### 739. 每日温度 (Daily Temperatures)
 
**题目描述：** 
给定一个整数数组 `temperatures`，表示每天的温度，返回一个数组 `answer`，其中 `answer[i]` 是指对于第 `i` 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。 
**示例：** 
 
```python
输入：temperatures = [73,74,75,71,69,72,76,73]
输出：[1,1,4,2,1,1,0,0]
```
 
**解题思路：** 
单调栈： 
- 使用栈存储温度的下标
- 遍历数组，对于每个温度，如果当前温度大于栈顶温度，则找到了下一个更高温度
- 计算天数差，更新结果数组
- 将当前下标入栈
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：每日温度
# 思路：单调栈：；使用栈存储温度的下标遍历数组，对于每个温度，如果当前温度大于栈顶温度，则找到了下一个更高温度计算天数差，更新结果数组将当前下标入栈
def dailyTemperatures(temperatures):  # 定义函数 dailyTemperatures，接收参数: temperatures
    n = len(temperatures)  # n: 获取长度
    result = [0] * n  # result: 乘积计算
    stack = []  # stack: 空列表

    for i in range(n):  # 遍历: i 依次取 range(n) 中的每个值
        while stack and temperatures[i] > temperatures[stack[-1]]:  # 当 stack and temperatures[i] > te 时循环
            prev_index = stack.pop()  # prev_index: 赋值/计算
            result[prev_index] = i - prev_index  # result[prev_index]: 赋值/计算
        stack.append(i)  # 追加到列表末尾

    return result  # 返回计算结果
```
 
### 84. 柱状图中最大的矩形 (Largest Rectangle in Histogram)
 
**题目描述：** 
给定 `n` 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1。 
求在该柱状图中，能够勾勒出来的矩形的最大面积。 
**示例：** 
 
```python
输入：heights = [2,1,5,6,2,3]
输出：10
解释：最大的矩形为图中红色区域，面积为 10
```
 
**解题思路：** 
单调栈： 
- 使用单调递增栈
- 对于每个柱子，找到左右两边第一个比它矮的柱子
- 计算以当前柱子为高的最大矩形面积
- 更新最大面积
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：柱状图中最大的矩形
# 思路：单调栈：；使用单调递增栈对于每个柱子，找到左右两边第一个比它矮的柱子计算以当前柱子为高的最大矩形面积更新最大面积
def largestRectangleArea(heights):  # 定义函数 largestRectangleArea，接收参数: heights
    stack = []  # stack: 空列表
    max_area = 0  # max_area: 计数器/下标初始化为0

    for i, h in enumerate(heights):  # 遍历: i, h 依次取 enumerate(heights) 中的每个值
        while stack and heights[stack[-1]] > h:  # 当 stack and heights[stack[-1]] > 时循环
            height = heights[stack.pop()]  # height: 赋值/计算
            width = i if not stack else i - stack[-1] - 1  # width: 赋值/计算
            max_area = max(max_area, height * width)  # max_area: 乘积计算
        stack.append(i)  # 追加到列表末尾

    while stack:  # 当 stack 时循环
        height = heights[stack.pop()]  # height: 赋值/计算
        width = len(heights) if not stack else len(heights) - stack[-1] - 1  # width: 获取长度
        max_area = max(max_area, height * width)  # max_area: 乘积计算

    return max_area  # 返回计算结果
```
 
### 42. 接雨水 (Trapping Rain Water)
 
**题目描述：** 
给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。 
**示例：** 
 
```python
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。
```
 
**解题思路：** 
方法1：双指针 
- 使用左右指针，维护左右两边的最大高度
- 对于每个位置，能接的雨水 = min(left_max, right_max) - height[i]
- 移动较小的一边
 
方法2：单调栈 
- 使用单调递减栈
- 当遇到更高的柱子时，计算能接的雨水
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)（双指针）或 O(n)（单调栈）
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：接雨水
# 思路：方法1：双指针；使用左右指针，维护左右两边的最大高度对于每个位置，能接的雨水 = min(left_max, right_max) - height[i]移动较小的一边；方法2：单调栈
def trap(height):  # 定义函数 trap，接收参数: height
    if not height:  # 判断: not height
        return 0  # 返回 0 (标志/空值)

    left, right = 0, len(height) - 1  # 交换/多重赋值
    left_max, right_max = 0, 0  # 交换/多重赋值
    water = 0  # water: 计数器/下标初始化为0

    while left < right:  # 当 left < right 时循环
        if height[left] < height[right]:  # 判断: height[left] < height[right]
            if height[left] >= left_max:  # 判断: height[left] >= left_max
                left_max = height[left]  # left_max: 赋值/计算
            else:  # 否则 (以上条件都不满足时执行)
                water += left_max - height[left]  # water 自反赋值+=
            left += 1  # left 自反赋值+=
        else:  # 否则 (以上条件都不满足时执行)
            if height[right] >= right_max:  # 判断: height[right] >= right_max
                right_max = height[right]  # right_max: 赋值/计算
            else:  # 否则 (以上条件都不满足时执行)
                water += right_max - height[right]  # water 自反赋值+=
            right -= 1  # right 自反赋值-=

    return water  # 返回计算结果
```
 
## 七、字符串处理
 
### 14. 最长公共前缀 (Longest Common Prefix)
 
**题目描述：** 
编写一个函数来查找字符串数组中的最长公共前缀。 
如果不存在公共前缀，返回空字符串 `""`。 
**示例：** 
 
```python
输入：strs = ["flower","flow","flight"]
输出："fl"
```
 
**解题思路：** 
方法1：横向扫描 
- 以第一个字符串为基准
- 逐个比较每个字符串的对应字符
- 找到第一个不匹配的位置
 
方法2：纵向扫描 
- 同时比较所有字符串的同一位置
- 找到第一个不匹配的位置
 
**复杂度分析：** 
- 时间复杂度：O(S)，S 是所有字符串字符的总数
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：最长公共前缀
# 思路：方法1：横向扫描；以第一个字符串为基准逐个比较每个字符串的对应字符找到第一个不匹配的位置；方法2：纵向扫描
def longestCommonPrefix(strs):  # 定义函数 longestCommonPrefix，接收参数: strs
    if not strs:  # 判断: not strs
        return ""  # 返回计算结果

    prefix = strs[0]  # prefix: 赋值/计算
    for i in range(1, len(strs)):  # 遍历: i 依次取 range(1, len(strs)) 中的每个值
        while not strs[i].startswith(prefix):  # 当 not strs[i].startswith(prefix) 时循环
            prefix = prefix[:-1]  # prefix: 赋值/计算
            if not prefix:  # 判断: not prefix
                return ""  # 返回计算结果
    return prefix  # 返回计算结果
```
 
### 344. 反转字符串 (Reverse String)
 
**题目描述：** 
编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 `s` 的形式给出。 
不要给另外的数组分配额外的空间，你必须**原地修改输入数组**、使用 O(1) 的额外空间解决这一问题。 
**示例：** 
 
```python
输入：s = ["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```
 
**解题思路：** 
双指针： 
- 使用左右指针
- 交换左右指针指向的字符
- 向中间移动指针
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：反转字符串
# 思路：双指针：；使用左右指针交换左右指针指向的字符向中间移动指针
def reverseString(s):  # 定义函数 reverseString，接收参数: s
    left, right = 0, len(s) - 1  # 交换/多重赋值
    while left < right:  # 当 left < right 时循环
        s[left], s[right] = s[right], s[left]  # 交换/多重赋值
        left += 1  # left 自反赋值+=
        right -= 1  # right 自反赋值-=
```
 
### 151. 反转字符串中的单词 (Reverse Words in a String)
 
**题目描述：** 
给你一个字符串 `s`，请你反转字符串中 **单词** 的顺序。 
**单词** 是由非空格字符组成的字符串。`s` 中使用至少一个空格将字符串中的 **单词** 分隔开。 
返回 **单词顺序颠倒且单词之间用单个空格连接的结果字符串**。 
**注意：** 输入字符串 `s` 中可能会存在前导空格、尾随空格或者单词间的多个空格。返回的结果字符串中，单词间应当仅用单个空格分隔，且不包含任何额外的空格。 
**示例：** 
 
```python
输入：s = "the sky is blue"
输出："blue is sky the"
```
 
**解题思路：** 
- 先反转整个字符串
- 再反转每个单词
- 处理多余空格
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：反转字符串中的单词
# 思路：先反转整个字符串再反转每个单词处理多余空格
def reverseWords(s):  # 定义函数 reverseWords，接收参数: s
    s = s.strip()  # s: 赋值/计算
    words = s.split()  # words: 赋值/计算
    return "hl-string">' '.join(reversed(words))  # 返回计算结果
```
 
### 43. 字符串相乘 (Multiply Strings)
 
**题目描述：** 
给定两个以字符串形式表示的非负整数 `num1` 和 `num2`，返回 `num1` 和 `num2` 的乘积，它们的乘积也表示为字符串形式。 
**注意：** 不能使用任何内置的 BigInteger 库或直接将输入转换为整数。 
**示例：** 
 
```python
输入：num1 = "2", num2 = "3"
输出："6"
```
 
**解题思路：** 
模拟竖式乘法： 
- 创建一个数组存储结果
- 从右到左逐位相乘
- 处理进位
- 去除前导零
 
**复杂度分析：** 
- 时间复杂度：O(m × n)
- 空间复杂度：O(m + n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：字符串相乘
# 思路：模拟竖式乘法：；创建一个数组存储结果从右到左逐位相乘处理进位去除前导零
def multiply(num1, num2):  # 定义函数 multiply，接收参数: num1、num2
    if num1 == "0" or num2 == "0":  # 判断: num1 == "0" or num2 == "0"
        return "0"  # 返回计算结果

    m, n = len(num1), len(num2)  # 交换/多重赋值
    result = [0] * (m + n)  # result: 乘积计算

    for i in range(m - 1, -1, -1):  # 遍历: i 依次取 range(m - 1, -1, -1) 中的每个值
        for j in range(n - 1, -1, -1):  # 遍历: j 依次取 range(n - 1, -1, -1) 中的每个值
            mul = int(num1[i]) * int(num2[j])  # mul: 乘积计算
            p1, p2 = i + j, i + j + 1  # 交换/多重赋值
            total = mul + result[p2]  # total: 赋值/计算
            result[p2] = total % 10  # result[p2]: 赋值/计算
            result[p1] += total // 10  # result[p1] 自反赋值+=

    start = 0  # start: 计数器/下标初始化为0
    while start < len(result) and result[start] == 0:  # 当 start < len(result) and result 时循环
        start += 1  # start 自反赋值+=

    return "hl-string">''.join(map(str, result[start:]))  # 返回列表结果
```
 
### 71. 简化路径 (Simplify Path)
 
**题目描述：** 
给你一个字符串 `path`，表示指向某一文件或目录的 Unix 风格 **绝对路径**（以 `'/'` 开头），请你将其转化为更加简洁的规范路径。 
在 Unix 风格的文件系统中，一个点（`.`）表示当前目录本身；此外，两个点 （`..`） 表示将目录切换到上一级（指向父目录）；两者都可以是复杂相对路径的组成部分。任意多个连续的斜杠（即，`'//'`）都被视为单个斜杠 `'/'`。 对于此问题，任何其他格式的点（例如，`'...'`）均被视为文件/目录名称。 
返回 **简化后** 的 **规范路径**。 
**示例：** 
 
```python
输入：path = "/home//foo/"
输出："/home/foo"
解释：在规范路径中，多个连续斜杠需要用一个斜杠替换，并且末尾的斜杠也需要被移除。
```
 
**解题思路：** 
使用栈： 
- 按 ‘/‘ 分割路径
- 遇到 ‘.’ 或空字符串，跳过
- 遇到 ‘..’，弹出栈顶
- 其他情况，入栈
- 最后用 ‘/‘ 连接栈中元素
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：简化路径
# 思路：使用栈：；按 ‘/‘ 分割路径遇到 ‘.’ 或空字符串，跳过遇到 ‘..’，弹出栈顶其他情况，入栈最后用 ‘/‘ 连接栈中元素
def simplifyPath(path):  # 定义函数 simplifyPath，接收参数: path
    stack = []  # stack: 空列表
    parts = path.split("hl-string">'/')  # parts: 赋值/计算

    for part in parts:  # 遍历: part 依次取 parts 中的每个值
        if part == "hl-string">'..':  # 判断: part == '..'
            if stack:  # 判断: stack
                stack.pop()  # 弹出末尾元素
        elif part and part != "hl-string">'.':  # 否则如果
            stack.append(part)  # 追加到列表末尾

    return "hl-string">'/' + "hl-string">'/'.join(stack)  # 返回计算结果
```
 
### 72. 编辑距离 (Edit Distance)
 
**题目描述：** 
给你两个单词 `word1` 和 `word2`，请返回将 `word1` 转换成 `word2` 所使用的最少操作数。 
你可以对一个单词进行如下三种操作： 
- 插入一个字符
- 删除一个字符
- 替换一个字符
 
**示例：** 
 
```python
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
```
 
**解题思路：** 
动态规划： 
- `dp[i][j]` 表示 word1 的前 i 个字符转换为 word2 的前 j 个字符的最少操作数
- 如果 `word1[i-1] == word2[j-1]`，`dp[i][j] = dp[i-1][j-1]`
- 否则，`dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1`
 
**复杂度分析：** 
- 时间复杂度：O(m × n)
- 空间复杂度：O(m × n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：编辑距离
# 思路：动态规划：；dp[i][j] 表示 word1 的前 i 个字符转换为 word2 的前 j 个字符的最少操作数如果 word1[i-1] == word2[j-1]，dp[i][j] = dp[i-1][j-1]否则，dp[i][j] = min(dp[i-1][j], dp[i][j-1], 
def minDistance(word1, word2):  # 定义函数 minDistance，接收参数: word1、word2
    m, n = len(word1), len(word2)  # 交换/多重赋值
    dp = [[0] * (n + 1) for _ in range(m + 1)]  # dp: 列表推导式

    for i in range(m + 1):  # 遍历: i 依次取 range(m + 1) 中的每个值
        dp[i][0] = i
    for j in range(n + 1):  # 遍历: j 依次取 range(n + 1) 中的每个值
        dp[0][j] = j

    for i in range(1, m + 1):  # 遍历: i 依次取 range(1, m + 1) 中的每个值
        for j in range(1, n + 1):  # 遍历: j 依次取 range(1, n + 1) 中的每个值
            if word1[i-1] == word2[j-1]:  # 判断: word1[i-1] == word2[j-1]
                dp[i][j] = dp[i-1][j-1]
            else:  # 否则 (以上条件都不满足时执行)
                dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1

    return dp[m][n]  # 返回列表结果
```
 
## 八、位运算
 
### 136. 只出现一次的数字 (Single Number)
 
**题目描述：** 
给定一个**非空**整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。 
**说明：** 你的算法应该具有线性时间复杂度。你可以不使用额外空间来实现吗？ 
**示例：** 
 
```python
输入：[2,2,1]
输出：1
```
 
**解题思路：** 
异或运算： 
- 任何数与0异或等于它本身
- 任何数与自身异或等于0
- 异或运算满足交换律和结合律
- 将所有数字异或，结果就是只出现一次的数字
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：只出现一次的数字
# 思路：异或运算：；任何数与0异或等于它本身任何数与自身异或等于0异或运算满足交换律和结合律将所有数字异或，结果就是只出现一次的数字
def singleNumber(nums):  # 定义函数 singleNumber，接收参数: nums
    result = 0  # result: 计数器/下标初始化为0
    for num in nums:  # 遍历: num 依次取 nums 中的每个值
        result ^= num
    return result  # 返回计算结果
```
 
### 137. 只出现一次的数字 II (Single Number II)
 
**题目描述：** 
给你一个整数数组 `nums`，除某个元素仅出现 **一次** 外，其余每个元素都恰出现 **三次**。请你找出并返回那个只出现了一次的元素。 
**示例：** 
 
```python
输入：nums = [2,2,3,2]
输出：3
```
 
**解题思路：** 
位运算： 
- 统计每一位上1出现的次数
- 如果某位上1出现的次数是3的倍数，说明只出现一次的数字在该位为0
- 否则，只出现一次的数字在该位为1
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：只出现一次的数字 II
# 思路：位运算：；统计每一位上1出现的次数如果某位上1出现的次数是3的倍数，说明只出现一次的数字在该位为0否则，只出现一次的数字在该位为1
def singleNumber(nums):  # 定义函数 singleNumber，接收参数: nums
    result = 0  # result: 计数器/下标初始化为0
    for i in range(32):  # 遍历: i 依次取 range(32) 中的每个值
        count = 0  # count: 计数器/下标初始化为0
        for num in nums:  # 遍历: num 依次取 nums 中的每个值
            count += (num >> i) & 1  # count 自反赋值+=
        result |= (count % 3) << i
    return result if result < 2**31 else result - 2**32  # 返回计算结果
```
 
### 260. 只出现一次的数字 III (Single Number III)
 
**题目描述：** 
给定一个整数数组 `nums`，其中恰好有两个元素只出现一次，其余所有元素均出现两次。找出只出现一次的那两个元素。你可以按 **任意顺序** 返回答案。 
**示例：** 
 
```python
输入：nums = [1,2,1,3,2,5]
输出：[3,5]
解释：[5, 3] 也是有效的答案。
```
 
**解题思路：** 
位运算： 
- 将所有数字异或，得到两个只出现一次数字的异或结果
- 找到异或结果中任意一个为1的位（说明两个数字在该位不同）
- 根据该位将数组分成两组，每组各包含一个只出现一次的数字
- 分别对两组进行异或，得到两个结果
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：只出现一次的数字 III
# 思路：位运算：；将所有数字异或，得到两个只出现一次数字的异或结果找到异或结果中任意一个为1的位（说明两个数字在该位不同）根据该位将数组分成两组，每组各包含一个只出现一次的数字分别对两组进行异或，得到两个结果
def singleNumber(nums):  # 定义函数 singleNumber，接收参数: nums
    xor_all = 0  # xor_all: 计数器/下标初始化为0
    for num in nums:  # 遍历: num 依次取 nums 中的每个值
        xor_all ^= num

    diff_bit = xor_all & (-xor_all)  # diff_bit: 赋值/计算

    result = [0, 0]  # result: 赋值/计算
    for num in nums:  # 遍历: num 依次取 nums 中的每个值
        if num & diff_bit:  # 判断: num & diff_bit
            result[0] ^= num
        else:  # 否则 (以上条件都不满足时执行)
            result[1] ^= num

    return result  # 返回计算结果
```
 
## 九、其他重要题目
 
### 41. 缺失的第一个正数 (First Missing Positive)
 
**题目描述：** 
给你一个未排序的整数数组 `nums`，请你找出其中没有出现的最小的正整数。 
请你实现时间复杂度为 `O(n)` 并且只使用常数级别额外空间的解决方案。 
**示例：** 
 
```python
输入：nums = [1,2,0]
输出：3
```
 
**解题思路：** 
原地哈希： 
- 将数组视为哈希表，将数字 i 放在位置 i-1
- 遍历数组，如果 `nums[i]` 在 [1, n] 范围内，将其放到正确位置
- 再次遍历数组，找到第一个 `nums[i] != i+1` 的位置
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：缺失的第一个正数
# 思路：原地哈希：；将数组视为哈希表，将数字 i 放在位置 i-1遍历数组，如果 nums[i] 在 [1, n] 范围内，将其放到正确位置再次遍历数组，找到第一个 nums[i] != i+1 的位置
def firstMissingPositive(nums):  # 定义函数 firstMissingPositive，接收参数: nums
    n = len(nums)  # n: 获取长度

    for i in range(n):  # 遍历: i 依次取 range(n) 中的每个值
        while 1 <= nums[i] <= n and nums[nums[i] - 1] != nums[i]:  # 当 1 <= nums[i] <= n and nums[num 时循环
            nums[nums[i] - 1], nums[i] = nums[i], nums[nums[i] - 1]  # 交换/多重赋值

    for i in range(n):  # 遍历: i 依次取 range(n) 中的每个值
        if nums[i] != i + 1:  # 判断: nums[i] != i + 1
            return i + 1  # 返回计算结果

    return n + 1  # 返回计算结果
```
 
### 287. 寻找重复数 (Find the Duplicate Number)
 
**题目描述：** 
给定一个包含 `n + 1` 个整数的数组 `nums`，其数字都在 `[1, n]` 范围内（包括 `1` 和 `n`），可知至少存在一个重复的整数。 
假设 `nums` 只有 **一个重复的整数**，返回 **这个重复的数**。 
你设计的解决方案必须 **不修改** 数组 `nums` 且只用常量级 `O(1)` 的额外空间。 
**示例：** 
 
```python
输入：nums = [1,3,4,2,2]
输出：2
```
 
**解题思路：** 
快慢指针（Floyd判圈算法）： 
- 将数组视为链表，`nums[i]` 指向 `nums[nums[i]]`
- 使用快慢指针找到环的入口
- 环的入口就是重复的数字
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：寻找重复数
# 思路：快慢指针（Floyd判圈算法）：；将数组视为链表，nums[i] 指向 nums[nums[i]]使用快慢指针找到环的入口环的入口就是重复的数字
def findDuplicate(nums):  # 定义函数 findDuplicate，接收参数: nums
    slow = fast = nums[0]  # slow: 赋值/计算

    while True:  # 当 True 时循环
        slow = nums[slow]  # slow: 赋值/计算
        fast = nums[nums[fast]]  # fast: 赋值/计算
        if slow == fast:  # 判断: slow == fast
            break  # 跳出当前循环

    slow = nums[0]  # slow: 赋值/计算
    while slow != fast:  # 当 slow != fast 时循环
        slow = nums[slow]  # slow: 赋值/计算
        fast = nums[fast]  # fast: 赋值/计算

    return slow  # 返回计算结果
```
 
### 4. 寻找两个正序数组的中位数 (Median of Two Sorted Arrays)
 
**题目描述：** 
给定两个大小分别为 `m` 和 `n` 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的 **中位数**。 
算法的时间复杂度应该为 `O(log (m+n))`。 
**示例：** 
 
```python
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
```
 
**解题思路：** 
二分查找： 
- 将问题转化为寻找第 k 小的元素
- 每次比较两个数组的第 k/2 个元素
- 排除较小的一半，继续查找
 
**复杂度分析：** 
- 时间复杂度：O(log(m + n))
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：寻找两个正序数组的中位数
# 思路：二分查找：；将问题转化为寻找第 k 小的元素每次比较两个数组的第 k/2 个元素排除较小的一半，继续查找
def findMedianSortedArrays(nums1, nums2):  # 定义函数 findMedianSortedArrays，接收参数: nums1、nums2
    def getKth(k):  # 定义函数 getKth，接收参数: k
        i, j = 0, 0  # 交换/多重赋值
        while True:  # 当 True 时循环
            if i == len(nums1):  # 判断: i == len(nums1)
                return nums2[j + k - 1]  # 返回列表结果
            if j == len(nums2):  # 判断: j == len(nums2)
                return nums1[i + k - 1]  # 返回列表结果
            if k == 1:  # 判断: k == 1
                return min(nums1[i], nums2[j])  # 返回列表结果

            mid = k // 2  # mid: 赋值/计算
            idx1 = min(i + mid - 1, len(nums1) - 1)  # idx1: 获取长度
            idx2 = min(j + mid - 1, len(nums2) - 1)  # idx2: 获取长度

            if nums1[idx1] <= nums2[idx2]:  # 判断: nums1[idx1] <= nums2[idx2]
                k -= idx1 - i + 1  # k 自反赋值-=
                i = idx1 + 1  # i: 赋值/计算
            else:  # 否则 (以上条件都不满足时执行)
                k -= idx2 - j + 1  # k 自反赋值-=
                j = idx2 + 1  # j: 赋值/计算

    m, n = len(nums1), len(nums2)  # 交换/多重赋值
    if (m + n) % 2 == 1:  # 判断: (m + n) % 2 == 1
        return getKth((m + n) // 2 + 1)  # 返回计算结果
    else:  # 否则 (以上条件都不满足时执行)
        return (getKth((m + n) // 2) + getKth((m + n) // 2 + 1)) / 2.0  # 返回计算结果
```
 
### 10. 正则表达式匹配 (Regular Expression Matching)
 
**题目描述：** 
给你一个字符串 `s` 和一个字符规律 `p`，请你来实现一个支持 `'.'` 和 `'*'` 的正则表达式匹配。 
- `'.'` 匹配任意单个字符
- `'*'` 匹配零个或多个前面的那一个元素
 
所谓匹配，是要涵盖 **整个** 字符串 `s` 的，而不是部分字符串。 
**示例：** 
 
```python
输入：s = "aa", p = "a*"
输出：true
解释：因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
```
 
**解题思路：** 
动态规划： 
- `dp[i][j]` 表示 s 的前 i 个字符和 p 的前 j 个字符是否匹配
- 如果 `p[j-1] == '*'`，可以选择匹配0次或多次
- 如果 `p[j-1] == '.'` 或 `s[i-1] == p[j-1]`，匹配一个字符
 
**复杂度分析：** 
- 时间复杂度：O(m × n)
- 空间复杂度：O(m × n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：正则表达式匹配
# 思路：动态规划：；dp[i][j] 表示 s 的前 i 个字符和 p 的前 j 个字符是否匹配如果 p[j-1] == '*'，可以选择匹配0次或多次如果 p[j-1] == '.' 或 s[i-1] == p[j-1]，匹配一个字符
def isMatch(s, p):  # 定义函数 isMatch，接收参数: s、p
    m, n = len(s), len(p)  # 交换/多重赋值
    dp = [[False] * (n + 1) for _ in range(m + 1)]  # dp: 列表推导式
    dp[0][0] = True

    for j in range(2, n + 1):  # 遍历: j 依次取 range(2, n + 1) 中的每个值
        if p[j-1] == "hl-string">'*':  # 判断: p[j-1] == '*'
            dp[0][j] = dp[0][j-2]

    for i in range(1, m + 1):  # 遍历: i 依次取 range(1, m + 1) 中的每个值
        for j in range(1, n + 1):  # 遍历: j 依次取 range(1, n + 1) 中的每个值
            if p[j-1] == "hl-string">'*':  # 判断: p[j-1] == '*'
                dp[i][j] = dp[i][j-2] or (dp[i-1][j] and (s[i-1] == p[j-2] or p[j-2] == "hl-string">'.'))
            else:  # 否则 (以上条件都不满足时执行)
                dp[i][j] = dp[i-1][j-1] and (s[i-1] == p[j-1] or p[j-1] == "hl-string">'.')

    return dp[m][n]  # 返回列表结果
```
 
### 44. 通配符匹配 (Wildcard Matching)
 
**题目描述：** 
给定一个字符串 (`s`) 和一个字符模式 (`p`)，实现一个支持 `'?'` 和 `'*'` 的通配符匹配。 
- `'?'` 可以匹配任何单个字符。
- `'*'` 可以匹配任意字符串（包括空字符串）。
 
两个字符串**完全匹配**才算匹配成功。 
**示例：** 
 
```python
输入：s = "adceb", p = "*a*b"
输出：true
解释：第一个 '*' 可以匹配空字符串, 第二个 '*' 可以匹配字符串 "dce".
```
 
**解题思路：** 
动态规划： 
- `dp[i][j]` 表示 s 的前 i 个字符和 p 的前 j 个字符是否匹配
- 如果 `p[j-1] == '*'`，可以匹配0个或多个字符
- 如果 `p[j-1] == '?'` 或 `s[i-1] == p[j-1]`，匹配一个字符
 
**复杂度分析：** 
- 时间复杂度：O(m × n)
- 空间复杂度：O(m × n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：通配符匹配
# 思路：动态规划：；dp[i][j] 表示 s 的前 i 个字符和 p 的前 j 个字符是否匹配如果 p[j-1] == '*'，可以匹配0个或多个字符如果 p[j-1] == '?' 或 s[i-1] == p[j-1]，匹配一个字符
def isMatch(s, p):  # 定义函数 isMatch，接收参数: s、p
    m, n = len(s), len(p)  # 交换/多重赋值
    dp = [[False] * (n + 1) for _ in range(m + 1)]  # dp: 列表推导式
    dp[0][0] = True

    for j in range(1, n + 1):  # 遍历: j 依次取 range(1, n + 1) 中的每个值
        if p[j-1] == "hl-string">'*':  # 判断: p[j-1] == '*'
            dp[0][j] = dp[0][j-1]

    for i in range(1, m + 1):  # 遍历: i 依次取 range(1, m + 1) 中的每个值
        for j in range(1, n + 1):  # 遍历: j 依次取 range(1, n + 1) 中的每个值
            if p[j-1] == "hl-string">'*':  # 判断: p[j-1] == '*'
                dp[i][j] = dp[i][j-1] or dp[i-1][j]
            else:  # 否则 (以上条件都不满足时执行)
                dp[i][j] = dp[i-1][j-1] and (s[i-1] == p[j-1] or p[j-1] == "hl-string">'?')

    return dp[m][n]  # 返回列表结果
```
 
### 121. 买卖股票的最佳时机 (Best Time to Buy and Sell Stock)
 
**题目描述：** 
给定一个数组 `prices`，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。 
你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。 
返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0`。 
**示例：** 
 
```python
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5。
```
 
**解题思路：** 
一次遍历： 
- 维护一个变量记录最低价格
- 遍历数组，计算当前价格与最低价格的差值
- 更新最大利润
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：买卖股票的最佳时机
# 思路：一次遍历：；维护一个变量记录最低价格遍历数组，计算当前价格与最低价格的差值更新最大利润
def maxProfit(prices):  # 定义函数 maxProfit，接收参数: prices
    min_price = float("hl-string">'inf')  # min_price: 赋值/计算
    max_profit = 0  # max_profit: 计数器/下标初始化为0

    for price in prices:  # 遍历: price 依次取 prices 中的每个值
        min_price = min(min_price, price)  # min_price: 赋值/计算
        max_profit = max(max_profit, price - min_price)  # max_profit: 赋值/计算

    return max_profit  # 返回计算结果
```
 
### 122. 买卖股票的最佳时机 II (Best Time to Buy and Sell Stock II)
 
**题目描述：** 
给你一个整数数组 `prices`，其中 `prices[i]` 表示某支股票第 `i` 天的价格。 
在每一天，你可以决定是否购买和/或出售股票。你在任何时候 **最多** 只能持有 **一股** 股票。你也可以先购买，然后在 **同一天** 出售。 
返回你能获得的 **最大** 利润。 
**示例：** 
 
```python
输入：prices = [7,1,5,3,6,4]
输出：7
解释：在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4。
随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3。
总利润: 4 + 3 = 7。
```
 
**解题思路：** 
贪心算法： 
- 只要后一天价格高于前一天，就进行交易
- 累加所有正收益
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：买卖股票的最佳时机 II
# 思路：贪心算法：；只要后一天价格高于前一天，就进行交易累加所有正收益
def maxProfit(prices):  # 定义函数 maxProfit，接收参数: prices
    profit = 0  # profit: 计数器/下标初始化为0
    for i in range(1, len(prices)):  # 遍历: i 依次取 range(1, len(prices)) 中的每个值
        if prices[i] > prices[i-1]:  # 判断: prices[i] > prices[i-1]
            profit += prices[i] - prices[i-1]  # profit 自反赋值+=
    return profit  # 返回计算结果
```
 
### 123. 买卖股票的最佳时机 III (Best Time to Buy and Sell Stock III)
 
**题目描述：** 
给定一个数组，它的第 `i` 个元素是一支给定的股票在第 `i` 天的价格。 
设计一个算法来计算你所能获取的最大利润。你最多可以完成 **两笔** 交易。 
**注意：** 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。 
**示例：** 
 
```python
输入：prices = [3,3,5,0,0,3,1,4]
输出：6
解释：在第 4 天（股票价格 = 0）的时候买入，在第 6 天（股票价格 = 3）的时候卖出，这笔交易所能获得利润 = 3-0 = 3。
随后，在第 7 天（股票价格 = 1）的时候买入，在第 8 天 （股票价格 = 4）的时候卖出，这笔交易所能获得利润 = 4-1 = 3。
```
 
**解题思路：** 
动态规划： 
- `dp[i][k][0/1]` 表示第 i 天，最多 k 次交易，持有/不持有股票的最大利润
- 状态转移：
- `dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])`
- `dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])`
 
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：买卖股票的最佳时机 III
# 思路：动态规划：；dp[i][k][0/1] 表示第 i 天，最多 k 次交易，持有/不持有股票的最大利润状态转移：dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])dp[i][k][1] = max(dp[i-1][k][1], dp
def maxProfit(prices):  # 定义函数 maxProfit，接收参数: prices
    buy1 = buy2 = float("hl-string">'-inf')  # buy1: 赋值/计算
    sell1 = sell2 = 0  # sell1: 赋值/计算

    for price in prices:  # 遍历: price 依次取 prices 中的每个值
        buy1 = max(buy1, -price)  # buy1: 赋值/计算
        sell1 = max(sell1, buy1 + price)  # sell1: 赋值/计算
        buy2 = max(buy2, sell1 - price)  # buy2: 赋值/计算
        sell2 = max(sell2, buy2 + price)  # sell2: 赋值/计算

    return sell2  # 返回计算结果
```
 
### 188. 买卖股票的最佳时机 IV (Best Time to Buy and Sell Stock IV)
 
**题目描述：** 
给定一个整数数组 `prices`，其中 `prices[i]` 是一支给定的股票在第 `i` 天的价格。 
设计一个算法来计算你所能获取的最大利润。你最多可以完成 `k` 笔交易。 
**注意：** 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。 
**示例：** 
 
```python
输入：k = 2, prices = [2,4,1]
输出：2
解释：在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2。
```
 
**解题思路：** 
动态规划： 
- 如果 `k >= n/2`，相当于可以无限次交易，使用贪心算法
- 否则，使用动态规划，类似第79题
 
**复杂度分析：** 
- 时间复杂度：O(n × k)
- 空间复杂度：O(k)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：买卖股票的最佳时机 IV
# 思路：动态规划：；如果 k >= n/2，相当于可以无限次交易，使用贪心算法否则，使用动态规划，类似第79题
def maxProfit(k, prices):  # 定义函数 maxProfit，接收参数: k、prices
    n = len(prices)  # n: 获取长度
    if k >= n // 2:  # 判断: k >= n // 2
        profit = 0  # profit: 计数器/下标初始化为0
        for i in range(1, n):  # 遍历: i 依次取 range(1, n) 中的每个值
            if prices[i] > prices[i-1]:  # 判断: prices[i] > prices[i-1]
                profit += prices[i] - prices[i-1]  # profit 自反赋值+=
        return profit  # 返回计算结果

    buy = [float("hl-string">'-inf')] * (k + 1)  # buy: 乘积计算
    sell = [0] * (k + 1)  # sell: 乘积计算

    for price in prices:  # 遍历: price 依次取 prices 中的每个值
        for j in range(1, k + 1):  # 遍历: j 依次取 range(1, k + 1) 中的每个值
            buy[j] = max(buy[j], sell[j-1] - price)  # buy[j]: 赋值/计算
            sell[j] = max(sell[j], buy[j] + price)  # sell[j]: 赋值/计算

    return sell[k]  # 返回列表结果
```
 
### 85. 最大矩形 (Maximal Rectangle)
 
**题目描述：** 
给定一个仅包含 `0` 和 `1`、大小为 `rows x cols` 的二维二进制矩阵，找出只包含 `1` 的最大矩形，并返回其面积。 
**示例：** 
 
```python
输入：matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
输出：6
```
 
**解题思路：** 
将问题转化为柱状图中最大的矩形： 
- 对于每一行，计算以该行为底部的柱状图高度
- 对每行应用”柱状图中最大的矩形”的算法
 
**复杂度分析：** 
- 时间复杂度：O(m × n)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：最大矩形
# 思路：将问题转化为柱状图中最大的矩形：；对于每一行，计算以该行为底部的柱状图高度对每行应用”柱状图中最大的矩形”的算法
def maximalRectangle(matrix):  # 定义函数 maximalRectangle，接收参数: matrix
    if not matrix:  # 判断: not matrix
        return 0  # 返回 0 (标志/空值)

    m, n = len(matrix), len(matrix[0])  # 交换/多重赋值
    heights = [0] * n  # heights: 乘积计算
    max_area = 0  # max_area: 计数器/下标初始化为0

    for i in range(m):  # 遍历: i 依次取 range(m) 中的每个值
        for j in range(n):  # 遍历: j 依次取 range(n) 中的每个值
            heights[j] = heights[j] + 1 if matrix[i][j] == "hl-string">'1' else 0

        stack = []  # stack: 空列表
        for j in range(n + 1):  # 遍历: j 依次取 range(n + 1) 中的每个值
            h = heights[j] if j < n else 0  # h: 赋值/计算
            while stack and heights[stack[-1]] > h:  # 当 stack and heights[stack[-1]] > 时循环
                height = heights[stack.pop()]  # height: 赋值/计算
                width = j if not stack else j - stack[-1] - 1  # width: 赋值/计算
                max_area = max(max_area, height * width)  # max_area: 乘积计算
            stack.append(j)  # 追加到列表末尾

    return max_area  # 返回计算结果
```
 
### 416. 分割等和子集 (Partition Equal Subset Sum)
 
**题目描述：** 
给你一个 **只包含正整数** 的 **非空** 数组 `nums`。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。 
**示例：** 
 
```python
输入：nums = [1,5,11,5]
输出：true
解释：数组可以分割成 [1, 5, 5] 和 [11]。
```
 
**解题思路：** 
0-1背包问题： 
- 如果总和为奇数，不可能分割
- 目标是找到子集，使其和为总和的一半
- 使用动态规划：`dp[i][j]` 表示前 i 个元素能否组成和为 j
 
**复杂度分析：** 
- 时间复杂度：O(n × sum)
- 空间复杂度：O(sum)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：分割等和子集
# 思路：0-1背包问题：；如果总和为奇数，不可能分割目标是找到子集，使其和为总和的一半使用动态规划：dp[i][j] 表示前 i 个元素能否组成和为 j
def canPartition(nums):  # 定义函数 canPartition，接收参数: nums
    total = sum(nums)  # total: 赋值/计算
    if total % 2 != 0:  # 判断: total % 2 != 0
        return False  # 返回 False

    target = total // 2  # target: 赋值/计算
    dp = [False] * (target + 1)  # dp: 乘积计算
    dp[0] = True  # dp[0]: 布尔值True

    for num in nums:  # 遍历: num 依次取 nums 中的每个值
        for j in range(target, num - 1, -1):  # 遍历: j 依次取 range(target, num - 1, -1) 中的每个值
            dp[j] = dp[j] or dp[j - num]  # dp[j]: 赋值/计算

    return dp[target]  # 返回列表结果
```
 
### 494. 目标和 (Target Sum)
 
**题目描述：** 
给你一个整数数组 `nums` 和一个整数 `target`。 
向数组中的每个整数前添加 `'+'` 或 `'-'`，然后串联起所有整数，可以构造一个 **表达式**： 
例如，`nums = [2, 1]`，可以在 `2` 之前添加 `'+'`，在 `1` 之前添加 `'-'`，然后串联起来得到表达式 `"+2-1"`。 
返回可以通过上述方法构造的、运算结果等于 `target` 的不同 **表达式** 的数目。 
**示例：** 
 
```python
输入：nums = [1,1,1,1,1], target = 3
输出：5
```
 
**解题思路：** 
动态规划： 
- 设正数和为 P，负数和为 N，则 P - N = target，P + N = sum
- 得到 P = (target + sum) / 2
- 转化为0-1背包问题：找到和为 P 的子集数目
 
**复杂度分析：** 
- 时间复杂度：O(n × sum)
- 空间复杂度：O(sum)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：目标和
# 思路：动态规划：；设正数和为 P，负数和为 N，则 P - N = target，P + N = sum得到 P = (target + sum) / 2转化为0-1背包问题：找到和为 P 的子集数目
def findTargetSumWays(nums, target):  # 定义函数 findTargetSumWays，接收参数: nums、target
    total = sum(nums)  # total: 赋值/计算
    if (total + target) % 2 != 0 or total < abs(target):  # 判断: (total + target) % 2 != 0 or total 
        return 0  # 返回 0 (标志/空值)

    p = (total + target) // 2  # p: 赋值/计算
    dp = [0] * (p + 1)  # dp: 乘积计算
    dp[0] = 1  # dp[0]: 初始化为1

    for num in nums:  # 遍历: num 依次取 nums 中的每个值
        for j in range(p, num - 1, -1):  # 遍历: j 依次取 range(p, num - 1, -1) 中的每个值
            dp[j] += dp[j - num]  # dp[j] 自反赋值+=

    return dp[p]  # 返回列表结果
```
 
### 62. 不同路径 (Unique Paths)
 
**题目描述：** 
一个机器人位于一个 `m x n` 网格的左上角（起始点在下图中标记为 “Start”）。 
机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish”）。 
问总共有多少条不同的路径？ 
**示例：** 
 
```python
输入：m = 3, n = 7
输出：28
```
 
**解题思路：** 
动态规划： 
- `dp[i][j]` 表示到达位置 (i, j) 的路径数
- `dp[i][j] = dp[i-1][j] + dp[i][j-1]`
- 边界条件：第一行和第一列都是1
 
**复杂度分析：** 
- 时间复杂度：O(m × n)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：不同路径
# 思路：动态规划：；dp[i][j] 表示到达位置 (i, j) 的路径数dp[i][j] = dp[i-1][j] + dp[i][j-1]边界条件：第一行和第一列都是1
def uniquePaths(m, n):  # 定义函数 uniquePaths，接收参数: m、n
    dp = [1] * n  # dp: 乘积计算

    for i in range(1, m):  # 遍历: i 依次取 range(1, m) 中的每个值
        for j in range(1, n):  # 遍历: j 依次取 range(1, n) 中的每个值
            dp[j] += dp[j-1]  # dp[j] 自反赋值+=

    return dp[n-1]  # 返回列表结果
```
 
### 63. 不同路径 II (Unique Paths II)
 
**题目描述：** 
一个机器人位于一个 `m x n` 网格的左上角（起始点在下图中标记为 “Start”）。 
机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish”）。 
现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？ 
网格中的障碍物和空位置分别用 `1` 和 `0` 来表示。 
**示例：** 
 
```python
输入：obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
输出：2
```
 
**解题思路：** 
动态规划： 
- 如果 `obstacleGrid[i][j] == 1`，`dp[i][j] = 0`
- 否则，`dp[i][j] = dp[i-1][j] + dp[i][j-1]`
 
**复杂度分析：** 
- 时间复杂度：O(m × n)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：不同路径 II
# 思路：动态规划：；如果 obstacleGrid[i][j] == 1，dp[i][j] = 0否则，dp[i][j] = dp[i-1][j] + dp[i][j-1]
def uniquePathsWithObstacles(obstacleGrid):  # 定义函数 uniquePathsWithObstacles，接收参数: obstacleGrid
    m, n = len(obstacleGrid), len(obstacleGrid[0])  # 交换/多重赋值
    dp = [0] * n  # dp: 乘积计算
    dp[0] = 1 if obstacleGrid[0][0] == 0 else 0

    for i in range(m):  # 遍历: i 依次取 range(m) 中的每个值
        for j in range(n):  # 遍历: j 依次取 range(n) 中的每个值
            if obstacleGrid[i][j] == 1:  # 判断: obstacleGrid[i][j] == 1
                dp[j] = 0  # dp[j]: 计数器/下标初始化为0
            elif j > 0:  # 否则如果
                dp[j] += dp[j-1]  # dp[j] 自反赋值+=

    return dp[n-1]  # 返回列表结果
```
 
### 64. 最小路径和 (Minimum Path Sum)
 
**题目描述：** 
给定一个包含非负整数的 `m x n` 网格 `grid`，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。 
**说明：** 每次只能向下或者向右移动一步。 
**示例：** 
 
```python
输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
输出：7
解释：因为路径 1→3→1→1→1 的总和最小。
```
 
**解题思路：** 
动态规划： 
- `dp[i][j]` 表示到达位置 (i, j) 的最小路径和
- `dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]`
 
**复杂度分析：** 
- 时间复杂度：O(m × n)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：最小路径和
# 思路：动态规划：；dp[i][j] 表示到达位置 (i, j) 的最小路径和dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]
def minPathSum(grid):  # 定义函数 minPathSum，接收参数: grid
    m, n = len(grid), len(grid[0])  # 交换/多重赋值
    dp = [float("hl-string">'inf')] * n  # dp: 乘积计算
    dp[0] = grid[0][0]  # dp[0]: 赋值/计算

    for i in range(m):  # 遍历: i 依次取 range(m) 中的每个值
        for j in range(n):  # 遍历: j 依次取 range(n) 中的每个值
            if i == 0 and j == 0:  # 判断: i == 0 and j == 0
                continue  # 跳过本轮迭代
            if j > 0:  # 判断: j > 0
                dp[j] = min(dp[j], dp[j-1]) + grid[i][j]  # dp[j]: 赋值/计算
            else:  # 否则 (以上条件都不满足时执行)
                dp[j] = dp[j] + grid[i][j]  # dp[j]: 赋值/计算

    return dp[n-1]  # 返回列表结果
```
 
### 120. 三角形最小路径和 (Triangle)
 
**题目描述：** 
给定一个三角形 `triangle`，找出自顶向下的最小路径和。 
每一步只能移动到下一行中相邻的结点上。相邻的结点在这里指的是 **下标** 与 **上一层结点下标** 相同或者等于 **上一层结点下标 + 1** 的两个结点。也就是说，如果正位于当前行的下标 `i`，那么下一步可以移动到下一行的下标 `i` 或 `i + 1`。 
**示例：** 
 
```python
输入：triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
输出：11
解释：自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。
```
 
**解题思路：** 
动态规划（自底向上）： 
- 从倒数第二行开始，向上计算
- `dp[j] = min(dp[j], dp[j+1]) + triangle[i][j]`
 
**复杂度分析：** 
- 时间复杂度：O(n²)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：三角形最小路径和
# 思路：动态规划（自底向上）：；从倒数第二行开始，向上计算dp[j] = min(dp[j], dp[j+1]) + triangle[i][j]
def minimumTotal(triangle):  # 定义函数 minimumTotal，接收参数: triangle
    n = len(triangle)  # n: 获取长度
    dp = triangle[n-1][:]  # dp: 赋值/计算

    for i in range(n-2, -1, -1):  # 遍历: i 依次取 range(n-2, -1, -1) 中的每个值
        for j in range(len(triangle[i])):  # 遍历: j 依次取 range(len(triangle[i])) 中的每个值
            dp[j] = min(dp[j], dp[j+1]) + triangle[i][j]  # dp[j]: 赋值/计算

    return dp[0]  # 返回列表结果
```
 
### 91. 解码方法 (Decode Ways)
 
**题目描述：** 
一条包含字母 `A-Z` 的消息通过以下映射进行了 **编码**： 
 
```python
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```
 
要 **解码** 已编码的消息，所有数字必须基于上述映射的方法，反向映射回字母（可能有多种方法）。例如，`"11106"` 可以映射为： 
- `"AAJF"`，将消息分组为 `(1 1 10 6)`
- `"KJF"`，将消息分组为 `(11 10 6)`
 
注意，消息不能分组为 `(1 11 06)`，因为 `"06"` 不能映射为 `"F"`，这是由于 `"6"` 和 `"06"` 在映射中并不等价。 
给你一个只含数字的 **非空** 字符串 `s`，请计算并返回 **解码方法的总数**。 
**示例：** 
 
```python
输入：s = "226"
输出：3
解释：它可以解码为 "BZ" (2 26), "VF" (22 6), 或者 "BBF" (2 2 6)。
```
 
**解题思路：** 
动态规划： 
- `dp[i]` 表示前 i 个字符的解码方法数
- 如果 `s[i-1] != '0'`，可以单独解码：`dp[i] += dp[i-1]`
- 如果 `s[i-2:i]` 在 10-26 之间，可以组合解码：`dp[i] += dp[i-2]`
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：解码方法
# 思路：动态规划：；dp[i] 表示前 i 个字符的解码方法数如果 s[i-1] != '0'，可以单独解码：dp[i] += dp[i-1]如果 s[i-2:i] 在 10-26 之间，可以组合解码：dp[i] += dp[i-2]
def numDecodings(s):  # 定义函数 numDecodings，接收参数: s
    if not s or s[0] == "hl-string">'0':  # 判断: not s or s[0] == '0'
        return 0  # 返回 0 (标志/空值)

    n = len(s)  # n: 获取长度
    prev2, prev1 = 1, 1  # 交换/多重赋值

    for i in range(1, n):  # 遍历: i 依次取 range(1, n) 中的每个值
        current = 0  # current: 计数器/下标初始化为0
        if s[i] != "hl-string">'0':  # 判断: s[i] != '0'
            current += prev1  # current 自反赋值+=
        if 10 <= int(s[i-1:i+1]) <= 26:  # 判断: 10 <= int(s[i-1:i+1]) <= 26
            current += prev2  # current 自反赋值+=
        prev2, prev1 = prev1, current  # 交换/多重赋值

    return prev1  # 返回计算结果
```
 
### 140. 单词拆分 II (Word Break II)
 
**题目描述：** 
给定一个字符串 `s` 和一个字符串字典 `wordDict`，在字符串中增加空格来构建一个句子，使得句子中所有的单词都在词典中。以任意顺序 **返回所有这些可能的句子**。 
**注意：** 词典中的同一个单词可能在分段中被重复使用多次。 
**示例：** 
 
```python
输入：s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]
输出：["cats and dog","cat sand dog"]
```
 
**解题思路：** 
回溯 + 记忆化： 
- 使用回溯法尝试所有可能的分割
- 使用记忆化避免重复计算
- 如果当前子串在字典中，继续递归
 
**复杂度分析：** 
- 时间复杂度：O(2^n)，最坏情况
- 空间复杂度：O(2^n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：单词拆分 II
# 思路：回溯 + 记忆化：；使用回溯法尝试所有可能的分割使用记忆化避免重复计算如果当前子串在字典中，继续递归
def wordBreak(s, wordDict):  # 定义函数 wordBreak，接收参数: s、wordDict
    wordSet = set(wordDict)  # wordSet: 赋值/计算
    memo = {}  # memo: 空字典

    def backtrack(start):  # 定义函数 backtrack，接收参数: start
        if start in memo:  # 判断: start in memo
            return memo[start]  # 返回列表结果

        if start == len(s):  # 判断: start == len(s)
            return [""]  # 返回列表结果

        result = []  # result: 空列表
        for end in range(start + 1, len(s) + 1):  # 遍历: end 依次取 range(start + 1, len(s) + 1) 中的每个值
            word = s[start:end]  # word: 赋值/计算
            if word in wordSet:  # 判断: word in wordSet
                for sentence in backtrack(end):  # 遍历: sentence 依次取 backtrack(end) 中的每个值
                    result.append(word + ("" if not sentence else " " + sentence))  # 追加到列表末尾

        memo[start] = result  # memo[start]: 赋值/计算
        return result  # 返回计算结果

    return backtrack(0)  # 返回计算结果
```
 
### 647. 回文子串 (Palindromic Substrings)
 
**题目描述：** 
给你一个字符串 `s`，请你统计并返回这个字符串中 **回文子串** 的数目。 
回文字符串是正着读和倒着读一样的字符串。 
子字符串是字符串中的由连续字符组成的一个序列。 
具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。 
**示例：** 
 
```python
输入：s = "abc"
输出：3
解释：三个回文子串: "a", "b", "c"
```
 
**解题思路：** 
中心扩展法： 
- 对于每个可能的中心（单个字符或两个字符之间），向两边扩展
- 统计所有回文子串的数量
 
**复杂度分析：** 
- 时间复杂度：O(n²)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：回文子串
# 思路：中心扩展法：；对于每个可能的中心（单个字符或两个字符之间），向两边扩展统计所有回文子串的数量
def countSubstrings(s):  # 定义函数 countSubstrings，接收参数: s
    n = len(s)  # n: 获取长度
    count = 0  # count: 计数器/下标初始化为0

    def expandAroundCenter(left, right):  # 定义函数 expandAroundCenter，接收参数: left、right
        nonlocal count
        while left >= 0 and right < n and s[left] == s[right]:  # 当 left >= 0 and right < n and s[ 时循环
            count += 1  # count 自反赋值+=
            left -= 1  # left 自反赋值-=
            right += 1  # right 自反赋值+=

    for i in range(n):  # 遍历: i 依次取 range(n) 中的每个值
        expandAroundCenter(i, i)
        expandAroundCenter(i, i + 1)

    return count  # 返回计算结果
```
 
### 32. 最长有效括号 (Longest Valid Parentheses)
 
**题目描述：** 
给你一个只包含 `'('` 和 `')'` 的字符串，找出最长有效（格式正确且连续）括号子串的长度。 
**示例：** 
 
```python
输入：s = ")()())"
输出：4
解释：最长有效括号子串是 "()()"
```
 
**解题思路：** 
动态规划： 
- `dp[i]` 表示以 s[i] 结尾的最长有效括号长度
- 如果 `s[i] == ')'` 且 `s[i-1] == '('`，`dp[i] = dp[i-2] + 2`
- 如果 `s[i] == ')'` 且 `s[i-1] == ')'` 且 `s[i-dp[i-1]-1] == '('`，`dp[i] = dp[i-1] + dp[i-dp[i-1]-2] + 2`
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：最长有效括号
# 思路：动态规划：；dp[i] 表示以 s[i] 结尾的最长有效括号长度如果 s[i] == ')' 且 s[i-1] == '('，dp[i] = dp[i-2] + 2如果 s[i] == ')' 且 s[i-1] == ')' 且 s[i-dp[i-1]-1] == '('，dp[i] = dp[i
def longestValidParentheses(s):  # 定义函数 longestValidParentheses，接收参数: s
    n = len(s)  # n: 获取长度
    dp = [0] * n  # dp: 乘积计算
    max_len = 0  # max_len: 计数器/下标初始化为0

    for i in range(1, n):  # 遍历: i 依次取 range(1, n) 中的每个值
        if s[i] == "hl-string">')':  # 判断: s[i] == ')'
            if s[i-1] == "hl-string">'(':  # 判断: s[i-1] == '('
                dp[i] = (dp[i-2] if i >= 2 else 0) + 2  # dp[i]: 赋值/计算
            elif i - dp[i-1] > 0 and s[i - dp[i-1] - 1] == "hl-string">'(':  # 否则如果
                dp[i] = dp[i-1] + (dp[i - dp[i-1] - 2] if i - dp[i-1] >= 2 else 0) + 2  # dp[i]: 赋值/计算
            max_len = max(max_len, dp[i])  # max_len: 赋值/计算

    return max_len  # 返回计算结果
```
 
### 312. 戳气球 (Burst Balloons)
 
**题目描述：** 
有 `n` 个气球，编号为 `0` 到 `n - 1`，每个气球上都标有一个数字，这些数字存在数组 `nums` 中。 
现在要求你戳破所有的气球。戳破第 `i` 个气球，你可以获得 `nums[i - 1] * nums[i] * nums[i + 1]` 枚硬币。这里的 `i - 1` 和 `i + 1` 代表和 `i` 相邻的两个气球的序号。如果 `i - 1` 或 `i + 1` 超出了数组的边界，那么就当它是一个数字为 `1` 的气球。 
求所能获得硬币的最大数量。 
**示例：** 
 
```python
输入：nums = [3,1,5,8]
输出：167
解释：
nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
coins =  3*1*5    +   3*5*8   +  1*3*8  + 1*8*1 = 15 + 120 + 24 + 8 = 167
```
 
**解题思路：** 
区间DP： 
- 在数组两端添加1
- `dp[i][j]` 表示戳破区间 (i, j) 内所有气球能获得的最大硬币数
- 枚举最后一个戳破的气球 k，`dp[i][j] = max(dp[i][k] + dp[k][j] + nums[i] * nums[k] * nums[j])`
 
**复杂度分析：** 
- 时间复杂度：O(n³)
- 空间复杂度：O(n²)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：戳气球
# 思路：区间DP：；在数组两端添加1dp[i][j] 表示戳破区间 (i, j) 内所有气球能获得的最大硬币数枚举最后一个戳破的气球 k，dp[i][j] = max(dp[i][k] + dp[k][j] + nums[i] * nums[k] * nums[j])
def maxCoins(nums):  # 定义函数 maxCoins，接收参数: nums
    nums = [1] + nums + [1]  # nums: 赋值/计算
    n = len(nums)  # n: 获取长度
    dp = [[0] * n for _ in range(n)]  # dp: 列表推导式

    for length in range(2, n):  # 遍历: length 依次取 range(2, n) 中的每个值
        for i in range(n - length):  # 遍历: i 依次取 range(n - length) 中的每个值
            j = i + length  # j: 赋值/计算
            for k in range(i + 1, j):  # 遍历: k 依次取 range(i + 1, j) 中的每个值
                dp[i][j] = max(dp[i][j],
                               dp[i][k] + dp[k][j] + nums[i] * nums[k] * nums[j])

    return dp[0][n-1]  # 返回列表结果
```
 
### 213. 打家劫舍 II (House Robber II)
 
**题目描述：** 
你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都 **围成一圈**，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。 
给定一个代表每个房屋存放金额的非负整数数组，计算你 **在不触动警报装置的情况下**，今晚能够偷窃到的最高金额。 
**示例：** 
 
```python
输入：nums = [2,3,2]
输出：3
解释：你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。
```
 
**解题思路：** 
将问题分解为两个子问题： 
- 不偷第一间房：计算 nums[1:] 的最大值
- 不偷最后一间房：计算 nums[:-1] 的最大值
- 取两者的最大值
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：打家劫舍 II
# 思路：将问题分解为两个子问题：；不偷第一间房：计算 nums[1:] 的最大值不偷最后一间房：计算 nums[:-1] 的最大值取两者的最大值
def rob(nums):  # 定义函数 rob，接收参数: nums
    if len(nums) == 1:  # 判断: len(nums) == 1
        return nums[0]  # 返回列表结果

    def robRange(start, end):  # 定义函数 robRange，接收参数: start、end
        prev2, prev1 = 0, 0  # 交换/多重赋值
        for i in range(start, end):  # 遍历: i 依次取 range(start, end) 中的每个值
            current = max(prev1, prev2 + nums[i])  # current: 赋值/计算
            prev2, prev1 = prev1, current  # 交换/多重赋值
        return prev1  # 返回计算结果

    return max(robRange(0, len(nums) - 1), robRange(1, len(nums)))  # 返回计算结果
```
 
### 337. 打家劫舍 III (House Robber III)
 
**题目描述：** 
小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 `root`。 
除了 `root` 之外，每栋房子有且只有一个”父”房子与之相连。一番侦察之后，聪明的小偷意识到”这个地方的所有房屋的排列类似于一棵二叉树”。如果 **两个直接相连的房子在同一天晚上被打劫**，房屋将自动报警。 
给定二叉树的 `root`。返回 **在不触动警报的情况下**，小偷能够盗取的最高金额。 
**示例：** 
 
```python
输入：root = [3,2,3,null,3,null,1]
输出：7
解释：小偷一晚能够盗取的最高金额 3 + 3 + 1 = 7
```
 
**解题思路：** 
树形DP： 
- 对于每个节点，返回两个值：[不偷该节点的最大值, 偷该节点的最大值]
- 如果偷当前节点，则不能偷子节点
- 如果不偷当前节点，可以偷或不偷子节点
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(h)，h 是树的高度
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：打家劫舍 III
# 思路：树形DP：；对于每个节点，返回两个值：[不偷该节点的最大值, 偷该节点的最大值]如果偷当前节点，则不能偷子节点如果不偷当前节点，可以偷或不偷子节点
def rob(root):  # 定义函数 rob，接收参数: root
    def dfs(node):  # 定义函数 dfs，接收参数: node
        if not node:  # 判断: not node
            return [0, 0]  # 返回列表结果

        left = dfs(node.left)  # left: 赋值/计算
        right = dfs(node.right)  # right: 赋值/计算

        rob_current = node.val + left[0] + right[0]  # rob_current: 赋值/计算
        not_rob_current = max(left) + max(right)  # not_rob_current: 赋值/计算

        return [not_rob_current, rob_current]  # 返回列表结果

    return max(dfs(root))  # 返回计算结果
```
 
### 132. 分割回文串 II (Palindrome Partitioning II)
 
**题目描述：** 
给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是回文。 
返回符合要求的 **最少分割次数**。 
**示例：** 
 
```python
输入：s = "aab"
输出：1
解释：只需一次分割就可将 s 分割成 ["aa","b"] 这样两个回文子串。
```
 
**解题思路：** 
动态规划： 
- 先用DP判断所有子串是否为回文
- `dp[i]` 表示前 i 个字符的最少分割次数
- 如果 `s[j:i+1]` 是回文，`dp[i] = min(dp[i], dp[j-1] + 1)`
 
**复杂度分析：** 
- 时间复杂度：O(n²)
- 空间复杂度：O(n²)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：分割回文串 II
# 思路：动态规划：；先用DP判断所有子串是否为回文dp[i] 表示前 i 个字符的最少分割次数如果 s[j:i+1] 是回文，dp[i] = min(dp[i], dp[j-1] + 1)
def minCut(s):  # 定义函数 minCut，接收参数: s
    n = len(s)  # n: 获取长度
    is_palindrome = [[False] * n for _ in range(n)]  # is_palindrome: 列表推导式

    for i in range(n):  # 遍历: i 依次取 range(n) 中的每个值
        is_palindrome[i][i] = True

    for length in range(2, n + 1):  # 遍历: length 依次取 range(2, n + 1) 中的每个值
        for i in range(n - length + 1):  # 遍历: i 依次取 range(n - length + 1) 中的每个值
            j = i + length - 1  # j: 赋值/计算
            if s[i] == s[j]:  # 判断: s[i] == s[j]
                if length == 2:  # 判断: length == 2
                    is_palindrome[i][j] = True
                else:  # 否则 (以上条件都不满足时执行)
                    is_palindrome[i][j] = is_palindrome[i+1][j-1]

    dp = [0] * n  # dp: 乘积计算
    for i in range(n):  # 遍历: i 依次取 range(n) 中的每个值
        if is_palindrome[0][i]:  # 判断: is_palindrome[0][i]
            dp[i] = 0  # dp[i]: 计数器/下标初始化为0
        else:  # 否则 (以上条件都不满足时执行)
            dp[i] = i  # dp[i]: 赋值/计算
            for j in range(1, i + 1):  # 遍历: j 依次取 range(1, i + 1) 中的每个值
                if is_palindrome[j][i]:  # 判断: is_palindrome[j][i]
                    dp[i] = min(dp[i], dp[j-1] + 1)  # dp[i]: 赋值/计算

    return dp[n-1]  # 返回列表结果
```
 
### 516. 最长回文子序列 (Longest Palindromic Subsequence)
 
**题目描述：** 
给你一个字符串 `s`，找出其中最长的回文子序列，并返回该序列的长度。 
子序列定义为：不改变剩余字符顺序的情况下，删除某些字符或者不删除任何字符形成的一个序列。 
**示例：** 
 
```python
输入：s = "bbbab"
输出：4
解释：一个可能的最长回文子序列为 "bbbb"。
```
 
**解题思路：** 
动态规划： 
- `dp[i][j]` 表示 s[i:j+1] 的最长回文子序列长度
- 如果 `s[i] == s[j]`，`dp[i][j] = dp[i+1][j-1] + 2`
- 否则，`dp[i][j] = max(dp[i+1][j], dp[i][j-1])`
 
**复杂度分析：** 
- 时间复杂度：O(n²)
- 空间复杂度：O(n²)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：最长回文子序列
# 思路：动态规划：；dp[i][j] 表示 s[i:j+1] 的最长回文子序列长度如果 s[i] == s[j]，dp[i][j] = dp[i+1][j-1] + 2否则，dp[i][j] = max(dp[i+1][j], dp[i][j-1])
def longestPalindromeSubseq(s):  # 定义函数 longestPalindromeSubseq，接收参数: s
    n = len(s)  # n: 获取长度
    dp = [[0] * n for _ in range(n)]  # dp: 列表推导式

    for i in range(n):  # 遍历: i 依次取 range(n) 中的每个值
        dp[i][i] = 1

    for length in range(2, n + 1):  # 遍历: length 依次取 range(2, n + 1) 中的每个值
        for i in range(n - length + 1):  # 遍历: i 依次取 range(n - length + 1) 中的每个值
            j = i + length - 1  # j: 赋值/计算
            if s[i] == s[j]:  # 判断: s[i] == s[j]
                dp[i][j] = dp[i+1][j-1] + 2
            else:  # 否则 (以上条件都不满足时执行)
                dp[i][j] = max(dp[i+1][j], dp[i][j-1])

    return dp[0][n-1]  # 返回列表结果
```
 
### 354. 俄罗斯套娃信封问题 (Russian Doll Envelopes)
 
**题目描述：** 
给你一个二维整数数组 `envelopes`，其中 `envelopes[i] = [wi, hi]`，表示第 `i` 个信封的宽度和高度。 
当另一个信封的宽度和高度都比这个信封大的时候，这个信封就可以放进另一个信封里，如同俄罗斯套娃一样。 
请计算 **最多能有多少个** 信封能组成一组”俄罗斯套娃”信封（即可以把一个信封放到另一个信封里面）。 
**注意：** 不允许旋转信封。 
**示例：** 
 
```python
输入：envelopes = [[5,4],[6,4],[6,7],[2,3]]
输出：3
解释：最多信封的个数为 3, 组合为: [2,3] => [5,4] => [6,7]。
```
 
**解题思路：** 
排序 + 最长递增子序列： 
- 按宽度升序排序，宽度相同时按高度降序排序
- 对高度数组求最长递增子序列
 
**复杂度分析：** 
- 时间复杂度：O(n log n)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：俄罗斯套娃信封问题
# 思路：排序 + 最长递增子序列：；按宽度升序排序，宽度相同时按高度降序排序对高度数组求最长递增子序列
def maxEnvelopes(envelopes):  # 定义函数 maxEnvelopes，接收参数: envelopes
    envelopes.sort(key=lambda x: (x[0], -x[1]))  # 原地排序

    heights = [env[1] for env in envelopes]  # heights: 列表推导式
    dp = []  # dp: 空列表

    for h in heights:  # 遍历: h 依次取 heights 中的每个值
        left, right = 0, len(dp)  # 交换/多重赋值
        while left < right:  # 当 left < right 时循环
            mid = (left + right) // 2  # mid: 赋值/计算
            if dp[mid] < h:  # 判断: dp[mid] < h
                left = mid + 1  # left: 赋值/计算
            else:  # 否则 (以上条件都不满足时执行)
                right = mid  # right: 赋值/计算

        if left == len(dp):  # 判断: left == len(dp)
            dp.append(h)  # 追加到列表末尾
        else:  # 否则 (以上条件都不满足时执行)
            dp[left] = h  # dp[left]: 赋值/计算

    return len(dp)  # 返回计算结果
```
 
### 300. 最长递增子序列 (Longest Increasing Subsequence)
 
**题目描述：** 
给你一个整数数组 `nums`，找到其中最长严格递增子序列的长度。 
**子序列** 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，`[3,6,2,7]` 是数组 `[0,3,1,6,2,2,7]` 的子序列。 
**示例：** 
 
```python
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,18]，因此长度为 4。
```
 
**解题思路：** 
动态规划 + 二分查找： 
- 使用数组 `dp` 存储长度为 i+1 的递增子序列的最小末尾元素
- 对于每个元素，使用二分查找找到应该插入的位置
- 如果元素大于所有已有元素，追加到数组末尾
 
**复杂度分析：** 
- 时间复杂度：O(n log n)
- 空间复杂度：O(n)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：最长递增子序列
# 思路：动态规划 + 二分查找：；使用数组 dp 存储长度为 i+1 的递增子序列的最小末尾元素对于每个元素，使用二分查找找到应该插入的位置如果元素大于所有已有元素，追加到数组末尾
def lengthOfLIS(nums):  # 定义函数 lengthOfLIS，接收参数: nums
    dp = []  # dp: 空列表

    for num in nums:  # 遍历: num 依次取 nums 中的每个值
        left, right = 0, len(dp)  # 交换/多重赋值
        while left < right:  # 当 left < right 时循环
            mid = (left + right) // 2  # mid: 赋值/计算
            if dp[mid] < num:  # 判断: dp[mid] < num
                left = mid + 1  # left: 赋值/计算
            else:  # 否则 (以上条件都不满足时执行)
                right = mid  # right: 赋值/计算

        if left == len(dp):  # 判断: left == len(dp)
            dp.append(num)  # 追加到列表末尾
        else:  # 否则 (以上条件都不满足时执行)
            dp[left] = num  # dp[left]: 赋值/计算

    return len(dp)  # 返回计算结果
```
 
### 309. 最佳买卖股票时机含冷冻期 (Best Time to Buy and Sell Stock with Cooldown)
 
**题目描述：** 
给定一个整数数组 `prices`，其中第 `prices[i]` 表示第 `i` 天股票的价格。 
设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）: 
- 卖出股票后，你无法在第二天买入股票（即冷冻期为 1 天）。
 
**注意：** 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。 
**示例：** 
 
```python
输入：prices = [1,2,3,0,2]
输出：3
解释：对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
```
 
**解题思路：** 
动态规划： 
- `dp[i][0]` 表示第 i 天持有股票的最大利润
- `dp[i][1]` 表示第 i 天不持有股票（处于冷冻期）的最大利润
- `dp[i][2]` 表示第 i 天不持有股票（不处于冷冻期）的最大利润
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：最佳买卖股票时机含冷冻期
# 思路：动态规划：；dp[i][0] 表示第 i 天持有股票的最大利润dp[i][1] 表示第 i 天不持有股票（处于冷冻期）的最大利润dp[i][2] 表示第 i 天不持有股票（不处于冷冻期）的最大利润
def maxProfit(prices):  # 定义函数 maxProfit，接收参数: prices
    hold = float("hl-string">'-inf')  # hold: 赋值/计算
    frozen = 0  # frozen: 计数器/下标初始化为0
    free = 0  # free: 计数器/下标初始化为0

    for price in prices:  # 遍历: price 依次取 prices 中的每个值
        hold, frozen, free = max(hold, free - price), hold + price, max(free, frozen)  # 交换/多重赋值

    return max(frozen, free)  # 返回计算结果
```
 
### 714. 买卖股票的最佳时机含手续费 (Best Time to Buy and Sell Stock with Transaction Fee)
 
**题目描述：** 
给定一个整数数组 `prices`，其中 `prices[i]` 表示第 `i` 天的股票价格；整数 `fee` 代表了交易股票的手续费用。 
你可以无限次地完成交易，但是你每笔交易都需要付手续费。如果你已经购买了一个股票，在卖出它之前你就不能再继续购买股票了。 
返回获得利润的最大值。 
**注意：** 这里的一笔交易指买入持有并卖出股票的整个过程，每笔交易你只需要为支付一次手续费。 
**示例：** 
 
```python
输入：prices = [1, 3, 2, 8, 4, 9], fee = 2
输出：8
解释：能够达到的最大利润:  
在此处买入 prices[0] = 1
在此处卖出 prices[3] = 8
在此处买入 prices[4] = 4
在此处卖出 prices[5] = 9
总利润: ((8 - 1) - 2) + ((9 - 4) - 2) = 8
```
 
**解题思路：** 
动态规划： 
- `hold` 表示持有股票的最大利润
- `sold` 表示不持有股票的最大利润
- 买入时扣除手续费，卖出时扣除手续费（或只在卖出时扣除）
 
**复杂度分析：** 
- 时间复杂度：O(n)
- 空间复杂度：O(1)
 
**Python 解答（含详细注释）：** 
 
```python
# 目的：买卖股票的最佳时机含手续费
# 思路：动态规划：；hold 表示持有股票的最大利润sold 表示不持有股票的最大利润买入时扣除手续费，卖出时扣除手续费（或只在卖出时扣除）
def maxProfit(prices, fee):  # 定义函数 maxProfit，接收参数: prices、fee
    hold = -prices[0]  # hold: 赋值/计算
    sold = 0  # sold: 计数器/下标初始化为0

    for i in range(1, len(prices)):  # 遍历: i 依次取 range(1, len(prices)) 中的每个值
        hold = max(hold, sold - prices[i])  # hold: 赋值/计算
        sold = max(sold, hold + prices[i] - fee)  # sold: 赋值/计算

    return sold  # 返回计算结果
```
 整理自 yukinoshitasherry.github.io/Leetcode100 · 排版日期 2026-05-27