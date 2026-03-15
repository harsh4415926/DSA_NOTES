# tree Matching(cses)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Tree Matching (Maximum Matching)
 * ----------------------------------------------------------------------
 * LOGIC (DP on Trees):
 * 1. dp[u][0]: Max matching in subtree 'u', 'u' is NOT matched with any child.
 * - Sum of max(dp[v][0], dp[v][1]) for all children 'v'.
 * 2. dp[u][1]: Max matching in subtree 'u', 'u' IS matched with one child.
 * - Try pairing 'u' with each child 'v'.
 * - Formula: (Sum of others) + (1 + dp[v][0]).
 * - Optimization: Use dp[u][0] value, subtract 'v's contribution, add (1 + dp[v][0]).
 * ----------------------------------------------------------------------
 */

import java.util.*;

class TreeMatching {

    public static void dfs(int u, int parent, int[][] dp, List<List<Integer>> adj) {
        // CASE 1: 'u' is NOT paired with any child
        // We simply take the best result from every child branch (matched or unmatched)
        dp[u][0] = 0;
        for (int v : adj.get(u)) {
            if (v != parent) {
                dfs(v, u, dp, adj);
                dp[u][0] += Math.max(dp[v][0], dp[v][1]);
            }
        }

        // CASE 2: 'u' IS paired with exactly one child 'v'
        // We iterate through each child to see which one gives max value when paired with 'u'
        dp[u][1] = 0;
        for (int v : adj.get(u)) {
            if (v != parent) {
                // Calculation:
                // 1. Start with dp[u][0] (sum of best of all children)
                // 2. Remove v's current contribution: - Math.max(dp[v][0], dp[v][1])
                // 3. Add new contribution (edge u-v exists + v is unmatched with its kids): + 1 + dp[v][0]
                int val = dp[u][0] - Math.max(dp[v][0], dp[v][1]) + 1 + dp[v][0];
                dp[u][1] = Math.max(dp[u][1], val);
            }
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        if (!sc.hasNextInt()) return; // Safety check
        int n = sc.nextInt();
        
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i <= n; i++) adj.add(new ArrayList<>());
        
        for (int i = 0; i < n - 1; i++) {
            int u = sc.nextInt();
            int v = sc.nextInt();
            adj.get(u).add(v);
            adj.get(v).add(u);
        }
        
        int[][] dp = new int[n + 1][2];
        dfs(1, -1, dp, adj);
        
        // Result is max of root being unmatched or matched
        System.out.println(Math.max(dp[1][0], dp[1][1]));
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (DFS visits every node once).
 * Space: O(N) (DP array + Recursion stack).
 */
````
