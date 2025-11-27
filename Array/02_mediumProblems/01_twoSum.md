# Problem: Two Sum
Given an array of integers nums and an integer target, your task is to find and return the indices of two numbers in the array that add up to the target.

You can assume that each input will have exactly one solution, and you may not use the same element twice. The order of the returned indices does not matter.

Example: Input: nums = [2, 7, 11, 15], target = 9 Output: [0, 1] Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].

---
## brute force 
````
compare every element with every other element
````

## better approach(using hashmap)
````java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int n=nums.length;
        // HashMap to store numbers we've seen and their indices
        HashMap<Integer,Integer> map=new HashMap<>();
        
        for(int i=0;i<n;i++){
            // Calculate the complement needed to reach the target
            int complement = target-nums[i];
            
            // Check if the complement exists in the map
            if(map.containsKey(complement)){
                // If it exists, we found a pair. Return their indices.
                return new int[]{map.get(complement),i};
            }
            else{
                // If complement is not found, add the current number and its index to the map
                map.put(nums[i],i);
            }
        }
        // Return a default value if no solution is found (per problem statement, this won't be reached)
        return new int[]{-1,-1};
        // Time Complexity: O(N) because we iterate through the array once. HashMap lookups are O(1) on average.
        // Space Complexity: O(N) because, in the worst case, the HashMap might store all N elements.
    }
}
````
## 2nd approach (to tell me sum exists or not)
````java
class Solution {
boolean twoSum(int arr[], int target) {
// Sort the array to enable the two-pointer approach
Arrays.sort(arr);
int n=arr.length;

        // Initialize pointers at the start and end of the array
        int left=0;
        int right=n-1;
        
        // Continue as long as the pointers haven't crossed
        while(left<right){
            int currentSum = arr[left]+arr[right];
            // If the sum is too large, decrease it by moving the right pointer
            if(currentSum>target) {
                right--;
            }
            // If the sum is too small, increase it by moving the left pointer
            else if(currentSum<target) {
                left++;
            }
            // If the sum matches the target, we've found a pair
            else {
                return true;
            }
        }
        // If the loop finishes, no pair was found
        return false;
        // Time Complexity: O(N log N) due to the sorting step. The subsequent two-pointer scan is O(N).
        // Space Complexity: O(1) or O(log N), depending on the space used by the sorting algorithm.
    }
}

````

