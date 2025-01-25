# Median of Two Sorted Arrays

leetcode-id:: 4
leetcode-link:: https://leetcode.com/problems/median-of-two-sorted-arrays
difficulty:: hard
topics:: array, binary-search, divide-and-conquer

## Description

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

###### Example 1

**Input:** nums1 = [1,3], nums2 = [2]
**Output:** 2.00000
**Explanation:** merged array = [1,2,3] and median is 2.

###### Example 2

**Input:** nums1 = [1,2], nums2 = [3,4]
**Output:** 2.50000

###### Explanation

merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.

###### Constraints

- `nums1.length == m`
- `nums2.length == n`
- `0 <= m <= 1000`
- `0 <= n <= 1000`
- `1 <= m + n <= 2000`
- `-106 <= nums1[i], nums2[i] <= 106`

## Solution

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2)
    {
        vector<int> nums;
        nums.reserve(nums1.size() + nums2.size());

        nums.insert(nums.end(), nums1.begin(), nums1.end());
        nums.insert(nums.end(), nums2.begin(), nums2.end());

        sort(nums.begin(), nums.end());
        int size = nums.size();
        int mid = size / 2;

        if (size % 2 == 0)
            return (nums[mid - 1] + nums[mid]) / 2.0;
        else return nums[mid];
    }
};
```