# Word Ladder 2 - GFG (All shortest paths)

**Intuition / Approach:**  
Think of the problem like a graph, from `startWord` to `targetWord`.
- Each word represents a node.
- From a word at level `L`, we can change one letter to form multiple words at level `L+1`.
- Use BFS to traverse levels.
- Instead of storing just a word and its level (like Word Ladder 1), store the **entire path** as a list of words in the queue.
- **Important difference:** We cannot remove a word from the set immediately when inserting into the queue.
    - Example: At level 2, we have paths `{bat, pat}` and `{bat, bot}`.
        - Forming level 3: first pop `{bat, pat}` → add `{bat, pat, pot}`
        - Do not remove `pot` yet, because next path `{bat, bot}` can also form `{bat, bot, pot}`.
    - Remove words from the set **only after finishing the current level**.

**Code:**

```java
class Solution {
    public ArrayList<ArrayList<String>> findSequences(String startWord,
                                                      String targetWord,
                                                      String[] wordList) {
     HashSet<String> st=new HashSet<>();
     for(int i=0;i<wordList.length;i++) st.add(wordList[i]);
     
     Queue<ArrayList<String>> q=new LinkedList<>();
     ArrayList<String> l=new ArrayList<>();
     l.add(startWord);
     q.add(l);
     
     ArrayList<String> usedOnLevel=new ArrayList<>();
     usedOnLevel.add(startWord);
     int level=0;
     
     ArrayList<ArrayList<String>> ans=new ArrayList<>();
     
     while(q.size()>0){
         ArrayList<String> vec=q.remove();
         
         if(vec.size()>level){
             level++;
             for(String s:usedOnLevel){
                 st.remove(s);
             }
             usedOnLevel.clear();
         }
         
         if(vec.get(vec.size()-1).equals(targetWord)){
             if(ans.size()==0) ans.add(vec);
             else if(vec.size()==ans.get(0).size()) ans.add(vec);
             else break;
         }
         
         String word=vec.get(vec.size()-1);
         char[] wordArr=word.toCharArray();
         
         for(int i=0;i<word.length();i++){
             char orgChar=wordArr[i];
             for(char ch='a';ch<='z';ch++){
                 wordArr[i]=ch;
                 String newWord=new String(wordArr);
                 wordArr[i]=orgChar;
                 
                 if(st.contains(newWord)){
                     ArrayList<String> temp=new ArrayList<>(vec);
                     temp.add(newWord);
                     q.add(temp);
                     usedOnLevel.add(newWord);
                 }
             }
         }
     }
     
     return ans;
        
    }
}
```
Time Complexity  
For each word at a BFS level, we try 26 * L transformations (L = word length).  
In worst case, BFS processes all words → O(N * L * 26), where N = number of words.  
Maintaining paths increases overhead slightly but is still polynomial.

Space Complexity  
HashSet for dictionary: O(N)  
Queue can hold multiple paths: worst case O(N * L)  
Result list can hold multiple sequences of size up to O(N * L)  
Overall: O(N * L)
