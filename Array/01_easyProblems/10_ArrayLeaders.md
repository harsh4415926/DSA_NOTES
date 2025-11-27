# Problem: Leaders in an Array
You are given an integer array arr. Your task is to find all the "leaders" in the array. An element is considered a leader if it is greater than or equal to all the elements to its right side. The rightmost element is always a leader by default. The leaders should be returned in the same order they appeared in the original array.

Example: Input: arr = [16, 17, 4, 3, 5, 2] Output: [17, 5, 2] Explanation: 17, 5, and 2 are leaders because they are greater than or equal to all elements on their right.

---
````java
class Solution {
    static ArrayList<Integer> leaders(int arr[]) {
        // code here
        int n=arr.length;
        // 'max' stores the maximum element found so far while iterating from the right.
        int max=arr[n-1];
        ArrayList<Integer> ans=new ArrayList<>();
        
        // The rightmost element is always a leader.
        ans.add(arr[n-1]);
        
        // Iterate from the second-to-last element down to the beginning.
        for(int i=n-2;i>=0;i--){
            // If the current element is greater or equal to the max from its right, it's a leader.
            if(arr[i]>=max){
                ans.add(arr[i]);
            }
            // Update the maximum element seen so far.
            max=Math.max(max,arr[i]);
        }
        
        // The leaders were added in reverse order (right-to-left).
        // Reverse the list to get the correct order (left-to-right).
        Collections.reverse(ans);
        return ans;
        
        // Time Complexity: O(N)
        // - The first loop runs O(N) times.
        // - Collections.reverse() takes O(N) time in the worst case (or O(K) where K is num of leaders).
        // - Total: O(N) + O(N) = O(N).

        // Space Complexity: O(K)
        // - Where K is the number of leaders found.
        // - In the worst case (e.g., [5, 4, 3, 2, 1]), K = N, so space is O(N).
    }
}
````