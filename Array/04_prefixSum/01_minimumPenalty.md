# problem : minimum penalty for a shop (leetcode)

## O(n) time + O(n) space
````java
class Solution {
    public int bestClosingTime(String customers) {
        int n = customers.length();
        
        // come[j] = how many 'Y' (customers) from index j to the end
        int[] come = new int[n]; 
        // notCome[i] = how many 'N' (no customers) from index 0 to i
        int[] notCome = new int[n]; 

        // Fill the prefix/suffix arrays in a single loop
        for (int i = 0; i < n; i++) {
            // Calculate prefix 'N's (notCome) from the left
            if (i == 0) {
                notCome[i] = (customers.charAt(i) == 'N' ? 1 : 0);
            } else {
                notCome[i] = notCome[i - 1] + (customers.charAt(i) == 'N' ? 1 : 0);
            }
            
            // Calculate suffix 'Y's (come) from the right
            int j = n - 1 - i; // j goes from n-1 down to 0
            if (j == n - 1) {
                come[j] = (customers.charAt(j) == 'Y' ? 1 : 0);
            } else {
                come[j] = come[j + 1] + (customers.charAt(j) == 'Y' ? 1 : 0);
            }
        }

        int penalty = Integer.MAX_VALUE;
        int ans = 0; // Stores the best hour to close

        // Iterate through all possible closing hours (0 to n)
        for (int i = 0; i <= n; i++) {
            // if closing the shop at the ith hour:
            // Open hours: [0, i-1] -> Penalty = 'N's in this range
            // Closed hours: [i, n-1] -> Penalty = 'Y's in this range
            
            int leftPenalty = 0;  // Penalty from 'N' when open
            int rightPenalty = 0; // Penalty from 'Y' when closed

            // Penalty for [0, i-1]
            if (i != 0) {
                leftPenalty = notCome[i - 1];
            }
            
            // Penalty for [i, n-1]
            if (i != n) {
                rightPenalty = come[i];
            }
            
            // Check if this hour gives a new minimum penalty
            if (leftPenalty + rightPenalty < penalty) {
                penalty = leftPenalty + rightPenalty;
                ans = i;
            }
        }
        return ans;
        
        // Time Complexity: O(N)
        // - O(N) to build the 'come' and 'notCome' arrays.
        // - O(N) to check all possible closing hours (0 to n).
        // - Total: O(N) + O(N) = O(N).

        // Space Complexity: O(N)
        // - O(N) for the 'come' array.
        // - O(N) for the 'notCome' array.
        // - Total: O(N).
    }
}
````
## O(n) time+ O(1) space
````java
class Solution {
    public int bestClosingTime(String customers) {
        int n=customers.length();

        // precalculating penalty if closed shop at 0th hour
        int penalty=0;
        for(int i=0;i<n;i++){
            if(customers.charAt(i)=='Y')penalty++;
        }
        int minPenalty=penalty;
        int minHour=0;
        for (int i =1 ; i<=n ;i++){
            if(customers.charAt(i-1)=='Y') penalty--;
            else penalty++;
            if(penalty < minPenalty){
                minPenalty=penalty;
                minHour=i;
            }
        }
      return minHour ;
    }
}
````
