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
    public List<Integer> majorityElement(int[] nums) {
        int n = nums.length;

        // Step 1: Find candidates
        int candidate1 = 0, candidate2 = 0;
        int count1 = 0, count2 = 0;

        for (int num : nums) {
            if (candidate1 == num) {
                count1++;
            } else if (candidate2 == num) {
                count2++;
            } else if (count1 == 0) {
                candidate1 = num;
                count1 = 1;
            } else if (count2 == 0) {
                candidate2 = num;
                count2 = 1;
            } else {
                count1--;
                count2--;
            }
        }

        // Step 2: Verify candidates
        count1 = 0;
        count2 = 0;

        for (int num : nums) {
            if (num == candidate1) count1++;
            else if (num == candidate2) count2++;
        }

        // Step 3: Add valid results
        List<Integer> result = new ArrayList<>();

        if (count1 > n / 3) result.add(candidate1);
        if (count2 > n / 3) result.add(candidate2);

        return result;
    }
}
        // Time Complexity: O(N)
        // - We have two separate loops, each running 'n' times.
        // - O(N) + O(N) = O(N).

        // Space Complexity: O(1)
        // - We only use a few constant-space variables.
        // - The 'ans' list will have at most 2 elements, which is O(1).
    

````