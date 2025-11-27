# Problem: Next Permutation(leetcode)
You are given an array of integers nums. Your task is to implement the "next permutation" function, which rearranges the numbers into the lexicographically next greater permutation of numbers.

If such an arrangement is not possible (i.e., the array is already sorted in descending order), you must rearrange it as the lowest possible order (sorted in ascending order). The modification must be done in-place and use only constant extra memory.

Example: Input: nums = [1, 2, 3] Output: [1, 3, 2]

Input: nums = [3, 2, 1] Output: [1, 2, 3]

---
## brute force

````
find all permutaions put in a list in increasing order..
````
## ------------------------------------------
# optimal(must remember)
## -------------explaination ------------
# LeetCode: Next Permutation

## 💡 The Core Intuition

The problem is to find the *next* lexicographically greater permutation. The easiest way to think about this is like **counting up with numbers**.

Think about finding the next number after `21499`.
1.  You scan from the right: `9` is maxed. The next `9` is maxed.
2.  You find the first digit that is **not maxed**: the `4`.
3.  You **increase** that digit by the smallest amount possible: `4` $\rightarrow$ `5`.
4.  You **reset** all the digits to its right to their smallest possible value: `99` $\rightarrow$ `00`.
5.  The result is `21500`.

The "Next Permutation" algorithm does the *exact same thing*:
* "Maxed out" for a permutation is a subarray sorted in **descending order** (e.g., `[7, 6, 3]`).
* "Resetting" a subarray means sorting it in **ascending order** (e.g., `[3, 6, 7]`).

## 🔢 The Algorithm: Step-by-Step

We'll use this example: `nums = [1, 5, 8, 4, 7, 6, 5, 3, 1]`

### Step 1: Find the Pivot

**Goal:** Find the first number from the right that isn't "maxed out" (i.e., it's smaller than the number to its right).

**Action:** Scan from right-to-left. Find the first index `i` such that `nums[i] < nums[i+1]`.

`[1, 5, 8,` **4** `,` `7, 6, 5, 3, 1]`
$\qquad\qquad\quad$ `^` $\qquad$ `^`
$\qquad\quad$ `i=3` $\quad$ **(This suffix is descending)**

* We find `4` at index `i = 3` because `4 < 7`.
* This `4` is our **pivot**. It's the first digit we can "increase".
* The subarray to its right `[7, 6, 5, 3, 1]` is the "maxed out" part.

### Step 2: Find the Successor to Swap

**Goal:** Increase the pivot (`4`) by the *smallest amount possible*.

**Action:** Scan the "maxed out" suffix `[7, 6, 5, 3, 1]` from right-to-left. Find the first number `nums[j]` that is *just larger* than the pivot `nums[i]`.

`[1, 5, 8, 4, 7, 6,` **5** `, 3, 1]`
$\qquad\qquad\qquad\qquad\qquad$ `^`
$\qquad\qquad\qquad\qquad\quad$ `j=6`

* We scan `1... 3... 5...`
* We stop at `5` (at index `j = 6`) because `5 > 4`.
* This `5` is our **successor**.

### Step 3: Swap the Pivot and Successor

**Goal:** Perform the "increase" action.

**Action:** Swap `nums[i]` and `nums[j]`.

* **Before:** `[1, 5, 8,` **4** `, 7, 6,` **5** `, 3, 1]`
* **After:** `[1, 5, 8,` **5** `, 7, 6,` **4** `, 3, 1]`

### Step 4: Reset the Suffix

**Goal:** "Reset" the subarray to the right of the pivot's original position to its smallest possible value (ascending order).

**Action:** Reverse the subarray from `i + 1` to the end of the array.

* The suffix was `[7, 6, 4, 3, 1]`.
* **Before:** `[1, 5, 8, 5,` **7, 6, 4, 3, 1** `]`
* **After:** `[1, 5, 8, 5,` **1, 3, 4, 6, 7** `]`

**Why reverse?** Because the suffix was already in descending order (it was "maxed out"), reversing it is the fastest way to sort it in ascending order.

**Final Answer:** `[1, 5, 8, 5, 1, 3, 4, 6, 7]`

---

## ⚠️ The Edge Case

> **What if the array is already the largest permutation?** (e.g., `[3, 2, 1]`)

1.  **Find Pivot (Step 1):** You will scan the whole array and find no `i` such that `nums[i] < nums[i+1]`.
2.  **Action:** This means the *entire* array is "maxed out". The "next" permutation is the smallest one (it wraps around).
3.  **Solution:** Simply **reverse the entire array**.
    * `[3, 2, 1]` $\rightarrow$ `[1, 2, 3]`
    * 

````java
class Solution {
    /**
     * Helper function to swap two elements in an array.
     */
    public void swap(int i,int j,int[] arr){
            int temp=arr[i];
            arr[i]=arr[j];
            arr[j]=temp;
    }

    /**
     * Helper function to reverse a portion of an array in-place.
     */
    public void reverse(int[] arr,int i,int j){
           while(i<j){
               int temp=arr[i];
               arr[i]=arr[j];
               arr[j]=temp;
               i++;
               j--;
           }
    }

    public void nextPermutation(int[] nums) {
        int n=nums.length;
        if(n==1)return; // No permutation possible for a single element

        int pivot=-1;
        // 1. Find the pivot: the first element from the right that is smaller than its neighbor on the right.
        for(int i=n-2;i>=0;i--){
            if(nums[i]<nums[i+1]){
                pivot=i;
                break;
            }
        } 
        
        // 2. If no pivot exists, the array is in descending order (e.g., [3, 2, 1]).
        // Reverse it to get the smallest permutation (e.g., [1, 2, 3]).
        if(pivot==-1){
            reverse(nums,0,n-1);
            return ;
        }
        
       // 3. Find the smallest element from the right that is strictly greater than the pivot.
       for(int i=n-1;i>pivot;i--){
           if(nums[i]>nums[pivot]){
            // 4. Swap this element with the pivot.
            swap(i,pivot,nums);
            break;
           }
       }
       
       // 5. Reverse the subarray to the right of the pivot.
       // This ensures the suffix is in its smallest possible order.
       reverse(nums,pivot+1,n-1);

       // Time Complexity: O(N)
       // - Finding the pivot takes O(N).
       // - Finding the element to swap takes O(N).
       // - Reversing the suffix takes O(N).
       // - Overall: O(N) + O(N) + O(N) = O(N).

       // Space Complexity: O(1)
       // - All operations are performed in-place.
    }
}
````
