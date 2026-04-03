# Right Rotate Array

## using reverse
````java
class Solution {
    public void rightRotate(int[] nums, int k) {
        int n = nums.length;
        k = k % n;

        // Step 1: reverse entire array
        reverse(nums, 0, n - 1);

        // Step 2: reverse first k elements
        reverse(nums, 0, k - 1);

        // Step 3: reverse remaining elements
        reverse(nums, k, n - 1);
    }

    private void reverse(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
}
````

## using extra space
````java
class Solution {
    public void rightRotate(int[] nums, int k) {
        int n = nums.length;
        k = k % n;

        int[] temp = new int[n];

        for (int i = 0; i < n; i++) {
            temp[(i + k) % n] = nums[i];
        }

        for (int i = 0; i < n; i++) {
            nums[i] = temp[i];
        }
    }
}
````