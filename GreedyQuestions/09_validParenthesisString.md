# valid parenthesis String

## recustion+dp
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 678 - Valid Parenthesis String
 * ----------------------------------------------------------------------
 * LOGIC (Recursion + Memoization):
 * 1. State: `idx` (current string position) & `count` (balance of open brackets).
 * 2. Valid path: `count` must never drop below 0. At the end, `count` must be 0.
 * 3. Transitions:
 * - '(': increment count.
 * - ')': decrement count.
 * - '*': Branch into 3 choices (treat as empty, open, or close).
 * 4. Memoize results in `dp[][]` to avoid redundant calculations.
 * ----------------------------------------------------------------------
 */

class Solution {
    // helper returns 1 (true) or 0 (false)
    public int helper(int idx, String s, int count, int[][] dp) {
        
        // Base case 1: Invalid state (more closing than opening brackets)
        if (count < 0) return 0;
        
        // Base case 2: Reached end. Valid if all open brackets are closed.
        if (idx == s.length()) {
            if (count == 0) return 1;
            else return 0;
        }
        
        // Return memoized result
        if (dp[idx][count] != -1) return dp[idx][count]; 
        
        char ch = s.charAt(idx);
        
        if (ch == '(') {
            return dp[idx][count] = helper(idx + 1, s, count + 1, dp);
        } 
        else if (ch == ')') {
            return dp[idx][count] = helper(idx + 1, s, count - 1, dp);
        } 
        else { // ch == '*'
            // Try all 3 possibilities for '*'
            int emptyString = helper(idx + 1, s, count, dp);     // Treat as ""
            int open = helper(idx + 1, s, count + 1, dp);        // Treat as "("
            int close = helper(idx + 1, s, count - 1, dp);       // Treat as ")"
            
            // If ANY path is valid, this state is valid
            if (emptyString == 1 || open == 1 || close == 1) {
                return dp[idx][count] = 1;
            }
        }
        
        return dp[idx][count] = 0;
    }
    
    public boolean checkValidString(String s) {
        int n = s.length();
        // Max possible open brackets count is 'n', so size is [n][n+1]
        int[][] dp = new int[n][n + 1];
        
        for (int i = 0; i < n; i++) {
            Arrays.fill(dp[i], -1);
        }
        
        return helper(0, s, 0, dp) == 1;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N^2) (State space is N indices * N max open brackets).
 * Space: O(N^2) (DP Array + Recursion Stack).
 
````
## greedy(optimal)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 678 - Valid Parenthesis String
 * ----------------------------------------------------------------------
 * LOGIC (Greedy O(N)):
 * 1. Track a RANGE of possible open brackets: [low, high].
 * - `low`: Minimum possible open brackets (treating '*' as ')').
 * - `high`: Maximum possible open brackets (treating '*' as '(').
 * 2. '(': Both bounds increase.
 * 3. ')': Both bounds decrease.
 * 4. '*': `high` increases (try '('), `low` decreases (try ')').
 * 5. Rules:
 * - If `high < 0`: Too many ')', absolutely invalid.
 * - If `low < 0`: Can't have negative open brackets. Reset `low = 0` 
 * (this just means we chose '*' as "" instead of ')').
 * 6. Valid if `low == 0` at the end.
 * ----------------------------------------------------------------------
 */

class Solution {
    public boolean checkValidString(String s) {
        int low = 0;   // Min open brackets possible
        int high = 0;  // Max open brackets possible
        
        for (char ch : s.toCharArray()) {
            
            if (ch == '(') {
                low++;
                high++;
            } 
            else if (ch == ')') {
                low--;
                high--;
            } 
            else { // '*'
                low--;      // Case 1: '*' acts as ')' (decreases open count)
                high++;     // Case 2: '*' acts as '(' (increases open count)
                            // Case 3: '*' acts as "" (bounds implicitly cover this)
            }
            
            // Check 1: Even with max possible '(', we still have too many ')'
            if (high < 0) {
                return false;
            }
            
            // Check 2: We can't have negative open brackets.
            // If low < 0, it means some '*' we forced to be ')' made the string invalid.
            // We just flip those '*' to be "" instead, resetting low to 0.
            if (low < 0) {
                low = 0;
            }
        }
        
        // Valid if the minimum possible open bracket count is exactly 0
        return low == 0;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (Single pass).
 * Space: O(1) (Only tracking two variables).
 */
````