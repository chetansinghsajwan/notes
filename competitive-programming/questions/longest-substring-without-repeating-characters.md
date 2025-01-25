# Longest Substring Without Repeating Characters

leetcode-id:: 3
leetcode-link:: https://leetcode.com/problems/longest-substring-without-repeating-characters
topics:: hash-table, string, sliding-window
difficulty:: medium

## Description

Given a string `s`, find the length of the longest substring without repeating characters.

###### Example 1

**Input:** s = "abcabcbb"
**Output:** 3
**Explanation:** The answer is "abc", with the length of 3.

###### Example 2

**Input:** s = "bbbbb"
**Output:** 1
**Explanation:** The answer is "b", with the length of 1.

###### Example 3

**Input:** s = "pwwkew"
**Output:** 3

###### Explanation

The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

###### Constraints

- `0 <= s.length <= 5 * 104`
- `s` consists of English letters, digits, symbols and spaces.

## Solution

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s)
    {
        char map[128] = { 0 };

        int max = 0;
        int front = 0;
        int back = 0;
        int size = s.size();
        for (; back < size; back++)
        {
            char backChar = s[back];
            map[backChar]++;

            if (map[backChar] > 1)
            {
                for (; front < back; front++)
                {
                    char frontChar = s[front];
                    map[frontChar]--;

                    if (frontChar == backChar)
                    {
                        front++;
                        break;
                    }
                }

                continue;
            }

            int diff = back - front + 1;
            if (diff > max)
                max = diff;
        }

        return max;
    }
};
```