# Problem: product of array except self
Given an integer array nums, return an array ans such that ans[i] is equal to the product of all the elements of nums except nums[i].

---

## using devide operator
    /*
    💡 Intuition & Approach
    This solution first calculates the product of all **non-zero** elements in the
    array and simultaneously counts the number of zeros. This is done in a single
    initial pass.

    The logic then branches based on the number of zeros found:
    1.  If `nums[i]` is NOT zero:
        * If there was **one or more zero** in the original array, the product of
          all elements *except* `nums[i]` will always be 0 (because the other
          zero is still part of the product).
        * If there were **no zeros**, we can simply take the total product
          (`prodWithoutZero`) and divide it by the current element `nums[i]` to
          get the answer for that index.
    2.  If `nums[i]` IS zero:
        * If there was **more than one zero** in the original array, the product
          *except* this zero will still contain at least one other zero, making
          the result 0.
        * If this was the **only zero**, then the product *except* this zero is
          simply the product of all the *other* (non-zero) elements, which we
          already calculated as `prodWithoutZero`.
    */
````java
    public int[] productExceptSelf(int[] nums) {
      int n = nums.length;
      int cntZero = 0; // Counter for the number of zeros in the array
      int prodWithoutZero = 1; // Stores the product of all NON-ZERO elements

      // First pass: Calculate product of non-zeros and count zeros
      for (int i = 0; i < n; i++) {
        if (nums[i] == 0) {
          cntZero++;
        } else {
          prodWithoutZero *= nums[i];
        }
      }

      int[] ans = new int[n]; // The result array

      // Second pass: Populate the result array based on zero counts
      for (int i = 0; i < n; i++) {
        if (nums[i] != 0) {
           // If the current element is not zero
           if (cntZero > 0) {
             // If there's any zero elsewhere, the product will be 0
             ans[i] = 0;
           } else {
             // No zeros in the array, so we can safely divide
             ans[i] = prodWithoutZero / nums[i];
           }
        } else {
            // If the current element IS zero
            if (cntZero > 1) {
              // If there are multiple zeros, the product is 0
              ans[i] = 0;
            } else {
              // If this is the *only* zero, the product is all other non-zero elements
              ans[i] = prodWithoutZero;
            }
        }
      }
      return ans;
    }
````
    /*
    📊 Complexity Analysis
    * Time Complexity: O(n)
        * We perform two separate loops over the array, each taking O(n) time.
        * O(n) + O(n) simplifies to O(n).
    * Space Complexity: O(n)
        * We create an answer array `ans` of size `n`.
        * (This is O(1) if you don't count the output array as extra space).
    */

## without using devide operator


    /*
    💡 Intuition & Approach
    Since division is not allowed, we can't just get the total product and divide.
    The product of all elements *except* `nums[i]` can be thought of as two separate parts:
    1.  The product of all elements to the **left** of `nums[i]`.
    2.  The product of all elements to the **right** of `nums[i]`.

    The final answer `ans[i]` is simply `(product of left elements) * (product of right elements)`.

    This algorithm implements this by:
    1.  Creating a `leftProd` array where `leftProd[i]` stores the product of all
        elements from `nums[0]` to `nums[i-1]`. This is built in a forward pass.
    2.  Creating a `rightProd` array where `rightProd[i]` stores the product of all
        elements from `nums[i+1]` to `nums[n-1]`. This is built in a backward pass.
    3.  Iterating one final time and setting `ans[i] = leftProd[i] * rightProd[i]`.
    */

````java   
 public int[] productExceptSelf(int[] nums) {
      int n = nums.length;

       // 1. Initialize Left and Right Product Arrays
       int[] leftProd = new int[n];  // leftProd[i] = product of all elements *before* i
       int[] rightProd = new int[n]; // rightProd[i] = product of all elements *after* i

       // 2. Build the Left Product Array (Prefix Product)
       leftProd[0] = 1; // Base case: No elements to the left of index 0
       for (int i = 1; i < n; i++) {
        // The product to the left of i is (product to the left of i-1) * (nums[i-1])
        leftProd[i] = leftProd[i-1] * nums[i-1];
       }

       // 3. Build the Right Product Array (Suffix Product)
       rightProd[n - 1] = 1; // Base case: No elements to the right of the last index
       for (int i = n - 2; i >= 0; i--) {
        // The product to the right of i is (product to the right of i+1) * (nums[i+1])
        rightProd[i] = rightProd[i+1] * nums[i+1];
       }

       // 4. Construct the final answer array
       int[] ans = new int[n];
       for (int i = 0; i < n; i++) {
        // The answer is the product of the left-side and right-side products
        ans[i] = leftProd[i] * rightProd[i];
       }

       return ans;
    }
````
    /*
    📊 Complexity Analysis
    * Time Complexity: O(n)
        * We perform three separate loops, each iterating through the array once.
        * O(n) + O(n) + O(n) simplifies to O(n).
    * Space Complexity: O(n)
        * We use two auxiliary arrays, `leftProd` and `rightProd`, each of size `n`.
        * (The `ans` array is also O(n), but sometimes output arrays are
        * excluded from space complexity analysis. Even so, the auxiliary
        * arrays make it O(n)).
    */

