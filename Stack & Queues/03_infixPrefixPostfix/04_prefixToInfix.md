# prefix to infix
## steps
````
1) Traverse prefix expression right to left

2)If operand → push onto stack

3)If operator:
     Pop operand1 
     Pop operand2

     Form: (operand1 operator operand2)

     Push back to stack
4)
Final stack top is the infix expression
````
````java
class Solution {
    static String preToInfix(String pre_exp) {
        // code here
        Stack<String> st=new Stack<>();
        for(int i=pre_exp.length()-1;i>=0;i--){
            char ch=pre_exp.charAt(i);
            if((ch>='a' && ch<='z')
                    || (ch>='A' && ch<='Z')
                    || (ch>='0' && ch<='9'))
            {
                st.push(""+ch);
            }

            else {
                String a=st.pop();
                String b=st.pop();
                st.push("("+a+ch+b+")");
            }
        }
        return st.pop();
    }
}


````