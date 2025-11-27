# maximum path sum(leetcode)
````java
class Solution {

    int max;

  
    public int solve(TreeNode root) {
        // Base case: A null node contributes 0 to any path sum.
        if (root == null)
            return 0;

        // 1. Recursively find max path sum from left and right children.
        // The return value 'l' is the max path *from the left child down*.
        int l = solve(root.left);
        int r = solve(root.right);

        // 2. Discard negative path sums.
        // A negative path will only reduce our total. It's better to not take that path.
        if (l < 0) l = 0;
        if (r < 0) r = 0;

        // 3. Check for a new global maximum (the "arch" or "split" path).
        // This is the path (left_path + root.val + right_path).
        // This path *cannot* be extended up to the parent.
        max = Math.max(max, root.val + l + r);

        // 4. Return the max path that *can* be extended to the parent.
        // This is (root.val + *either* left *or* right, whichever is larger).
        return root.val + Math.max(l, r);
    }

    public int maxPathSum(TreeNode root) {
        // Initialize max to a very small number, as node values can be negative.
        max = Integer.MIN_VALUE;
        
        // Start the recursion. The function's return value is just to fuel the
        // recursion. We only care about the side effect that updates `max`.
        solve(root);
        
        // The global 'max' variable now holds the answer.
        return max;
    }

    /*
    📊 Complexity Analysis
    * Time Complexity: O(n)
        * We visit every node in the tree exactly once.
    
    * Space Complexity: O(h) (height of tree)
        * This is for the recursion call stack.
        * O(n) in the worst case (a skewed tree).
        * O(log n) in the average/best case (a balanced tree).
    */
}
````