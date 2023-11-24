---
Topics:
  - Array
Coding Ninja: 6682399
---
## Description
You are given an array _**'a'**_ of size _**'n'**_ and an integer _**'k'**_. Find the length of the longest subarray of 'a' whose sum is equal to 'k'.
### **Expected Time Complexity**
```
The expected time complexity is O(n).
```
### **Constraints**
```
1 <= 'n', 'k' <= 10 ^ 3
0 <= 'a[i]' <= 10 ^ 9
Time Limit: 1-second
```
### Example
```
Input: ‘n’ = 7 ‘k’ = 3
‘a’ = [1, 2, 3, 1, 1, 1, 1]
Output: 3
Explanation: Subarrays whose sum = ‘3’ are:
[1, 2], [3], [1, 1, 1] and [1, 1, 1]
Here, the length of the longest subarray is 3, which is our final answer.
```
### **Sample Input 1 :**
```
7 3
1 2 3 1 1 1 1
```
### **Sample Output 1 :**
```
3
```
### **Explanation Of Sample Input 1 :**
```
Subarrays whose sum = ‘3’ are:
[1, 2], [3], [1, 1, 1] and [1, 1, 1]
Here, the length of the longest subarray is 3, which is our final answer.
```
### **Sample Input 2 :**
```
4 2
1 2 1 3
```
### **Sample Output 2 :**
```
1
```
### **Sample Input 3 :**
```
5 2
2 2 4 1 2
```
### **Sample Output 3 :**
```
1
```
## Solution
### Optimal
```
int longestSubarrayWithSumK(vector<int> arr, long long k)
{
    int left = 0;
    int right = 0;
    int max = 0;
    long long sum = 0;
    while (right < arr.size())
    {
        sum += arr[right];
        right++;
        while(sum > k)
        {
            sum -= arr[left];
            left++;
        }
        if (sum == k)
        {
            int count = right - left;
            if (count > max)
                max = count;
        }
    }
    return max;
}
```