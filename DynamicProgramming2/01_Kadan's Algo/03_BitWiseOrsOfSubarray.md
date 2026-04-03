````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 898 - Bitwise ORs of Subarrays
 * ----------------------------------------------------------------------
 * LOGIC (Dynamic Programming with Sets / Bitwise Math):
 * The magic of the Bitwise OR operator (|) is that bits can only turn ON, 
 * they never turn OFF. Because integers in Java/C++ are 32 bits,
 *  the value can only grow and change a maximum of 32 times before it becomes completely filled with 1s.
 *  This means that no matter how long the array is, if you keep track of the OR results ending at a specific index, 
 * there will never be more than 32 unique values.
 * 1. Goal: Find the total number of unique bitwise OR results across
 * all possible contiguous subarrays.
 * 2. 'prevOrs' tracks all valid bitwise ORs for subarrays ending exactly
 * at the previous index.
 * 3. At the current element 'x', a subarray ending at 'x' can either:
 * - Start and end at 'x' (just 'x' itself).
 * - Extend a previous subarray (represented by 'x | y' for 'y' in prevOrs).
 *
 * ----------------------------------------------------------------------
 */

import java.util.HashSet;
import java.util.Set;

class Solution {
    public int subarrayBitwiseORs(int[] arr) {
        // Stores all unique OR results across the entire array
        Set<Integer> globalSet = new HashSet<>();

        // Stores the unique OR results of subarrays ending at the PREVIOUS index
        Set<Integer> prevOrs = new HashSet<>();

        for (int x : arr) {
            // Stores the unique OR results of subarrays ending at the CURRENT index
            Set<Integer> currOrs = new HashSet<>();

            // 1. Start a new subarray here (just the element itself)
            currOrs.add(x);

            // 2. Extend all previous subarrays ending at the step before this one
            for (int y : prevOrs) {
                currOrs.add(x | y);
            }

            // Add all current results to our global tracker
            globalSet.addAll(currOrs);

            // Move forward: current becomes previous for the next iteration
            prevOrs = currOrs;
        }

        return globalSet.size();
    }
}

/*
 * COMPLEXITY:
 * Time: O(32 * N) -> O(N). Because 'prevOrs' never exceeds 32 elements, the inner loop runs in O(1) time.
 * Space: O(32 * N) -> O(N) worst case to store all unique results in 'globalSet'.
 */


````
