## 二叉树

度小于$2$的,且分别最多有一个左子树和右子树的树称为二叉树

> 二叉树是顺序树,其子节点具有严格的左右关系

### 二叉树性质
1. 若规定根节点的层数为$1$，则一棵非空二叉树的第$i$层上最多有$2^{i-1}$个结点.
2. 若规定根节点的层数为$1$，则深度为$h$的二叉树的最大结点数是$2^h + 1$
3. 对任何一棵二叉树, 如果度为$0$的叶结点个数为$n_0$, 度为$2$的分支结点个数为$n_2$,则有$n_0 ＝ n_2 + 1$
4. 若规定根节点的层数为1，具有n个结点的满二叉树的深度，$h=log_2(n+1)$
5. 对于具有n个结点的完全二叉树，如果按照从上至下从左至右的数组顺序对所有节点从0开始编号，则对于序号为i的结点有：
	1. 若$i>0$，$i$位置节点的双亲下标为：$\frac{i-1}{2}$；$i=0$，$i$为根节点编号，无双亲节点
	2. 若$2i+1<n$，左孩子序号：$2i+1，2i + 1 \geq n$否则无左孩子
	3. 若$2i+2<n$，右孩子序号：$2i+2$，$2i + 2 \geq n$否则无右孩子
### 特殊的二叉树
满二叉树:一个二叉树，如果每一个层的结点数都达到最大值，则这个二叉树就是满二叉树。

>满2叉树的节点数为: $2^k-1$


完全二叉树:最后一层节点从左到右排列,中间没有空缺.其他层符合满二叉树,则为完全二叉树.

> 满二叉树是一种特殊的完全二叉树
> 
### 二叉树的结构

1. 顺序结构存储:

就是使用数组来存储，一般使用数组只适合表示完全二叉树

因为不是完全二叉树会有空间的浪费。

而现实中使用中只有堆才会使用数组来存储。

二叉树顺序存储在物理上是一个数组，在逻辑上是一颗二叉树。

3. 链式结构储存:

二叉树有三叉链表示法和二叉链表示法

三叉链表示法存储父节点和左右儿子节点.

双链表示法有两种,一个是孩子表示法,一个是儿子兄弟表示法.
## 二叉树的遍历
- 前序遍历 :按根节点,左子树,右子树的方式递归遍历
- 中序遍历:按左子树,根节点,右子树的方式递归遍历
- 后序遍历:按左子树,右子树,根节点的方式递归遍历
- 层序遍历:从左到右遍历当前深度所有节点,然后遍历下一层.

> 层序遍历使用队列实现，其他三个使用递归实现，且代码类似

### 前、中、后序遍历
前序:
```c
void PreOrder(BTNode* Node){
	if(Node == NULL)
		return;
		
	printf("%d",Node->data);
	PreOrder(Node->left);
	PreOrder(Node->right);
}
```
中序：
```c
void InOrder(BTNode* Node){
	if(Node == NULL)
		return;
		
	InOrder(Node->left);
	printf("%d",Node->data);
	InOrder(Node->right);
}
```
后序：
```c
void PostOrder(BTNode* Node){
	if(Node == NULL)
		return;
	
	PostOrder(Node->left);
	PostOrder(Node->right);
	printf("%d",Node->data);
}
```
### 层序遍历
层序遍历使用队列实现

1. 若根节点不为空，根节点入队。
2. 队首出队，同时将队首节点的不为空的左右子节点入队。
3. 重复第二步骤，直到队列为空。

```c
void levelOrder(BTNode* Node){
	Queue Q;
	PushQueue(Q,Node);
	while(EmptyQueue(Q)){
		BTNode* tmp = PopQueue(Q);
		if(tmp){
			printf("%d", tmp->data);
			PushQueue(Q,tmp->left);
			PushQueue(Q,tmp->right)
		}
	}
}
```

