You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

# 思路

```java
public static ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode l1_index = null;
        ListNode l2_index = null;
        ListNode result_index = null;
        int currentCarray = 0;
        ListNode result = null;
        result_index = result;
        if (l1 != null)
            l1_index = l1;
        if (l2 != null)
            l2_index = l2;
        while (l1_index != null && l2_index != null) {
            int temp = l1_index.val + l2_index.val + currentCarray;
            if (temp >= 10) {
                temp = temp - 10;
                currentCarray = 1;
            } else {
                currentCarray = 0;
            }
            if (result == null) {
                result = new ListNode(temp);
                result_index = result;
            } else {
                result_index.next = new ListNode(temp);
                result_index = result_index.next;
            }
            l1_index = l1_index.next;
            l2_index = l2_index.next;
        }
        if (l1_index != null) {
            while (l1_index != null) {
                int temp = l1_index.val + currentCarray;
                if (temp >= 10) {
                    temp = temp - 10;
                    currentCarray = 1;
                } else {
                    currentCarray = 0;
                }
                result_index.next = new ListNode(temp);
                result_index = result_index.next;
                l1_index = l1_index.next;
            }
        }
        if (l2_index != null) {
            while (l2_index != null) {
                int temp = l2_index.val + currentCarray;
                if (temp >= 10) {
                    temp = temp - 10;
                    currentCarray = 1;
                } else {
                    currentCarray = 0;
                }
                result_index.next = new ListNode(temp);
                result_index = result_index.next;
                l2_index = l2_index.next;
            }

        }
        if (currentCarray != 0) {
            result_index.next = new ListNode(1);
        }
        return result;
    }
```

我的代码如上，思路是先考虑两个list长度一样的情况，listA与listB每一位（0～9）逐步相加，每一位的结果是该位两个数以及上一位的进位一共三个数相加，而进位只能取0或者1，因此我们只要从到右依次相加，并用一个变量记录进位即可，最后考虑一种特殊情况，假设listA为12，listB为18，则结果为201，即如果list没一位都处理完毕之后还需考虑进位是否为1，如果为1，则需要在结果后面添一个1。leetcode给出的代码如下，其实思路是一样的，它写的比较简练，**重点学习这种精练的写法**：

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummyHead = new ListNode(0);
    ListNode p = l1, q = l2, curr = dummyHead;
    int carry = 0;
    while (p != null || q != null) {
        int x = (p != null) ? p.val : 0;
        int y = (q != null) ? q.val : 0;
        int sum = carry + x + y;
        carry = sum / 10;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
        if (p != null) p = p.next;
        if (q != null) q = q.next;
    }
    if (carry > 0) {
        curr.next = new ListNode(carry);
    }
    return dummyHead.next;
}
```





