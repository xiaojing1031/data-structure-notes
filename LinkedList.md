## LinkedList 

### 206.Reverse Linked List 
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

### 92.Reverse Linked List II
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

### 


