# Problem: Longest Consecutive Sequence
You are given an unsorted array of integers arr. Your task is to find the length of the longest sequence of consecutive elements. The sequence does not need to be contiguous in the original array, but the numbers themselves must be consecutive (e.g., 100, 1, 3, 2 has a consecutive sequence [1, 2, 3] of length 3).

Example: Input: arr = [100, 4, 200, 1, 3, 2] Output: 4 Explanation: The longest consecutive sequence is [1, 2, 3, 4].

---
## better solution
````java
public class Solution {
    public static int lengthOfLongestConsecutiveSequence(int[] arr) {
        // Write your code her
        int n = arr.length;
        if (n == 0) return 0; // Handle empty array case

        // Sort the array to bring consecutive elements together.
        Arrays.sort(arr);

        int maxLen = 1; // Stores the maximum length found
        int len = 1;    // Stores the current consecutive length
        
        for (int i = 1; i < n; i++) {
            // Check if the current element is part of the sequence
            if (arr[i] == arr[i - 1] + 1 || arr[i] == arr[i - 1]) {
                
                // Only increment length if it's the next number (not a duplicate)
                if (arr[i] == arr[i - 1] + 1)
                    len++;

            } else {
                // The sequence is broken, reset the length
                len = 1;
            }

            // Update the maximum length found so far
            maxLen = Math.max(len, maxLen);
            
            
        }
        return maxLen;

        // Time Complexity: O(N log N)
        // - Arrays.sort() dominates the runtime.
        // - The single pass loop takes O(N).
        // - Total: O(N log N) + O(N) = O(N log N).

        // Space Complexity: O(log N) or O(N)
        // - This depends on the sorting algorithm's implementation.
        // - Java's Arrays.sort() for primitives uses a dual-pivot Quicksort,
        //   which takes O(log N) stack space on average, but O(N) in the worst case.
        // - If we consider only the extra variables, it's O(1).
    }
}
````
## optimal(using hashSet)
````java
public class Solution {
    public static int lengthOfLongestConsecutiveSequence(int[] arr, int N) {
        // Write your code here.
        if (N == 0) return 0; // Handle empty array case

        HashSet<Integer> set = new HashSet<>();
        // Add all elements to the set for O(1) lookups.
        for (int i = 0; i < N; i++) set.add(arr[i]);
        
        int max = 1; // Store the maximum length found
        
        // Iterate through each unique element in the set
        for (int elem : set) {
            // Check if 'elem' is the *start* of a sequence.
            // If 'elem-1' exists, then 'elem' is not the start, so we skip it.
            if (set.contains(elem - 1)) {
                // we will start from the starting of the chain so skipping this since it is not the starting
                continue;
            } else {
                // 'elem' is the start of a sequence.
                int count = 1;
                int temp = elem;
                
                // Count the length of the sequence starting from 'elem'.
                while (set.contains(temp + 1)) {
                    count++;
                    temp++;
                }
                // Update the global maximum length.
                max = Math.max(max, count);
            }
        }
        return max;

        // Time Complexity: O(N)
        // - O(N) to insert all elements into the HashSet.
        // - O(2N) for the second loop. Although there's a nested while loop,
        //   the 'while' loop only ever runs for elements that are the *start* of a sequence.
        //   In total, every element is visited at most twice (once by the 'for' loop,
        //   once by the 'while' loop). So, it's O(N + N) which simplifies to O(N).

        // Space Complexity: O(N)
        // - In the worst case, the HashSet will store all N elements from the array.
    }
}
````

