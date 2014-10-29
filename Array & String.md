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
-[LeetCode:Next Permutation](#Anchor8)  
-[LeetCode:Permutation Sequence](#Anchor9)  
-[Leetcode:Multiply Strings](#Anchor10)  
-[Leetcode:Implement strStr()](#Anchor11)  
-[Leetcode:String to Integer (atoi)](#Anchor12)  
-[LeetCode:Maximum Subarray](#Anchor13)  
-[Maximum SubMatrix](#Anchor14)  
-[LeetCode:Two Sum](#Anchor15)  
-[LeetCode:3Sum](#Anchor16)  
-[LeetCode:4Sum](#Anchor17)  
-[LeetCode:Combination Sum](#Anchor18)  
-[LeetCode:Combination Sum II](#Anchor19)  

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
        unordered_set< char > charDict;
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

-------
<a name="Anchor8" id="Anchor8"></a>
-**[LeetCode:Next Permutation](http://oj.leetcode.com/problems/next-permutation/)**([Back to Index](#AnchorIndex))  

高效的方法求下一个Permutation。定义单调递增为最小的序列（如1,2,3,4,5），单调递减为最大的序列（如5,4,3,2,1），序列1,2,3,4,5的下一个Permutation为1,2,3,5,4，序列5,4,3,2,1的下一个Permutation为1,2,3,4,5。

可以这样理解该方法，想象当前数列形成一个开口向下的折线，折线的左半部分单调递增，折线的右半部分单调递减。对于右半单调递减部分（包含顶点），已经没有办法调换其中的数字使得整体数列变大，因为其本身已经是右半部分子序列的最大情形，因此通过交换左半递增部分和右半递减部分来实现，交换的两数为**折线顶点左侧的第一个数x**和**右半递减部分第一个大于x的数y**，这样保证了产生了比原来更大的序列（因为y > x）。为了完成目标，还需要对原右半递减部分进行升序排序，保证产生了恰比原来大的序列（选用了第一个大于x的y且右半部分经过排序成为右半部分的最小序列）。

这里可以用倒置右半部分代替排序，原因是y是第一个大于x的数，交换这两个数不会打破右半部分的单调递减特性。  

注意，该方法适用于具有重复元素的数组生成下一个排列。
```cpp
class Solution {
public:
    void nextPermutation(vector<int> &num) {
        for(int i = num.size()-1; i>=1; i--){//改变右起的第一个升序对可以使序列变大
            if(num[i]>num[i-1]){//num[i-1]和num[i]是右起第一个升序对，此时下标从i到size()-1的元素是降序
                for(int j = num.size()-1; j>=i; j--){
                    if(num[j]>num[i-1]){
                        swap(num[j], num[i-1]);
                        reverse(num.begin()+i,num.end());
                        return;
                    }
                }
            }
        }
        reverse(num.begin(),num.end());//说明是降序，直接反转为升序为最小的排列
    }
};
``` 

-------
<a name="Anchor9" id="Anchor9"></a>
-**[LeetCode:Permutation Sequence](http://oj.leetcode.com/problems/permutation-sequence/)**([Back to Index](#AnchorIndex))  

此题要求寻找给定位数的Permutation的第k个序列，以单调递增序列（如1,2,3,4,5）作为第1个序列。可以应用[Next Permutation](#Anchor8)方法，但效率太低。  

此处使用另外一种方法求解。*TODO*:算法说明。  

注意，给定的k可能超过n！。

```cpp
string getPermutation(int n, int k) {
    int o[n+1];
    o[0]=1;
    for(int i=1;i<=n;i++)
        o[i]=o[i-1]*i;
    vector<int> v;
    string s = "";
    if(k>o[n])
        return s;
    for(int i=1;i<=n;i++)
        v.push_back(i);
    while(n>0){
        int cnt = (k-1)/o[n-1];
        s+=v[cnt]+'0';
        v.erase(v.begin()+cnt);
        k=k%o[n-1];
        if(k==0)
            k=o[n-1];
        n--;
    }    
    return s;
}
```

-------
<a name="Anchor10" id="Anchor10"></a>
-**[Leetcode:Multiply Strings](http://oj.leetcode.com/problems/multiply-strings/)**([Back to Index](#AnchorIndex))  

限定此题的输入为两个正整数字符串，要求这两个正整数的乘积。思路比较清晰：
    
    a) 对num1逆序
    b) 对num2逆序
    c) res[i+j] += num1[i] * num2[j];
    d) res[i+j+1] += res[i+j] / 10;
    e) res[i+j] = res[i+j] % 10; 

注意最后检查结果是否全为0，并去除结果的前导0。

```cpp
class Solution {
public:
    string multiply(string num1, string num2) {
        int len1 = num1.length();
        int len2 = num2.length();
        vector<int> res(len1+len2);
        vector<int> n1;
        vector<int> n2;
        for(int i=num1.length()-1;i>=0;--i)
            n1.push_back(num1[i]-'0');
        for(int i=num2.length()-1;i>=0;--i)
            n2.push_back(num2[i]-'0');
        for(int i=0;i<n1.size();++i){
            for(int j=0;j<n2.size();++j){
                res[i+j] += n1[i]*n2[j];
                res[i+j+1] += res[i+j]/10;
                res[i+j] = res[i+j]%10;
            }
        }
        string ret;
        int i=res.size()-1;
        while(res[i]==0 && i>=0)
            --i;
        if(i<0)
            return "0";
        while(i>=0){
            ret.push_back('0'+res[i]-0);
            --i;
        }
        return ret;
    }
};
```

-------
<a name="Anchor11" id="Anchor11"></a>
-**[Leetcode:Implement strStr()](oj.leetcode.com/problems/implement-strstr/)**([Back to Index](#AnchorIndex))  

haystack为主串，needle为匹配串。思路比较简单：每次选取主串中的一个字符作为主串的起始匹配位置，选取子串的头作为另一个起始匹配位置，匹配跳出条件：
    
    * 对应位置字符不同
    * 两个字符串的其中一个或两个均到达'\0'位置

可以这些跳出情况，只需要判断跳出时needle是否已经到达'\0'位置即可，如果到达，则说明找到该子串，返回该子串在主串中的头字符位置即可。

```cpp
class Solution {
public:
    char *strStr(char *haystack, char *needle) {
        int len1 = strlen(haystack);
        int len2 = strlen(needle);
        if (len1 < len2)
            return NULL;
        if (len2 == 0)
            return haystack;
        char* p2 = haystack;
        for (int i = 0; i < len1 - len2 + 1; i++) {
            char* p1 = needle;
            char* p_old = (char*) p2;
            while (*p1 && *p2) {
                if (*p1 == *p2) {
                    p1++;
                    p2++;
                } else
                    break;
            }
            if (!*p1)
                return p_old;
            p2 = p_old + 1;
        }
        return NULL;
    }
};
```

-------
<a name="Anchor12" id="Anchor12"></a>
-**[Leetcode:String to Integer (atoi)](oj.leetcode.com/problems/string-to-integer-atoi/) **([Back to Index](#AnchorIndex))  

注意判断先导空格，符号和溢出情况。  

```cpp
class Solution {
public:

    int atoi(const char *str) {
        int i = 0;
        int len = strlen(str);
        bool positive = true;
        while (str[i] == ' ' && i < len)
            i++;
        if (str[i] == '+')
            i++;
        else if (str[i] == '-') {
            positive = false;
            i++;
        }
        long long sum = 0;
        for (; i < len; i++) {
            if (str[i] < '0' || str[i] > '9')
                break;
            sum = sum * 10 + str[i] - '0';
        }
        sum = positive == true ? sum : -1 * sum;
        int min = 0x80000000;
        int max = 0x7fffffff;
        if (sum < min)
            return 0x80000000;
        else if (sum > max)
            return 0x7fffffff;
        return (int) sum;
    }
};
```

-------
<a name="Anchor13" id="Anchor13"></a>
-**[LeetCode:Maximum Subarray](http://oj.leetcode.com/problems/maximum-subarray/)**([Back to Index](#AnchorIndex))  

经典问题，求解连续子数组最大和。核心思想是维护一个当前和值sum，如果sum比max大，则更新；如果sum小于0，则对后面的子数组没有贡献，肯定不属于可行解的一部分，舍去，最终求出结果。
```cpp
class Solution {
public:
    int maxSubArray(int A[], int n) {
        int sum = 0, max = INT_MIN;
        for(int i = 0; i <= n-1; i++){
            sum += A[i];
            if(sum > max){
                max = sum;
            }
            if(sum < 0){
                sum = 0;
            }
        }
        return max;
    }
};
```

-------
<a name="Anchor14" id="Anchor14"></a>
-**Maximum SubMatrix**([Back to Index](#AnchorIndex))   

求解一个二维数组A[N][M]拥有最大和值的子数组，如  
1 -1 -1  
-1 5 7  
-1 6 8  
结果子数组为  
5 7  
6 8  
和值为26  

考虑将二维数组降维为一维处理，可以获得O(N<sup>2</sup>M)的时间复杂度。具体做法是使用s，e作为上下界确定矩形的高，将s和e之间的**一列元素的和**等同于一维问题的一个元素。对于特定的s和e，应用一维的O(n)算法求出特定s和e情形下的最大值；穷举s和e的组合，求出全局的最大值。使用dp的方法事先求解列和，dp[i][j]表示第j列中第0个至第i个的和值。

```cpp
int Solution(vector<vector<int> > input){
    if(input.empty()) return INT_MIN;
    if(input[0].empty()) return INT_MIN;
    
    int ret = INT_MIN;
    int row = input.size(), col = input[0].size();
    
    int dp[row][col];
    for(int i = 0; i <= row-1; i++){
        for(int j = 0; j <= col-1;j++){
            if(i == 0){
                dp[i][j] = input[i][j];
            }else{
                dp[i][j] = input[i][j] + dp[i-1][j];
            }
        }
    }
    
    int A[col];
    for(int s = 0; s <= row-1; s++){
        for(int e = s; e <= row-1; e++){
            for(int k = 0; k <= col-1; k++){
                if(s == 0){
                    A[k] = dp[e][k];
                }else{
                    A[k] = dp[e][k] - dp[s-1][k];
                }
            }
            int temp = MaxSubArray(A, col);
            ret = temp > ret?temp:ret;
        }
    }
    return ret;
}

int MaxSubArray(int A[], int n){
    int sum = 0, max = INT_MIN;
    for(int i = 0; i <= n-1; i++){
        sum += A[i];
        if(sum > max){
            max = sum;
        }
        if(sum < 0){
            sum = 0;
        }
    }
    return max;
}
```

-------
<a name="Anchor15" id="Anchor15"></a>
-**[LeetCode:Two Sum](http://oj.leetcode.com/problems/two-sum/)**([Back to Index](#AnchorIndex))   

基础题，寻找两个数和为给定值，可以采用两指针贪心法。这里展示map方法，即把数列以(value,positon)的形式存入map，然后使用target-numbers[i]查找。注意输出格式。
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int> &numbers, int target) {
        map<int,int> temp;
        vector<int> result;
        
        for(int i = 0;i<=numbers.size()-1;i++){
            temp.insert(pair<int,int>(numbers.at(i),i+1));
        }
        for(int i = 0;i<=numbers.size()-1;i++){
            map<int,int>::iterator itr = temp.find(target-numbers.at(i));
            if(itr!=temp.end()){
                if(itr->second == i+1){
                    continue;
                }
                if(itr->second<i+1){
                    result.push_back(itr->second);
                    result.push_back(i+1);
                }else{
                    result.push_back(i+1);
                    result.push_back(itr->second);
                }
                return result;
            }
        }
        return result;
    }
};
```

-------
<a name="Anchor16" id="Anchor16"></a>
-**[LeetCode:3Sum](http://oj.leetcode.com/problems/3sum/)**([Back to Index](#AnchorIndex))   

寻找和为定值的3个数可以通过事先选定一个数降维为two sum问题，同理寻找和为定值的k个数都可以通过多次降维最终通过two sum解决，此题虽然求得是3sum，这里给出ksum的通用解法。其中k==2的递归出口使用的就是两指针贪心法。

**注意**，为了使用两指针方法，必须对数列**预先排序**。
```cpp
class Solution {
public:
    vector<vector<int> > threeSum(vector<int> &num) {
        vector<vector<int>> ret;
        if(num.size()<3) return ret;
        sort(num.begin(), num.end());
        ret = findKSum(num, 0, 3, 0);
        return ret;
    }
    
    vector<vector<int> > findKSum(vector<int> &num, int start, int k, int target){//require sorted vector
        vector<vector<int>> ret;
        unordered_set<int> visited;
        if(k == 2){
            int i = start, j = num.size()-1;
            while(i < j){
                if(visited.find(num[i]) != visited.end()){
                    i++;
                    continue;
                }

                int sum = num[i] + num[j];
                if(sum == target){
                    vector<int> tempV;
                    tempV.push_back(num[i]);
                    tempV.push_back(num[j]);
                    visited.insert(num[i]);
                    visited.insert(num[j]);
                    ret.push_back(tempV);
                    i++;
                    j--;
                }else if(sum < target){
                    i++;
                }else{
                    j--;
                }
            }
        }else{
            for(int i = start; i <= num.size()-1; i++){
                if(visited.find(num[i]) != visited.end()){
                    continue;
                }else{
                    visited.insert(num[i]);
                    vector<vector<int>> tempRet = findKSum(num, i+1, k-1, target-num[i]);
                    if(!tempRet.empty()){
                        for(int j = 0; j <= tempRet.size()-1; j++){
                            tempRet[j].insert(tempRet[j].begin(), num[i]);
                            ret.push_back(tempRet[j]);
                        }
                    }
                }
            }
        }
        return ret;
    }
};
```

-------
<a name="Anchor17" id="Anchor17"></a>
-**[LeetCode:4Sum](http://oj.leetcode.com/problems/4sum/)**([Back to Index](#AnchorIndex))  

选定区间的两个端点，对区间应用Two Sum算法，时间复杂度O(n^3)。

class Solution {
public:
    vector<vector<int> > fourSum(vector<int> &num, int target) {
        set<vector<int> > tempRet;
        int n = num.size();
        sort(num.begin(), num.end());
        for(int s = 0; s <= n-1; s++){
            for(int e = s+3; e <= n-1; e++){
                int a = s+1, b = e-1;
                while(a < b){
                    int sum = num[s] + num[e] + num[a] + num[b];
                    if(sum == target){
                        vector<int> temp;
                        temp.push_back(num[s]);
                        temp.push_back(num[a]);
                        temp.push_back(num[b]);
                        temp.push_back(num[e]);
                        tempRet.insert(temp);
                        a++;
                        b--;
                    }else if(sum < target){
                        a++;
                    }else{
                        b--;
                    }
                }
            }
        }
        vector<vector<int> > ret(tempRet.begin(), tempRet.end());
        return ret;
    }
};
```

-------
<a name="Anchor18" id="Anchor18"></a>
-**[LeetCode:Combination Sum](http://oj.leetcode.com/problems/combination-sum/)**([Back to Index](#AnchorIndex))  

此题要求给定目标和的组合数，对于每个数有不选，选1，选2个...选n个共n+1个选择，注意剪枝。使用Set去重，暂时没考虑在求解过程中自动去重（对candidates排序，看当前数字和前一个数字是否相同进行去重可能可行）。
```cpp
class Solution {
public:
    set<vector<int> > resultSet;
    vector<vector<int> > combinationSum(vector<int> &candidates, int target) {
        vector<vector<int> > result;
        vector<int> temp;
        if(candidates.size() == 0) return result;
        sort(candidates.begin(),candidates.end());
        compute(temp, candidates, 0, 0, target);
        for(set<vector<int> >::iterator itr = resultSet.begin(); itr != resultSet.end(); itr++){
            result.push_back(*itr);
        }
        return result;
    }
    
    void compute(vector<int> temp, vector<int>& candidates, int index, int currentVal, int target){
        if(index == candidates.size()) return;
        
        compute(temp, candidates, index+1, currentVal, target);
        
        for(int i = 1; currentVal + i * candidates[index] <= target; i++){
            temp.push_back(candidates[index]);
            if(currentVal + i * candidates[index] == target){
                resultSet.insert(temp);
            }else{
                compute(temp, candidates, index+1, currentVal+i*candidates[index], target);
            }
        }
        
        return;
    }
};
```

-------
<a name="Anchor19" id="Anchor19"></a>
-**[LeetCode:Combination Sum II](http://oj.leetcode.com/problems/combination-sum-ii/)**([Back to Index](#AnchorIndex))  

此题与Ex5区别在于每个数只能用一次，该问题更确切的说是一个01背包问题，使用Set去重，应用以下剪枝方法需要对待选数字升序排序。

01的背包方法剪枝:下述方法的前提是物品价值W<sub>i</sub>是升序的（降序也可以使用，较为麻烦）。X是解向量，t=Σ<sub>(1...k-1)</sub>W<sub>i</sub>*X<sub>i</sub>，即为k-1个已选的物品的总价值，r=Σ<sub>(k...n)</sub>W<sub>i</sub>，即为剩余物品的总价值在t+k != M的前提下，X={X<sub>1</sub>,X<sub>2</sub>...X<sub>(k-1)</sub>,X<sub>k</sub>,0...0}已经可以判定不是有效解，只能看第k+1的物品的情况，考虑第k个物品的两种情况

    **选择k**：若t+W<sub>k</sub>+W<sub>(k+1)</sub><=M，则说明选入作为剩余物品中价值最小的物品W<sub>(k+1)</sub>使得X的解可能存在（反过来说如果t+W<sub>k</sub>+W<sub>(k+1)</sub>>M则说明t+W<sub>k</sub>+Σ<sub>(k+1..n)</sub>W<sub>i</sub>*X<sub>i</sub>>M，则X无解），令Xk =1，递归左儿子；否则剪枝。  
    
    **不选择k**：若t+r-k>=M&&t+(k+1)<=M，一方面判断剩下的物品还足够填满M，同时同选择k情形判断第k+1个物品是否使得X的解可能存在。

```cpp
class Solution {
public:
    
    set<vector<int> > resultSet;
    vector<vector<int> > combinationSum2(vector<int> &num, int target) {
        vector<vector<int> > result;
        if(num.size() == 0) return result;
        vector<int> temp;
        temp.clear();
        sort(num.begin(), num.end());
        int rest = 0;
        for(int i = 0; i<=num.size()-1; i++){
            rest += num[i];
        }
        compute(temp, num, 0, rest, target);
        for(set<vector<int> >::iterator itr = resultSet.begin(); itr != resultSet.end(); itr++){
            result.push_back(*itr);
        }
        
        return result;
    }
    
    
    //优化剪枝的递归函数 92ms
    void compute(vector<int>& temp, vector<int> num, int currentVal, int restVal, int target){
        if(num.size() == 0) return;
        
        if(currentVal + num[0] == target){
            temp.push_back(num[0]);
            resultSet.insert(temp);
            temp.pop_back();
            return;
        }
        
        if(num.size() == 1) return;
        
        if(currentVal + num[0] + num[1] <= target){
            temp.push_back(num[0]);
            compute(temp, vector<int>(num.begin()+1, num.end()), currentVal+num[0], restVal-num[0], target);
            temp.pop_back();
        }
        
        if(currentVal + restVal - num[0] >= target && currentVal + num[1] <= target){
            compute(temp, vector<int>(num.begin()+1, num.end()), currentVal, restVal-num[0], target);
        }
        
        return;
    }
    
    
    //未优化剪枝的递归函数 1160ms
    /*
    void compute(vector<int>& temp, vector<int> num, int currentVal, int restVal, int target){
        if(num.size() == 0) return;
        
        if(currentVal + num[0] == target){
            temp.push_back(num[0]);
            resultSet.insert(temp);
            temp.pop_back();
            return;
        }
        
        if(currentVal + num[0] <= target){
            temp.push_back(num[0]);
            compute(temp, vector<int>(num.begin()+1, num.end()), currentVal+num[0], restVal-num[0], target);
            temp.pop_back();
        }
        
        compute(temp, vector<int>(num.begin()+1, num.end()), currentVal, restVal-num[0], target);
        
        return;
    }*/
};
```