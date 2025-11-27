# Problem: Minimum Multiplications to Reach End (GFG)
## Problem Description
You are given:
- An array `arr` of integers.
- Two integers `start` and `end`.

In one step, you can multiply the current number by any element in `arr` and take the result modulo `100000`.  
Return the **minimum number of multiplications** needed to transform `start` into `end`. If it's not possible, return `-1`.

**Example:**  
Input: `arr = [2, 5, 7]`, `start = 3`, `end = 30`  
Output: `2`  
Explanation:
- Step 1: `3 * 5 = 15`
- Step 2: `15 * 2 = 30`

## Intuition & Approach
- Treat numbers `0..99999` as nodes in a graph.
- There is an edge from `u` to `(u * arr[i]) % 100000` with weight `1`.
- We need the shortest path from `start` to `end`.
- Since all edges have equal weight `1`, **Breadth-First Search (BFS)** works perfectly.
- Maintain `dist[]` to store the minimum steps to reach each number.

## Code
```java
class Solution {
    int minimumMultiplications(int[] arr, int start, int end) {
        Queue<Pair> q = new LinkedList<>();
        int[] dist = new int[100000];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[start] = 0;

        q.add(new Pair(0, start));
        while (!q.isEmpty()) {
            Pair p = q.remove();
            int node = p.second;
            int distance = p.first;

            if (node == end) return distance;

            for (int it : arr) {
                int m = (it * node) % 100000;
                if (distance + 1 < dist[m]) {
                    dist[m] = distance + 1;
                    q.add(new Pair(distance + 1, m));
                }
            }
        }
        return -1;
    }
}
```

Time Complexity: O(100000 × n) in the worst case (where n = size of arr), but practically less since we visit each node at most once.

Space Complexity: O(100000) for the dist[] array and BFS queue.