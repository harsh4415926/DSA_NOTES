# odd even linked List
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 328 - Odd Even Linked List
 * ----------------------------------------------------------------------
 * LOGIC (Two Pointers / List Weaving):
 * 1. The goal is to group all nodes at ODD indices together, followed by 
 * all nodes at EVEN indices. (Note: This is based on node position, 
 * not the actual integer values inside the nodes).
 * 2. We maintain two separate tracks: an 'odd' track and an 'even' track.
 * 3. CRITICAL: We must save the head of the even list ('evenHead') before 
 * we start modifying pointers, so we know where to attach the tail of 
 * the odd list at the very end.
 * 4. In the while loop, we essentially make the pointers "leapfrog" over 
 * each other. The odd pointer skips the next even node to connect to the 
 * next odd node, and vice versa.
 * 5. Finally, we link the end of our newly formed odd chain to the 
 * beginning of our even chain using 'evenHead'.
 * ----------------------------------------------------------------------
 */

class Solution {
    public ListNode oddEvenList(ListNode head) {
        // Base case: If the list is empty, has 1 node, or has 2 nodes, 
        // it is already grouped correctly.
        if (head == null || head.next == null) return head;

        // Initialize our two runners. Head is node 1 (odd).
        ListNode odd = head;
        ListNode even = head.next;
        
        // Anchor the start of the even list so we don't lose it in the void
        ListNode evenHead = even;

        // The loop condition only needs to check 'even' and 'even.next' 
        // because the 'even' pointer will always be ahead of the 'odd' pointer.
        while (even != null && even.next != null) {
            
            // Leapfrog 1: Connect current odd node to the NEXT odd node
            odd.next = even.next;
            // Move the odd runner forward
            odd = odd.next;

            // Leapfrog 2: Connect current even node to the NEXT even node
            even.next = odd.next;
            // Move the even runner forward
            even = even.next;
        }

        // Connect the tail of the odd list to the head of the even list
        odd.next = evenHead;
        
        return head;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N). We traverse the linked list exactly once.
 * Space: O(1). We strictly rearrange existing pointers without allocating 
 * any new nodes, satisfying the problem's in-place requirement.
 */
````