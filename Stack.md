##Stack
<a name="AnchorIndex" id="AnchorIndex"></a>
Index:  
-[Leetcode:Evaluate Reverse Polish Notation](#Anchor1)  
-[Leetcode:Longest Valid Parentheses](#Anchor2)   
-[LeetCode:Largest Rectangle in Histogram](#Anchor3)   
-[Maximal Rectangle](#Anchor4)  

-------
<a name="Anchor1" id="Anchor1"></a>
-**[Leetcode:Evaluate Reverse Polish Notation](http://oj.leetcode.com/problems/evaluate-reverse-polish-notation/)**([Back to Index](#AnchorIndex))   
 
逆波兰表达式计算，原理比较简单，用一个栈保存操作数，当遇到操作符时，弹出两个操作数，计算完后将结果压入栈。需要注意的是两个操作数在栈中的先后顺序。
```java 
public class Solution {
    public int evalRPN(String[] tokens) {
 
        int returnValue = 0;
 
        String operators = "+-*/";
 
        Stack<String> stack = new Stack<String>();
 
        for(String t : tokens){
            if(!operators.contains(t)){
                stack.push(t);
            }else{
                int a = Integer.valueOf(stack.pop());
                int b = Integer.valueOf(stack.pop());
                int index = operators.indexOf(t);
                switch(index){
                    case 0:
                        stack.push(String.valueOf(a+b));
                        break;
                    case 1:
                        stack.push(String.valueOf(b-a));
                        break;
                    case 2:
                        stack.push(String.valueOf(a*b));
                        break;
                    case 3:
                        stack.push(String.valueOf(b/a));
                        break;
                }
            }
        }
 
        returnValue = Integer.valueOf(stack.pop());
 
        return returnValue;
 
    }
}
```

-------
<a name="Anchor2" id="Anchor2"></a>
-**[Leetcode:Longest Valid Parentheses](http://oj.leetcode.com/problems/longest-valid-parentheses/)**([Back to Index](#AnchorIndex))   

“连续的括号”这个状态的中断当且仅当出现访问到')'而栈中无'('用于匹配时才会发生。  
因此每次匹配成功时分成两种情况来考虑。
    * 栈为空：当前匹配成功位置与上次失配位置之间可能有0到多个匹配成功的括号对，因此长度为i-last
    * 栈不为空：尚有未被匹配的'('，此时的失配位置就是栈顶的'('的位置，因此长度i-lefts.top()

```cpp
class Solution {
public:

    int longestValidParentheses(string s) {
        int max_len = 0, last = -1; // the position of the last ')'
        stack<int> lefts; // keep track of the positions of non-matching '('s
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] == '(') {
                lefts.push(i);
            } else {
                if (lefts.empty()) {
                    // no matching left
                    last = i;
                } else {
                    // find a matching pair
                    lefts.pop();
                    if (lefts.empty()) {
                        max_len = max(max_len, i - last);
                    } else {
                        max_len = max(max_len, i - lefts.top());
                    }
                }
            }
        }
        return max_len;
    }
};
```

-------
<a name="Anchor3" id="Anchor3"></a>
-**[LeetCode:Largest Rectangle in Histogram](http://oj.leetcode.com/problems/largest-rectangle-in-histogram/)**([Back to Index](#AnchorIndex))  
此题如果用暴力求解同样十分繁琐，以下为O(n)解法，注意到
*  以小矩形A[i]为高的矩形在整个直方图取到的最大面积取决于A[i]右侧的第一个比A[i]小的小矩形A[j]与A[i]形成的宽度
*  如果小矩形A[i]左侧没有比A[i]小的小矩形，则宽度为0到A[j]
*  如果小矩形A[i]右侧没有比A[i]小的小矩形，且左侧也没有（即全局最小），则宽度为直方图的宽度  

这里非常巧妙的运用了stack作为数据结构，用stack维护了以最大递增序列，一旦遇到比栈顶小的小矩形高度则开始计算面积。类似的应用将在Ex4中提及。

**注意**：计算面积时不要使用~~temp = (i - low) * height[low]~~，因为(i - low)并不是以height[low]为高的。举个栗子：  
对于序列1,3,5,4,2的进出栈和运算过程如下：
*  1,3,5顺序入栈
*  4使得5出栈计算面积 temp = (3 - 1 - 1) * 5 = 5 （~~使用错误方式计算为temp = (3 - 2) * 5 = 5，一致~~）
*  4入栈
*  2使得4出栈计算面积 temp = (4 - 1 - 1) * 4 = 8 （~~使用错误方式计算为temp = (4 - 3) * 4 = 4，不一致~~）
*  ...  

显然错误的计算方式会使得计算过程漏掉中间已被出栈的元素形成的宽度，须知这些已出栈的元素高度肯定大于当前元素，是可以与当前元素形成矩形的。

```cpp
    int largestRectangleArea(vector<int> &height) {
        if(height.empty()) return 0;
        stack<int> stk;
        int i = 0, max = 0, temp = 0;
        while(i<=height.size()-1){
            if(stk.empty()||height[i]>height[stk.top()]){//如果递增或者是第一个，入栈
                stk.push(i);
                i++;
            }else{
                int low = stk.top();//出栈
                stk.pop();
                if(stk.empty()){//栈为空说明low的左边都大于height[i]，同时low到i也是大于height[i]的
                    temp = i * height[low];
                }else{
                    temp = (i - stk.top() - 1) * height[low];
                }
                max = max > temp?max:temp;
            }
        }
        while(!stk.empty()){
                int low = stk.top();
                stk.pop();
                if(stk.empty()){
                    temp = height.size() * height[low];
                }else{
                    temp = (height.size() - stk.top() -1) * height[low];
                }
                max = max > temp?max:temp;
        }

        return max;
    }
};
```

-------
<a name="Anchor4" id="Anchor4"></a>
-**[Maximal Rectangle](http://oj.leetcode.com/problems/maximal-rectangle/)**([Back to Index](#AnchorIndex))

一个较为巧妙并高效的解法是将其降维至一维，并应用[LeetCode:Largest Rectangle in Histogram](#Anchor3)的算法解决。 

降维举例：  
1 1 1  
1 0 1  
0 1 1  
分别以第1行，第1、2行和第1、2、3行为目标，得到  
1 1 1      ->     1 1 1     ->     应用一维算法    

1 1 1  
1 0 1      ->     2 0 2     ->     应用一维算法   

1 1 1  
1 0 1  
0 1 1      ->     0 0 3     ->     应用一维算法  
故最大面积为3。
```cpp
class Solution {
public:
    int maximalRectangle(vector<vector<char> > &matrix) {
        if(matrix.empty()) return 0;
        if(matrix[0].empty()) return 0;
        vector<int> temp(matrix[0].size(), 0);//record histogram value
        stack<int> stk;
        int curIndex = 0, maxArea = 0;
        for(int i = 0; i <= matrix.size()-1; i++){
            for(int k = 0; k <= matrix[0].size()-1; k++){
                if(matrix[i][k] == '1'){
                    temp[k] += 1;
                }else{
                    temp[k] = 0; 
                }
            }
            for(int j = 0; j <= matrix[0].size()-1;){
                if(stk.empty()|| temp[j]>temp[stk.top()]){
                    stk.push(j);
                    j++;
                }else{
                    curIndex = stk.top();
                    stk.pop();
                    int tempArea = 0;
                    if(!stk.empty()){
                        tempArea = (j-stk.top()-1)*temp[curIndex];
                    }else{
                        tempArea = j*temp[curIndex];
                    }
                    maxArea = maxArea>tempArea?maxArea:tempArea;
                }
            }
            while(!stk.empty()){
                    curIndex = stk.top();
                    stk.pop();
                    int tempArea = 0;
                    if(!stk.empty()){
                        tempArea = (temp.size()-stk.top()-1)*temp[curIndex];
                    }else{
                        tempArea = temp.size()*temp[curIndex];
                    }
                    maxArea = maxArea>tempArea?maxArea:tempArea;
            }
        }
        return maxArea;
    }
};
```