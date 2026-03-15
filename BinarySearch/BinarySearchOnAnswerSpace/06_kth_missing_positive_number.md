# kth missing positive number
## brute force 1
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 1539 - Kth Missing Positive Number
 * ----------------------------------------------------------------------
 * LOGIC (Linear Search):
 * 1. The number of missing integers before any element arr[i] is 
 * strictly: arr[i] - (i + 1).
 * (e.g., if arr[2] = 5, expected is 3. Missing count = 5 - 3 = 2).
 * 2. As we iterate, we check if the missing count reaches or exceeds 'k'.
 * 3. If missing >= k, the k-th missing number is somewhere BEFORE arr[i].
 * We calculate its exact value by stepping backward from arr[i].
 * 4. If the loop finishes, the k-th missing number falls AFTER the 
 * last element of the array.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int findKthPositive(int[] arr, int k) {
        int n = arr.length;
        
        for (int i = 0; i < n; i++) {
            // Count how many numbers are missing before the current element
            int missing = arr[i] - (i + 1);
            
            // If the missing count reaches 'k', our target is before arr[i]
            if (missing >= k) {
                // Step back from arr[i] by the excess missing numbers
                // Note: This mathematically simplifies to (i + k)
                return (arr[i] - (missing - k) - 1);
            }
        }
        
        // If we exhausted the array, the answer is beyond the last element.
        // The last element is at index n-1, so missing numbers continue from there.
        return n + k;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (Linear scan in the worst case).
 * Space: O(1) (Only tracking scalar variables).
 */
````

## brute force 2
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 1539 - Kth Missing Positive Number
 * ----------------------------------------------------------------------
 * LOGIC (Shift-based Linear Search):
 * 1. Assume the array is empty. If so, the k-th missing number is simply 'k'.
 * 2. As we iterate through the array, if we see a number that is less than 
 * or equal to our current guess 'k', it means one of the spots we counted 
 * as "missing" is actually present.
 * 3. To compensate for this "stolen" spot, we shift our guess 'k' forward by 1.
 * 4. Once we encounter an array element strictly greater than 'k', it can 
 * no longer affect our missing count up to 'k', so we break.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int findKthPositive(int[] arr, int k) {
        int n = arr.length;
        
        for(int i = 0; i < n; i++) {
            // If the current number is smaller than or equal to our target 'k',
            // it occupies a position that would otherwise be a missing number.
            if(arr[i] <= k) {
                k++; // Shift the target forward to account for the present number
            } 
            else {
                // Once arr[i] > k, the current and future array elements are 
                // too large to interfere with the first 'k' missing numbers.
                break;
            }
        }
        
        // Return the appropriately shifted 'k'
        return k;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) (Linear scan, stops early if arr[i] > k).
 * Space: O(1) (Only tracking 'k' and loop variables).
 */
````

## optimal (binary Search)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 1539 - Kth Missing Positive Number
 * ----------------------------------------------------------------------
 * LOGIC (Binary Search O(log N)):
 * 1. The number of missing elements before index 'mid' is exactly 
 * arr[mid] - (mid + 1).
 * 2. If missing < k: The k-th missing number is further to the right.
 * 3. If missing >= k: The k-th missing number is to the left.
 * 4. After the loop, 'high' points to the closest index where missing < k.
 * - Answer = arr[high] + (k - missing_at_high)
 * - Answer = arr[high] + k - (arr[high] - (high + 1))
 * - Answer = high + 1 + k
 * 5. Since the loop terminates when low = high + 1, we can beautifully 
 * simplify the final return statement to: low + k.
 * ----------------------------------------------------------------------
 */

class Solution {
    public int findKthPositive(int[] arr, int k) {
        int n = arr.length;
        
        int low = 0;
        int high = n - 1;
        
        // Binary Search for the boundary where missing numbers >= k
        while (low <= high) {
            int mid = low + (high - low) / 2;
            
            // Calculate how many numbers are missing before arr[mid]
            int missing = arr[mid] - (mid + 1);
            
            if (missing < k) {
                // Not enough missing numbers yet, search right
                low = mid + 1;
            } 
            else {
                // Too many (or exactly enough) missing numbers, search left
                high = mid - 1;
            }
        }
        
        // The mathematical simplification of: arr[high] + (k - missing)
        // Since low = high + 1 at the end of the loop, this reduces to low + k.
        return low + k;
    }
}

/*
 * COMPLEXITY:
 * Time: O(log N) (Binary Search).
 * Space: O(1) (Only tracking pointers).
 */
````