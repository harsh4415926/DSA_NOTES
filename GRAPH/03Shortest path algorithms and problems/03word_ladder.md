# leetcode: Word Ladder (LeetCode 127)

## Problem Description
You are given two words `beginWord` and `endWord`, and a dictionary `wordList`.  
Transform `beginWord` into `endWord` by changing **exactly one letter at a time**, such that each intermediate word must exist in the dictionary.  
Return the **number of words** in the shortest transformation sequence, or `0` if no sequence exists.

**Example:**  
Input: `beginWord = "hit"`, `endWord = "cog"`, `wordList = ["hot","dot","dog","lot","log","cog"]`  
Output: `5`  
Explanation: `"hit" -> "hot" -> "dot" -> "dog" -> "cog"`

## Intuition & Approach
1. This is a **shortest path in an unweighted graph** problem.
2. Treat each word as a node and add edges between words that differ by exactly one letter.
3. Perform **Breadth-First Search (BFS)** starting from `beginWord`.
4. For every dequeued word, generate all possible one-letter transformations and enqueue valid ones present in the dictionary.
5. The level of BFS represents the transformation length.
6. If `endWord` is reached, return its level. Otherwise, return `0`.

**Key Points:**
- Use a `HashSet` for `wordList` to allow `O(1)` lookups and removals.
- Avoid revisiting nodes by removing words from the set once processed.

## Code
```java
class Pair {
    String first;
    int second;
    Pair(String first, int second) {
        this.first = first;
        this.second = second;
    }
}

class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        HashSet<String> set = new HashSet<>(wordList);
        Queue<Pair> q = new LinkedList<>();
        q.add(new Pair(beginWord, 1));
        set.remove(beginWord);

        while (!q.isEmpty()) {
            Pair p = q.remove();
            String word = p.first;
            int level = p.second;

            if (word.equals(endWord)) return level;

            for (int i = 0; i < word.length(); i++) {
                char original = word.charAt(i);
                char[] charArray = word.toCharArray();

                for (char ch = 'a'; ch <= 'z'; ch++) {
                    if (ch == original) continue;
                    charArray[i] = ch;
                    String newWord = new String(charArray);

                    if (set.contains(newWord)) {
                        q.add(new Pair(newWord, level + 1));
                        set.remove(newWord);
                    }

                    charArray[i] = original;
                }
            }
        }
        return 0;
    }
}
```
Time Complexity  
For each word, we try 26 * L transformations (L = word length).  
BFS processes each word once → O(N * L * 26)where N = number of words  
Overall: O(N * L * 26)

Space Complexity  
HashSet for word list: O(N)  
Queue for BFS: up to O(N)  
Overall: O(N)

