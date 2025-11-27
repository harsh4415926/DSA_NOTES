# Course Schedule II (leetcode 210)
## Problem
Find a valid order of courses to finish all courses given prerequisites.
You are given numCourses and prerequisites as a list of [course, prerequisite] pairs.
Return an array of courses in a valid order, or an empty array if impossible.

## Approach
1. Build an adjacency list for the graph where edges point from prerequisite → course.
2. Compute indegree of each vertex (number of prerequisites).
3. Use Kahn's algorithm (BFS):
    - Add all courses with indegree 0 to a queue.
    - While queue is not empty:
        - Pop a course and add it to the result list.
        - Reduce indegree of all its neighbors.
        - If any neighbor's indegree becomes 0, add it to the queue.
4. If all courses are processed, return the list as an array.
   Otherwise, return an empty array (cycle exists, no valid order).

## Code
class Solution {
public int[] findOrder(int numCourses, int[][] prerequisites) {
List<List<Integer>> adj = new ArrayList<>();
int V = numCourses;
for (int i = 0; i < V; i++) {
adj.add(new ArrayList<>());
}
for (int i = 0; i < prerequisites.length; i++) {
adj.get(prerequisites[i][1]).add(prerequisites[i][0]);
}

        int[] indegree = new int[V];
        for (int i = 0; i < V; i++) {
            for (int it : adj.get(i)) {
                indegree[it]++;
            }
        }

        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) q.add(i);
        }

        List<Integer> l = new ArrayList<>();
        while (!q.isEmpty()) {
            int node = q.poll();
            l.add(node);
            for (int it : adj.get(node)) {
                indegree[it]--;
                if (indegree[it] == 0) q.add(it);
            }
        }

        if (l.size() != V) return new int[0];
        int[] ans = new int[V];
        for (int i = 0; i < V; i++) {
            ans[i] = l.get(i);
        }
        return ans;
    }
}

## Complexity
- Time: O(V + E) — building adjacency + BFS processing.
- Space: O(V + E) — adjacency list + indegree array + queue + result list.
  
