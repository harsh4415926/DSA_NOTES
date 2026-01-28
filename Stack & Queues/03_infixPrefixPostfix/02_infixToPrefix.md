# infix to prefix
## steps
````
1) reverse the string 
2) interchange the brackets '(' and ')'
3) apply infix to postfix , just one change in the condition (ch!='^') ---> (ch == '^')
4) reverse the postfix expression to get final prefix
````
````java
class Solution {

    // Operator precedence
    static int priority(char ch) {
        if (ch == '^') return 3;
        if (ch == '*' || ch == '/') return 2;
        if (ch == '+' || ch == '-') return 1;
        return -1;
    }

    // Check if operand
    static boolean isOperand(char ch) {
        return (ch >= 'a' && ch <= 'z') ||
               (ch >= 'A' && ch <= 'Z') ||
               (ch >= '0' && ch <= '9');
    }

    public static String infixToPrefix(String infix) {

        // Step 1: Reverse infix and swap brackets
        StringBuilder rev = new StringBuilder();
        for (int i = infix.length() - 1; i >= 0; i--) {
            char ch = infix.charAt(i);
            if (ch == '(') rev.append(')');
            else if (ch == ')') rev.append('(');
            else rev.append(ch);
        }

        // Step 2: Convert reversed infix to postfix
        Stack<Character> stack = new Stack<>();
        StringBuilder postfix = new StringBuilder();

        for (int i = 0; i < rev.length(); i++) {
            char ch = rev.charAt(i);

            if (isOperand(ch)) {
                postfix.append(ch);
            }
            else if (ch == '(') {
                stack.push(ch);
            }
            else if (ch == ')') {
                while (!stack.isEmpty() && stack.peek() != '(') {
                    postfix.append(stack.pop());
                }
                stack.pop(); // remove '('
            }
            else {
                while (!stack.isEmpty() &&
                       (priority(stack.peek()) > priority(ch) ||
                       (priority(stack.peek()) == priority(ch) && ch == '^'))) {
                    postfix.append(stack.pop());
                }
                stack.push(ch);
            }
        }

        while (!stack.isEmpty()) {
            postfix.append(stack.pop());
        }

        // Step 3: Reverse postfix to get prefix
        return postfix.reverse().toString();
    }
}
````