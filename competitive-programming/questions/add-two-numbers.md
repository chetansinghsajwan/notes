# Add Two Numbers

leetcode-id:: 2
leetcode-link:: https://leetcode.com/problems/add-two-numbers
topics:: linked-list, math, recursion
difficulty:: medium

## Description

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

###### Example 1

**Input:** l1 = [2,4,3], l2 = [5,6,4]
**Output:** [7,0,8]
**Explanation:** 342 + 465 = 807.

###### Example 2

**Input:** l1 = [0], l2 = [0]
**Output:** [0]

###### Example 3

**Input:** l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
**Output:** [8,9,9,9,0,0,0,1]

###### Constraints

- The number of nodes in each linked list is in the range `[1, 100]`.
- `0 <= Node.val <= 9`
- It is guaranteed that the list represents a number that does not have leading zeros.

## Solution

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2)
    {
        int carry = 0;
        ListNode* first = new ListNode();
        ListNode* last = first;

        while (l1 != nullptr || l2 != nullptr || carry > 0)
        {
            int v1 = 0;
            if (l1 != nullptr)
            {
                v1 = l1->val;
                l1 = l1->next;
            }

            int v2 = 0;
            if (l2 != nullptr)
            {
                v2 = l2->val;
                l2 = l2->next;
            }

            int sum = v1 + v2 + carry;
            int digit = sum % 10;
            carry = sum / 10;

            last->next = new ListNode(digit);
            last = last->next;
        }

        return first->next;
    }
};
```