# median of two sorted Arrays

## brute force
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 4 - Median of Two Sorted Arrays
 * ----------------------------------------------------------------------
 * LOGIC (Brute Force / Merge):
 * 1. Create a new array large enough to hold elements from both arrays.
 * 2. Use two pointers (i, j) to iterate through both arrays simultaneously.
 * 3. Compare the elements at the pointers and place the smaller one
 * into the new merged array.
 * 4. Once one array is exhausted, copy the remaining elements from the
 * other array.
 * 5. Finally, find the median using the fully merged and sorted array:
 * - If length is odd: return the exact middle element.
 * - If length is even: return the average of the two middle elements.
 * ----------------------------------------------------------------------
 */

class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {

        // Create an array to hold the merged result
        int[] arr = new int[nums1.length + nums2.length];

        int i = 0;   // Pointer for nums1
        int j = 0;   // Pointer for nums2
        int idx = 0; // Pointer for the merged array

        // Traverse both arrays and merge them in sorted order
        while(i < nums1.length && j < nums2.length) {
            if(nums1[i] <= nums2[j]) {
                arr[idx++] = nums1[i];
                i++;
            }
            else {
                arr[idx++] = nums2[j];
                j++;
            }
        }

        // If elements are left in nums1, copy them over
        while(i < nums1.length) {
            arr[idx++] = nums1[i];
            i++;
        }

        // If elements are left in nums2, copy them over
        while(j < nums2.length) {
            arr[idx++] = nums2[j];
            j++;
        }

        // If total length is odd, the median is the middle element
        if(arr.length % 2 == 1) return (double) arr[arr.length / 2];

        // If total length is even, the median is the average of the two middle elements
        return (arr[(arr.length / 2) - 1] + arr[arr.length / 2]) / 2.0;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N1 + N2) (We iterate through both arrays to merge them).
 * Space: O(N1 + N2) (We allocate a new array to store all elements).
 * Note: While highly intuitive, this approach does not meet the O(log (m+n)) requirement of the problem statement.
 */
````

## optimised (binary search)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 4 - Median of Two Sorted Arrays
 * ----------------------------------------------------------------------
 * LOGIC (Binary Search on Partitions):
 * 1. Goal: Partition BOTH arrays into a Left Half and a Right Half such 
 * that all elements in the Left Half <= all elements in the Right Half.
 * 2. To optimize, always run the binary search on the SMALLER array.
 * 3. 'mid1': Number of elements taken from nums1 for the left partition.
 * 4. 'mid2': Remaining elements needed from nums2 for the left partition.
 * 5. Valid Partition Condition:
 * - l1 <= r2 (Max of nums1 left half <= Min of nums2 right half)
 * - l2 <= r1 (Max of nums2 left half <= Min of nums1 right half)
 * 6. Median calculation:
 * - Odd total length: max(l1, l2)
 * - Even total length: (max(l1, l2) + min(r1, r2)) / 2.0
 * ----------------------------------------------------------------------
 */

class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int n1 = nums1.length;
        int n2 = nums2.length;
        
        // Ensure nums1 is always the smaller array to minimize the search space.
        // This also prevents 'mid2' from becoming negative.
        if (n2 < n1) return findMedianSortedArrays(nums2, nums1);

        int low = 0;
        int high = n1;
        
        // Total elements required in the combined left partition.
        // Adding 1 before dividing by 2 neatly handles both even and odd total lengths.
        int totalRequired = (n1 + n2 + 1) / 2;
        
        while (low <= high) {
            
            // Elements to take from the smaller array (nums1)
            int mid1 = low + (high - low) / 2;
            
            // Elements to take from the larger array (nums2) to satisfy the total required
            int mid2 = totalRequired - mid1;

            // Define boundaries for the partitions.
            // Use MIN_VALUE / MAX_VALUE to handle cases where a partition is completely empty.
            int l1 = (mid1 - 1) >= 0 ? nums1[mid1 - 1] : Integer.MIN_VALUE; // Max of left nums1
            int l2 = (mid2 - 1) >= 0 ? nums2[mid2 - 1] : Integer.MIN_VALUE; // Max of left nums2
            int r1 = mid1 < n1 ? nums1[mid1] : Integer.MAX_VALUE;           // Min of right nums1
            int r2 = mid2 < n2 ? nums2[mid2] : Integer.MAX_VALUE;           // Min of right nums2

            // CROSS-CHECK: Is the partition valid?
            if (l1 <= r2 && l2 <= r1) {
                // If the total number of elements is even, average the two middle values
                return ((n1 + n2) % 2 == 0) ?
                    (Math.max(l1, l2) + Math.min(r1, r2)) / 2.0 : 
                    Math.max(l1, l2); // If odd, the median is just the max of the left side
            }
            
            // l1 is too large. We need to shrink the left partition of nums1.
            else if (l1 > r2) {
                high = mid1 - 1;
            }
            
            // l2 is too large (meaning we didn't take enough elements from nums1).
            // We need to expand the left partition of nums1.
            else {
                low = mid1 + 1;
            }
        }

        return -1.0; // Fallback (should never be reached for valid input arrays)
    }
}

/*
 * COMPLEXITY:
 * Time: O(log(min(N1, N2))) - We only binary search the smaller array.
 * Space: O(1) - Only a few tracking variables are used.
 */
````
