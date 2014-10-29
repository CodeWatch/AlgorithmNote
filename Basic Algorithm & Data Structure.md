##Basic Algorithm & Data Structure
<a name="AnchorIndex" id="AnchorIndex"></a>
Index:  
-[Heap Sort](#Anchor1)  
-[Union Find Sets](#Anchor2)  
-[Nonrecursive Tree Traversal](#Anchor3)  
-[KMP](#Anchor4)  
-[QuickSort in Linklist](#Anchor5)  
-[Topological Sort](#Anchor6)  
-[Prim](#Anchor7)  
-[Dijkstra](#Anchor8)  
-[Floyd](#Anchor9)  
-[Tarjan](#Anchor10)  

-------
<a name="Anchor1" id="Anchor1"></a>
-**Heap Sort**([Back to Index](#AnchorIndex))   

堆排序算法主要包括建堆和堆调整两大步骤，其中建堆的时间复杂度为O(n),堆调整的总时间复杂度为O(nlogn)，因此堆排序的总时间复杂度为O(nlogn)，关于该算法的时间复杂度分析证明可查阅相关资料。

```cpp
#include <iostream>

using namespace std;

void heapSort(int A[], int len);
void buildHeap(int A[], int len);
void adjust(int A[], int i, int max);
void swap(int * a, int * b);

int main(){
	int A[7] = {1,3,4,2,7,5,6};
	heapSort(A, 7);
	for(int i = 0; i <= 6; i++){
		cout << A[i] << " ";
	}
}

void heapSort(int A[], int len){
	buildHeap(A,len);
	for(int i = len-1; i >= 0; i--){
		swap(&A[0], &A[i]);
		adjust(A, 1, i);
	}
}

void buildHeap(int A[], int len){
	for(int i = len/2; i >= 1; i--){
		adjust(A, i, len);
	}
}

void adjust(int A[], int i, int max){
	int left = 2 * i;
	int right = 2 * i + 1;
	int maxIndex = i;
	if(left <= max&&A[maxIndex-1] < A[left-1]){
		maxIndex = left;
	}
	if(right <= max&&A[maxIndex-1] < A[right-1]){
		maxIndex = right;
	}
	if(maxIndex != i){
		swap(&A[maxIndex-1], &A[i-1]);
		adjust(A, maxIndex, max);
	}
}

void swap(int * a, int * b){
	if(*a != *b){
		*a ^= *b;
 		*b ^= *a;
 		*a ^= *b;
	}
}
```

-------
<a name="Anchor2" id="Anchor2"></a>
-**Union Find Sets**([Back to Index](#AnchorIndex))   

-------
<a name="Anchor3" id="Anchor3"></a>
-**Nonrecursive Tree Traversal**([Back to Index](#AnchorIndex))   

掌握三种树的遍历的非递归版本是非常必要的，代码如下。  

先序遍历：
```cpp
void preOrderTraversalNonRecursive(TreeNode * root){
	stack<TreeNode*> stk;
	for(;;){
		while(root != NULL){
			cout << root->val << " ";//visit
			stk.push(root);
			root = root->left;
		}
		if(!stk.empty()){
			root = stk.top();
			root = root->right;
			stk.pop();
		}else{
			return;
		}
	}
}
```

中序遍历：
```cpp
void inOrderTraversalNonRecursive(TreeNode * root){
	stack<TreeNode*> stk;
	for(;;){
		while(root != NULL){
			stk.push(root);
			root = root->left;
		}
		if(!stk.empty()){
			root = stk.top();
			stk.pop();
			cout << root->val << " ";//visit
			root = root->right;
		}else{
			return;
		}
	}
}
```

后序遍历：
```cpp
void postOrderTraversalNonRecursive(TreeNode * root){
	stack<TreeNode*> stk;
	TreeNode * lastVisit = NULL;
	for(;;){
		while(root != NULL){
			stk.push(root);
			root = root->left;
		}
		while(!stk.empty()){
			root = stk.top();
			if(root->right == lastVisit || root->right == NULL){
				cout << root->val << " ";//visit
				lastVisit = root;
				stk.pop();
			}else{
				root = root->right;//root can't be NULL here
				break;
			}
		}
		if(stk.empty()) return;
	}
}
```

-------
<a name="Anchor4" id="Anchor4"></a>
-**KMP**([Back to Index](#AnchorIndex))  
KMP算法，首先要算出字符串的覆盖数组，当发生在j长度失配时，只要把pattern向右移动j-overlay(j)长度就可以了。
  
```cpp
#include<iostream>
#include<string>
#include<vector>
using namespace std;

int kmp_find(const string& target, const string& pattern) {
    const int target_length = target.size();
    const int pattern_length = pattern.size();
    int * overlay_value = new int[pattern_length];
    overlay_value[0] = -1;
    int index = 0;
    for (int i = 1; i < pattern_length; ++i) {
        index = overlay_value[i - 1];
        while (index >= 0 && pattern[index + 1] != pattern[i]) {
            index = overlay_value[index];
        }
        if (pattern[index + 1] == pattern[i]) {
            overlay_value[i] = index + 1;
        } else {
            overlay_value[i] = -1;
        }
    }
    //match algorithm start
    int pattern_index = 0;
    int target_index = 0;
    while (pattern_index < pattern_length && target_index < target_length) {
        if (target[target_index] == pattern[pattern_index]) {
            ++target_index;
            ++pattern_index;
        } else if (pattern_index == 0) {
            ++target_index;
        } else {
            pattern_index = overlay_value[pattern_index - 1] + 1;
        }
    }
    if (pattern_index == pattern_length) {
        return target_index - pattern_index;
    } else {
        return -1;
    }
    delete [] overlay_value;
}

int main() {
    string source = " annbcdanacadsannannabnna";
    string pattern = " annacanna";
    cout << kmp_find(source, pattern) << endl;
    return 0;
}

```

-------
<a name="Anchor5" id="Anchor5"></a>
-**QuickSort in Linklist**([Back to Index](#AnchorIndex))  

主要思路是在做partition的时候将链表分为三个部分，做递归之前注意将每一段的尾部置空，合并时注意判断边界条件。在做链表拆分的时候使用了虚拟节点(dummy node)作为链表头，可以大大简化程序逻辑，推荐使用。 

```cpp
#include <iostream>

using namespace std;

class ListNode{
public:

	int val;
	ListNode * next;
	ListNode(int _val):val(_val),next(NULL){}
};

ListNode * quickSort(ListNode * head){
	if(head == NULL || head->next == NULL){
		return head;
	}
	ListNode dummyFront(-1), dummyMid(-1), dummyBack(-1);
	ListNode * frontTail = NULL, * midTail = NULL, * backTail = NULL;
	int num = head->val;
	for(ListNode * itr = head; itr != NULL; itr = itr->next){
		if(itr->val == num){
			if(midTail == NULL){
				dummyMid.next = itr;
				midTail = itr;
			}else{
				midTail->next = itr;
				midTail = midTail->next;
			}
		}else if(itr->val < num){
			if(frontTail == NULL){
				dummyFront.next = itr;
				frontTail = itr;
			}else{
				frontTail->next = itr;
				frontTail = frontTail->next;
			}
		}else{
			if(backTail == NULL){
				dummyBack.next = itr;
				backTail = itr;
			}else{
				backTail->next = itr;
				backTail = backTail->next;
			}
		}
	}
	if(frontTail != NULL) frontTail->next = NULL;
	if(midTail != NULL) midTail->next = NULL;
	if(backTail != NULL) backTail->next = NULL;
	ListNode * frontRet = quickSort(dummyFront.next);
	ListNode * backRet = quickSort(dummyBack.next);
	ListNode * ret = frontRet == NULL ?dummyMid.next:frontRet;
	if(frontRet != NULL){
		ListNode * itr = NULL;
		for(itr = frontRet; itr->next != NULL; itr = itr->next){}
		itr->next = dummyMid.next;
	}
	midTail->next = backRet;
	return ret;
}
```

-------
<a name="Anchor6" id="Anchor6"></a>
-**Topological Sort**([Back to Index](#AnchorIndex))  

拓扑排序是对有向无环图的顶点的一种排序，它使得如果存在一条从顶点A到顶点B的路径，那么在排序中B出现在A的后面。  

首先收集所有入度为0的顶点。将入度为0的顶点加入结果集。之后移除这个顶点连接的所有边，加入入度为零的顶点，如此循环。若加入结果集的节点个数少于总个数，说明有环，无法导出拓扑序列。

另外，拓扑排序算法是非常好的判断有向图中是否有环存在的算法，对于连通图和非连通图均适用。
```cpp

 #include <iostream>
 #include <stdio.h>
 #include <string.h>
 #include <stack>
 using namespace std;
 //toposort的三种情况
 const int N=27;
 int n,m;
 int graph[N][N],indegree[N],list[N];
 
 int toposort(int n)
 {
     int in[N];
     memcpy(in,indegree,sizeof(indegree)); //复制入度数组，以免对主函数中的indegree有影响
     stack<int> s;
     int i;
     for(i=0;i<n;i++)
         if(!in[i])
             s.push(i);//所有入度为0的点入栈，如果这些点多于1的话，序列不确定
     int flag=0;
     int t,j=0;
     while(!s.empty())
     {
         if(s.size()>1)
             flag=1;    //序列不确定
         t=s.top();
         s.pop();
         list[j++]=t;   //记录出栈的数字
         for(i=0;i<n;i++)
             if(graph[t][i])
                 if(--in[i]==0)
                     s.push(i);//入度为0的点入栈
     }
     if(j!=n)//不能拓扑排序，即有环
         return 1;
     else if(flag==1)//有多种排序方式，不能唯一确定
         return 2;
     return 0;//序列能够被唯一确定
 }
 
 int main()
 {
     int determined,inconsistency;
     int i,j,res;
     char a,b;
     while(scanf("%d%d",&n,&m) && n!=0 && m!=0)
     {
         getchar();
         determined=0;
         inconsistency=0;
         memset(graph,0,sizeof(graph));
         memset(indegree,0,sizeof(indegree));
         for(i=1;i<=m;i++)
         {
             scanf("%c<%c",&a,&b);
             getchar();
             if(!determined && !inconsistency)
             {
                 if(graph[b-'A'][a-'A']==1)//存在反向边，则发现矛盾
                 {
                     inconsistency=1;
                     printf("Inconsistency found after %d relations.\n",i);
                     continue;
                 }
                 if(graph[a-'A'][b-'A']==0)
                 {
                     graph[a-'A'][b-'A']=1;
                     indegree[b-'A']++;
                 }
                 res=toposort(n);//toposort
                 if(res==0)//序列能够被唯一确定
                 {
                     printf("Sorted sequence determined after %d relations: ",i);
                     for(j=0;j<n;j++)
                         printf("%c",list[j]+'A');
                     printf(".\n");
                     determined=1;
                 }
                 else if(res==1)//不能拓扑排序，即有环，发现矛盾
                 {
                     printf("Inconsistency found after %d relations.\n",i);
                     inconsistency=1;
                 }
             }
 
         }
         if(!determined && !inconsistency)//既没有唯一确定，也没发现矛盾（有环），即不能确定
             printf("Sorted sequence cannot be determined.\n");
     }
     return 0;
 }
```

-------
<a name="Anchor7" id="Anchor7"></a>
-**Prim**([Back to Index](#AnchorIndex))  

标准的prim算法，即求一个图的最小生成树。用邻接矩阵来表示图，用一个数组来表示最小生成树。  

```java

public class Main {
    int num;
    int[][] dis;

    public Main(int num, int[][] dis) {
        this.num = num;
        this.dis = dis;
    }

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        while (n-- != 0) {
            int num = in.nextInt();
            int[][] dis = new int[num + 1][num + 1];
            for (int i = 1; i <= num; i++)
                for (int j = 1; j <= num; j++)
                    dis[i][j] = in.nextInt();

            Main m = new Main(num, dis);
            int result = m.prim();
            System.out.println(result);
        }
    }

    public int prim() {
        boolean[] in = new boolean[num + 1];
        in[1] = true;
        int max = 0;
        while (!allin(in)) {
            int min = Integer.MAX_VALUE;
            int target = 0;
            for (int i = 1; i <= num; i++)
                if (in[i])
                    for (int j = 1; j <= num; j++)
                        if (i != j && !in[j] && dis[i][j] < min) {
                            min = dis[i][j];
                            target = j;
                        }
            in[target] = true;
            if (min > max)
                max = min;
        }

        return max;
    }

    public boolean allin(boolean[] in) {
        for (boolean a : in)
            if (!a)
                return false;

        return true;
    }
}
```

-------
<a name="Anchor8" id="Anchor8"></a>
-**Dijkstra**([Back to Index](#AnchorIndex))  

标准的Dijkstra算法，即求某个定点到其他所有顶点的单源最短路径。用邻接矩阵来表示图，每次加入一个点并更新最小距离  
```java
public class Dijkstra {
    static int M = 10000;

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        int[][] weight1 = {// 邻接矩阵
        { 0, 3, 2000, 7, M }, { 3, 0, 4, 2, M }, { M, 4, 0, 5, 4 },
                { 7, 2, 5, 0, 6 }, { M, M, 4, 6, 0 } };

        int[][] weight2 = { { 0, 10, M, 30, 100 }, { M, 0, 50, M, M },
                { M, M, 0, M, 10 }, { M, M, 20, 0, 60 }, { M, M, M, M, 0 } };
        int start = 0;
        int[] shortPath = Dijsktra(weight1, start);

        for (int i = 0; i < shortPath.length; i++)
            System.out.println("从" + start + "出发到" + i + "的最短距离为："
                    + shortPath[i]);
    }

    public static int[] Dijsktra(int[][] weight, int start) {
        // 接受一个有向图的权重矩阵，和一个起点编号start（从0编号，顶点存在数组中）
        // 返回一个int[] 数组，表示从start到它的最短路径长度
        int n = weight.length; // 顶点个数
        int[] shortPath = new int[n]; // 存放从start到其他各点的最短路径
        String[] path = new String[n]; // 存放从start到其他各点的最短路径的字符串表示
        for (int i = 0; i < n; i++)
            path[i] = new String(start + "-->" + i);
        int[] visited = new int[n]; // 标记当前该顶点的最短路径是否已经求出,1表示已求出

        // 初始化，第一个顶点求出
        shortPath[start] = 0;
        visited[start] = 1;

        for (int count = 1; count <= n - 1; count++) // 要加入n-1个顶点
        {
            int k = -1; // 选出一个距离初始顶点start最近的未标记顶点
            int dmin = Integer.MAX_VALUE;
            for (int i = 0; i < n; i++) {
                if (visited[i] == 0 && weight[start][i] < dmin) {
                    dmin = weight[start][i];
                    k = i;
                }
            }
            // System.out.println("k="+k);
            // 将新选出的顶点标记为已求出最短路径，且到start的最短路径就是dmin
            shortPath[k] = dmin;
            visited[k] = 1;
            // 以k为中间点，修正从start到未访问各点的距离
            for (int i = 0; i < n; i++) {
                if (visited[i] == 0
                        && weight[start][k] + weight[k][i] < weight[start][i]) {
                    weight[start][i] = weight[start][k] + weight[k][i];
                    path[i] = path[k] + "-->" + i;
                }
            }
        }
        for (int i = 0; i < n; i++)
            System.out.println("从" + start + "出发到" + i + "的最短路径为：" + path[i]);
        System.out.println("=====================================");

        return shortPath;
    }
}
```
  
-------
<a name="Anchor9" id="Anchor9"></a>
-**Floyd**([Back to Index](#AnchorIndex))  
  
标准的Floyd算法，即求每一个顶点到其他所有定点的最短长度。   

```java
public class Floyd {

    public double[][] matrix;

    public Floyd(double[][] mat) {
        matrix = mat;
    }

    public double[][] doFloyd() {
        int size = InitMatrix.size;
        for (int k = 1; k <= size; k++)
            for (int i = 1; i <= size; i++)
                for (int j = 1; j <= size; j++)
                    if (matrix[i][j] > matrix[i][k] + matrix[k][j])
                        matrix[i][j] = matrix[i][k] + matrix[k][j];
        return matrix;
    }
}
```

-------
<a name="Anchor10" id="Anchor10"></a>
-**Tarjan**([Back to Index](#AnchorIndex))  
Tarjan算法用于求一个有向连通图的割点，割点的定义是，如果除去此节点和与其相关的边，有向图不再连通。  

Tarjan算法是基于对图深度优先搜索的算法，每个强连通分量为搜索树中的一棵子树。搜索时，把当前搜索树中未处理的节点加入一个堆栈，回溯时可以判断栈顶到栈中的节点是否为一个强连通分量。

定义DFN(u)为节点u搜索的次序编号(时间戳)，Low(u)为u或u的子树能够追溯到的最早的栈中节点的次序号。
    
    * 对于树枝边(u,v)，有low[u]=min(low[u],low[v]).
    * 对于后向边(u,v)(指向在当前栈中节点的边)，有low[u]=min(low[u],dfn[v]).

当DFN(u)=Low(u)时，以u为根的搜索子树上所有节点是一个强连通分量。

```cpp
#include<iostream>
using namespace std;
int times=1,low[1000],dfn[1000];

int stack[1000],top=0;

bool instack[1000]={false};

struct LIST
{
    int v;
    LIST *next;
};

LIST *head[1000]={NULL};

int min(int a,int b)
{
    if(a<b)
        return a;
    return b;
}


void tarjan(int v)      /*tarjan求强连通分支*/
{
    dfn[v]=low[v]=times++;      /*标记点v的DFS遍历序号*/
    stack[top++]=v;             /*将点v入栈*/
    instack[v]=true;            /*标记点v已经在栈中*/
    for(LIST *p=head[v];p!=NULL;p=p->next)      /*遍历V能直接到达的点*/
        if(!dfn[p->v])  /*如果v的邻接点没有入过栈*/
        {
            tarjan(p->v);
            low[v]=min(low[v],low[p->v]);   /*如果v能直接到达的这个点没在栈中,v的最早祖先为他们中的较小值*/
        }
        else if(instack[p->v])  /*如果在栈中*/
            low[v]=min(low[v],dfn[p->v]);   /*如果在栈中，则v的最早祖先是他的序号和那个点的序号较小的*/

    if(dfn[v]==low[v])      /*如果dfn[v]和low[v]相等，则说明v点是其所属强连通分支DFS遍历起点，这个强连通分支说有点都在v点之上*/
    {
        cout<<"{ ";
        do
        {
            v=stack[--top];
            instack[v]=false;
            cout<<v<<' ';
        }while(dfn[v]!=low[v]);
        cout<<"}"<<endl;        
    }
}


int main()
{
    int i,j,n,m;
    cin>>n;

    memset(dfn,0,sizeof(char)*4000);
    for(i=1;i<=n;i++)
    {   
        cout<<i<<"的邻接点数量:";
        cin>>m;
        cout<<"输入每个邻接点编号";
        LIST *rear=head[i];
        for(j=0;j<m;j++)        /*创建邻接表*/
        {
            if(!j)
            {
                rear=new LIST;
                head[i]=rear;
            }
            else
            {
                rear->next=new LIST;
                rear=rear->next;
            }
            rear->next=NULL;
            cin>>rear->v;
        }
    }
    
    for(i=1;i<=n;i++)
        if(!dfn[i])     /*如果i没有入过栈*/
            tarjan(i);

    system("pause");
    return 0;
}
```