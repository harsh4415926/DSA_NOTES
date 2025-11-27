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
````
## better solution(hashmap)
````java
class Solution {
    public int longestSubarray(int[] arr, int k) {
        int n=arr.length;
        // HashMap to store the first occurrence of each prefix sum
        HashMap<Integer,Integer> map=new HashMap<>();
        int len=0;
        int sum=0;
        for(int i=0;i<n;i++){
            // Calculate the prefix sum up to the current index
            sum+=arr[i];
            // If a prefix sum hasn't been seen before, store it with its index
            // we will not update value of map[sum] if it already exists because we will need the longest
            //      length of subarray so we must keep the track of the leftmost occurence of the prefixsum
            if(!map.containsKey(sum)){
                map.put(sum,i);
            }

            // Case 1: The subarray from the beginning (index 0) sums to k
            if(sum==k){
                len=Math.max(len,i+1);
            }
            // Case 2: Check if a subarray with sum k exists elsewhere
            else{
                int remSum=sum-k;
                // If (sum - k) is found in the map, a subarray with sum k exists
                if(map.containsKey(remSum)){
                    // The length is the current index minus the index where remSum was found
                    len=Math.max(len,i-map.get(remSum));
                }
            }
        }
        return len;
        // Time Complexity: O(N) because we traverse the array once.
        // Space Complexity: O(N) in the worst case, as the HashMap can store up to N unique prefix sums.
    }
}


````
## optimal approach (only for array with positives )
````java
class Solution {
    public int longestSubarray(int[] arr, int k) {
        int n=arr.length;
        // Initialize two pointers for the sliding window
        int left=0;
        int right=0;
        
        long sum=0; // Use long to avoid overflow if numbers are large
        int maxLen=0;
        
        // Iterate through the array with the right pointer
        while(right < n){
            // Add the current element to the window sum
            sum += arr[right];
            
            // If sum exceeds k, shrink the window from the left
            while(sum > k){
                sum -= arr[left];
                left++;
            }
            
            // If the window sum is exactly k, update the max length
            if(sum == k){
                maxLen = Math.max(maxLen, right - left + 1);
            }
            
            // Expand the window to the right
            right++;
        }
        return maxLen;
        // Time Complexity: O(N) because each pointer (left and right) traverses the array at most once.
        // Space Complexity: O(1) as we are not using any extra space that scales with input size.
    }
}
````

