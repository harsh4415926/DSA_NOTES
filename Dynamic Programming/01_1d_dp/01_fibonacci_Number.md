



# fibonacci number
## memoization
````java
class Solution {
    // Helper function to calculate the nth Fibonacci number using memoization
    public int helper(int[] dp, int n){
        // Base case: if n is 0 or 1, return n itself
        if(n <= 1) return n;
        
        // If we already have the answer for n, return it from dp array
        if(dp[n] != -1) return dp[n];
        
        // Otherwise, calculate the result recursively and store it in dp array
        dp[n] = helper(dp, n - 1) + helper(dp, n - 2);
        
        // Return the calculated result
        return dp[n];
    }

    // Main function to calculate the nth Fibonacci number
    public int fib(int n) {
        // Create a dp array to store intermediate results, initialized with -1
        int[] dp = new int[n + 1];
        Arrays.fill(dp, -1);
        
        // Call the helper function to compute the result
        return helper(dp, n);

        // Time Complexity: O(n)
// - Every Fibonacci number from 0 to n is calculated only once.
// - Each subproblem is solved once and stored in the dp array.

       // Space Complexity: O(n)
// - We use an extra dp array of size n+1 to store results.
// - The recursion stack can go as deep as n, so additional O(n) space is used for the recursion call stack.

    }
}
````
## tabulation
````java
class Solution {
    // Function to calculate the nth Fibonacci number using tabulation (bottom-up approach)
    public int fib(int n) {
        // Handle base case when n is 0
        if(n == 0) return 0;
        
        // Handle base case when n is 1
        if(n == 1) return 1;
        
        // Create a dp array to store Fibonacci numbers from 0 to n
        int[] dp = new int[n + 1];
        
        // Initialize the first two Fibonacci numbers
        dp[0] = 0;
        dp[1] = 1;
        
        // Fill the dp array in a bottom-up manner
        for(int i = 2; i <= n; i++){
            dp[i] = dp[i - 1] + dp[i - 2]; // Fibonacci relation
        }
        
        // Return the nth Fibonacci number
        return dp[n];
    }
}

// Time Complexity: O(n)
// - We loop from 2 to n once, performing constant time operations at each step.

// Space Complexity: O(n)
// - We use an array of size n+1 to store all Fibonacci numbers up to n.
````

## space optimization
````java
class Solution {
    // Function to calculate the nth Fibonacci number using space optimization
    public int fib(int n) {
        // Handle base case when n is 0
        if(n == 0) return 0;
        
        // Handle base case when n is 1
        if(n == 1) return 1;
        
        // Initialize the first two Fibonacci numbers
        int prev1 = 0;  // Fibonacci of n-2
        int prev2 = 1;  // Fibonacci of n-1
        int curi = 0;   // Current Fibonacci number
        
        // Build up the Fibonacci numbers iteratively from 2 to n
        for(int i = 2; i <= n; i++){
            curi = prev1 + prev2; // Calculate current Fibonacci number
            prev1 = prev2;        // Move prev2 to prev1
            prev2 = curi;        // Move curi to prev2 for the next iteration
        }
        
        // Return the nth Fibonacci number
        return curi;
    }
}

// Time Complexity: O(n)
// - We loop from 2 to n once, performing constant time operations at each step.

// Space Complexity: O(1)
// - We only store the last two Fibonacci numbers and the current one, using a constant amount of space.
````
