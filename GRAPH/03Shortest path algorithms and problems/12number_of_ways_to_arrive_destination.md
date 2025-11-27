# Problem: Number of Ways to Arrive at Destination (LeetCode 1976)

## Intuition
We need to find not just the shortest time to reach the destination city but also the **number of distinct 
shortest paths**.  
This can be solved using **Dijkstra's algorithm** because edge weights are positive.
- Use a `time[]` array to store the minimum time to reach each node.
- Use a `ways[]` array to store how many shortest paths reach each node.
- For each relaxation:
    - If we find a strictly shorter path → update `time[v]` and set `ways[v] = ways[u]`.
    - If we find an equal shortest path → increment `ways[v]` by `ways[u]`.

## Code
```java
class Pair {
    int first;
    long second;
    Pair(int first, long second) {   // used long for distances because of large test cases in leatcode 
        this.first = first;
        this.second = second;
    }
}

class Solution {
    public int countPaths(int n, int[][] roads) {
        List<List<Pair>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }
        for (int i = 0; i < roads.length; i++) {
            adj.get(roads[i][0]).add(new Pair(roads[i][1], roads[i][2]));
            adj.get(roads[i][1]).add(new Pair(roads[i][0], roads[i][2]));
        }
        
        PriorityQueue<Pair> q = new PriorityQueue<>((a, b) -> Long.compare(a.second, b.second));
        long[] time = new long[n];
        Arrays.fill(time, Long.MAX_VALUE);
        time[0] = 0;
        
        int[] ways = new int[n];
        ways[0] = 1;
        q.add(new Pair(0, 0L));
        
        int mod = (int) 1e9 + 7;
        
        while (q.size() > 0) {
            Pair p = q.poll();
            int u = p.first;
            long minTime = p.second;
            
            for (Pair it : adj.get(u)) {
                int v = it.first;
                long edge = it.second;
                
                if (minTime + edge < time[v]) {
                    time[v] = minTime + edge;
                    q.add(new Pair(v, time[v]));
                    ways[v] = ways[u];
                } else if (minTime + edge == time[v]) {
                    ways[v] = (ways[u] + ways[v]) % mod;
                }
            }
        }
        return ways[n - 1] % mod;
    }
}

time complexity same as dijsktra
