# balanced Paranthesis
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM SUMMARY: LeetCode 20 - Valid Parentheses
 * ----------------------------------------------------------------------
 * Given a string s containing just the characters '(', ')', '{', '}', '[' and ']',
 * determine if the input string is valid.
 * * An input string is valid if:
 * 1. Open brackets must be closed by the same type of brackets.
 * 2. Open brackets must be closed in the correct order.
 * * Example 1: s = "()" -> Output: true
 * Example 2: s = "()[]{}" -> Output: true
 * Example 3: s = "(]" -> Output: false
 * Example 4: s = "([)]" -> Output: false
 * ----------------------------------------------------------------------
 */

class Solution {
    public boolean isValid(String s) {
        // We use a Stack to keep track of opening brackets.
        // Last-In-First-Out (LIFO) behavior is perfect for nested structures.
        Stack<Character> st = new Stack<>();
        
        // Iterate through each character in the string
        for (char ch : s.toCharArray()) {
            
            // If it is an opening bracket, push it onto the stack.
            // We expect to find a matching closing bracket later.
            if (ch == '(' || ch == '{' || ch == '[') {
                st.push(ch);
            } 
            // If it is a closing bracket
            else {
                // Edge Case: If stack is empty but we found a closing bracket,
                // it means there is no matching opening bracket (e.g., "]").
                if (st.size() == 0) return false;
                
                // Get the most recent opening bracket
                char top = st.pop();
                
                // Check if the current closing bracket matches the most recent opening bracket.
                // If they don't match (e.g., top is '(' but ch is '}'), it's invalid.
                if ((ch == ')' && top != '(') 
                    || (ch == '}' && top != '{') 
                    || (ch == ']' && top != '[')) {
                    return false;
                }
            }
        }
        
        // After iterating through the entire string, the stack must be empty.
        // If it's not empty, it means we have unmatched opening brackets left (e.g., "(()").
        return st.isEmpty();
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - We traverse the string exactly once. Push and Pop operations on the 
 * stack are O(1).
 * * Space Complexity: O(N)
 * - In the worst case (e.g., "((((("), we might push all characters onto 
 * the stack.
 * ----------------------------------------------------------------------
 */
````