## 二叉树的常见操作
### 从前序序列创建二叉树
创建新节点
```c
BTNode* BuyBTNode(BTDataType data) {
    BTNode* Node = malloc(sizeof(BTNode));
    assert(Node);
    Node->_data = data;
    Node->_left = NULL;
    Node->_right = NULL;
    return Node;
}
```

递归的创建树
```c
BTNode* _BinaryTreeCreate(BTDataType* PreOrderStr, int n, int* pi) {
    if (*pi >= n) {
        return NULL;
    }
    if (PreOrderStr[*pi] == '#') {
        (*pi)++;
        return NULL;
    }
    BTNode* root = BuyBTNode(PreOrderStr[(*pi)]);
    (*pi)++;
    root->_left = _BinaryTreeCreate(PreOrderStr, n, pi);
    root->_right = _BinaryTreeCreate(PreOrderStr, n, pi);
    return root;
}
```
主要用来创建pi，可以在调用时不手动创建
```c
BTNode* BinaryTreeCreate(BTDataType* PreOrderStr, int n) {
    int pi = 0;
    return _BinaryTreeCreate(PreOrderStr, n, &pi);
}
```
### 二叉树的节点数

二叉树的节点数 = 根节点 + 左树的节点数 + 右树的节点数
```c
int BinaryTreeSize(BTNode* root) {
    if (root == NULL) {
        return 0;
    }
    return BinaryTreeSize(root->_left) + BinaryTreeSize(root->_right) + 1;
}
```
### 二叉树的叶子数

二叉树的叶子数 = 左树的叶子数 + 右树的叶子数

叶子的特征 ：没有子节点
```c
int BinaryTreeLeafSize(BTNode* root) {
    if (root == NULL) 
        return 0;

    if (root->_left == NULL && root->_right == NULL) 
        return 1;
    

    return 
		BinaryTreeLeafSize(root->_left) +
		BinaryTreeLeafSize(root->_right);
}
```
### 二叉树是否为完全二叉树

层序遍历二叉树，如果遇到空节点后又遇到有效节点，则这个树不是完全二叉树
```c
bool BinaryTreeComplete(BTNode* root){
    Queue* Q = QueueCreate(root);
    while(!QueueEmpty(Q)){
        BTNode* Node = QueuePop(Q);
        if(Node == NULL)
            break;
        QueuePush(Q, Node->_left);
        QueuePush(Q, Node->_right);
    }

    while(!QueueEmpty(Q)){
        BTNode* Node = QueuePop(Q);
        if(Node != NULL)
            return false;
    }

    return true;
}
```
### 二叉树查找节点
前序遍历，遇到目标返回目标地址，否则返回空。返回空时继续查找，直到遍历结束。
```c
BTNode* BinaryTreeFind(BTNode* root, BTDataType x) {
    if (root == NULL)
        return NULL;

    if (root->_data == x)
        return root;

    BTNode* left = BinaryTreeFind(root->_left, x);
    if (left)
        return left;
    return BinaryTreeFind(root->_right, x);
}

```
### 二叉树的深度
二叉树的深度 = 左树的深度和右树的深度最大的那个+1
```c
int BinaryTreeDepth(BTNode*root){
    if(root == NULL)
        return 0;
    int left = BinaryTreeDepth(root->_left);
    int right = BinaryTreeDepth(root->_right);

    return left > right ? left + 1 : right + 1;
}
```
### 二叉树第K层的节点数

二叉树k层的节点数 = 左树的k-1层的节点数 + 右树的k-1层的节点数
```c
int BinaryTreeLevelKSize(BTNode* root, int k) {
    if (root == NULL)
        return 0;
    if (k == 0)
        return 1;

    return BinaryTreeLevelKSize( root->_left, k-1) + BinaryTreeLevelKSize(root->_right, k - 1);
}
```
### 二叉树的销毁
```c
void BinaryTreeDestory(BTNode** root) {
    _BinaryTreeDestory(*root);
    *root = NULL;
}
```