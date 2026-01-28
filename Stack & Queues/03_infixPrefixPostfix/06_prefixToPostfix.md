# prefix to postfix
## steps
````
1) traverse from right -> left
2? if operand -> push into stack
  otherwise(operator) ->
                        pop from stack term 1
                        pop from stack term 2
                      push into stack term1 +term 2 +operator
3) return top of stack
````
````java
class Solution {
    static String preToPost(String pre_exp) {
        // code here
        Stack<String> st=new Stack<>();
        for(int i=pre_exp.length()-1;i>=0;i--){
            char ch=pre_exp.charAt(i);
            if((ch>='a' && ch<='z') ||
            (ch>='A' && ch<='Z') ||
            (ch>='0' && ch<='9')){
                st.push(""+ch);
            }
            else {
                String term1=st.pop();
                String term2=st.pop();
                st.push(term1+term2+ch);
            }
        }
        return st.pop();
    }
}
````