##Probability
<a name="AnchorIndex" id="AnchorIndex"></a>
Index:  
-[Select m elems from n equiprobably](#Anchor1)  
-[Select k elems from a stream equiprobably](#Anchor2)  
-[Shuffle](#Anchor3)  
-[Rand5 to Rand3](#Anchor4)  
-[Rand5 to Rand7](#Anchor5)  
-[Rand2 to RandN](#Anchor6)  

-------
<a name="Anchor1" id="Anchor1"></a>
-**[Select m elems from n equiprobably]**([Back to Index](#AnchorIndex))    

已知元素个数n，按顺序等概率的选出m个元素(m < n)，不允许重复选取同一元素。注意到“按顺序”，可采用如下算法：    
```cpp
void GenRandom(int n, int m){
     for(int i = 0; i <= n-1; i++){
          if(rand()%(n-i)<m){
               cout << i;
               m--;
          }
     }
}
```
概率分析如下：  
0号元素被选取的概率：m/n                                                                                                                         
1号元素被选取的概率，分别看0被选中或不被选中的情况：(n-m)/n * m/(n-1) + m/n * (m-1)/(n-1) = m/n  
2号元素被选取的概率，分别看0，1是否被选中的情况：(m-2)/(n-2) * m/n * m/n + (m-1)/(n-2) * m/n * (n-m)*2/m + m/(n-2) * (n-m)/n * (n-m)/n = m/n  
……  
从0~n-1依次计算每个i被输出的概率，发现都是m/n，因此是等概率的随机选取。  

-------
<a name="Anchor2" id="Anchor2"></a>
-**[Select k elems from a stream equiprobably]**([Back to Index](#AnchorIndex))    

不知道元素个数n，随机等概率选出k个元素，不允许重复选取同一元素。这里为示意便利使用了大小为n的数组，但事实上该算法并不依赖于n。    
```cpp
void pickKElemsFromStream(int A[],int n,int k){
     int pick[k];
     for(int i = 0; i <= k-1; i++){
          pick[i] = i+1;     
     }
     int i = k;
     for(; i <= n-1; i++){
          int r = rand(1,i+1);
          if(r >= 1 && r <= k){
               pick[r-1] = i+1;
          }
     }
}
```
1号结点被选取的概率，遍历到1号结点时被选中，且之后不会被覆盖：k/(k+1) * (k+1)/(k+2) * … * (N-1)/N = K/N  
2号结点被选取的概率，遍历到2号结点时被选中，且之后不会被覆盖：k/(k+1) * (k+1)/(k+2) * … * (N-1)/N = K/N  
k+1号结点被选取的概率，遍历到k+1号结点时被选中，且之后不会被覆盖：k/(k+1) * (k+1)/(k+2) * … * (N-1)/N = K/N  
最后一个结点被选取的概率，遍历到N号结点时被选中：K/N  
从1~N依次计算每个i被最终选取的概率，发现都是K/N。  

-------
<a name="Anchor3" id="Anchor3"></a>
-**[Shuffle]**([Back to Index](#AnchorIndex))    
目标是等概率随机排列数组（洗牌算法）。假设有一个数组，包含n个元素。现在要重新排列这些元素，要求每个元素被放到任何一个位置的概率都相等（即1/n），并且直接在数组上重排（in place），不要生成新的数组。用O(n) 时间、O(1)辅助空间。  

先想想如果可以开辟另外一块长度为n的辅助空间时该怎么处理，显然只要对n个元素做n次（不放回的）随机抽取就可以了。先从n个元素中任选一个，放入新空间的第一个位置，然后再从剩下的n-1个元素中任选一个，放入第二个位置，依此类推。  

按照同样的方法，但这次不开辟新的存储空间。第一次被选中的元素就要放入这个数组的第一个位置，但这个位置原来已经有别的（也可能就是这个）元素了，这时候只要把原来的元素跟被选中的元素互换一下就可以了。很容易就避免了辅助空间

-------
<a name="Anchor4" id="Anchor4"></a>
-**[Rand5 to Rand3]**([Back to Index](#AnchorIndex))  
利用等概率函数Rand5产生等概率函数Rand3。其中,Rand5是能够等概率产生0,1,2,3,4的随机函数，Rand3是能够等概率产生0,1,2的随机函数。
```cpp
int Rand3(){
     int x;
     do{
          x = Rand5();
     }while(x >= 3);
     return x;//0,2,1
}
```
只要保证返回的3个数是等概率的即可，每个数的概率都为1/5

<a name="Anchor5" id="Anchor5"></a>
-**[Rand5 to Rand7]**([Back to Index](#AnchorIndex))  
利用等概率函数Rand5产生等概率函数Rand7。其中,Rand5是能够等概率产生0,1,2,3,4的随机函数，Rand3是能够等概率产生0,1,2的随机函数。
```cpp
int Rand7(){
     int x;
     do{
          x = Rand()5*5+Rand5();
     }while(x >= 7);
     return x;
}
```
先生成一个比目标大的概率函数，然后在采用之前的方法即可。在生成较大的目标概率函数时，可以使用RandB()*B+RandB()的形式，可保证不重不漏，且每个数产生的概率相同。  

<a name="Anchor6" id="Anchor6"></a>
-**[Rand2 to RandN]**([Back to Index](#AnchorIndex))  
利用等概率函数Rand2产生等概率函数RandN。其中,Rand5是能够等概率产生0,1的随机函数，RandN是能够等概率产生0...N-1的随机函数。算法中注意对大于小于号的控制，与前两题不同，需要先确定“调用多少次rand2”。
```cpp
int randN(int n){
    assert(n >= 0);
    int factor = 1, factorUpperLimit = 1;
    int ret = 0;
    while(factorUpperLimit < n){
        factorUpperLimit = factorUpperLimit << 1;
    }
    do{
        factor = 1;
        ret = 0;
        while(factor < factorUpperLimit){
            int r = rand2(); // generate 0 or 1
            ret += factor * r;
            factor = factor << 1;
        }
    }while(ret > n-1);
    return ret;
}
```

更一般化的：于已知的RandX()函数，可以使用模拟X进制生成一个比目标大的概率函数,如   
     X^3RandX()+X^2RandX()+XRandX()+RandX()   
可以保证不重不漏,因而能等概率的生成数字  


