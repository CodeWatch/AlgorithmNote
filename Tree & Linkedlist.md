##Tree & Linkedlist
<a name="AnchorIndex" id="AnchorIndex"></a>
Index:  
-[*TODO*::Delete one node from BST](#Anchor1)  
-[Transfer BST to sorted double linked list](#Anchor2)  
-[LCA](#Anchor3)  
-[*TODO*::Judge two tree are the same under swap operation](#Anchor4)  
-[Leetcode:Binary Tree Zigzag Level Order Traversal](#Anchor5)   
-[Leetcode:Flatten Binary Tree to Linked List](#Anchor6)  
-[Leetcode:Construct Binary Tree from Inorder and Postorder Traversal](#Anchor7)  
-[Leetcode:Recover Binary Search Tree](#Anchor8)  

-------
<a name="Anchor1" id="Anchor1"></a>
-**[Delete one node from BST]**([Back to Index](#AnchorIndex))  


-------
<a name="Anchor2" id="Anchor2"></a>
-**[Transfer BST to sorted double linked list]**([Back to Index](#AnchorIndex))  
把二元查找树转变成排序的双向链表。输入一棵二元查找树，将该二元查找树转换成一个排序的双向链表，要求不能创建任何新的结点，只调整指针的指向。  
  
该方法采用后序遍历的思想，每次后序遍历时，传入两个指针，分别指向子链表的头尾，之后将两边的子链表和当前节点穿成一个新链表返回。  
```cpp
void transfer(TreeNode* node, TreeNode *& head, TreeNode *& tail){
    if(node==NULL){
        head=NULL;
        tail=NULL;
        return;
    }
    TreeNode* l = NULL;
    TreeNode* r = NULL;
    visit(node->left,head,l);
    visit(node->right,r,tail);
    if(l!=NULL){
        l->right=node;
        node->left=l;
    }
    else
        head=node;
    if(r!=NULL){
        r->left=node;
        node->right=r;
    }
    else
        tail=node;
}
```

-------
<a name="Anchor3" id="Anchor3"></a>
-**[LCA]**([Back to Index](#AnchorIndex))
树中两个结点的最低公共祖先：给定一棵树，同时给出树中的两个结点，求它们的最低公共祖先。 
  
使用的是后序遍历方法，返回最低公共祖先。每当遍历到一个节点时，有以下几种情况：  
    * 当前节点等于任一个给定节点。返回当前节点。
    * 若不是，若遍历完左节点返回null，说明最低公共祖先在右边
    * 若右节点返回null，说明最低公共祖先在左边
    * 若都不为null，说明当前节点为最低公共祖先

```cpp
TreeNode* getLCA(TreeNode* root, TreeNode* X, TreeNode *Y) {
    if (root == NULL) 
        return NULL;
    if (X == root || Y == root) 
        return root;
    TreeNode * left = getLCA(root->m_pLeft, X, Y);
    TreeNode * right = getLCA(root->m_pRight, X, Y);
    if(left == NULL && right == NULL){
        return NULL;
    }else if (left == NULL){ 
        return right;
    }else if (right == NULL){ 
        return left;
    }else{
        return root;
    } 
}
```

-------
<a name="Anchor4" id="Anchor4"></a>
-**[Judge two tree are the same under swap operation]**([Back to Index](#AnchorIndex))  

-------
<a name="Anchor5" id="Anchor5"></a>
-**[Leetcode:Binary Tree Zigzag Level Order Traversal](http://oj.leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)**([Back to Index](#AnchorIndex))  
无论是zigzag或是正常的层序遍历，利用两个向量都不失为一个通用的好办法，每层遍历后两个向量调换角色即可。

```cpp
void zigzagTraversal(TreeNode * root){
    vector<TreeNode*> q[2];
    int cur = 0, last = 1;//cur is to push, last is to pop
    int direction = 1;
    q[last].push_back(root);
    while(!q[last].empty()){
        while(!q[last].empty()){
            cout << q[last].front()->val << endl;
            if(direction){
                if(q[last].front()->left != NULL) q[cur].push_back(q[last].front()->left);
                if(q[last].front()->right != NULL) q[cur].push_back(q[last].front()->right);
            }else{
                if(q[last].front()->right != NULL) q[cur].push_back(q[last].front()->right);
                if(q[last].front()->left != NULL) q[cur].push_back(q[last].front()->left);
            }
        
            q[last].erase(q[last].begin());
        }
        
        reverse(q[cur].begin(),q[cur].end());
        cur = 1 - cur;
        last = 1 - last;
        direction = 1 - direction;
    }
}
```

-------
<a name="Anchor6" id="Anchor6"></a>
-**[Leetcode:Flatten Binary Tree to Linked List](http://oj.leetcode.com/problems/flatten-binary-tree-to-linked-list/)**([Back to Index](#AnchorIndex))  

采用先序遍历，使用一个指针记录上一个访问的节点，递归的对左右子树操作。操作步骤为

    * 首先将自身与上一个访问的节点相连，如果存在上一个访问节点的话
    * 将上一个访问节点置为自身，为访问左右孩子做准备
    * 保存右子的指针，因为需要将左子置为右子
    * 对左子进行递归
    * 将左子置空
    * 对右子进行递归

```cpp
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode * preVisit;
    void flatten(TreeNode *root) {
        preVisit = NULL;
        recursive(root);
        return;
    }
    
    void recursive(TreeNode * root){
        if(root == NULL) return;
        if(preVisit != NULL){
            preVisit->right = root;//1.首先将自身与上一个访问的节点相连，如果存在上一个访问节点的话
        }
        preVisit = root;//2.将上一个访问节点置为自身，为访问左右孩子做准备
        TreeNode * right = root->right;//3.保存右子的指针，因为需要将左子置为右子
        if(root->left != NULL){
            recursive(root->left);//4.对左子进行递归
            root->left = NULL;//5.将左子置空
        }
        if(right != NULL){
            recursive(right);//6.对右子进行递归
        }
        return;
    }
};
```

-------
<a name="Anchor7" id="Anchor7"></a>
-**[Leetcode:Construct Binary Tree from Inorder and Postorder Traversal](http://oj.leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)**([Back to Index](#AnchorIndex))   

类似的问题还有利用前序和中序来还原树，都是先确定根的位置并生成根节点，再寻找左右孩子的区间，递归，并将递归返回的左右孩子赋值给根节点即可。

```cpp
class Solution {
public:
    TreeNode *buildTree(vector<int> &inorder, vector<int> &postorder) {
        if(inorder.size() == 0 || postorder.size() == 0) return NULL;
        
        return build(inorder, postorder, 0, inorder.size()-1, 0, postorder.size()-1);
    }
    
    TreeNode *build(vector<int> &inorder,vector<int> &postorder, int inBegin, int inEnd, int postBegin, int postEnd){
        if(inBegin > inEnd) return NULL;
        
        TreeNode * root = new TreeNode(postorder[postEnd]);
        TreeNode * left = NULL, * right = NULL;
        int index = 0;
        
        for(int i = inBegin; i <= inEnd; i++){
            if(inorder[i] == postorder[postEnd]){
                index = i;
            }
        }
        
        int len = index - inBegin;
        
        left = build(inorder, postorder, inBegin, index-1, postBegin, postBegin+len-1);
        right = build(inorder, postorder, index+1, inEnd, postBegin+len, postEnd-1);
        
        root->left = left;
        root->right = right;
        
        return root;
    }
};
```

-------
<a name="Anchor8" id="Anchor8"></a>
-**[Leetcode:Recover Binary Search Tree](http://oj.leetcode.com/problems/recover-binary-search-tree/)**([Back to Index](#AnchorIndex))   

对于二叉排序树来说，中序遍历序列应该是单调递增的，利用这个性质，检查在本应在递增序列中的值是否出现递减情况。注意相邻两个值交换的情况。

```cpp
class Solution {
public:
    void recoverTree(TreeNode *root) {
        TreeNode * n1 = NULL, * n2 = NULL, *last = NULL;
        InOrder(root,last, n1, n2);
        if(n1 != NULL && n2 != NULL){
            int temp = n1->val;
            n1->val = n2->val;
            n2->val = temp;
        }
    }
    
    void InOrder(TreeNode *root,TreeNode *& last, TreeNode *& n1, TreeNode *& n2){
        if(root == NULL) return;
        InOrder(root->left, last, n1, n2);
        if(last != NULL && last->val > root->val){
            if(n1 == NULL){
                n1 = last;
            }
            n2 = root;//提前记录该值以满足相邻两点交换的情况
        }
        last = root;
        InOrder(root->right, last, n1, n2);
    }
};
```

