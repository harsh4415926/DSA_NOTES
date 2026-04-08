# reverse a doubly linked list

## using stack
 ````
traverse and store values in a stack and then traverse again and update the values while popping values from the stack

````

## pointers exchange (prev-> next ,, next-> prev)

````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Reverse a Doubly Linked List
 * ----------------------------------------------------------------------
 * LOGIC (In-Place Pointer Swapping):
 * 1. The core idea of reversing a doubly linked list is simply swapping 
 * the 'next' and 'prev' pointers for every single node in the list.
 * 2. We use a 'curr' pointer to iterate through the list, starting at 'head'.
 * 3. For each node, we temporarily store its 'prev' pointer in 'temp'.
 * 4. Then, we perform the swap: curr.prev = curr.next, and curr.next = temp.
 * 5. To move to the "next" node in our original list, we actually have to 
 * move to 'curr.prev', because the pointers have now been flipped!
 * 6. Once the loop finishes, 'curr' is null. The 'temp' pointer will be 
 * sitting at the original second-to-last node. Because of the swaps, 
 * 'temp.prev' accurately points to the new head of the reversed list.
 * ----------------------------------------------------------------------
 */

class Solution {
    public Node reverse(Node head) {
        // Base case: If the list is empty or has only one node, 
        // it is already reversed.
        if(head == null || head.next == null) return head;
        
        Node temp = null;
        Node curr = head;
       
        // Traverse until we fall off the end of the list
        while(curr != null){
            // Store the previous node temporarily
            temp = curr.prev;
            
            // Swap the 'prev' and 'next' pointers of the current node
            curr.prev = curr.next;
            curr.next = temp;
            
            // Move to the next node in the original sequence. 
            // Since we just swapped the pointers, the original 'next' 
            // node is now located at 'curr.prev'.
            curr = curr.prev;
        }
        
        // At the end of the loop, 'curr' is null, and 'temp' is pointing 
        // to the old
````