# split Array largest sum (leetcode 410)

## exactly same as previous book allocation 

--------------------
## java solution 
````java
class Solution {
    public int neededDevisions(int maxSum,int[] nums){
        int divisions=1;
        int currSum=0;
        for(int i=0;i<nums.length;i++){
            if(currSum+nums[i]<=maxSum){
                currSum+=nums[i];
            }
            else{
                divisions++;
                currSum=nums[i];
            }
        }
        return divisions;

    }
    public int splitArray(int[] nums, int k) {
        int n=nums.length;
        if(k>n)return -1;

        int low=0;
        int sum=0;
        for(int i=0;i<n;i++){
            sum+=nums[i];
            low=Math.max(low,nums[i]);
        }

        int high=sum;
        int ans=sum;
        while(low<=high){
            int mid=low+(high-low)/2;  // max allowed sum

            if(neededDevisions(mid,nums)>k){
                low=mid+1;
            }
            else {
                ans=mid;
                high=mid-1;
                
            }
        }
        return ans;
    }
}
````