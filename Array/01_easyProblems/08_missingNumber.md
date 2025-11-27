# Problem: Missing Number
You are given an array nums containing n distinct numbers, taken from the range [0, 1, 2, ..., n]. Since the array only has n numbers, exactly one number from that range is missing. Your task is to find and return that missing number.

 Example: Input: nums = [3, 0, 1] 
 Output: 2 
 Explanation: n is 3, so the numbers are in the range [0, 3]. The number 2 is missing.
 
---
## xor method
````java
class Solution {
    public int missingNumber(int[] nums) {
        int n=nums.length;
        // xor1 will store the XOR of all numbers in the array
        int xor1=0;
        // xor2 will store the XOR of all numbers from 0 to n
        int xor2=0;

        // Loop from 0 to n-1
        for(int i=0;i<n;i++){
            // XOR the current array element into xor1
            xor1^=nums[i];
            // XOR the current index into xor2
            xor2^=i;
        }
        // XOR the final number 'n' into xor2 to complete the range [0, n]
        xor2^=n;
        
        // since XOR of two same numbers is 0 and X0R of 0 with a number is the number itself
        // The XOR of both sums cancels out all common numbers, leaving the missing one
        return xor1^xor2;
    }
}
````
