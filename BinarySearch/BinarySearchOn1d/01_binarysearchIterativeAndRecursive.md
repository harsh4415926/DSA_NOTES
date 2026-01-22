# iterative code
````java
public static int search(int[] arr, int target) {
        // Initialize pointers for the start and end of the array segment.
        int low = 0;
        int high = arr.length - 1;

        // Loop until the pointers cross each other.
        while (low <= high) {
            // Calculate the middle index.
            // This way of calculating mid avoids potential integer overflow
            // compared to (low + high) / 2.
            int mid = low + (high - low) / 2;

            // Check if the middle element is the target.
            if (arr[mid] == target) {
                return mid; // Target found, return its index.
            }

            // If the target is greater, ignore the left half.
            if (arr[mid] < target) {
                low = mid + 1;
            }
            // If the target is smaller, ignore the right half.
            else {
                high = mid - 1;
            }
        }

        // If the loop finishes, the target was not in the array.
        return -1;
    }
````
# recursive code 
````java
 private static int recursiveSearchHelper(int[] arr, int target, int low, int high) {
        // Base case: If the search space is invalid, the element is not in the array.
        if (low > high) {
            return -1;
        }

        // Calculate the middle index.
        int mid = low + (high - low) / 2;

        // If the middle element is the target, return its index.
        if (arr[mid] == target) {
            return mid;
        }

        // If the target is in the right half, recurse on the right subarray.
        if (arr[mid] < target) {
            return recursiveSearchHelper(arr, target, mid + 1, high);
        }
        // If the target is in the left half, recurse on the left subarray.
        else {
            return recursiveSearchHelper(arr, target, low, mid - 1);
        }
    }
````
