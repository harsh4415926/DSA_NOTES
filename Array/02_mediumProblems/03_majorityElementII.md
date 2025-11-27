# Problem: Majority Element II
You are given an integer array nums of size n. Your task is to find all the elements that appear more than ⌊n / 3⌋ times and return them in a list.

Note: There can be at most two such elements. This solution uses an extended Boyer-Moore Voting Algorithm to find the candidates in a single pass.

Example: Input: nums = [3, 2, 3] Output: [3]

Input: nums = [1, 2, 3] Output: []

Input: nums = [1, 1, 1, 3, 3, 2, 2, 2] Output: [1, 2]

---
## by moore's voting algorithm
````java
class Solution {
    /**
     * Finds all elements that appear more than n/3 times.
     * @param nums The input array.
     * @return A list of the majority elements.
     */
    public List<Integer> majorityElement(int[] nums) {
        int n = nums.length;

        // Boyer-Moore Voting Algorithm (Extended for two candidates)
        // We can have at most two elements that appear > n/3 times.
        int cand1 = nums[0];
        int cand2 = nums[0]; // Can be initialized to anything, will be overwritten.
        int count1 = 0;
        int count2 = 0;

        // Pass 1: Find the two potential candidates
        for (int i = 0; i < n; i++) {
            if (nums[i] == cand1) {
                count1++;
            } else if (nums[i] == cand2) {
                count2++;
            } else if (count1 == 0) {
                // First candidate is voted out, pick a new one
                cand1 = nums[i];
                count1 = 1;
            } else if (count2 == 0) {
                // Second candidate is voted out, pick a new one
                cand2 = nums[i];
                count2 = 1;
            } else {
                // Current element matches neither, "vote" against both
                count1--;
                count2--;
            }
        }
            
        // Pass 2: Verify the candidates
        // The first pass only gives us *potential* candidates.
        // We must re-count their actual occurrences.
        count1 = 0;
        count2 = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] == cand1) {
                count1++;
            } else if (nums[i] == cand2) {
                count2++;
            }
        }
                
        List<Integer> ans = new ArrayList<>();

        // Check if the first candidate is a majority element
        if (count1 > n / 3) {
            ans.add(cand1);
        }
        
        // Check the second candidate, ensuring it's not the same as the first
        if (count2 > n / 3 && cand2 != cand1) {
            ans.add(cand2);
        }
        
        return ans;

        // Time Complexity: O(N)
        // - We have two separate loops, each running 'n' times.
        // - O(N) + O(N) = O(N).

        // Space Complexity: O(1)
        // - We only use a few constant-space variables.
        // - The 'ans' list will have at most 2 elements, which is O(1).
    }
}
````