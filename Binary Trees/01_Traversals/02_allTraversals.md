````java
public class BinaryTreeTraversals {

    // ===================================================================
    // 1. Recursive Pre-order (Root, Left, Right)
    // ===================================================================
    /**
     * Time Complexity: O(n)
     * - We must visit every node exactly once.
     * Space Complexity: O(h)
     * - This is the space used by the recursion call stack.
     * - Worst case (skewed tree): O(n). Average case (balanced tree): O(log n).
     */
    public List<Integer> preorderRecursive(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        preorderHelper(root, result);
        return result;
    }

    private void preorderHelper(TreeNode node, List<Integer> result) {
        if (node == null) {
            return;
        }
        result.add(node.val); // Visit Root
        preorderHelper(node.left, result); // Go Left
        preorderHelper(node.right, result); // Go Right
    }

    // ===================================================================
    // 2. Recursive In-order (Left, Root, Right)
    // ===================================================================
    /**
     * Time Complexity: O(n)
     * - We must visit every node exactly once.
     * Space Complexity: O(h)
     * - This is the space used by the recursion call stack.
     * - Worst case (skewed tree): O(n). Average case (balanced tree): O(log n).
     */
    public List<Integer> inorderRecursive(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        inorderHelper(root, result);
        return result;
    }

    private void inorderHelper(TreeNode node, List<Integer> result) {
        if (node == null) {
            return;
        }
        inorderHelper(node.left, result); // Go Left
        result.add(node.val); // Visit Root
        inorderHelper(node.right, result); // Go Right
    }

    // ===================================================================
    // 3. Recursive Post-order (Left, Right, Root)
    // =================================S==================================
    /**
     * Time Complexity: O(n)
     * - We must visit every node exactly once.
     * Space Complexity: O(h)
     * - This is the space used by the recursion call stack.
     * - Worst case (skewed tree): O(n). Average case (balanced tree): O(log n).
     */
    public List<Integer> postorderRecursive(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        postorderHelper(root, result);
        return result;
    }

    private void postorderHelper(TreeNode node, List<Integer> result) {
        if (node == null) {
            return;
        }
        postorderHelper(node.left, result); // Go Left
        postorderHelper(node.right, result); // Go Right
        result.add(node.val); // Visit Root
    }

    // ===================================================================
    // 4. Level-order (Iterative, using Queue)
    // ===================================================================
    /**
     * Time Complexity: O(n)
     * - We must visit, enqueue, and dequeue every node exactly once.
     * Space Complexity: O(w)
     * - Where 'w' is the maximum width of the tree.
     * - In the worst case (a full, balanced tree), the queue will hold all
     * nodes at the widest level, which can be up to O(n) (approx. n/2).
     */
    public List<Integer> levelOrder(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root); // Add root to the queue

        while (!queue.isEmpty()) {
            TreeNode current = queue.poll(); // Dequeue a node
            result.add(current.val); // "Visit" the node

            // Enqueue left child
            if (current.left != null) {
                queue.offer(current.left);
            }
            // Enqueue right child
            if (current.right != null) {
                queue.offer(current.right);
            }
        }
        return result;
    }

    // ===================================================================
    // 5. Iterative Pre-order (using Stack)
    // ===================================================================
    /**
     * Time Complexity: O(n)
     * - Each node is pushed and popped from the stack exactly once.
     * Space Complexity: O(h)
     * - This is the space used by the explicit stack. The stack holds at most
     * 'h' nodes on the path from the root to a leaf.
     * - Worst case (skewed tree): O(n). Average case (balanced tree): O(log n).
     */
    public List<Integer> preorderIterative(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }

        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);

        while (!stack.isEmpty()) {
            TreeNode current = stack.pop();
            result.add(current.val); // Visit Root

            // Push RIGHT child first, so LEFT is on top
            if (current.right != null) {
                stack.push(current.right);
            }
            if (current.left != null) {
                stack.push(current.left);
            }
        }
        return result;
    }

    // ===================================================================
    // 6. Iterative In-order (using Stack)
    // ===================================================================
    /**
     * Time Complexity: O(n)
     * - Each node is pushed and popped from the stack exactly once.
     * Space Complexity: O(h)
     * - This is the space used by the explicit stack. The stack holds at most
     * 'h' nodes on the path from the root to the leftmost leaf.
     * - Worst case (skewed tree): O(n). Average case (balanced tree): O(log n).
     */
    public List<Integer> inorderIterative(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode current = root;

        while (current != null || !stack.isEmpty()) {
            // Phase 1: Go as far left as possible
            while (current != null) {
                stack.push(current);
                current = current.left;
            }

            // Phase 2: Visit and Go Right
            current = stack.pop();
            result.add(current.val); // Visit Root
            current = current.right; // Go Right
        }
        return result;
    }

    // ===================================================================
    // 7. Iterative Post-order (using Stack)
    // ===================================================================
    /**
     * This method uses the "Root, Right, Left" modified pre-order
     * and then adds to the *front* of the list to reverse it.
     *
     * Time Complexity: O(n)
     * - Each node is pushed and popped once.
     * - `addFirst()` on a LinkedList is O(1).
     * Space Complexity: O(n)
     * - The stack itself uses O(h) auxiliary space.
     * - However, the `result` LinkedList grows to O(n) size.
     * - Total space complexity is dominated by the output list, making it O(n).
     * - (The two-stack method is also O(n) space).
     */
    public List<Integer> postorderIterative(TreeNode root) {
        // Use LinkedList for efficient addFirst()
        LinkedList<Integer> result = new LinkedList<>();
        if (root == null) {
            return result;
        }

        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);

        while (!stack.isEmpty()) {
            TreeNode current = stack.pop();
            // Add to the front of the list (this reverses the order)
            result.addFirst(current.val);

            // Push LEFT child first, so RIGHT is on top
            if (current.left != null) {
                stack.push(current.left);
            }
            if (current.right != null) {
                stack.push(current.right);
            }
        }
        return result;
    }
}

````