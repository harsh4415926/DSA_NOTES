# Problem: Maximum Subarray
You are given an integer array nums. Your task is to find the contiguous subarray (containing at least one number) that has the largest sum and return that sum. This is a classic problem that can be solved efficiently using Kadane's Algorithm.

Example: Input: nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4] Output: 6 Explanation: The subarray [4, -1, 2, 1] has the largest sum of 6.

---
## Kadan's algorithm
````java
class Solution {
    public int maxSubArray(int[] nums) {
        int n=nums.length;
        // 'max' will store the maximum subarray sum found so far.
        int max=Integer.MIN_VALUE;
        // 'sum' will store the sum of the current contiguous subarray.
        int sum=0;
        
        // This is Kadane's Algorithm.
        for(int i=0;i<n;i++){
            // Add the current element to the running sum.
            sum+=nums[i];
            
            // Update the overall maximum sum if the current sum is greater.
            max=Math.max(max,sum);
            
            // If the running sum becomes negative, it's better to start a new subarray.
            // A negative sum will only decrease the sum of any future subarray.
            if(sum<0) {
                sum=0;
            }
        }
        return max;
        // Time Complexity: O(N) because it's a single pass through the array.
        // Space Complexity: O(1) as we only use a few variables.
    }
}
````

## if we were told to print the subarray
````java
class Solution {
public ArrayList<Integer> maxSubArray(int[] nums) {
int n = nums.length;
// 'maxSum' will store the maximum subarray sum found so far.
int maxSum = Integer.MIN_VALUE;
// 'currentSum' will store the sum of the current contiguous subarray.
int currentSum = 0;

        // 'start' tracks the beginning of the subarray being considered.
        int start = 0;
        // 'ansStart' and 'ansEnd' track the final start and end of the max subarray.
        int ansStart = -1, ansEnd = -1;

        // This is a modified Kadane's Algorithm to track indices.
        for (int i = 0; i < n; i++) {
            
            if(CurrentSum==0)start=i;
            
            // Add the current element to the running sum.
            currentSum += nums[i];

            // If the current sum is greater than the max sum found so far,
            // update the max sum and record the start and end indices.
            if (currentSum > maxSum) {
                maxSum = currentSum;
                ansStart = start;
                ansEnd = i;
            }

            // If the running sum becomes negative, it's better to start a new subarray.
            // A negative sum will only decrease the sum of any future subarray.
            if (currentSum < 0) {
                currentSum = 0;
                
            }
        }
        
        // Build the result list from the found indices.
        ArrayList<Integer> result = new ArrayList<>();
        if (ansStart != -1) {
            for (int i = ansStart; i <= ansEnd; i++) {
                result.add(nums[i]);
            }
        }
        
        return result;
        // Time Complexity: O(N) because it's a single pass through the array.
        // Space Complexity: O(1) for the algorithm, but O(K) for the returned list
        // where K is the length of the maximum subarray.
    }
}
````