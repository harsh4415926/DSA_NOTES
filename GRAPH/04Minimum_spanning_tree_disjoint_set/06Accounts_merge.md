# 📝 Leetcode 721: Accounts Merge

## 📌 Problem Statement
- You are given a list of accounts.
- Each account has a **name** and a list of **emails**.
- Some emails may appear in multiple accounts (same person, different accounts).
- **Task**: Merge accounts that belong to the same person (same email means same user).
- Return the merged accounts with:
    - Name first
    - Sorted list of unique emails

---

## ⚡ Intuition
- **Key Idea**: If two accounts share an email, they must belong to the same person.
- This forms a **graph connectivity** problem:
    - Accounts → Nodes
    - Shared emails → Edges
- We need to **find connected components** of accounts and merge their emails.

✅ Best data structure: **Disjoint Set Union (DSU / Union-Find)**



### Disjoint Set Class
```java
// disjoint set implementation
class DisjointSet {
    ArrayList<Integer> parent = new ArrayList<>();
    ArrayList<Integer> rank = new ArrayList<>();
    ArrayList<Integer> size = new ArrayList<>();

    DisjointSet(int n) {
        for (int i = 0; i <= n; i++) {
            parent.add(i);   // each node is its own parent
            rank.add(0);     // rank (tree height)
            size.add(1);     // size of set
        }
    }

    public int ulpar(int n) {
        if (n == parent.get(n)) return n;
        int up = ulpar(parent.get(n));
        parent.set(n, up);   // Path compression
        return up;
    }

    public void unionBySize(int u, int v) {
        int ulp_u = ulpar(u);
        int ulp_v = ulpar(v);
        if (ulp_u == ulp_v) return;

        if (size.get(ulp_u) < size.get(ulp_v)) {
            parent.set(ulp_u, ulp_v);
            size.set(ulp_v, size.get(ulp_v) + size.get(ulp_u));
        } else {
            parent.set(ulp_v, ulp_u);
            size.set(ulp_u, size.get(ulp_u) + size.get(ulp_v));
        }
    }
}
// actual problem
class Solution {
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        HashMap<String,Integer> map=new HashMap<>();
        int n=accounts.size();
        DisjointSet ds=new DisjointSet(n);

        // Step 1: Build DSU connections
        for(int i=0;i<n;i++){
            for(int j=1;j<accounts.get(i).size();j++){
                String email=accounts.get(i).get(j);
                if(!map.containsKey(email)){
                    map.put(email,i);
                } else {
                    ds.unionBySize(map.get(email),i);
                }
            }
        }

        // Step 2: Group emails by leader
        List<List<String>> merged=new ArrayList<>();
        for(int i=0;i<n;i++) merged.add(new ArrayList<>());

        for(String email:map.keySet()){
            int accountIndex=map.get(email);
            int leader=ds.ulpar(accountIndex);
            merged.get(leader).add(email);
        }

        // Step 3: Build final answer
        List<List<String>> ans=new ArrayList<>();
        for(int i=0;i<n;i++){
            if(merged.get(i).size()>0){
                List<String> temp=new ArrayList<>(merged.get(i));
                Collections.sort(temp);
                temp.add(0,accounts.get(i).get(0)); // add name at start
                ans.add(temp);
            }
        }
        return ans;
    }
}
