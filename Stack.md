##Stack
<a name="AnchorIndex" id="AnchorIndex"></a>
Index:  
-[Leetcode:Evaluate Reverse Polish Notation](#Anchor1)  
-[Leetcode:Longest Valid Parentheses](#Anchor2)   

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
