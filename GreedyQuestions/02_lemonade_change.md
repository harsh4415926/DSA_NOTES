# lemonade change
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 860 - Lemonade Change
 * ----------------------------------------------------------------------
 * LOGIC (Greedy):
 * 1. Track count of $5 and $10 bills (don't need to track $20s).
 * 2. If $5: Keep it.
 * 3. If $10: Give $5 back.
 * 4. If $20: Give $15 back. 
 * - GREEDY CHOICE: Prioritize giving ($10 + $5) over ($5 + $5 + $5).
 * - Why? $5 bills are more versatile (needed for $10 change).
 * ----------------------------------------------------------------------
 */

class Solution {
    public boolean lemonadeChange(int[] bills) {
        int five = 0;
        int ten = 0;
        
        for (int bill : bills) {
            
            if (bill == 5) {
                five++; // Collect $5
            }
            
            else if (bill == 10) {
                if (five == 0) return false; // Need $5 to give change
                five--;
                ten++;
            }
            
            else { // bill == 20
                // Option A: Give $10 + $5 (Best choice, save $5s)
                if (ten > 0 && five > 0) {
                    ten--;
                    five--;
                } 
                // Option B: Give $5 + $5 + $5 (Only if Option A fails)
                else if (five >= 3) {
                    five -= 3;
                } 
                else {
                    return false; // Cannot make $15 change
                }
            }
        }
        
        return true;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (One pass through bills).
 * Space: O(1) (Two integer variables).
 */
````