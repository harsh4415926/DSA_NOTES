# remove k digits
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 402 - Remove K Digits
 * ----------------------------------------------------------------------
 * Remove k digits from string num to form the smallest possible integer.
 * * STEPS:
 * 1. Iterate digits -> Pop stack top if it's > current digit (Greedy: remove peaks).
 * 2. If k > 0 after loop, remove from end. Handle leading zeros.
 * ----------------------------------------------------------------------
 */

class Solution {
    public String removeKdigits(String num, int k) {
        Stack<Character> st = new Stack<>();

        for (char c : num.toCharArray()) {
            // Greedy: If current digit is smaller than previous (top), 
            // discard previous to make the number smaller (as it sits in a higher place value).
            while (!st.isEmpty() && k > 0 && st.peek() > c) {
                st.pop();
                k--;
            }
            st.push(c);
        }

        // Edge case: If digits are increasing (e.g., "12345"), loop won't pop.
        // We must remove remaining k digits from the end.
        while (k > 0 && !st.isEmpty()) {
            st.pop();
            k--;
        }

        // Build string (Stack comes out in reverse order)
        StringBuilder sb = new StringBuilder();
        while (!st.isEmpty()) {
            sb.append(st.pop());
        }
        sb.reverse();

        // Remove leading zeros (e.g., "0200" -> "200")
        int i = 0;
        while (i < sb.length() && sb.charAt(i) == '0') {
            i++;
        }

        String ans = sb.substring(i);

        // Return "0" if string is empty (all digits removed or were zeros)
        return ans.length() == 0 ? "0" : ans;
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - We traverse the string once. Each digit is pushed and popped at most once.
 * * Space Complexity: O(N)
 * - Stack stores digits; StringBuilder stores result.
 * ----------------------------------------------------------------------
 */
````