# sort linked list of 0's , 1's , 2's 
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Sort a Linked List of 0s, 1s, and 2s
 * ----------------------------------------------------------------------
 * LOGIC (Three Dummy Nodes / Sublist Segregation):
 * 1. Instead of counting the occurrences and overwriting the data (which 
 * is technically a 2-pass approach), this is an elegant 1-pass solution 
 * that physically rearranges the nodes.
 * 2. We create three separate "dummy" lists to catch the 0s, 1s, and 2s.
 * 3. We traverse the original list with a 'curr' pointer. Depending on 
 * 'curr.data', we append the node to the end of the corresponding dummy list.
 * 4. Once the original list is exhausted, we have three distinct chains.
 * 5. We then link the tail of the 'zeros' list to the head of the 'ones' 
 * list. (Crucially, we check if the 'ones' list is empty. If it is, we 
 * link 'zeros' directly to the 'twos' list).
 * 6. We link the tail of the 'ones' list to the 'twos' list, and ensure 
 * the tail of the 'twos' list points to null to terminate the final list.
 * ----------------------------------------------------------------------
 */

class Solution {
    static Node segregate(Node head) {
        // Base case: empty list or single node
        if (head == null || head.next == null) return head;

        // Create three dummy nodes to act as starting anchors
        Node zeroHead = new Node(-1);
        Node oneHead = new Node(-1);
        Node twoHead = new Node(-1);

        // Tail pointers for each of the three lists
        Node zero = zeroHead, one = oneHead, two = twoHead;

        Node curr = head;

        // Pass 1: Distribute the nodes into their respective buckets
        while (curr != null) {
            if (curr.data == 0) {
                zero.next = curr;
                zero = zero.next;
            } else if (curr.data == 1) {
                one.next = curr;
                one = one.next;
            } else {
                two.next = curr;
                two = two.next;
            }
            curr = curr.next; // Move forward in the original list
        }

        // Pass 2: Stitch the three lists together.
        
        // Connect the zeros to the ones (or directly to twos if there are no ones)
        zero.next = (oneHead.next != null) ? oneHead.next : twoHead.next;
        
        // Connect the ones to the twos
        one.next = twoHead.next;
        
        // Terminate the end of the newly combined list to prevent cycles
        two.next = null;

        // Return the true head of the sorted list (skipping the zero dummy node)
        return zeroHead.next;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N). We traverse the original linked list exactly one time.
 * Space: O(1). We only allocate three dummy nodes and manipulate existing pointers, 
 * regardless of how large the list grows.
 */
````