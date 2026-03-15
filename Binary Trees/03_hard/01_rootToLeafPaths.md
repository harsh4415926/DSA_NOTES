# root to leaf paths
````java
class Solution {
    public ArrayList<ArrayList<Integer>> Paths(Node root) {
        ArrayList<ArrayList<Integer>> result = new ArrayList<>();
        ArrayList<Integer> path = new ArrayList<>();
        dfs(root, path, result);
        return result;
    }
    
    private void dfs(Node node, ArrayList<Integer> path,
                     ArrayList<ArrayList<Integer>> result) {
        if (node == null) return;
        
        // add current node to path
        path.add(node.data);
        
        // if leaf node, store the path
        if (node.left == null && node.right == null) {
            result.add(new ArrayList<>(path));
        } else {
            dfs(node.left, path, result);
            dfs(node.right, path, result);
        }
        
        // backtrack
        path.remove(path.size() - 1);
    }
}

````