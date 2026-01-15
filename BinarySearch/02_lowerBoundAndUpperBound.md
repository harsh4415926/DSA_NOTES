# lower bound
````java
    /**
     * Finds the index of the first element in the array that is not less than the target.
     * (i.e., the first element >= target).
     *
     * @param arr    The sorted array.
     * @param target The target value.
     * @return The index of the lower bound. Returns arr.length if all elements are smaller than target.
     */
    public static int lowerBound(int[] arr, int target) {
        int low = 0;
        int high = arr.length - 1;
        int ans = arr.length; // Default answer if target is greater than all elements

        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (arr[mid] >= target) {
                ans = mid; // This might be the answer, but we look for a smaller index
                high = mid - 1;
            } else {
                low = mid + 1; // The answer must be in the right half
            }
        }
        return ans;
    }

 
````
# upper bound
````java
    /**
     * Finds the index of the first element in the array that is greater than the target. (i.e., the first element > target).
     * return The index of the upper bound. Returns arr.length if no element is greater than target.
     */
    public static int upperBound(int[] arr, int target) {
        int low = 0;
        int high = arr.length - 1;
        int ans = arr.length; // Default answer if target is >= all elements

        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (arr[mid] > target) {
                ans = mid; // This might be the answer, but we look for a smaller index
                high = mid - 1;
            } else {
                low = mid + 1; // The answer must be in the right half
            }
        }
        return ans;
    }
````
