# postfix to infix
## steps
````
1) Traverse postfix expression left to right

2)If operand → push onto stack

3)If operator:
     Pop operand2 
     Pop operand1

     Form: (operand1 operator operand2)

     Push back to stack
4)
Final stack top is the infix expression
````
````java
class Solution {

    static boolean isOperand(char ch) {
        return (ch >= 'a' && ch <= 'z') ||
               (ch >= 'A' && ch <= 'Z') ||
               (ch >= '0' && ch <= '9');
    }

    public static String postToInfix(String postfix) {

        Stack<String> st = new Stack<>();

        for (int i = 0; i < postfix.length(); i++) {
            char ch = postfix.charAt(i);

            // If operand, push to stack
            if (isOperand(ch)) {
                st.push(Character.toString(ch));
            }
            // If operator
            else {
                String op2 = st.pop();
                String op1 = st.pop();

                String expr = "(" + op1 + ch + op2 + ")";
                st.push(expr);
            }
        }

        return st.peek();
    }
}
````