##Array & String
<a name="AnchorIndex" id="AnchorIndex"></a>
Index:  
-[Leetcode:single number II](#Anchor1)  
-[Find Reverse Pair](#Anchor2)  
-[Find Min K](#Anchor3)  
-[Find Number occurs half times](#Anchor4)  
-[Leetcode:Median of Two Sorted Arrays](#Anchor5)  
-[Leetcode:Longest Substring Without Repeating Characters](#Anchor6)  
-[Leetcode:Substring with Concatenation of All Words](#Anchor7)  

-------
<a name="Anchor1" id="Anchor1"></a>
-**[Leetcode:single number II](http://oj.leetcode.com/problems/single-number-ii/)**([Back to Index](#AnchorIndex))    
  
两个变量，ones和twos，顺序遍历并且求出异或值，ones表示异或值哪些位 比特1出现一次，twos表示异或值哪些位 比特1出现两次，当异或值的某些位 比特1出现三次的时候，就要对其进行清除，这样就能保证最后得到的ones就是出现一次的数  
```java
public class Solution {
	public int singleNumber(int[] A) {
		int one = 0, two = 0, erase = 0;
		for (int i = 0; i < A.length; i++) {
			two |= one & A[i];
			one ^= A[i];
			erase = ~(one & two);
			two &= erase;
			one &= erase;
		}

		return one;
	}
}
```
另外一个更直观更通用的解法是模拟法，但此种解法在OJ上可能超时。如下：
```cpp
#define ARY 3

int singleNumber(int A[], int n){
	int ret = 0;
	int count[32];
	memset(count,0,sizeof(count));
	for(int i = 0; i <= 31; i++){
		int b = 1 << i;
		for(int j = 0; j <= n-1; j++){
			if(A[j] & b){
				count[i]++;
			}
		}
		if(count[i]%ARY){
			ret |= b;
		}
	}
	return ret;
}
```
在解决复杂同类问题时，这种方法更显得直观，如一组数据中出现两个不同的数字各一次（或k次），其余均出现a次，求出那两个数字，代码如下：
```cpp
int singleNumber(int A[], int n){
	int ret = 0;
	int count[32];
	memset(count,0,sizeof(count));
	for(int i = 0; i <= 31; i++){
		int b = 1 << i;
		for(int j = 0; j <= n-1; j++){
			if(A[j] & b){
				count[i]++;
			}
		}
		if(count[i]%ARY){
			ret |= b;
		}
	}
	return ret;
}

int singleNumberSpecial(int A[], int n, int k){
	int ret = 0;
	int count[32];
	memset(count,0,sizeof(count));
	for(int i = 0; i <= 31; i++){
		int b = 1 << i;
		for(int j = 0; j <= n-1; j++){
			if(A[j] & b){
				count[i]++;
			}
		}
		if(count[i]%ARY == k){
			ret |= b;
		}
	}
	return ret;
}

vector<int> twoSingleNumber(int A[], int n, int k){
	int twoNum = singleNumberSpecial(A, n, k);
	int b = twoNum & (-twoNum);
	int B[n],C[n];
	int bi = 0,ci = 0;
	int single1 = 0, single2 = 0;
	for(int i = 0; i <= n-1; i++){
		if(A[i] & b){
			B[bi++] = A[i]; 
		}else{
			C[ci++] = A[i];
		}
	}
	single1 = singleNumber(B, bi);
	single2 = singleNumber(C, ci);
	vector<int> vec;
	vec.push_back(single1);
	vec.push_back(single2);
	return vec;
}
```
singleNumberSpecial()先找到两个数字不同的bit位，然后利用该bit位将数组分成两组，再应用之前的方法求解。

-------
<a name="Anchor2" id="Anchor2"></a>
-**Find Reverse Pair**([Back to Index](#AnchorIndex))    
问题描述：在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。      
使用普通的比较时间复杂度是n方。这里介绍一种复杂度为nlogn的方法。该方法要借助归并排序，即将数组分为两部分分别排序，然后merge。之后在对两个子数组进行merge的时候，去比较子数组1的当前元素和子数组2的当前元素，若1大于2，则说明1后面的元素都大于2，所以都为逆序对，将这些数加入结果集。  
```java
public class Main {
    static long count = 0;
 
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        while (in.hasNext()) {
            int n = in.nextInt();
            int[] a = new int[n];
            for (int i = 0; i < n; i++)
                a[i] = in.nextInt();
            count = 0;
            mergeSort(a);
            System.out.println(count);
        }
    }
 
    // 将有二个有序数列a[first...mid]和a[mid...last]合并。
    static void mergearray(int a[], int first, int mid, int last, int temp[]) {
        int i = first, j = mid + 1;
        int m = mid, n = last;
        int k = 0;
        while (i <= m && j <= n) {
            if (a[i] > a[j]) {
                // 左数组比右数组大
                temp[k++] = a[j++];
                // 因为如果a[i]此时比右数组的当前元素a[j]大，
                // 那么左数组中a[i]后面的元素就都比a[j]大
                // 【因为数组此时是有序数组】
                count += mid - i + 1;
            } else {
                temp[k++] = a[i++];
            }
        }
        while (i <= m) {
            temp[k++] = a[i++];
        }
        while (j <= n) {
            temp[k++] = a[j++];
        }
        for (i = 0; i < k; i++)
            a[first + i] = temp[i];
    }
 
    static void mergesort(int a[], int first, int last, int temp[]) {
        if (first < last) {
            int mid = (first + last) / 2;
            mergesort(a, first, mid, temp); // 左边有序
            mergesort(a, mid + 1, last, temp); // 右边有序
            mergearray(a, first, mid, last, temp); // 再将二个有序数列合并
        }
    }
 
    static void mergeSort(int a[]) {
        int[] p = new int[a.length];
        mergesort(a, 0, a.length - 1, p);
    }
}
```

-------
<a name="Anchor3" id="Anchor3"></a>
-**Find Min K**([Back to Index](#AnchorIndex))  
问题描述：最小的K个数:输入n个整数，找出其中最小的K个数,并按从小到大顺序打印。     
Solution 1:
在这道题中我们利用快速排序的思想，每次都将范围内第一个数作为枢轴，找到前面大于枢轴值的数和后面小于枢轴值的数交换，最后将枢轴值和小于枢轴值的最后一个数交换，完成快速排序。现在数组被分成了两部分，一边小于枢轴值，一边大于枢轴值，等于枢轴值得中间数组下标为t。若k>t，则说明前k个数在后面的子数组里也有，则要对后面排序；若不大于，则不用管后面的子数组。该算法叫做Quick Select，时间复杂度O(n)。  
```cpp
#include<iostream>
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
using namespace std;
#define LL long long
void swap(LL *a, LL *b){
    LL tmp=*a;
    *a=*b;
    *b=tmp;
}
 
int partition_arr(int low,int high,LL *arr,const int k){
    if(low>=high)
        return 0;
    LL tmp=arr[low];
    int i=low,j=high+1;
    LL pivot=arr[low];
    while(1){
        while(arr[++i]<pivot);
        while(arr[--j]>pivot);
        if(i<j)
            swap(arr[i],arr[j]);
        else
            break;
         
    }
    swap(arr[low],arr[j]);
    if(low<j-1) partition_arr(low,j-1,arr,k);
    if(j+1<high&&j+1<=k) partition_arr(j+1,high,arr,k);
    return 0;
}
int main()
{
    int n,k,i;
    while(~scanf("%d%d",&n,&k)){
        LL *arr=new LL [n];
        for(i=0;i<n;i++)
            scanf("%lld",&arr[i]);
        partition_arr(0,n-1,arr,k-1);
        if(n>=k){
            printf("%d",arr[0]);
            for(i=1;i<k;i++)
                printf(" %lld",arr[i]);
            printf("\n");
        }
    }
    return 0;
}
```

Solution 2:
求解Min K也可以使用堆排序，使用容量为K大根堆，初始时，使用数组的前K个元素建堆，然后对N-K个元素进行如下操作：  

    if A[i] >= heap.top 
    	continue  
    else 
    	pop the heap.top
    	insert A[i] to heap  

这里的操作说明大根堆里时刻维护的是下标为0到i-1中最小的K个数，如果后续某个数小于这K个数中最大的数，则应该取代该数并成为最小的K个数中的一员。该算法的时间复杂度为O(K+(N-K)logK)，空间复杂度为O(K)。
```cpp
#include <iostream>
#include <priority_queue>

using namespace std;

int main(){
    int N, K;
    cin >> N >> K;// we suppose N > K here and do nothing for unexpected input
    int A[N];
    for(int i = 0; i <= N-1; i++){
        cin >> A[i];
    }
    priority_queue<int> heap;// we omit two other paras because we just use max heap
    for(int i = 0; i <= K-1; i++){
        heap.push(A[i]);
    }
    for(int i = K; i <= N-1; i++){
    	if(A[i] < heap.top()){
    	    heap.pop();
    	    heap.push(A[i]);
    	}
    }
    while(!heap.empty()){
    	cout << heap.top();
    	heap.pop();
    }
    return;
}
```

-------
<a name="Anchor4" id="Anchor4"></a>
-**Find Number occurs half times**([Back to Index](#AnchorIndex))   
问题描述：找出出现次数刚好是一半的数字。有N个数，其中有一个数刚好出现一半次数，要求在线性时间内求出这个数。  
首先，该数字占总数的一半（假设为x），说明总数必为偶数；其次，最后一个元素或者是x，或者不是x，因此只要在扫描数组的时候每一个元素都与最后一个元素做比较，如果相等则最后一个元素的个数加1，否则不处理。如果最后一个元素的个数为N/2,（N为数组元素个数）则它就是x，否则x就是前面N-1个元素中选出的candidate。  
```cpp
int MoreThanHalf(int a[], int N)
{
    int sum1 = 0;//最后一个元素的个数
    int sum2 = 0;
    int candidate;
    int i;
    for(i=0;i<N-1;i++)//扫描前N-1个元素
    {
        if(a[i] == a[N-1])//判断当前元素与最后一个是否相等
        sum1++;
        if(sum2 == 0)
        {
             candidate = a;
             sum2++;
        }
        else
        {
             if(a[i] == candidate)
                 sum2++;
             else
                 sum2--;
        }
     }

     if((sum1+1) == N/2)
         return a[N-1];
     else
         return candidate;
}
```

-------
<a name="Anchor5" id="Anchor5"></a>
-**[Leetcode:Median of Two Sorted Arrays](http://oj.leetcode.com/problems/median-of-two-sorted-arrays/)**([Back to Index](#AnchorIndex))     
每次A B数组的k/2位置的元素进行比较，舍弃值较小的数所在数组的前k/2个数，如此迭代，这样每次能去除掉一半的元素，时间复杂度为O(log(m+n))。    
```java
class Solution {
public:

    double findMedianSortedArrays(int A[], int m, int B[], int n) {
        int total = m + n;
        if (total & 0x1)
            return find_kth(A, m, B, n, total / 2 + 1);
        else
            return (find_kth(A, m, B, n, total / 2)
                + find_kth(A, m, B, n, total / 2 + 1)) / 2;
    }
private:

    static double find_kth(int A[], int m, int B[], int n, int k) {
        //always assume that m is equal or smaller than n
        if (m > n) return find_kth(B, n, A, m, k);
        if (m == 0) return B[k - 1];
        if (k == 1) return min(A[0], B[0]);
        //divide k into two parts
        int pa = min(k / 2, m), pb = k - pa;
        if (A[pa - 1] < B[pb - 1])
            return find_kth(A + pa, m - pa, B, n, k - pa);
        else if (A[pa - 1] > B[pb - 1])
            return find_kth(A, m, B + pb, n - pb, k - pb);
        else
            return A[pa - 1];
    }
};
```

-------
<a name="Anchor6" id="Anchor6"></a>
-**[Leetcode:Longest Substring Without Repeating Characters](http://oj.leetcode.com/problems/longest-substring-without-repeating-characters/)**([Back to Index](#AnchorIndex))  

使用快慢指针维护一个区间，使用set时刻保持区间内没有相同字符，核心思想：

    * 如果有重复字符，移动slow指针
    * 如果无重复字符，移动fast指针

注意最后一次更新最长区间值。

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int ret = 0;
        int n = s.size();
        if(n == 0) return ret;
        int fast = 0, slow = 0;
        unordered_set<char> charDict;
        while(fast <= n-1){
            if(charDict.find(s[fast]) != charDict.end()){
                int len = fast - slow;
                if(len > ret) ret = len;
                charDict.erase(charDict.find(s[slow]));
                slow++;
            }else{
                charDict.insert(s[fast]);
                fast++;
            }
        }
        if(fast-slow > ret) ret = fast-slow;
        return ret;        
    }
};
```

-------
<a name="Anchor7" id="Anchor7"></a>
-**[Leetcode:Substring with Concatenation of All Words](http://oj.leetcode.com/problems/substring-with-concatenation-of-all-words/)**([Back to Index](#AnchorIndex))  

一个比较直观的解法（不一定最优）。维护一个< 单词，个数 >map，假设L中单词长度为wordLen，每次从S中截取L.size()*wordLen的子串，判断该子串是否能完全消耗map中的单词，如果可以，则该起始位置为有效解；如果不能，则将起始位置向后推进wordLen个位置。  

```cpp
class Solution {
public:
    vector<int> findSubstring(string S, vector<string> &L) {
        vector<int> ret;
        int n = S.size();
        int m = L.size();
        if(n == 0 || m == 0) return ret;
        int wordLen = L[0].size();
        map<string, int> dict;
        for(int i = 0 ; i <= m-1; i++){
            if(dict.find(L[i]) != dict.end()){
                dict[L[i]]++;
            }else{
                dict[L[i]] = 1;
            }
        }
        for(int i = 0; i <= n-m*wordLen; i++){
            string sub = S.substr(i, m*wordLen);
            map<string, int> dictCopy = dict;
            bool isValid = true;
            for(int j = 0; j <= m-1; j++){
                string word = sub.substr(j*wordLen, wordLen);
                map<string, int>::iterator itr = dictCopy.find(word);
                if(itr != dictCopy.end()){
                    if(itr->second == 0){
                        isValid = false;
                        break;
                    }else{
                        dictCopy[word]--;
                    }
                }else{
                    isValid = false;
                    break;
                }
            }
            if(isValid){
                ret.push_back(i);
            }
        }
        return ret;
    }
};
```