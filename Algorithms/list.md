链表是顺序表的一种,将每个元素存入一个节点，标识在每个节点存入下一个节点的指针。通过指针将所有元素连接起来。

链表分为以下几种：
- 带头和不带头
- 单向和双向
- 循环和不循环

其中最常见的有两种：
1. 带头双向不循环链表
2. 不带头单项循环链表

一般将这两种简称为单向链表和双向链表。
## 单向链表

单向链表只能从当前节点向下个节点前进，无法直接寻找前一个节点。

常见的单向链表结构为
```C
strcut forward_list{
	ListDataType data;
	struct forward_list* next; 
}forward_list;
```

data成员变量存储数据，next成员变量存储前往下一个链表节点的指针。

## 双向链表

双向链表指可以通过当前节点搜索上一个节点和下一个节点的链表。

常见的双向链表结构为
```C
strcut list{
	ListDataType data;
	struct list* next; 
	struct list* prev; 
}list;
```

data成员变量存储数据，next成员变量存储前往下一个链表节点的指针，prev成员变量存储前往上一个节点的指针。
## 链表的头节点

链表的头节点有两种：带头和不带头。

带头的节点指的是，链表的头节点只做标识作用，不能不存储如何有效数据，只存储寻找其他节点的指针。

不带头节点指的是：链表的头节点也存储有效数据，同时也存储前往其他节点的指针。

## 循环链表

链表最后一个节点的next指向链表的头节点，这种链表称作循环链表；

链表的最后一个节点的next指向NULL，这种链表称为不循环链表。
## 单向链表的常见操作
### 创建新节点
1. 给新节点申请空间
2. 给新节点赋值
3. 返回新节点的地址
```c
// 动态申请一个节点
SListNode* BuySListNode(SLTDateType x) {
    SListNode* ret = malloc(sizeof(SListNode));
    if (ret == NULL) {
        printf("空间申请失败");
        exit(-1);
    }

    ret->data = x;
    ret->next = NULL;
    return ret;
}
```
### 打印
1.  因为不循环且不带头
2. 从头节点开始遍历打印
3. 遍历到NULL时终止
```
// 单链表打印
void SListPrint(SListNode* plist) {
    while (plist) {
        printf(" %d ->", plist->data);
        plist = plist->next;
    }

    printf(" NULL\n");
}
```

### 查找
1. 从头节点开始遍历
2. 找到时返回当前节点地址
3. 未找到时返回NULL
```c
// 单链表查找

SListNode* SListFind(SListNode* plist, SLTDateType x) {
    while (plist) {
        if (plist->data == x) {
            return plist;
        }
        plist = plist->next;
    }

    return NULL;
}
```

### 插入
在pos后插入:
1. 创建新节点
2. 插入pos后
```
// 单链表在pos位置之后插入x
void SListInsertAfter(SListNode* pos, SLTDateType x) {
    assert(pos);
    SListNode* NewNode= BuySListNode(x);
    NewNode->next = pos->next;
    pos->next = NewNode;
}
```
在pos之前插入：
- pos为链表头节点
	1. 创建新节点
	2. 新节点成为头节点
- pos不是头节点：
	1. 寻找pos的前一个节点
	2. 在pos的前一个节点的后面插入新节点
- pos未找到：报错并返回
```c
// 在pos的前面插入
void SLTInsert(SListNode** pphead, SListNode* pos, SLTDateType x) {
    assert(pphead);
    assert(pos);
    SListNode* plist = *pphead;
    if (plist == pos) {
        *pphead = BuySListNode(x);
        (*pphead)->next = plist;
        return;
    }
    while (plist) {
        if (plist->next == pos) {
            SListInsertAfter(plist,x);
            return;
        }
        plist = plist->next;
    }
    printf("pos未找到，插入失败\n");

}
```
头插直接复用Insert：
```c
// 单链表的头插
void SListPushFront(SListNode** pplist, SLTDateType x) {
    SLTInsert(pplist,*pplist,x);
}
```
### 尾插
因为要找尾，而且链表可能为空，所以不复用PushAfter
- 链表为空时：
	1. 链表时创建新节点并成为头节点
- 链表不为空：
	1. 先从头节点遍历到尾节点
	2. 创建新节点
	3. 插入至尾节点最后，成为新的尾节点
```c
// 单链表尾插
void SListPushBack(SListNode** pplist, SLTDateType x) {
    assert(pplist);
    SListNode* plist = *pplist;
    if (plist) {
        while (plist->next) {
            plist = plist->next;
        }
        plist->next = BuySListNode(x);
    }
    else {
        *pplist = BuySListNode(x);
    }
}
```

### 删除
删除pos的下一个节点：
1. 标记pos的下一个位置
2. 使得pos的next指向标记位置的下一个位置
3. 是否标记位置
```c
// 单链表删除pos位置之后的值
void SListEraseAfter(SListNode* pos) {
    SListNode* del = pos->next;
    assert(del);
    pos->next = del->next;
    free(del);
}
```
删除pos节点：
- 链表只有头节点
	- 删除头节点
