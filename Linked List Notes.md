# Linked List Notes

## Dummy Node

Dummy Node is used as the placeholder to avoid the null pointer issue of the head, thus improving the readability of the code.

Every time when you want to create a new Linked List, you could use dummy node to simplify the boundary case of head node.

## Double Pointers

* Example -  Merge Two Sorted Lists

  > https://leetcode.com/problems/merge-two-sorted-lists/
  >
  > You are given the heads of two sorted linked lists `list1` and `list2`.
  >
  > Merge the two lists in a one **sorted** list. The list should be made by splicing together the nodes of the first two lists.
  >
  > Return *the head of the merged linked list*.

* Solution

  ```java
  //Time: O(N)   Space: O(1)
  ListNode mergeTwoLists(ListNode l1, ListNode l2) {
      // Dummy Node
      ListNode dummy = new ListNode(-1), p = dummy;
      ListNode p1 = l1, p2 = l2;
      
      while (p1 != null && p2 != null) {
          if (p1.val > p2.val) {
              p.next = p2;
              p2 = p2.next;
          } else {
              p.next = p1;
              p1 = p1.next;
          }
          p = p.next;
      }
      
      if (p1 != null) {
          p.next = p1;
      }
      
      if (p2 != null) {
          p.next = p2;
      }
      
      return dummy.next;
  }
  ```

  ![img](https://raw.githubusercontent.com/Hangming-Ye/All-Pic/main/pic/202301032004975.gif)



* Example - 86. Partition List

  > https://leetcode.com/problems/partition-list/
  >
  > Given the `head` of a linked list and a value `x`, partition it such that all nodes **less than** `x` come before nodes **greater than or equal** to `x`.
  >
  > You should **preserve** the original relative order of the nodes in each of the two partitions.

* Solution

  Divide the Linked List into two Linked Lists, then contact them together.

  ```java
  ListNode partition(ListNode head, int x) {
      ListNode dummy1 = new ListNode(-1);
      ListNode dummy2 = new ListNode(-1);
      ListNode p1 = dummy1, p2 = dummy2;
      ListNode p = head;
      while (p != null) {
          if (p.val >= x) {
              p2.next = p;
              p2 = p2.next;
          } else {
              p1.next = p;
              p1 = p1.next;
          }
          ListNode temp = p.next;
          p.next = null;
          p = temp;
      }
      p1.next = dummy2.next;
      return dummy1.next;
  }
  ```

  

* Example - 23.Merge k Sorted Lists

  Using heap to sort the node of all Linked List, pop the min, move to the next and add it to the heap until heap is empty.

  ```java
  public ListNode mergeKLists(ListNode[] lists) {
      if(lists.length==0){
          return null;
      }
      ListNode dummy = new ListNode();
      ListNode p = dummy;
      Comparator<ListNode> cmp = new Comparator<ListNode>() {
          @Override
          public int compare(ListNode n1, ListNode n2) {
              return n1.val - n2.val;
          }
      };
      PriorityQueue<ListNode> pq = new PriorityQueue<>(lists.length,cmp);
      for (ListNode tmp: lists){
          if(tmp!=null){
              pq.add(tmp);
          }
      }
      while(!pq.isEmpty()){
          ListNode min = pq.poll();
          p.next = min;
          if(min.next!=null){
              pq.add(min.next);
          }
          p = p.next;
      }
      return dummy.next;
  }
  ```

  
  
  Divide and Conquer
  
  ```java
  public ListNode mergeKLists(ListNode[] lists){
  
      return lists.length==0 ? null : mergeLists(lists, 0, lists.length-1);
  }
  
  ListNode mergeLists(ListNode[] lists, int start, int end){
      if(start > end)
          return null;
      if(start == end)
          return lists[start];
      int m = (end+start)/2;
  
      ListNode node1 = mergeLists(lists, start, m);
      ListNode node2 = mergeLists(lists, m+1, end);
      return mergeSort(node1, node2);
  }
  
  ListNode mergeSort(ListNode node1, ListNode node2){
      ListNode temp =  new ListNode();
      ListNode ans = temp;
  
      while(node1 != null && node2 != null){
          if(node1.val < node2.val){
              temp.next = node1;
              node1 = node1.next;
          } else {
              temp.next = node2;
              node2 = node2.next;
          }
          temp = temp.next;
      }
  
      if(node1 != null) 
          temp.next = node1;
      if(node2 != null) 
          temp.next = node2;
      return ans.next;
  }
  ```



## Fast and Slow Pointer

* Example - 19. Remove Nth Node From End of List

  ```java
  public ListNode removeNthFromEnd(ListNode head, int n) {
      ListNode dummy = new ListNode();
      dummy.next = head;
      ListNode p1 = dummy;
      for(int i = 0; i <= n; i++){
          p1 = p1.next;
      }
      ListNode p2 = dummy;
      while(p1!=null){
          p1 = p1.next;
          p2 = p2.next;
      }
      p2.next = p2.next.next;
      return dummy.next;
  }
  ```

  

* Example - 876.Middle of the Linked List

  ```java
  public ListNode middleNode(ListNode head) {
      ListNode slow = head, fast = head;
      while(fast!=null && fast.next!=null){
          slow = slow.next;
          fast = fast.next.next;
      }
      return slow;
  }
  ```

  

## Cycle Determine

* Example -  160. Intersection of Two Linked Lists

  ```java
  ListNode getIntersectionNode(ListNode headA, ListNode headB) {
      ListNode p1 = headA, p2 = headB;
      while (p1 != p2) {
          if (p1 == null) p1 = headB;
          else            p1 = p1.next;
          if (p2 == null) p2 = headA;
          else            p2 = p2.next;
      }
      return p1;
  }
  ```

* Example - 141.Linked List Cycle

  ```JAVA
  public boolean hasCycle(ListNode head) {
      ListNode fast = head, slow = head;
      while(fast!=null && fast.next!=null){
          fast = fast.next.next;
          slow = slow.next;
          if(fast==slow){
              return true;
          }
      }
      return false;
  }
  ```

* Example - 142. Linked List Cycle II

  ```java
  public ListNode detectCycle(ListNode head) {
      ListNode fast = head, slow = head;
      while(fast!=null && fast.next!=null){
          fast = fast.next.next;
          slow = slow.next;
          if(fast==slow){
              break;
          }
      }
      if(fast==null||fast.next==null){
          return null;
      }
      slow = head;
      while(fast!=slow){
          fast = fast.next;
          slow = slow.next;
      }
      return slow;
  }
  ```

  

