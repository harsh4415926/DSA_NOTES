# LeetCode 310 — Minimum Height Trees

## intuition
1. we need to find the root nodes from which traversal give shortest heigts so its never from the leaf nodes
2. so keep removing leaf nodes until you reach final 2 or 1 nodes because those nodes are from the middle of the graph from which the height is actually minimum

## approach
- Use **topological sort / BFS from leaves** approach:
    1. Treat nodes with degree 1 as **leaves**.
    2. Iteratively **remove leaves layer by layer**.
    3. Remaining 1 or 2 nodes are **roots of minimum height trees**.
- This works because the **centroids** of a tree minimize height.

---

## Pseudocode
```java
function findMinHeightTrees(n, edges):
if n == 1:
return [0]
build adjacency list and degree array
initialize queue with all leaves (degree == 1)

while n > 2:
remove all leaves:
decrease n by number of leaves
for each leaf:
decrement degree of neighbors
if neighbor becomes leaf:
add to queue

return remaining nodes in queue
   ```

---

## Code
```java
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        List<Integer> ans=new ArrayList<>();
        if(n==1){
            ans.add(0);
            return ans;
        }

        List<List<Integer>> adj=new ArrayList<>();
        for(int i=0;i<n;i++) adj.add(new ArrayList<>());

        int[] degree=new int[n];
        for(int i=0;i<edges.length;i++){
            int u=edges[i][0];
            int v=edges[i][1];
            adj.get(u).add(v);
            adj.get(v).add(u);
            degree[u]++;
            degree[v]++;
        }

        Queue<Integer> q=new LinkedList<>();
        for(int i=0;i<n;i++){
            if(degree[i]==1) q.add(i);
        }

        while(n>2){
            int size=q.size();
            n-=size;
            while(size>0){
                int node=q.poll();
                for(int nbr:adj.get(node)){
                    degree[nbr]--;
                    if(degree[nbr]==1) q.add(nbr);
                }
                size--;
            }
        }

        while(!q.isEmpty()){
            ans.add(q.poll());
        }
        return ans;
    }
}

