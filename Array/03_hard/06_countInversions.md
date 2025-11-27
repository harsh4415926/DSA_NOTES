# Problem: Count Inversions
You are given an array of integers. Your task is to find the Inversion Count for the array.

An inversion is a pair of indices (i, j) such that i < j and arr[i] > arr[j]. In other words, it's a pair of elements that are in the wrong order. This solution uses the Merge Sort algorithm to efficiently count inversions.

Example: Input: arr = [2, 4, 1, 3, 5] Output: 3 Explanation: The inversion pairs are (2, 1), (4, 1), and (4, 3).

---
## brute force
````java
class Solution {
    static int inversionCount(int arr[]) {
        // Code Here
        int n=arr.length;
        int count=0;
        for(int i=0;i<n;i++){
            for(int j=i+1;j<n;j++){
                if(arr[j]<arr[i]){
                   count++;
                }
            }
        }
        return count;
    }
}
````
## using merge sort
````java
class Solution {
    int count = 0; // Global counter for inversions

    /**
     * Merges two sorted subarrays and counts inversions between them.
     * The first subarray is arr[low...mid]
     * The second subarray is arr[mid+1...high]
     * @param arr The main array.
     * @param low The starting index of the first subarray.
     * @param mid The ending index of the first subarray.
     * @param high The ending index of the second subarray.
     */
    public void merge(int[] arr, int low, int mid, int high) {
        int i = low;      // Pointer for the first half
        int j = mid + 1;  // Pointer for the second half
        List<Integer> temp = new ArrayList<>(); // Temporary list to store merged elements

        while (i <= mid && j <= high) {
            if (arr[i] <= arr[j]) {
                temp.add(arr[i]);
                i++;
            } else {
                // This is the key step for counting inversions
                // If arr[i] > arr[j], then arr[j] is smaller than all
                // remaining elements in the first half (from i to mid).
                count += (mid - i + 1); 
                
                temp.add(arr[j]);
                j++;
            }
        }

        // If elements are left in the first half, add them
        while (i <= mid) {
            temp.add(arr[i]);
            i++;
        }

        // If elements are left in the second half, add them
        while (j <= high) {
            temp.add(arr[j]);
            j++;
        }

        // Copy the sorted elements from temp back into the original array
        for (i = low; i <= high; i++) {
            arr[i] = temp.get(i - low);
        }
    }

    /**
     * Recursive function to sort the array and count inversions.
     * @param low The starting index of the segment.
     * @param high The ending index of the segment.
     * @param arr The array.
     */
    public void mergesort(int low, int high, int[] arr) {
        if (low >= high) return; // Base case
        
        int mid = (low + high) / 2;
        
        mergesort(low, mid, arr);     // Sort and count in left half
        mergesort(mid + 1, high, arr); // Sort and count in right half
        merge(arr, low, mid, high);   // Merge and count split-inversions
    }

    /**
     * Entry point for the inversion count algorithm.
     * @param arr The array to process.
     * @return The total number of inversions.
     */
    int inversionCount(int arr[]) {
        // Code Here
        int n = arr.length;
        count = 0; // Reset global counter for safety
        mergesort(0, n - 1, arr);
        return count;

        // Time Complexity: O(N log N)
        // - Same as Merge Sort, as the counting logic is
        //   integrated into the O(N) merge step.
        
        // Space Complexity: O(N)
        // - Required for the temporary 'ArrayList' during the merge step.
    }
}
````
### writing the above code without using global variable
````java
class Solution {
 
        public int merge(int[] arr,int low,int mid,int high){
        int i=low;
        int j=mid+1;
        List<Integer> temp=new ArrayList<>();
        int count=0;
        while(i<=mid && j<=high){
            if(arr[i]<=arr[j]){
                temp.add(arr[i]);
                i++;
            }
            else{
                
                
                //just adding this into merge sort code
                //******
                count+=mid-i+1;
                // ******
                
                temp.add(arr[j]);
                j++;
            }
        }
        while(i<=mid){
            temp.add(arr[i]);
            i++;
            
        }
        while(j<=high){
            temp.add(arr[j]);
            j++;
        }
        for(i=low;i<=high;i++){
            arr[i]=temp.get(i-low);
        }
        return count;
    }
    public int mergesort(int low,int high,int[] arr){
        if(low>=high)return 0;
        int mid=(low+high)/2;
        int count=0;
        count+=mergesort(low,mid,arr);
        count+=mergesort(mid+1,high,arr);
       count+= merge(arr,low,mid,high);
        return count;
    }
     int inversionCount(int arr[]) {
        // Code Here
        int n=arr.length;
       return mergesort(0,n-1,arr);
       
    }
}
````
