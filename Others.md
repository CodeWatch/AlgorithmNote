##Others
<a name="AnchorIndex" id="AnchorIndex"></a>
Index:  
-[Find the number(not exceeding M) only formed by 0 and 1](#Anchor1)  
-[](#Anchor2)  
-[](#Anchor3)  
-[](#Anchor4)  
-[](#Anchor5)

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
