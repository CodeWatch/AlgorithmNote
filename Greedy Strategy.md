##Greedy Strategy
<a name="AnchorIndex" id="AnchorIndex"></a>
Index:  
-[Cutting digits to make min number](#Anchor1)  
-[LeetCode:Container With Most Water](#Anchor2)  

-------
<a name="Anchor1" id="Anchor1"></a>
-**Cutting digits to make min number**([Back to Index](#AnchorIndex))   

一个n位的数，去掉其中的k位，问怎样去使得留下来的（n-k）位数按原来的前后顺序组成的数最小贪心算法，在每次被访问的位置保证有最优解。求一共n位，求其中的m位组成的数最小。那么这个m位的数，最高位应该在原数的最高位到第m位区间找，要不然就不能当第m位了，其余依次递推。  
```cpp
#include <iostream>
#include <cstdlib>
#include <cassert>
using namespace std;

int findMinIndex(int arr[], int beg, int end) 
{
    if(beg > end)
        return -1;
    int minv = arr[beg];
    int minIndex = beg;
    for(int i = beg + 1; i <= end; ++i)
    {
        if(arr[i] < minv)
        {
            minv = arr[i];
            minIndex = i;
        }
    }
    return minIndex;
}

int getRemain(int arr[], int size, int k)
{
    int rev = 0, revIndex = -1;
    for(int i = size - k; i < size; ++i)
    {
        revIndex = findMinIndex(arr, revIndex + 1, i);
        rev = rev * 10 + arr[revIndex];
    }
    return rev;
}

int main()
{
    int arr[] = {3, 1, 6, 4, 8, 5, 7};
    size_t size = sizeof(arr) / sizeof(int);

    int remainNum;
    for (int k = size-1; k > 0; --k)
    {
        remainNum = getRemain(arr, size, size - k);
        cout << "When k = " << k << ", the remaining value is:" << remainNum << endl;
    }

}
```

-------
<a name="Anchor2" id="Anchor2"></a>
-**[LeetCode:Container With Most Water](http://oj.leetcode.com/problems/container-with-most-water/) **([Back to Index](#AnchorIndex))  

题意可理解为：给定一个数组，从中选出两个数字，从而使得以两个数字下标之差为宽、以两个数字较小者为高的矩形面积最大。使用两指针贪心法，总是移动数字较小的指针，贪心策略如下：  
*  矩形面积受制于两下标之差，但这个差在两指针靠拢过程中总是缩小的，只能考虑高
*  高是由较小者决定的，如果移动数字较大的，只能使面积变小（因为宽也变小了），而移动较小的则有可能变大

```cpp
class Solution {
public:
    int maxArea(vector<int> &height) {
        if(height.size() == 0 || height.size() == 1) return 0;
        int max = 0, S;
        int low = 0, high = height.size()-1;
        while(low<high){
            if(height[low]<=height[high]){
                S = (high-low)*height[low];
                low++;
            }else{
                S = (high-low)*height[high];
                high--;
            }
            if(S>max) max = S; 
        }
        return max;
    }
};
```