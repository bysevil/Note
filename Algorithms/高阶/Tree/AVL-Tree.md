AVL树又叫高度平衡搜索二叉树，其是对搜索二叉树的优化，他能保证搜索二叉树任何一个节点其左子树和右子树的高度差不超过1。

我们使用平衡因子的方式来实现AVL树

## 插入
插入新节点时，新节点的平衡因子为0。如果新节点在旧节点的左边，则旧节点平衡因子减1，在旧节点的右边，则平衡因子加1。

如果更新后，旧节点的平衡因子不为0，则继续更新旧节点的父节点，直到某个父节点的平衡因子为0或者到达根节点。

如果遍回归过程中某个节点的平衡因子大于1，则以其为根节点的树不平衡，需要进行旋转。

### 旋转

回归过程中，发现其父节点平衡因子大于1时进行旋转.此时有cur（当前节点）和parent（其父节点）。

设旋转的子树的根节点为root，根节点的左节点为left，根节点的右节点为right;

旋转分为四种情况：

1. 左单旋（ $parent = 2 \land  cur = 1$ )  
	1. $parent = oldRoot$
		1. $oldRight=>newRoot$
		2. $oldRight->left=>oldRoot->right$
		3. $oldRoot=>newleft$
	2. 更新平衡因子
		1. $oldRight = oldRoot =0$
2. 右单旋：（ $parent= -2 \land  cur = -1$ )
	1. $parent = oldRoot$
		1. $oldLeft=>newRoot$
		2. $oldLeft->right=>oldRoot->left$
		3. $oldRoot=>newRight$
	2. 更新平衡因子
		1. $oldLeft = oldRoot =0$
3. 先左后右双旋（ $parent = -2 \land  cur = 1$ )
	1. $parent->left = oldRoot$
		1. 左旋
	2. $parent = oldRoot$
		1. 右旋
	3. 更新平衡因子
		1. $parent = 0$
		3. $parent->left = -( parnet->left->right )$
		4. $parent->left = 0$
4. 先右后左双旋（ $parent= 2 \land cur = -1$)
	1. $parent->right = oldRoot$
		1. 右单旋一次
	2. $parent = oldRoot$
		1. 左单旋一次
	3. 更新平衡因子
		1. $parent = 0$
		3. $parent->right = -( parnet->right->left )$
		4. $parent->left = 0$

