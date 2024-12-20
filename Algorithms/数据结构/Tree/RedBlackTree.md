红黑树也是一种搜索二叉树，其给每个节点一个颜色（红色或者黑色），用来保证树的最长路径的长度和最短路径的长度相差不超过二倍，形成近似平衡。

红黑树符合：
1. 根节点始终是黑色
2. 如果一个节点是红色的，则其子节点必须是黑色的（黑色节点不要求孩子一定为红色）
3. 每个节点到其所有后代节点的路径中包含相同数量的黑色的节点。
4. 每个叶子节点都是黑色的。

> 注意，红黑树的叶子节点指的是空节点，算路径时也要包含空节点

红黑树的删改查效率不如AVL树，但是差别没有过大。但换来的是，红黑树在构建时，旋转的次数更少，因为其是近似平衡，并没有AVL树严格。这就导致了红黑树的综合效率更好，使用的也比AVL树更多。

## 插入

每次插入时，新插入的节点总为红色。
1. 父亲节点为黑色
	1. 直接插入
2. 父亲节点为红色
	1. 叔叔不存在/叔叔为黑色
		1. 直线（父亲和祖父的关系 == 自己和父亲的关系）
			1. 祖父为根单旋（方向取决去父亲存在的方向，父亲在左，则右旋）
			2. 变色（旋转前的关系：父亲变黑，祖父变红）
		2. 折线
			1. 父亲为根单旋（方向取决去自己存在的方向，自己在左，则右旋）
			3. 祖父为根反方向单旋
			4. 变色（旋转前的关系：自己变黑，祖父变红）
	2. 叔叔节点为红色
		1. 则使祖父节点变红，父亲和叔叔都变黑。
		2. 认为其祖父节点为新节点，向上继续判断叔叔节点