# kth element of two sorted arrays (gfg)
## optimal (binary search)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: K-th Element of Two Sorted Arrays
 * ----------------------------------------------------------------------
 * LOGIC (Binary Search on Partitions):
 * 1. This is a direct variation of the "Median of Two Sorted Arrays" problem.
 * 2. Goal: Partition the arrays such that exactly 'k' elements are on the 
 * left side across both arrays. The answer will be the max of the left side.
 * 3. Search Space Adjustments (Crucial):
 * - Minimum elements to take from 'a': If 'k' is greater than 'n2', we 
 * MUST take at least (k - n2) elements from 'a'. Otherwise, 0.
 * - Maximum elements to take from 'a': We cannot take more than 'k' 
 * elements, nor more than 'n1' (total size of 'a').
 * 4. Valid Partition: l1 <= r2 && l2 <= r1.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int kthElement(int a[], int b[], int k) {

        int n1 = a.length;
        int n2 = b.length;

        // Optimization: Always binary search on the smaller array 
        // to minimize the search space and prevent out-of-bounds issues.
        if(n1 > n2) return kthElement(b, a, k);

        // Calculate the valid range of elements we can pick from array 'a'
        int low = Math.max(0, k - n2); 
        int high = Math.min(k, n1);

        while(low <= high) {

            // Elements to pick from array 'a'
            int mid1 = (low + high) / 2;
            // Elements to pick from array 'b' to ensure exactly 'k' elements on the left
            int mid2 = k - mid1;

            // Determine boundary values, using MIN_VALUE/MAX_VALUE for empty partitions
            int l1 = (mid1 == 0) ? Integer.MIN_VALUE : a[mid1 - 1]; // Max of left 'a'
            int l2 = (mid2 == 0) ? Integer.MIN_VALUE : b[mid2 - 1]; // Max of left 'b'

            int r1 = (mid1 == n1) ? Integer.MAX_VALUE : a[mid1];    // Min of right 'a'
            int r2 = (mid2 == n2) ? Integer.MAX_VALUE : b[mid2];    // Min of right 'b'

            // CROSS-CHECK: Is the partition valid?
            if(l1 <= r2 && l2 <= r1) {
                // Since the left partition holds exactly 'k' elements, 
                // the largest element among them is the k-th element overall.
                return Math.max(l1, l2);
            }
            
            // l1 is too large, meaning we picked too many elements from 'a'.
            // Shrink the left partition of 'a'.
            else if(l1 > r2) {
                high = mid1 - 1;
            }
            
            // l2 is too large, meaning we didn't pick enough elements from 'a'.
            // Expand the left partition of 'a'.
            else {
                low = mid1 + 1;
            }
        }

        return -1; // Fallback
    }
}

/*
 * COMPLEXITY:
 * Time: O(log(min(N1, N2))) - We only perform binary search on the smaller array.
 * Space: O(1) - Only tracking boundary pointers.
 */
````