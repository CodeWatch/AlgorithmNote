##Array & String
Index:  
-[Leetcode:single number II](#Anchor1)  
-[Combination](#Anchor2)

<a name="Anchor1" id="Anchor1"></a>
-[Leetcode:single number II](http://oj.leetcode.com/problems/single-number-ii/)  
  
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

int singleNumberSpecial(int A[], int n){
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
		if(count[i]%ARY == 1){
			ret |= b;
		}
	}
	return ret;
}

vector<int> twoSingleNumber(int A[], int n){
	int twoNum = singleNumberSpecial(A, n);
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

<a name="Anchor2" id="Anchor2"></a>
