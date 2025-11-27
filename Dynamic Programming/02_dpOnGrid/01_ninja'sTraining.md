# ninjas training(gfg as geeks training)
## Problem Statement
This code solves the "Ninja's Training" problem 🥋. A ninja trains for N days. Each day, there are three possible activities, each earning a specific number of points. The only rule is that the ninja cannot do the same activity on two consecutive days. The goal is to find the maximum total points the ninja can achieve over the N days.

d## memoization
````java
import java.util.Arrays;

class Solution {
    // Recursive helper to find max points from day 0 to 'idx'.
    public int helper(int idx, int last, int[][] arr, int[][] dp) {
        // Base case: On day 0, pick the best valid activity.
        if (idx == 0) {
            int max = 0;
            for (int i = 0; i < 3; i++) {
                if (i != last) {
                    max = Math.max(max, arr[0][i]);
                }
            }
            return max;
        }

        // If this state is already computed, return the result.
        if (dp[idx][last] != -1) return dp[idx][last];

        int max = 0;
        // Try all valid activities for the current day 'idx'.
        for (int i = 0; i < 3; i++) {
            // Constraint: current activity 'i' must not be the same as 'last'.
            if (i != last) {
                // Points = current activity's points + max points from previous days.
                int points = arr[idx][i] + helper(idx - 1, i, arr, dp);
                max = Math.max(max, points);
            }
        }
        
        // Store the result in the dp table and return it.
        return dp[idx][last] = max;
    }

    public int maximumPoints(int arr[][]) {
        int n = arr.length;
        // dp[day][last_activity] stores the max points.
        // The 4th column is for the initial call where no activity is restricted.
        int[][] dp = new int[n][4];
        for (int row[] : dp) {
            Arrays.fill(row, -1);
        }
        
        // Start from the last day. Pass '3' as 'last' to indicate no restrictions.
        return helper(n - 1, 3, arr, dp);
    }

    // --- Complexity Analysis ---
    // Time Complexity: O(N)
    // Space Complexity: O(N) for the DP table and recursion stack.
}
````
## tabulation
````java
class Solution {
    public int maximumPoints(int arr[][]) {
        // code here
        int n=arr.length;
        int[][] dp=new int[n][4];
        dp[0][0]=Math.max(arr[0][1],arr[0][2]);
        dp[0][1]=Math.max(arr[0][0],arr[0][2]);
        dp[0][2]=Math.max(arr[0][0],arr[0][1]);
        dp[0][3]=Math.max(arr[0][0],Math.max(arr[0][1],arr[0][2]));
        for(int i=1;i<n;i++){
            for(int j=0;j<4;j++){
                int max=0;
                int points=0;
                for(int k=0;k<3;k++){
                    if(k!=j)
                     points=arr[i][k]+dp[i-1][k];
                    max=Math.max(max,points);
                }
                dp[i][j]=max;
            }
        }
        int ans=0;
        for(int i=0;i<4;i++){
            ans=Math.max(dp[n-1][i],ans);
        }
        return ans;
    }
}