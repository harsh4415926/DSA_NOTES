# infix to postfix
## steps
````
1)Create an empty stack and an empty result string.

2)Scan the infix expression from left to right.

3)If the character is an operand, add it to the result.

4)If it is (, push it onto the stack.

5)If it is ), pop from the stack to the result until ( is found; discard (.

6)If it is an operator, pop operators from the stack with higher (or equal for left-associative) precedence, then push the current operator.

7)After the scan, pop all remaining operators from the stack to the result.

8)The final result is the postfix expression.
````

````java

class Solution {

    // Helper method to define operator precedence (higher value = higher priority)
    public static int priority(char ch) {
        if (ch == '^') return 3;
        else if (ch == '*' || ch == '/') return 2;
        else if (ch == '+' || ch == '-') return 1;
        else return 0; // For '(' or other characters
    }

    public static String infixToPostfix(String s) {
        Stack<Character> st = new Stack<>();
        StringBuilder sb = new StringBuilder();

        for (char ch : s.toCharArray()) {
            // Case 1: If character is an Operand (Letter or Digit)
            // Add it directly to the output string.
            if ((ch >= 'a' && ch <= 'z') || 
                (ch >= 'A' && ch <= 'Z') || 
                (ch >= '0' && ch <= '9')) {
                sb.append(ch);
            }
            // Case 2: If character is '(', push to stack
            else if (ch == '(') {
                st.push(ch);
            }
            // Case 3: If character is ')', pop and output until '(' is found
            else if (ch == ')') {
                while (st.size() > 0 && st.peek() != '(') {
                    sb.append(st.pop());
                }
                st.pop(); // Pop the opening '(' but don't append it to result
            }
            // Case 4: If character is an Operator (+, -, *, /, ^)
            else {
                // For ^      --- pop only if precedence is strictly greater
               // For others  --- pop if precedence is greater or equal
                while (st.size() > 0 && 
                      (priority(ch) < priority(st.peek()) || 
                      (priority(ch) == priority(st.peek()) && ch != '^'))) {
                    
                    sb.append(st.pop());
                }
                // Push the current operator onto the stack
                st.push(ch);
            }
        }

        // Pop all remaining operators from the stack
        while (!st.isEmpty()) {
            sb.append(st.pop());
        }

        return sb.toString();
    }
}

/*
 * ----------------------------------------------------------------------
 * COMPLEXITY ANALYSIS
 * ----------------------------------------------------------------------
 * Time Complexity: O(N)
 * - N is the length of the string.
 * - We iterate through the string once. Each element is pushed onto the 
 * stack and popped at most once.
 * * Space Complexity: O(N)
 * - We use a Stack to store operators and a StringBuilder for the answer.
 * - In the worst case, the stack can hold O(N) characters.
 * ----------------------------------------------------------------------
 */
````