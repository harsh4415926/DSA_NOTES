# Problem: Rotate Array
Given an integer array nums, rotate the array's elements to the right by k steps, where k is a non-negative integer. The array must be modified in-place, without creating a new one.

Example: Input: nums = [1, 2, 3, 4, 5, 6, 7], k = 3 Output: [5, 6, 7, 1, 2, 3, 4]

---
````java
class Solution {
    public void reverse(int[]  arr,int i,int j){
        // Reverses the elements of 'arr' from index 'i' to 'j'
        while(i<j){
            int temp=arr[i];
            arr[i]=arr[j];
            arr[j]=temp;
            i++;
            j--;
        }
    }
    public void rotate(int[] nums, int k) {
        int n=nums.length;
        // If k is larger than n, take the remainder
        k=k%n;

        // 1. Reverse the first part of the array (from start to n-k-1)
        reverse(nums,0,n-1-k);
        // 2. Reverse the second part of the array (from n-k to the end)
        reverse(nums,(n-1-k)+1,n-1);
        // 3. Reverse the entire array to get the final result
        reverse(nums,0,n-1);
    }
}
````