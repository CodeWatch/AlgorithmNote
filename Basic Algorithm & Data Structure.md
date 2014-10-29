##Basic Algorithm & Data Structure
<a name="AnchorIndex" id="AnchorIndex"></a>
Index:  
-[Heap Sort](#Anchor1)  
-[Union Find Sets](#Anchor2)  
-[Nonrecursive Tree Traversal](#Anchor3)  
-[KMP](#Anchor4)  
-[QuickSort in Linklist](#Anchor5)  

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