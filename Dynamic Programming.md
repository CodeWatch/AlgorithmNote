##Dynamic Programming
<a name="AnchorIndex" id="AnchorIndex"></a>
Index:  
-[Leetcode:Best Time to Buy and Sell Stock III](#Anchor1)  
-[Leetcode:Decode Ways](#Anchor2)  

-------
<a name="Anchor1" id="Anchor1"></a>
-**[Leetcode:Best Time to Buy and Sell Stock III](http://oj.leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)**([Back to Index](#AnchorIndex))     
  
分别记录到i时，0-i的最大利润和i+1到len-1的最大利润，相加即可。具体做法是
  * 使用forwardMax[i]记录0到i的最大利润（正向）
  * 使用backwardMax[i]记录n-1到i的最大利润（反向）
```java
public class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length < 2) return 0;
        int[] forwardMax = new int[prices.length];
        int[] backwardMax = new int[prices.length];
        
        int buyPoint = prices[0];
        int sellPoint = prices[0];
        int maxProfit = 0;
        forwardMax[0] = 0;
        
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] < buyPoint) {
                buyPoint = prices[i];
            }
            forwardMax[i] = Math.max(prices[i] - buyPoint, forwardMax[i - 1]);
        }
        sellPoint = prices[prices.length - 1];
        backwardMax[prices.length - 1] = 0;
        
        for (int i = prices.length - 2; i >= 0; i--) {
            if (prices[i] > sellPoint) {
                sellPoint = prices[i];
            }
            backwardMax[i] = Math.max(sellPoint - prices[i], backwardMax[i + 1]);
        }
        
        int max = 0;
        for (int i = 0; i < prices.length - 1; i++) {
            if (forwardMax[i] + backwardMax[i + 1] > max) {
                max = forwardMax[i] + backwardMax[i + 1];
            }
        }
        if (max < Math.max(forwardMax[prices.length - 1], backwardMax[0])) {
            max = Math.max(forwardMax[prices.length - 1], backwardMax[0]);
        }
        return max;
    }
}
```

-------
<a name="Anchor2" id="Anchor2"></a>
-**[Leetcode:Decode Ways](http://oj.leetcode.com/problems/decode-ways/)**([Back to Index](#AnchorIndex))   
分析题意可以发现字符串s的子串s.substr(0,i)对s.substr(0,i-1)和s.substr(0,i-2)存在依赖关系，即可以将原问题分解为小的子问题求解，通过记录子问题求解的结果来快速计算当前问题的结果。当字符串长度为1时，需要dp[0] = 1作为虚拟的初始子问题解。  

一般的,dp[i]表示s.substr(0,i)的解码方式数量，则dp数组的求解过程如下：

```cpp
class Solution {
public:
    int numDecodings(string s) {
        int n = s.size();
        if(n <= 0) return 0;
        int dp[n+1];
        memset(dp, 0, sizeof(dp));
        dp[0] = 1;
        for(int i = 1; i <= n; i++){
            if(s[i-1] == '0'){
                if(i == 1 || !(s[i-2] == '1' || s[i-2] == '2')){
                    dp[i] = 0;
                }else{
                    dp[i] += dp[i-2];
                }
            }else if(s[i-1] >= '1' && s[i-1] <= '6'){
                dp[i] = dp[i-1];
                if(i != 1 && (s[i-2] == '1' || s[i-2] == '2')){
                    dp[i] += dp[i-2];
                }
            }else{
                dp[i] = dp[i-1];
                if(i != 1 && s[i-2] == '1'){
                    dp[i] += dp[i-2];
                }
            }
        }
        return dp[n];
    }
};
```

