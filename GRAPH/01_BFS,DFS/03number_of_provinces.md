## LeetCode 547 - Number of Provinces

**Problem Description:**  
You are given an `n x n` matrix `isConnected`, where `isConnected[i][j] = 1` means city `i` and city `j` are directly connected. A province is a group of directly or indirectly connected cities. Return the total number of provinces.

**Intuition:**
- The matrix represents an undirected graph where cities are nodes and direct connections are edges.
- Use DFS to traverse each connected component.
- Start from each unvisited city, run DFS to mark all cities in that province, and increment the count.

**Code:**
```java
class Solution {
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        int[] vis = new int[n];
        int count = 0;

        for(int i = 0; i < n; i++) {
            if(vis[i] == 0) {
                dfs(i, isConnected, vis);
                count++;
            }
        } 
        return count;
    }

    public void dfs(int node, int[][] isConnected, int[] vis) {
        vis[node] = 1;
        int n = isConnected.length;
        for(int i = 0; i < n; i++) {
            if(isConnected[node][i] == 1 && vis[i] == 0) {
                dfs(i, isConnected, vis);
            }
        }
    }
}
```
## Time Complexity: 
- O(n²)    Traverses the entire adjacency matrix once.

## Space Complexity:
- O(n)      For the visited array and recursion stack.