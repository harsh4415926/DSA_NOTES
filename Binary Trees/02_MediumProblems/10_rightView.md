# right view 
````java
class Solution {
    List<Integer> ans;
    public void dfs(TreeNode root,int depth){
        if(root==null)return ;
        if(depth==ans.size()){
            ans.add(root.val);
        }
        dfs(root.right,depth+1);
        dfs(root.left,depth+1);
    }
    public List<Integer> rightSideView(TreeNode root) {
        ans=new ArrayList<>();
        dfs(root,0);
        return ans;

    }
}
````
