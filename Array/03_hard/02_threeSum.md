# Problem: 3Sum
You are given an integer array nums. Your task is to find all unique triplets [nums[i], nums[j], nums[k]] such that the indices i, j, and k are all distinct, and the sum of the three elements is zero (nums[i] + nums[j] + nums[k] == 0).

The solution set must not contain any duplicate triplets.

Example: Input: nums = [-1, 0, 1, 2, -1, -4] Output: [[-1, -1, 2], [-1, 0, 1]] Explanation: nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0. nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0. nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0. The unique triplets are [-1, 0, 1] and [-1, -1, 2].

---

## brute force (3 loops)
````java
    public List<List<Integer>> threeSum(int[] nums) {
        // A Set is used to store the results to automatically handle duplicates.
        // We store sorted lists in the set to ensure that triplets with
        // the same numbers in a different order are treated as identical.
        Set<List<Integer>> resultSet = new HashSet<>();
        int n = nums.length;

        // Loop 1: Iterate for the first number (i)
        for (int i = 0; i < n - 2; i++) {
            // Loop 2: Iterate for the second number (j)
            // Start from i + 1 to avoid reusing the same element
            for (int j = i + 1; j < n - 1; j++) {
                // Loop 3: Iterate for the third number (k)
                // Start from j + 1 to avoid reusing the same element
                for (int k = j + 1; k < n; k++) {
                    
                    // Check if the sum of the three numbers is zero
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        // Create a fixed-size list (more concise)
                        List<Integer> triplet = Arrays.asList(nums[i], nums[j], nums[k]);
                        
                        // Sort the triplet so that order doesn't matter
                        Collections.sort(triplet);
                        
                        // Add the sorted triplet to the set
                        resultSet.add(triplet);
                    }
                }
            }
        }

        // Convert the Set back to a List for the final return value
        return new ArrayList<>(resultSet);
    }

````
## better (two loops +hashing for the third)
````java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        int n = nums.length;
        // Use a HashSet to store unique triplets
        HashSet<List<Integer>> set = new HashSet<>();
        
        for (int i = 0; i < n; i++) {
            // Use a HashMap to store (value, index) for the 'j' loop
            HashMap<Integer, Integer> map = new HashMap<>();
            
            // Fix the first element (nums[i])
            for (int j = i + 1; j < n; j++) {
                // Fix the second element (nums[j])
                // We need to find a third element 'k' such that:
                // nums[i] + nums[j] + k = 0
                // k = -(nums[i] + nums[j])
                int thirdElement = -(nums[i] + nums[j]);
                
                // Check if this 'thirdElement' has been seen before (in the map)
                if (map.containsKey(thirdElement)) {
                    List<Integer> l = new ArrayList<>();
                    l.add(nums[i]);
                    l.add(nums[j]);
                    l.add(thirdElement); // Add the value
                    
                    // Sort the list to handle duplicates (e.g., [1, -1, 0] is same as [0, 1, -1])
                    Collections.sort(l);
                    set.add(l);
                }
                
                // Add the current 'j' element to the map for future 'j' iterations
                map.put(nums[j], j);
            }
        }
        
        // Convert the HashSet of lists to the final ArrayList
        List<List<Integer>> ans = new ArrayList<>();
        for (List<Integer> it : set) {
            ans.add(it);
        }
        return ans;

        // Time Complexity: O(N^2)
        // - The outer loop runs O(N) times.
        // - The inner loop runs O(N) times.
        // - Inside the inner loop, HashMap operations are O(1) on average.
        // - Sorting a triplet is O(3*log3), which is O(1).
        // - HashSet insertion is O(1) on average.
        // - Total: O(N^2).

        // Space Complexity: O(N) + O(M)
        // - O(N) for the HashMap in the inner loop (in the worst case).
        // - O(M) for the HashSet, where 'M' is the number of unique triplets.
        // - The 'ans' list also takes O(M) space.
    }
}
````
## optimised(two pointer)
````java
class Solution {
    /**
     * Finds all unique triplets in the array which give the sum of zero,
     * using an optimized two-pointer approach.
     * @param nums The input array of integers.
     * @return A list of unique triplets that sum to zero.
     */
    public List<List<Integer>> threeSum(int[] nums) {
        int n = nums.length;
        List<List<Integer>> ans = new ArrayList<>();
        
        // Sort the array. This is crucial for the two-pointer approach
        // and for skipping duplicates.
        Arrays.sort(nums);
        
        // Outer loop, fixing the first element (nums[i])
        for (int i = 0; i < n; i++) {
            // Skip duplicate 'i' elements to avoid duplicate triplets
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            // Use two pointers for the remaining part of the array
            int j = i + 1; // Left pointer
            int k = n - 1; // Right pointer

            while (j < k) {
                int sum = nums[i] + nums[j] + nums[k];
                
                if (sum < 0) {
                    // Sum is too small, move the left pointer to a larger number
                    j++;
                } else if (sum > 0) {
                    // Sum is too large, move the right pointer to a smaller number
                    k--;
                } else {
                    // Found a triplet that sums to zero
                    List<Integer> temp = Arrays.asList(nums[i], nums[j], nums[k]);
                    ans.add(temp);
                    
                    // Move both pointers
                    j++;
                    k--;
                    
                    // Skip duplicate 'j' and 'k' elements
                    while (j < k && nums[j] == nums[j - 1]) {
                        j++;
                    }
                    while (j < k && nums[k] == nums[k + 1]) {
                        k--;
                    }
                }
            }
        }
        return ans;

        // Time Complexity: O(N^2)
        // - Sorting the array takes O(N log N).
        // - The outer loop runs O(N) times.
        // - The inner 'while' loop (two-pointer scan) runs in O(N) time *for each 'i'*.
        // - Total time = O(N log N) + O(N * N) = O(N^2).

        // Space Complexity: O(log N) or O(N) + O(M)
        // - The space complexity of the sorting algorithm is O(log N) or O(N) depending
        //   on the language's implementation (e.g., in-place quicksort vs. Timsort/Mergesort).
        // - 'M' is the number of unique triplets found.
        // - We don't use any extra HashMaps or HashSets, so this is more space-efficient.
    }
}

````

