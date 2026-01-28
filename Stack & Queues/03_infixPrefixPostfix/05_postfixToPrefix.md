# postfix to prefix
## steps
````
1) traverse from left -> right
2? if operand -> push into stack
  otherwise(operator) ->
                        pop from stack term 2
                        pop from stack term 1
                      push into stack operator+term1 +term 2
3) return top of stack
````
````java
class Solution {
    static String postToPre(String post_exp) {
        // code here
        Stack<String> st=new Stack<>();
        for(int i=0;i<post_exp.length();i++){
            char ch=post_exp.charAt(i);
            if((ch>='a' && ch<='z') || 
            (ch>='A' && ch<='Z') ||
            (ch>='0' && ch<='9')){
                st.push(""+ch);
            }
            else {
                 String term2=st.pop();
                 String term1=st.pop();
                 st.push(ch+term1+term2);
            }
        }
        return st.pop();
    }
}
````