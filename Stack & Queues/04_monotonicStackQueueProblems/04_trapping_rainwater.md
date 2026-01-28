# trapping rainwater
## first approach(using two extra arrays for prefix sum and suffix sum)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 42 - Trapping Rain Water
 * ----------------------------------------------------------------------
 * Given n non-negative integers representing an elevation map where the 
 * width of each bar is 1, compute how much water it can trap after raining.
 * * STEPS:
 * 1. Precompute 'PrefixMax' (max height to the left) and 'SuffixMax' (max height to the right).
 * 2. For each bar, water = min(LeftMax, RightMax) - CurrentHeight.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int trap(int[] height) {
        int n = height.length;
        int[] prefixMax = new int[n];
        int[] suffixMax = new int[n];
        
        // STEP 1: Build Prefix Max Array (Left -> Right)
        // prefixMax[i] contains the maximum height encountered from index 0 to i
        for (int i = 0; i < n; i++) {
            if (i == 0) prefixMax[i] = height[i];
            else prefixMax[i] = Math.max(prefixMax[i - 1], height[i]);
        }

        // STEP 2: Build Suffix Max Array (Right -> Left)
        // suffixMax[i] contains the maximum height encountered from index n-1 to i
        for (int i = n - 1; i >= 0; i--) {
            if (i == n - 1) suffixMax[i] = height[i];
            else suffixMax[i] = Math.max(suffixMax[i + 1], height[i]);
        }

        int ans = 0;
        // STEP 3: Calculate trapped water for each position
        for (int i = 0; i < n; i++) {
            // Find the highest wall strictly to the left and strictly to the right
            int leftMax = (i - 1) >= 0 ? prefixMax[i - 1] : 0;
            int rightMax = (i + 1) < n ? suffixMax[i + 1] : 0;
            
            // Water can only be trapped if current bar is shorter than both 
            // the max bar to its left and the max bar to its right.
            if (height[i] < leftMax && height[i] < rightMax) {
                // The water level is determined by the shorter of the two bounding walls.
                ans += Math.min(leftMax, rightMax) - height[i];
            }
        }
        return ans;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - We traverse the array 3 times (Prefix loop, Suffix loop, Calculation loop).
 * * Space Complexity: O(N)
 * - We use two extra arrays (prefixMax and suffixMax) of size N.
 * - (Note: This can be optimized to O(1) space using the Two Pointer approach).
 * ----------------------------------------------------------------------
 */
````
## second approach(two pointer)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 42 - Trapping Rain Water (Two Pointer Approach)
 * ----------------------------------------------------------------------
 * Optimize space by determining the water level dynamically using two pointers.
 * * STEPS:
 * 1. Use two pointers (left, right) and track max heights (leftMax, rightMax).
 * 2. Move pointer with smaller height inward -> If current < sideMax, add water; else update sideMax.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int trap(int[] height) {
        int n = height.length;
        // Edge Case: Need at least 3 bars to trap any water (wall-valley-wall)
        if (n < 3) return 0;
        
        int left = 0;
        int right = n - 1;
        int leftMax = 0;
        int rightMax = 0;
        int ans = 0;
        
        while (left <= right) {
            // CRITICAL LOGIC: We always process the side with the smaller height.
            // Why? If height[left] < height[right], we know the water level at 'left'
            // is limited by 'leftMax' (because height[right] is taller, it acts as a reliable dam).
            if (height[left] < height[right]) {
                
                // If current bar is taller than max seen so far, it cannot trap water above it.
                if (height[left] >= leftMax) {
                    leftMax = height[left];
                } 
                // If current bar is shorter, water is trapped = (leftMax - current)
                else {
                    ans += leftMax - height[left];
                }
                left++; // Move left pointer
            } 
            // Symmetric logic for the right side
            else {
                if (height[right] >= rightMax) {
                    rightMax = height[right];
                } else {
                    ans += rightMax - height[right];
                }
                right--; // Move right pointer
            }
        }
        return ans;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - Single pass with two pointers meeting in the middle.
 * * Space Complexity: O(1)
 * - Uses only constant extra space for variables, unlike the O(N) array approach.
 * ----------------------------------------------------------------------
 */
````
