```
class Solution {

int INT_MIN = -2000 * 10000 - 1;

  

public int maxSum(int[] nums, int k, int m) {

int[][][] dp = new int[nums.length][m][k+1];

for(int i=0; i<nums.length; i++)

for(int j=0; j<m; j++)

for(int l=0; l<k+1; l++)

dp[i][j][l] = INT_MIN;

return getMaxSum(nums, 0, 0, k, dp, m);

}

  
  

private int getMaxSum(int[] nums, int index, int size, int rem, int[][][] DP, int m) {

if(index == nums.length && (size == 0) && (rem == 0)) {

return 0;

} else if (index == nums.length) {

return INT_MIN;

}

if (rem == 0) return 0;

if(dp[index][size][rem]!=INT_MIN) return dp[index][size][rem];

if(size == 0) {

int option1 = nums[index] + getMaxSum(nums, index+1, 1, rem, DP, m);

int option2 = getMaxSum(nums, index+1, 0, rem, DP, m);

dp[index][size][rem] = Math.max(option1, option2);

} else if(size < m-1) {

int option1 = nums[index] + getMaxSum(nums, index+1, size+1, rem, DP, m);

dp[index][size][rem] = option1;

} else {

int option1 = nums[index] + getMaxSum(nums, index+1, 0, rem-1, DP, m);

int option2 = nums[index] + getMaxSum(nums, index+1, size, rem, DP, m);

int option3 = getMaxSum(nums, index+1, 0, rem-1, DP, m);

dp[index][size][rem] = Math.max(Math.max(option1, option2), option3);

}

return dp[index][size][rem];

}

}
```