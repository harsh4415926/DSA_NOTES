# Problem: Intersection of Two Sorted Arrays
You are given two integer arrays, a and b, that are both sorted in non-decreasing order. Your task is to find their intersection. The intersection should contain all the elements that are common to both arrays, without any duplicates. The resulting list must also be sorted.

Example: Input: a = [1, 2, 2, 3, 3, 4, 5, 6] b = [2, 3, 3, 5, 6, 6, 7]

Output: [2, 3, 5, 6]

---
````java
class Solution {

    // Function to find the intersection of two arrays
    ArrayList<Integer> intersection(int[] a, int[] b) {
        int i=0; // Pointer for array 'a'
        int j=0; // Pointer for array 'b'
        ArrayList<Integer> ans=new ArrayList<>();

        // Continue until one of the pointers reaches the end of its array
        while(i<a.length && j<b.length){
            // If elements are equal, it's a common element
            if(a[i]==b[j] ){
                // Add to list only if it's the first element or not a duplicate
                if(ans.size()==0 || ans.get(ans.size()-1)!=a[i])
                    ans.add(a[i]);
                i++;
                j++;
            }
            // If element in 'a' is smaller, move pointer 'i' forward
            else if(a[i]<b[j]){
                i++;
            }
            // If element in 'b' is smaller, move pointer 'j' forward
            else j++;
        }
        return ans;
    }
}

````