# Problem: Reverse Pairs
You are given an integer array nums. A reverse pair is a pair of indices (i, j) such that 0 <= i < j < n (where n is the array length) and the condition nums[i] > 2 * nums[j] is met.

Your task is to find and return the total number of reverse pairs in the array. This solution uses a modified Merge Sort algorithm to efficiently count these pairs.

Example: Input: nums = [1, 3, 2, 3, 1] Output: 2 Explanation: The reverse pairs are (1, 4) (since 3 > 2 * 1) and (3, 4) (since 3 > 2 * 1).

---
````java
import java.util.*;

class Solution {
    int count;

    public void merge(int[] arr, int low, int mid, int high) {
        int i = low;      // Pointer for the first half
        int j = mid + 1;  // Pointer for the second half

        // Count reverse pairs
        while (i <= mid && j <= high) {
            if ((long) arr[i] > 2L * arr[j]) {
                count += (mid - i + 1);
                j++;
            } else {
                i++;
            }
        }

        List<Integer> temp = new ArrayList<>();
        i = low;
        j = mid + 1;

        // Merge step
        while (i <= mid && j <= high) {
            if (arr[i] <= arr[j]) {
                temp.add(arr[i]);
                i++;
            } else {
                temp.add(arr[j]);
                j++;
            }
        }

        // Remaining elements
        while (i <= mid) {
            temp.add(arr[i]);
            i++;
        }

        while (j <= high) {
            temp.add(arr[j]);
            j++;
        }

        // Copy back to original array
        for (i = low; i <= high; i++) {
            arr[i] = temp.get(i - low);
        }
    }

    public void mergesort(int low, int high, int[] arr) {
        if (low >= high) return;

        int mid = low + (high - low) / 2;

        mergesort(low, mid, arr);
        mergesort(mid + 1, high, arr);
        merge(arr, low, mid, high);
    }

    public int reversePairs(int[] arr) {
        int n = arr.length;
        count = 0;
        mergesort(0, n - 1, arr);
        return count;
    }
}
````