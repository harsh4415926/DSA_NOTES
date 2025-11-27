# Problem: Majority Element
You are given an array nums of size n. The majority element is defined as the element that appears more than ⌊n / 2⌋ times. Your task is to find and return this element.

You can assume that the majority element always exists in the array. This solution should be efficient in both time and space.

Example: Input: nums = [2, 2, 1, 1, 1, 2, 2] Output: 2 Explanation: The number 2 appears 4 times, which is greater than 7 / 2.

---
## by moores voting algorithm
````java
class Solution {
    public int majorityElement(int[] nums) {
        int n=nums.length;
        int count=0;
        int elem=0; // Stores the candidate for the majority element

        // This is Moore's Voting Algorithm
        for(int i=0;i<n;i++){
            // If count is 0, we choose a new candidate for the majority element
            if(count==0){
                elem=nums[i];
                count++;
            }
            // If the current element matches our candidate, increment the count
            else if(nums[i]==elem){
                count++;
            }
            // If the current element is different, decrement the count
            else {
                count--;
            }
        }
        
        // Since the problem guarantees that a majority element always exists,
        // the final candidate 'elem' will be the answer.
        // A second pass to verify if count > n/2 is not needed.
        // if it was not given in the qn then you can verify it by traversing in array and counting freq of elem if it is more than n/2 or not
        

        return elem;
        // Time Complexity: O(N) because it's a single pass through the array.
        // Space Complexity: O(1) as we only use a few variables.
    }
}