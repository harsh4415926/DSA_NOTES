# Shortest Path in Weighted Graph (GFG)

## Thought Process
- Use **Dijkstra's algorithm** with a priority queue and `dist[]` array to find the shortest path.
- In addition, maintain a `cameFrom[]` array to record **where each node came from** in its shortest path.
- After computing the shortest distances, **backtrack from the destination to the source** using `cameFrom[]` to reconstruct the actual path.
- If the destination is unreachable, return `-1`.

---

## Code
```java
class Pair{
    int first;
    int second;
    Pair(int first,int second){
        this.first=first;
        this.second=second;
    }
}

class Solution {
    public List<Integer> shortestPath(int n, int m, int edges[][]) {
        List<List<Pair>> adj = new ArrayList<>();
        for(int i = 0; i <= n; i++) adj.add(new ArrayList<>());

        // Build adjacency list
        for(int i = 0; i < m; i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            int w = edges[i][2];
            adj.get(u).add(new Pair(w, v));
            adj.get(v).add(new Pair(w, u));
        }

        PriorityQueue<Pair> q = new PriorityQueue<>((a,b) -> a.first - b.first);
        int[] dist = new int[n+1];
        int[] cameFrom = new int[n+1];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[1] = 0;
        q.add(new Pair(0,1));

        while(q.size() > 0) {
            Pair p = q.poll();
            int u = p.second;
            int distance = p.first;
            for(Pair it : adj.get(u)) {
                int v = it.second;
                int edge = it.first;
                if(distance + edge < dist[v]) {
                    dist[v] = distance + edge;
                    q.add(new Pair(dist[v], v));
                    cameFrom[v] = u;
                }
            }
        }

        List<Integer> l = new ArrayList<>();
        if(dist[n] == Integer.MAX_VALUE) {
            l.add(-1);
            return l;
        }

        int t = n;
        cameFrom[1] = -1;
        while(t != -1) {
            l.add(t);
            t = cameFrom[t];
        }

        Collections.reverse(l);
        l.add(0, dist[n]);  // Add shortest distance at the start
        return l;
    }
}



time complexity same as djisktra