- 链表有多个节点
	1. 寻找pos的前一个位置
	2. 使得pos前一个位置的next指向pos的下一个位置
	3. 删除pos位置
```c
// 删除pos位置
void SLTErase(SListNode** pphead, SListNode* pos) {
    assert(pphead);
    assert(pos);
    SListNode* plist = *pphead;
    assert(plist);
    if (plist == pos) {
        *pphead = plist->next;
        free(plist);
        return;
    }
    while (plist) {
        if (plist->next == pos) {
            plist->next = pos->next;
            free(pos);
            return;
        }
        plist = plist->next;
    }
    printf("未找到pos，删除失败");
}
```
头删直接复用Erase：
```c
// 单链表头删
void SListPopFront(SListNode** pplist) {
    SLTErase(pplist, *pplist);
}
```
### 尾删
- 链表只有头节点：
	1. 删除头节点
- 链表有多个节点
	1. 查找尾节点的上一个节点（这个节点是新的尾节点，因为要使得新尾节点的next变为NULL，所以不能找旧尾节点）
	3. 删除尾节点
```c
// 单链表的尾删
void SListPopBack(SListNode** pplist) {
    assert(pplist);
    SListNode* plist = *pplist;
    assert(plist);

    if (plist->next == NULL) {
        SLTErase(pplist, plist);
    }

    while (plist->next->next) {
        plist = plist->next;
    }
    
    SLTErase(&plist, plist->next);
}
```

### 销毁
链表不为空时头删，直到为空。
```c
void SLTDestroy(SListNode** pphead) {
    assert(pphead);
    while (*pphead) {
        SListPopFront(pphead);
    }
}
```
## 双向链表的常见操作
### 初始化
1. 带头，所以新建一个不存储有效数据节点作为头节点。
2. 循环，且现在只有头节点。所以使得next和prev都指向自己。
3. 返回头节点的指针。
```c
ListNode* ListCreate(){
    ListNode* NewNode = (ListNode*)malloc(sizeof(ListNode));
    if(!NewNode){
        perror("NewNode malloc fail");
        exit(EXIT_FAILURE);
    }
    NewNode->_next = NewNode->_prev = NewNode;
    return NewNode;
}
```
### 打印
1. 使用cur指针指向phead的next。
2. 当cur没有指向phead，则链表不为空
3. 不为空时打印cur的data
4. cur向下前进
5. 指向phead时停止
```c
// 双向链表打印
void ListPrint(ListNode* pHead){
    assert(pHead);
    ListNode* cur = pHead->_next;
    while(cur != pHead){
        printf(" %d ->", cur->_data);
        cur = cur->_next;
    }
    printf(" NULL\n");
}
```
### 查找
1. 遍历节点
2. 着到指定指后返回当前节点的指针
```
// 双向链表查找
ListNode* ListFind(ListNode* pHead, LTDataType x){
    ListNode* cur = pHead->_next;
    while(cur != pHead){
        if(cur->_data == x){
            return cur;
        }
        cur = cur->_next;
    }
    return NULL;
}
```

### 插入
1. 创建新节点
2. 给新节点赋值
3. 将新节点插入链表
```c
// 双向链表在pos的前面进行插入
void ListInsert(ListNode* pos, LTDataType x){
    assert(pos);
    ListNode* NewNode = (ListNode*)malloc(sizeof(ListNode));
    if(!NewNode){
        perror("Insert NewNode malloc fail");
        exit(EXIT_FAILURE);
    }
    NewNode->_data = x;
    NewNode->_next = pos;
    NewNode->_prev = pos->_prev;


    pos->_prev->_next = NewNode;
    pos->_prev = NewNode;
}
```

双向链表插入实现后尾插尾删直接复用
```c
// 双向链表尾插
void ListPushBack(ListNode* pHead, LTDataType x){
    assert(pHead);
    ListInsert(pHead,x);
}
// 双向链表头插
void ListPushFront(ListNode* pHead, LTDataType x){
    assert(pHead);
    ListInsert(pHead->_next,x);
}
```
### 删除
1. 将pos从链表中取下
2. 释放pos
```c
// 双向链表删除pos位置的节点
void ListErase(ListNode* pos){
    assert(pos);
    pos->_prev->_next = pos->_next;
    pos->_next->_prev = pos->_prev;
    free(pos);
}
```
Erase实现后，尾删和头删直接复用：
```c
// 双向链表尾删
void ListPopBack(ListNode* pHead){
    assert(pHead);
    assert(pHead->_next != pHead);
    ListErase(pHead->_prev);
}

// 双向链表头删
void ListPopFront(ListNode* pHead){
    assert(pHead);
    assert(pHead->_next != pHead);
    ListErase(pHead->_next);
}
```
### 销毁
1. 当链表为空时，pHead的next指向自己，所以当没有指向自己时，进行尾删
3. 链表成为空链表后，删除头节点。
```c
// 双向链表销毁
void ListDestory(ListNode* pHead){
    assert(pHead);
    while(pHead->_next != pHead){
        ListErase(pHead->_next);
    }
    free(pHead);
}
```