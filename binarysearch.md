
## 1. search range is value instead of index
- sometimes we can use value range instead of index
- the mechanism is: if the number of value smaller than our middle value is larger than target, it means our target is smaller than middle value, we can in this case reduce search range, vice versal
```
public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        int lo = matrix[0][0],hi = matrix[n-1][n-1];
        while(lo <= hi) {
            int mid = lo + (hi - lo)/2;
            int count = getSmaller(matrix, mid); //get elements smaller/equal to value
            if(count < k) {
                lo = mid+1;
            }else{
                hi = mid-1;
            }
        }
        return lo;
    }
    public int getSmaller(int[][] matrix, int mid) {
        int n = matrix.length;
        int i = 0, j = n-1, ret = 0;
        while(j >= 0 && i < n) {
            if(matrix[i][j] <= mid) {
                ret += j+1;
                i++; 
            }else{
                j--;
            }
        }
        return ret;
    }
```

##1.2 
1060. Missing Element in Sorted Array

```
class Solution {
    public int missingElement(int[] nums, int k) {
        int n = nums.length;
        int numMissing = nums[n-1] - nums[0] - n + 1;
        if(k > numMissing) {
            //we return upper bound number 
            return nums[n-1] + (k - numMissing);
        }
        int l = 0, r = n-1;
        while(l < r-1) {
            int mid = l + (r-l)/2;
            int missing = nums[mid] - nums[l] - (mid - l);
            if(missing < k) {
                k -= missing;
                l = mid;
            }else {
                r = mid;
            }
        }
        return nums[l]+k;
    }
}
```
