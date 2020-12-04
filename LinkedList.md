## LinkedList 

### 206.  Reverse Linked List 
Reverse a singly linked list

- Iterative
```
Time Complexity: O(n)
Space Complexity: O(1)
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    
    ListNode pre = null;
    ListNode cur = head;
    while (cur != null) {
        ListNode third = cur.next;
        cur.next = pre;
        pre = cur;
        cur = third;
    }
    return pre;
}
```

- Recursive
```
Time Complexity: O(n)
Space Complexity: O(n)
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    ListNode firstN = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    
    return firstN;
}
```

### 92. Reverse Linked List II
Reverse a linked list from position m to n. Do it in one-pass.
```
Input: 1->2->3->4->5->NULL, m = 2, n = 4
Output: 1->4->3->2->5->NULL
```

- Iterative
```
- edge case: m = 1
Time Complexity: O(n)
Space Complexity: O(1)
public ListNode reverseBetween(ListNode head, int m, int n) {
    if (head == null || head.next == null) {
        return head;
    }
    
    ListNode leftHead = null;
    ListNode leftTail = head;
    while (m > 1) {
        leftHead = leftTail;
        leftTail = leftTail.next;
    }
    
    ListNode cur = leftTail;
    ListNode pre = null;
    for (int i = 0; i <= n - m; i++) {
        ListNode third = cur.next;
        cur.next = pre;
        pre = cur;
        cur = third;
    }
    
    leftTail.next = cur;
    if (leftHead == null) return pre;
    else {
        leftHead.next = pre;
        return head;
    }
}
```

### 160. Intersection of Two Linked Lists
Find the node at which the intersection of two singly linked lists begins.

- Two Pointers
```
Time Complexity: O(n+m)
Space Complexity: O(1)
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    if (headA == null || headB == null) return null;
    ListNode a = headA;
    ListNode b = headB;
    while (a != b) {
        a = a == null ? headB : a.next;
        b = b == null ? headA : b.next;
    }
    
    return a;
}
```

### 203. Remove Linked List Elements (易错)
Remove all elements from a linked list of integers that have value val.

- Sentinel Node 
```
public ListNode removeElements(ListNode head, int val) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        ListNode pre = dummy;
        ListNode cur = dummy.next;
        
        while (pre != null && cur != null) {
            if (cur.val == val) {
                pre.next = cur.next;
            } else {
                pre = pre.next;
            }
            cur = cur.next;
        }
        
        return dummy.next;
    }
```

### 19. Remove Nth Node From End of List (注意认真审题 + 易错)
Given the head of a linked list, remove the nth node from the end of the list and return its head.
```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

- One Pass
```
- Time complexity : O(L)
The algorithm makes one traversal of the list of L nodes. Therefore time complexity is O(L).
- Space complexity : O(1)O(1).
We only used constant extra space.

public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode last = head;
        while (n > 0) {
            last = last.next;
            n--;
        }
        
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode pre = dummy;
        
        while (last != null) {
            pre = pre.next;
            last = last.next;
        }
        
        pre.next = pre.next.next;
        return dummy.next;
    }
```

### 445. Add Two Numbers II
Given two non-empty linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.
```
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
```
