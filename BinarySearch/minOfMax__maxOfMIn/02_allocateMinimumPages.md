# allocate minimum pages(gfg)

````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Allocate Books (Minimize the Maximum Pages)
 * ----------------------------------------------------------------------
 * LOGIC (Binary Search on Answer):
 * 1. Goal: Distribute 'n' books to 'm' students such that the maximum 
 * number of pages assigned to any single student is MINIMIZED.
 * 2. Search Space: 
 * - Lowest possible max: max(arr) (A student must take at least the 
 * largest single book, since books cannot be split).
 * - Highest possible max: sum(arr) (One student takes all the books).
 * 3. `isPossible` strategy (Greedy): Keep assigning books to a student 
 * until adding the next book exceeds the `maxPages` limit. Then, move 
 * to the next student. If we need more than `m` students, it's invalid.
 * 4. Binary Search:
 * - If `mid` is valid, record it and try to find an even SMALLER maximum 
 * (search left).
 * - If invalid, the limit is too strict. We need to allow students to 
 * take more pages (search right).
 * ----------------------------------------------------------------------
 */

class Solution {

    // Helper: Checks if we can allocate books so no student reads > maxPages
    public static boolean isPossible(int[] arr, int n, int m, int maxPages) {
        int studentCount = 1; // Start with the first student
        int pageSum = 0;      // Pages currently assigned to this student

        for (int i = 0; i < n; i++) {
            
            // If adding the next book keeps them within the limit
            if (pageSum + arr[i] <= maxPages) {
                pageSum += arr[i];
            } 
            else {
                // Limit exceeded: Assign this book to a NEW student
                studentCount++;
                pageSum = arr[i]; // New student starts with this book

                // If we require more students than we have, it's impossible
                if (studentCount > m) {
                    return false;
                }
            }
        }

        return true; // Successfully allocated to 'm' or fewer students
    }

    public static int findPages(int[] arr, int n, int m) {
        // Edge case: Every student must get at least one book. 
        // If there are more students than books, it's impossible.
        if (m > n) return -1;

        int low = 0;
        int high = 0;

        // Establish boundaries for the binary search
        for (int i = 0; i < n; i++) {
            low = Math.max(low, arr[i]); // Min possible answer
            high += arr[i];              // Max possible answer
        }

        int ans = -1;

        // Binary search for the MINIMUM possible maximum pages
        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (isPossible(arr, n, m, mid)) {
                ans = mid;        // 'mid' works, save it
                high = mid - 1;   // Try to push the maximum even lower (search left)
            } 
            else {
                low = mid + 1;    // 'mid' is too small, need a larger page limit (search right)
            }
        }

        return ans;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N * log(sum - max)) where N is the number of books.
 * Space: O(1) tracking scalar variables.
 */
````
