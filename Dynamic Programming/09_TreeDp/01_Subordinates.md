# subordinates (cses)
````java
import java.util.ArrayList;
import java.util.Scanner;

public class Subordinates {
    // Adjacency list to store the tree
    static ArrayList<ArrayList<Integer>> adj;
    // Array to store the result (count of subordinates)
    static int[] subtreeSize;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // Read number of employees
        if (!sc.hasNextInt()) return;
        int n = sc.nextInt();

        // Initialize adjacency list
        // Size is n + 1 because nodes are 1-indexed
        adj = new ArrayList<>();
        for (int i = 0; i <= n; i++) {
            adj.add(new ArrayList<>());
        }

        // Read parents for employees 2 to n
        // Input format: The (i-1)-th integer is the direct boss of employee i
        for (int employee = 2; employee <= n; employee++) {
            int boss = sc.nextInt();
            adj.get(boss).add(employee);
        }

        // Array to store answers (automatically initialized to 0)
        subtreeSize = new int[n + 1];

        // Start DFS from the General Director (Node 1)
        dfs(1);

        // Print the results
        // Using StringBuilder is faster than printing inside the loop
        StringBuilder sb = new StringBuilder();
        for (int i = 1; i <= n; i++) {
            sb.append(subtreeSize[i]).append(" ");
        }
        System.out.println(sb.toString());
        
        sc.close();
    }

    // Recursive function to calculate subtree sizes
    static void dfs(int node) {
        int count = 0;
        
        // Iterate over all direct children (subordinates) of the current node
        for (int child : adj.get(node)) {
            dfs(child); // First, calculate size for the child
            
            // Add the child's subtree size + 1 (for the child itself)
            count += (1 + subtreeSize[child]);
        }
        
        // Store the result for the current node
        subtreeSize[node] = count;
    }
}
````
