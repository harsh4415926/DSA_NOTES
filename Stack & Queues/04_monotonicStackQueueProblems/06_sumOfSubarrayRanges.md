# sum of subarray ranges
### just return sum of subarray maximums - sum of subarray minimums

````java
class Solution {
    public long subArrayRanges(int[] nums) {
      
        return sumSubarrayMaximums(nums)-sumSubarrayMinimums(nums);
    }




    public long sumSubarrayMinimums(int[] arr) {
        int n=arr.length;
        int[] nse=new int[n];
        int[] pse=new int[n];
        Stack<Integer> st=new Stack<>();
        for(int i=n-1;i>=0;i--){
            while(st.size()>0 && arr[st.peek()]>=arr[i])st.pop();
            if(st.size()==0)nse[i]=n;
            else nse[i]=st.peek();
            st.push(i);
        }
        st.clear();
        for(int i=0;i<n;i++){
            while(st.size()>0 && arr[st.peek()]>arr[i])st.pop();
            if(st.size()==0)pse[i]=-1;
            else pse[i]=st.peek();
            st.push(i);
        }
        
        long ans=0;
        for(int i=0;i<n;i++){
            int leftCovered=i-pse[i];
            int rightCovered=nse[i]-i;
            ans=ans+((long)leftCovered*rightCovered )*arr[i];
        }
        return ans;
    }

    public long sumSubarrayMaximums(int[] arr) {
        int n=arr.length;
        int[] nge=new int[n];
        int[] pge=new int[n];
        Stack<Integer> st=new Stack<>();
        for(int i=n-1;i>=0;i--){
            while(st.size()>0 && arr[st.peek()]<=arr[i])st.pop();
            if(st.size()==0)nge[i]=n;
            else nge[i]=st.peek();
            st.push(i);
        }
        st.clear();
        for(int i=0;i<n;i++){
            while(st.size()>0 && arr[st.peek()]<arr[i])st.pop();
            if(st.size()==0)pge[i]=-1;
            else pge[i]=st.peek();
            st.push(i);
        }
        
        long ans=0;
        for(int i=0;i<n;i++){
            int leftCovered=i-pge[i];
            int rightCovered=nge[i]-i;
            ans=ans+((long)leftCovered*rightCovered )*arr[i];
        }
        return ans;
    }
}
````