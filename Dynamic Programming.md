##Dynamic Programming
<a name="AnchorIndex" id="AnchorIndex"></a>
Index:  
-[Leetcode:Best Time to Buy and Sell Stock III](#Anchor1)  
-[Leetcode:Decode Ways](#Anchor2)  
-[LeetCode:Interleaving String](#Anchor3)  
-[LeetCode:Wildcard Matching](#Anchor3)  

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

-------
<a name="Anchor3" id="Anchor3"></a>
-**[LeetCode:Interleaving String](http://oj.leetcode.com/problems/interleaving-string/)**([Back to Index](#AnchorIndex))   

仔细观察可知，s3的子串需要由s1和s2的子串混合而成，但要求每次从s1和s2中随机按序取出，因此可以使用DP。  

dp[i][j]表达的意思是s3中的前（i+j）长度串是否为 s1中的前i长度串与 s2中的前j长度串混合组成，对于 i >= 1 和 j >= 1有：

                true, if s1[i-1] == s3[i+j-1] && dp[i-1][j] == true
    dp[i][j] =  true, if s2[j-1] == s3[i+j-1] && dp[i][j-1] == true
                false, otherwise

若s1的下标为i-1的字符与s3的下标为i+j-1的字符相同，且s1的前i-1子串与s2的前j子串能够拼合成s3的前i+j-1子串，则s1的前i子串可以与s2的前j串拼合成s3的前i+j子串，调换s1和s2的角色亦然。可以简单的理解为此时从s1中选取了下标为i-1的字符作为s3的下标为i+j-1的字符。
  
```java  
public class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        if (s3.length() != s1.length() + s2.length())
            return false;
        if (s1.length() == 0)
            if (!s2.equals(s3))
                return false;
            else
                return true;
        if (s2.length() == 0)
            if (!s1.equals(s3))
                return false;
            else
                return true;
        boolean[][] dp = new boolean[s1.length() + 1][s2.length() + 1];
        dp[0][0] = true;

        for (int i = 1; i <= s1.length(); i++)
            if (dp[i - 1][0] && s1.charAt(i - 1) == s3.charAt(i - 1))
                dp[i][0] = true;

        for (int j = 1; j <= s2.length(); j++)
            if (dp[0][j - 1] && s2.charAt(j - 1) == s3.charAt(j - 1))
                dp[0][j] = true;

        for (int i = 1; i <= s1.length(); i++)
            for (int j = 1; j <= s2.length(); j++) {
                if (dp[i - 1][j] && s1.charAt(i - 1) == s3.charAt(i + j - 1))
                    dp[i][j] = true;
                if (dp[i][j - 1] && s2.charAt(j - 1) == s3.charAt(i + j - 1))
                    dp[i][j] = true;
            }

        return dp[s1.length()][s2.length()];
    }

    public static void main(String[] args) {
        Solution m = new Solution();
        String s1 = "aabccabc";
        String s2 = "dbbabc";
        String s3 = "aabdbbccababcc";
        boolean result = m.isInterleave(s1, s2, s3);
        System.out.print(result);
    }
}
```  

-------
<a name="Anchor4" id="Anchor4"></a>
-**[LeetCode:Wildcard Matching](http://oj.leetcode.com/problems/wildcard-matching/)  **([Back to Index](#AnchorIndex)) 
  
这道题dp[i][j]表达的意思是s1长度为i的串是否和s2中长度为j的串匹配。但这道题有一个小技巧，就是用s2的字符去匹配s1的字符。当s2的第j个字符是\*时，这里有两种选择：既可以让\*去匹配s1的一个字符，使i前进1，也可以把*当成空白，此时dp[j][i]的状态就等于dp[j-1][i]的状态。当s2遇到?时，和s1的第i个字符与s2的第j个字符相等是等效的，此时dp[j][i]=dp[j-1][i-1]  
```cpp  
class Solution {   
public:   
  bool isMatchDp(string& t, string& p){
    int tLen = t.length();
    int pLen = p.length();
    
    bool dp[pLen+1][tLen+1];
    memset(dp, 0, sizeof(dp));
    
    dp[0][0] = true;
    
    for(int i = 1; i <= pLen; i++){
        if(dp[i-1][0]&&p[i-1]=='*'){
            dp[i][0] = true;
        }
    }
    
    for(int i = 1; i <= pLen; i++){
        for(int j = 1; j <= tLen; j++){
            if((p[i-1] == '?')||p[i-1] == t[j-1]){
                dp[i][j] = dp[i-1][j-1];
            }else if(p[i-1] == '*'){
                dp[i][j] = dp[i-1][j]||dp[i][j-1];
            }else{
                dp[i][j] = false;
            }
        }
    }
    
    return dp[pLen][tLen];
    }
};   
```  