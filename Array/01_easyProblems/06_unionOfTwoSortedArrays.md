# Problem: Union of Two Sorted Arrays
Given two arrays, a and b, both sorted in non-decreasing order, your task is to find the union of these two arrays. The union should be a list of all the distinct elements from both arrays, and the resulting list should also be sorted.

Example: Input: a = [1, 2, 3, 4, 5] b = [1, 2, 3, 6, 7]

Output: [1, 2, 3, 4, 5, 6, 7]

---

````java
class Solution {
    public static ArrayList<Integer> findUnion(int a[], int b[]) {
        ArrayList<Integer> ans=new ArrayList<>();
        int i=0; // Pointer for array 'a'
        int j=0; // Pointer for array 'b'

        // Traverse both arrays simultaneously
        while(i<a.length && j<b.length){
            if(a[i]<=b[j]){
                // Add element from 'a' if it's not a duplicate of the last element added
                if(ans.size()==0 || ans.get(ans.size()-1)!=a[i])
                    ans.add(a[i]);
                i++;
            }
            else{
                // Add element from 'b' if it's not a duplicate
                if(ans.size()==0 || ans.get(ans.size()-1)!=b[j])
                    ans.add(b[j]);
                j++;
            }
        }

        // Add remaining unique elements from array 'a'
        while(i<a.length){
            if(ans.size()==0 || ans.get(ans.size()-1)!=a[i])
                ans.add(a[i]);
            i++;
        }

        // Add remaining unique elements from array 'b'
        while(j<b.length){
            if(ans.size()==0 || ans.get(ans.size()-1)!=b[j])
                ans.add(b[j]);
            j++;
        }
        
        return ans;
    }
}
````
