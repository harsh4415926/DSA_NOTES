## BFS of Graph

**Intuition:**
- Perform a level-order traversal starting from node `0`.
- Use a queue to process nodes in FIFO order and a `vis[]` array to ensure no node is visited twice.
- Push the starting node into the queue, mark it visited, then repeatedly pop from the queue and explore unvisited neighbors.

**Code:**
```java
class Solution {
    // Function to return Breadth First Search Traversal of given graph.
    public ArrayList<Integer> bfs(ArrayList<ArrayList<Integer>> adj) {
        int n = adj.size();
        ArrayList<Integer> bfs = new ArrayList<>();
        Queue<Integer> q = new LinkedList<>();
        int[] vis = new int[n];

        q.add(0);
        vis[0] = 1;

        while(q.size() > 0) {
            int node = q.poll();
            bfs.add(node);
            for(int nbr : adj.get(node)) {
                if(vis[nbr] == 0) {
                    q.add(nbr);
                    vis[nbr] = 1;
                }
            }
        }
        return bfs;
    }
}
```
## time complexity-
- O(V + 2E) - O(V+E)     where O(V) is for calling recursion function for each node and 2E is total degree
## space complexity
- O(V)
