##Greedy Strategy
<a name="AnchorIndex" id="AnchorIndex"></a>
Index:  
-[Cutting digits to make min number](#Anchor1)  

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