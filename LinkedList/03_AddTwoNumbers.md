# Add two numbers 
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 2 - Add Two Numbers
 * ----------------------------------------------------------------------
 * LOGIC (Simulated Addition / Dummy Node Pattern):
 * 1. The linked lists store numbers in REVERSE order, which is actually 
 * highly convenient! It means the head of the list is the 1s place, 
 * the next node is the 10s place, etc. This perfectly matches how we 
 * do math on paper (right to left).
 * 2. We use a "Dummy Node" pattern. This gives our new result list a 
 * safe starting anchor, so we don't have to write messy edge cases for 
 * initializing the head.
 * 3. We iterate as long as there is data in `l1`, `l2`, OR a leftover `carry`.
 * 4. At each step, we sum the values and the carry:
 * - The new node gets the single digit: `sum % 10`.
 * - The new carry gets the tens digit: `sum / 10`.
 * ----------------------------------------------------------------------
 */

class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        // A dummy node acts as an anchor. Our actual answer will start at dummy.next.
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;
        
        int carry = 0;

        // Loop continues if either list has digits left, OR if we have a leftover 
        // carry (e.g., 5 + 5 = 10, creates a new node '1' at the end).
        while (l1 != null || l2 != null || carry != 0) {
            
            // Start the column sum with whatever carried over from the previous column
            int sum = carry;

            // If l1 has a node, add its value and move the pointer forward
            if (l1 != null) {
                sum += l1.val;
                l1 = l1.next;
            }

            // If l2 has a node, add its value and move the pointer forward
            if (l2 != null) {
                sum += l2.val;
                l2 = l2.next;
            }

            // The value for the current place is the remainder (e.g., 14 % 10 = 4)
            curr.next = new ListNode(sum % 10);
            
            // The carry for the next place is the quotient (e.g., 14 / 10 = 1)
            carry = sum / 10;

            // Move our result pointer forward to the newly created node
            curr = curr.next;
        }

        // Return the actual head of the resulting list, skipping the dummy anchor
        return dummy.next;
    }
}

/*
 * COMPLEXITY:
 * Time: O(max(N, M)) where N and M are the lengths of l1 and l2. We traverse 
 * the longest list exactly once.
 * Space: O(max(N, M)). The new list is created and its length is at most 
 * max(N, M) + 1 (if there is a final carry).
 */
````