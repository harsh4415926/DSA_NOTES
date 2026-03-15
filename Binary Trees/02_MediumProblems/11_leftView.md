# left view
````java
class Solution {
    ArrayList<Integer> ans;
    public void dfs(Node root, int depth){
        if(root==null)return ;
        if(depth==ans.size()){
            ans.add(root.data);
        }
        dfs(root.left,depth+1);
        dfs(root.right,depth+1);
    }
    public ArrayList<Integer> leftView(Node root) {
        // code here
        ans=new ArrayList<>();
        dfs(root,0);
        return ans;
        
    }
}
````
