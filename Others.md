##Others
<a name="AnchorIndex" id="AnchorIndex"></a>
Index:  
-[Find the number(not exceeding M) only formed by 0 and 1](#Anchor1)  
-[Leetcode:Gray Code](#Anchor2)  
-[Leetcode:Pow(x, n)](#Anchor3)  
-[Leetcode:Sqrt(x)](#Anchor4)  
-[*TODO*::Leetcode:Valid Number](#Anchor5)  

-------
<a name="Anchor1" id="Anchor1"></a>
-**Find the number(not exceeding M) only formed by 0 and 1**([Back to Index](#AnchorIndex))    
首先我们要维护一个余数信息数组，里面存放的是满足表示形式为1和0的，求余N为j的最小的数。若10的i次幂求余N等于j的话，就将这个数加入余数数组。然后遍历已经存在的余数信息数组。假设10的p次幂求余N等于k，则（10的p次幂+10的i次幂）% N =（j+k）%N，所以将余数为k的数和余数为j的数加入余数为（j+k）的数组中。下面是解决这道题的算法，想要看懂该算法需要明白两个条件：（1）（x*10）% N = （x%N*10）% N  （2）在一轮N次更新后，如果余数数组没有出现更新，那么将进入死循环，不再有结果，退出  
```java
import java.util.ArrayList;

public class MyOwn {
	public void Only10(int N) {
		ArrayList<ArrayList<Integer>> b = new ArrayList<ArrayList<Integer>>();
		for (int i = 0; i < N; i++)
			b.add(new ArrayList<Integer>());
		b.get(1).add(0);
		int noUpdate = 0;

		for (int i = 1, j = 10 % N;; i++, j = (j * 10) % N) {
			boolean flag = false;
			if (b.get(j).size() == 0) {
				flag = true;
				b.get(j).add(i);
			}
			for (int k = 1; k < N; k++) {
				if (b.get(k).size() > 0
						&& i > b.get(k).get(b.get(k).size() - 1)
						&& b.get((j + k) % N).size() == 0) {
					flag = true;
					b.set((j + k) % N, b.get(k));
					b.get((j + k) % N).add(i);
				}
			}

			if (flag)
				noUpdate = 0;
			else
				noUpdate++;

			if (noUpdate == N || b.get(0).size() > 0)
				break;
		}

		if (noUpdate == N)
			System.out.println("Not Exit");
		else {
			int max = b.get(0).get(b.get(0).size() - 1);
			long result = 0;
			for (int i = max; i >= 0; i--) {
				if (b.get(0).contains(i))
					result = result * 10 + 1;
				else
					result = result * 10;
			}
			// if (b.get(0).get(0) == 0)
			// result = result * 10;
			System.out.println("result: " + result);
			System.out.println("N: " + N + " M: " + result / N);
		}
	}

	public static void main(String[] args) {
		MyOwn m = new MyOwn();
		m.Only10(24);
	}
}
``` 

-------
<a name="Anchor2" id="Anchor2"></a>
-**[Leetcode:Gray Code](http://oj.leetcode.com/problems/gray-code/)**([Back to Index](#AnchorIndex))   
一个普通解法:
```cpp
class Solution {
public:
    vector<int> grayCode(int n) {
        vector<int> result;
        result.push_back(0);
        if(n <= 0) return result;
        int multiplier = 1;
        for(int i = 0;i<n;i++){
            for(int j = (1<<i)-1;j>=0;j--){
                result.push_back(result.at(j)+multiplier);
            }
            multiplier <<= 1;
        }
        return result;
    }
};
```
技巧性较强的解法，二进制转格雷码：gray = (binary) xor (binary >> 1)  
```cpp
class Solution {
public:

    vector<int> grayCode(int n) {
        vector<int> vec;
        int size = 1 << n;
        for (int i = 0; i < size; i++) {
            vec.push_back(i^(i >> 1));
        }
        return vec;
    }
};
```

-------
<a name="Anchor3" id="Anchor3"></a>
-**[Leetcode:Pow(x, n)](http://oj.leetcode.com/problems/powx-n/)**([Back to Index](#AnchorIndex))   

求解x的n次方，可以考虑如下分类法
    
    * 如果n = 0，则返回1
    * 如果n > 0，则先求解x的n/2次方结果，再根据奇偶性决定是否额外乘以x
    * 如果n < 0，则对n取绝对值，即求解x的-(n/2)次方的结果，在根据奇偶性决定是否额外乘以x，注意此时返回值需要取倒数 

```cpp
class Solution {
public:
    double pow(double x, int n) {
        // Start typing your C/C++ solution below
        // DO NOT write int main() function
        if(n == 0) return 1;
        else if(n > 0)
        {
            double half = pow(x, n/2);
            if(n%2 == 0) return half*half;
            else return half*half*x;
        } 
        else
        {
            n = -n;
            double half = pow(x, n/2);
            if(n%2 == 0) return 1.0/(half*half);
            else return 1.0/(half*half*x);
        }
    }
};
```

-------
<a name="Anchor4" id="Anchor4"></a>
-**[Leetcode:Sqrt(x)](http://oj.leetcode.com/problems/sqrtx/)**([Back to Index](#AnchorIndex)) 

二分法。

```cpp
class Solution {
public:

    int sqrt(int x) {
        int left = 1, right = x / 2;
        int mid;
        int last_mid; // 记录最近一次mid
        if (x < 2) return x;
        while (left <= right) {
            mid = left + (right - left) / 2;
            if (x / mid > mid) { // 不要用x > mid * mid，会溢出
                left = mid + 1;
                last_mid = mid;
            } else if (x / mid < mid) {
                right = mid - 1;
            } else {
                return mid;
            }
        }
        return last_mid;
    }
};
```

-------
<a name="Anchor5" id="Anchor5"></a>
-**[Leetcode:Valid Number](oj.leetcode.com/problems/valid-number/)**([Back to Index](#AnchorIndex)) 
