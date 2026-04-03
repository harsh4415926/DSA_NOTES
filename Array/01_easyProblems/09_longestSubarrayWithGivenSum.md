# Problem: Longest Subarray with Sum K (coding ninjas) [link](https://www.naukri.com/code360/problems/longest-subarray-with-sum-k_10870953?leftPanelTabValue=PROBLEM)
You are given an array of integers arr and a target integer k. Your task is to find the length of the longest contiguous subarray whose elements sum up to exactly k.

Example: Input: arr = [10, 5, 2, 7, 1, 9], k = 15 Output: 4 Explanation: The subarray [5, 2, 7, 1] sums to 15, and it is the longest such subarray with a length of 4

array may contain negative positives and zeroes

---

## brute force
````java
class Solution {
    public int longestSubarray(int[] arr, int k) {
        int n=arr.length;
        int maxLen=0;
        // Iterate through all possible starting points of a subarray
        for(int i=0;i<n;i++){
            int sum=0;
            // Iterate through all possible ending points for a subarray starting at i
            for(int j=i;j<n;j++){
                // Add the current element to the sum
                sum+=arr[j];
                // If the sum equals k, update the maximum length found so far
                if(sum==k){
                    maxLen=Math.max(maxLen,j-i+1);
                }
            }
        }
        return maxLen;
    }
}

// time- O(n²)
// space-O(1)
````
## better solution(hashmap)
````java
import java.util.*;

class Solution {
    public int longestSubarray(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<>();
        int sum = 0, maxLen = 0;

        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];

            if (sum == k) {
                maxLen = i + 1;
            }

            if (map.containsKey(sum - k)) {
                maxLen = Math.max(maxLen, i - map.get(sum - k));
            }

            // store only first occurrence
            if (!map.containsKey(sum)) {
                map.put(sum, i);
            }
        }
        return maxLen;
    }
}

// time- O(n) 
// space- O(n)

````
## sliding window (only for array with positives )
````java
class Solution {
    public int longestSubarray(int[] nums, int k) {
        int left = 0, sum = 0, maxLen = 0;

        for (int right = 0; right < nums.length; right++) {
            sum += nums[right];

            while (sum > k && left <= right) {
                sum -= nums[left];
                left++;
            }

            if (sum == k) {
                maxLen = Math.max(maxLen, right - left + 1);
            }
        }
        return maxLen;
    }
}

// time- O(n)
// space- O(1)
````

