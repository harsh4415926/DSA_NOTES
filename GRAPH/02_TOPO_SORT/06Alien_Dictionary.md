# GFG: Alien Dictionary

## Problem Description
Given a sorted dictionary (array of words) of an alien language, find the order of characters in the language.  
The words are sorted lexicographically according to the rules of this alien language.

**Example:**  
Input: `["baa","abcd","abca","cab","cad"]`  
Output: `"bdac"` (one possible valid order)

## Intuition & Approach
1. This is a **topological sort** problem on characters.
2. Compare adjacent words and find the first differing character; this defines a directed edge from the first character to the second.
3. Build a graph where each node is a character and edges represent order constraints.
4. Perform **topological sorting** (using Kahn’s algorithm) on the graph.
5. If the topological sort includes all unique characters, return the order; otherwise, return an empty string (invalid ordering).

**Steps:**
- Collect all unique characters from the words.
- Construct adjacency list for characters based on first mismatch between consecutive words.
- Compute indegree of all nodes.
- Use a queue to perform Kahn’s algorithm for topological sorting.
- Build the order string from the topological sort.

## Code
```java
class Solution {
    public String findOrder(String[] words) {
        HashSet<Character> set = new HashSet<>();
        for (String word : words) {
            for (char c : word.toCharArray()) {
                set.add(c);
            }
        }
        int k = set.size();
        
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < 26; i++) adj.add(new ArrayList<>());
        
        for (int i = 0; i < words.length - 1; i++) {
            String w1 = words[i];
            String w2 = words[i + 1];
            int len = Math.min(w1.length(), w2.length());
            boolean flag = false;
            for (int j = 0; j < len; j++) {
                if (w1.charAt(j) != w2.charAt(j)) {
                    adj.get(w1.charAt(j) - 'a').add(w2.charAt(j) - 'a');
                    flag = true;
                    break;
                }
            }
            if (!flag && w1.length() > w2.length()) return "";
        }
        
        int[] indegree = new int[26];
        for (int i = 0; i < 26; i++) {
            for (int j : adj.get(i)) {
                indegree[j]++;
            }
        }
        
        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < 26; i++) {
            if (set.contains((char)(i + 'a')) && indegree[i] == 0) {
                q.add(i);
            }
        }
        
        StringBuilder s = new StringBuilder();
        while (!q.isEmpty()) {
            int node = q.poll();
            s.append((char)(node + 'a'));
            for (int nbr : adj.get(node)) {
                indegree[nbr]--;
                if (indegree[nbr] == 0 && set.contains((char)(nbr + 'a'))) {
                    q.add(nbr);
                }
            }
        }
        
        return (s.length() == k) ? s.toString() : "";
    }
}
```
Time Complexity  
Building adjacency list: O(N * L) where N = number of words, L = average word length  
Topological sort: O(V + E) where V = number of unique characters, E = number of edges  
Overall: O(N*L + V + E)

Space Complexity  
Adjacency list: O(V + E)  
Indegree array + queue + set: O(V)  
StringBuilder: O(V)  
Overall: O(V + E)
