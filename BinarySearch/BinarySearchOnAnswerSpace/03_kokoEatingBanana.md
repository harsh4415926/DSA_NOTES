# koko Eating Banana
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 875 - Koko Eating Bananas
 * ----------------------------------------------------------------------
 * LOGIC (Binary Search on Answer):
 * 1. Minimum possible speed is 1. Maximum possible speed is the size of 
 * the largest pile.
 * 2. Search space for speed `k`: [1, max(piles)]
 * 3. Use Binary Search to find the minimum `k`:
 * - If Koko can eat all bananas at speed `mid` within `h` hours, 
 * then `mid` is a valid answer. We search the left half (smaller 
 * speeds) to find a potentially smaller valid speed.
 * - If not, we must search the right half (faster speeds).
 * ----------------------------------------------------------------------
 */

class Solution {
    
    // Helper function: Can Koko eat all bananas at 'speed' within 'h' hours?
    public boolean canEat(int[] piles, int h, int speed) {
        long hours = 0; // Use long to prevent overflow during sum

        for (int pile : piles) {
            // Equivalent to Math.ceil((double) pile / speed), but integer math is faster.
            // Adding (speed - 1) before dividing ensures any remainder results in rounding up.
            hours += (pile + speed - 1) / speed;

            // Optimization: If hours exceed 'h' early, no need to check remaining piles
            if (hours > h) return false; 
        }

        return hours <= h;
    }

    public int minEatingSpeed(int[] piles, int h) {
        int left = 1; // Minimum speed is 1 banana/hour
        int right = 0;

        // Find the maximum pile size to set the upper bound for speed.
        // If speed = max(pile), it takes exactly 1 hour per pile (minimum possible time).
        for (int pile : piles) {
            right = Math.max(right, pile);
        }

        int answer = right; // Store the best (minimum valid) speed found

        // Binary search for the minimum valid speed
        while (left <= right) {
            // Prevent potential integer overflow
            int mid = left + (right - left) / 2;

            if (canEat(piles, h, mid)) {
                answer = mid;       // 'mid' works, record it.
                right = mid - 1;    // Try to find a slower valid speed (search left)
            } else {
                left = mid + 1;     // 'mid' is too slow, must eat faster (search right)
            }
        }

        return answer;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N log M) where N is piles.length and M is the maximum element in piles.
 * Space: O(1) as we only use a few variables.
 */
````