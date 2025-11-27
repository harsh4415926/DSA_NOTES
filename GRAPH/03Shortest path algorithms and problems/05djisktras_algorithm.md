# Dijkstra's Algorithm (Shortest Path in Weighted Graph)

## Intuition / Approach
We need to find the shortest path from a source node to all other nodes in a weighted graph with
**non-negative weights**.

- Start with distance `0` for the source and infinity for all other nodes.
- Use a **priority queue (min-heap)** to always process the next node with the **smallest current distance**.
- Repeatedly pick the node with the smallest distance and **relax all its edges** (if `dist[u] + wt < dist[v]`, update `dist[v]`).
- Continue until all nodes are processed.

### Why Priority Queue and not a normal Queue?
- A priority queue ensures we **always expand the shortest available path first**, avoiding extra processing ,
- which lacks in a normal queue which unnecessarily creates unnecessary traversals

## Code
```java
class Pair {
    int node, dist;
    Pair(int node, int dist) {
        this.node = node;
        this.dist = dist;
    }
}

class Solution {
    static int[] dijkstra(int V, ArrayList<ArrayList<Pair>> adj, int S) {
        PriorityQueue<Pair> pq = new PriorityQueue<>((a, b) -> a.dist - b.dist);
        int[] dist = new int[V];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[S] = 0;
        
        pq.add(new Pair(S, 0));
        
        while (!pq.isEmpty()) {
            Pair p = pq.poll();
            int u = p.node;
            int d = p.dist;
            
            for (Pair edge : adj.get(u)) {
                int v = edge.node;
                int wt = edge.dist;
                
                if (d + wt < dist[v]) {
                    dist[v] = d + wt;
                    pq.add(new Pair(v, dist[v]));
                }
            }
        }
        return dist;
    }
}


