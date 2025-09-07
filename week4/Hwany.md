# Reverse LinkedList

## Solution

```java

class Solution {
    public int ReverseLinkedList(ListNode head) {

         ListNode prev = null;
        ListNode curr = head;

        while (curr != null) {
            ListNode nextTemp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextTemp;
        }

        return prev;
    }
}
```
# Middle of the Linkedlist

## Solution

```java
class Solution {
    public ListNode middleNode(ListNode head){
        ListNode slow = head;
        ListNode fast = head;

        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;  
        }

        return slow;
            
    }
}
```
# Palindromic Linkedlist

## Solution

```java
    class Solution {
        public boolean isPalindrome(ListNode head){
            if(head == null || head.next == null)
            return true;

            ListNode slow = head;
            ListNode fast = head;

            while(fast != null && fast.next != null) {
                slow = slow.next;
                fast = fast.next.next;
            }
        
            ListNode secondHalfHead = reverseList(slow);

            ListNode firstHalf = head;
            ListNode secondHalf = secondHalfHead;

            while(secondHalf != null) {
                if(firstHalf.val != secondHalf.val) return false;
                firstHalf = firstHalf.next;
                secondHalf = secondHalf.next;
            }

            return true;
        }

        private ListNode reverseList(ListNode head) {
            ListNode prev = null;
            while(head != null) {
                ListNode nextNode = head.next;
                head.next = prev;
                prev = head;
                head = nextNode;
            }
            return prev;
        }

    }

```
