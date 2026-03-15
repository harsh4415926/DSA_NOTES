# maximum width of binary tree
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Maximum Width of Binary Tree (LeetCode 662)
 * ----------------------------------------------------------------------
 * LOGIC (BFS + Indexing):
 * 1. Level-order traversal (BFS).
 * 2. Assign indices: Root=0, Left=2*i+1, Right=2*i+2.
 * 3. Width = (Last Index - First Index + 1) at each level.
 * 4. Normalization: Subtract 'minIndex' of the level to prevent overflow.
 * ----------------------------------------------------------------------
 */

class Solution {
    
    class Pair {
        TreeNode node;
        long index;    
        
        Pair(TreeNode n, long i){
            node = n;
            index = i;
        }
    }
    
    public int widthOfBinaryTree(TreeNode root) {
        if(root == null) return 0;
        
        Queue<Pair> q = new LinkedList<>();
        q.offer(new Pair(root, 0));
        int maxWidth = 0;
        
        // BFS Level Order Traversal
        while(!q.isEmpty()){
            int size = q.size();
            long minIndex = q.peek().index; // First index of current level (for normalization)
            
            long first = 0, last = 0;
            
            for(int i = 0; i < size; i++){
                Pair curr = q.poll();
                
                // Normalize: Shift indices to start near 0. Prevents overflow.
                long currIndex = curr.index - minIndex; 
                
                if(i == 0) first = currIndex;
                if(i == size - 1) last = currIndex;
                
                // Standard Heap Indexing: Left -> 2*i+1, Right -> 2*i+2
                if(curr.node.left != null)
                    q.offer(new Pair(curr.node.left, 2*currIndex + 1));
                
                if(curr.node.right != null)
                    q.offer(new Pair(curr.node.right, 2*currIndex + 2));
            }
            
            // Update max width for current level
            maxWidth = Math.max(maxWidth, (int)(last - first + 1));
        }
        
        return maxWidth;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (Visit every node).
 * Space: O(W) (Max width of tree in Queue).
 */
````