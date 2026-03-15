# tree Diameter(cses)
````java
import java.util.*;

class TreeDiameter {

    public static int height(int u, List<List<Integer>> adj, int p, int[] globalMax) {
        int maxH = 0;      // Max height found among children
        int secondMaxH = 0; // Second max height found

        for (int v : adj.get(u)) {
            if (v != p) {
                int childHeight = height(v, adj, u, globalMax);

                // Update the top two heights
                if (childHeight > maxH) {
                    secondMaxH = maxH;
                    maxH = childHeight;
                } else if (childHeight > secondMaxH) {
                    secondMaxH = childHeight;
                }
            }
        }

        // Diameter passing through 'u' is the sum of the two largest heights
        globalMax[0] = Math.max(globalMax[0], maxH + secondMaxH);

        // Return height of current node (1 edge + max height of children)
        return 1 + maxH;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        if (!sc.hasNextInt()) return;
        
        int n = sc.nextInt();
        
        // Initialize Adjacency List
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i <= n; i++) adj.add(new ArrayList<>());

        // Read edges (Tree has N-1 edges)
        for (int i = 0; i < n - 1; i++) {
            int u = sc.nextInt();
            int v = sc.nextInt();
            adj.get(u).add(v);
            adj.get(v).add(u);
        }

        int[] globalMax = new int[1]; // Use array to store result across recursion
        
        if (n > 0) {
            height(1, adj, -1, globalMax);
        }
        
        System.out.println(globalMax[0]);
    }
}
````
