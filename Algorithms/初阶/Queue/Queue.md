## 队列
只允许在一端进行插入数据操作，在另一端进行删除数据操作的特殊线性表

队列具有先进先出FIFO(First In First Out) 的性质

队列也可以数组和链表的结构实现，使用链表的结构实现更优一些，因为如果使用数组的结构，出队列在数组头上出数据，效率会比较低。
## 队列的常见操作
- 入队列：进行插入操作的一端称为队尾
- 出队列：进行删除操作的一端称为队头
- 判断队列是否为空
- 初始化队列
- 获取队头元素
- 获取队列有效元素个数
- 销毁队列
## C模拟队列

C语言使用链表模拟实现队列

QueueList.h :
```c
#pragma once
#include<stdio.h>
#include<stdlib.h>
// 链式结构：表示队列 
typedef int QDataType;
typedef struct QListNode
{
	struct QListNode* _next;
	QDataType _data;
}QNode;

// 队列的结构 
typedef struct Queue
{
	QNode* _front;
	QNode* _rear;
}Queue;

// 初始化队列 
void QueueInit(Queue* q);
// 队尾入队列 
void QueuePush(Queue* q, QDataType data);
// 队头出队列 
void QueuePop(Queue* q);
// 获取队列头部元素 
QDataType QueueFront(Queue* q);
// 获取队列队尾元素 
QDataType QueueBack(Queue* q);
// 获取队列中有效元素个数 
int QueueSize(Queue* q);
// 检测队列是否为空，如果为空返回非零结果，如果非空返回0 
int QueueEmpty(Queue* q);
// 销毁队列 
void QueueDestroy(Queue* q);
```
QueueList.c :
```C
#include"QueueList.h"

// 初始化队列 
void QueueInit(Queue* q) {
	q->_front = NULL;
	q->_rear = NULL;
}
// 队尾入队列 
void QueuePush(Queue* q, QDataType data) {
	QNode* NewNode = malloc(sizeof(QNode));
	if (NewNode == NULL) {
		perror("Funtion QueuePush : malloc failed");
		return;
	}
	NewNode->_data = data;
	NewNode->_next = NULL;
	if (QueueEmpty(q)) {
		q->_front = q->_rear = NewNode;
	}
	else {
		q->_rear->_next = NewNode;
		q->_rear = NewNode;
	}
}
// 队头出队列 
void QueuePop(Queue* q) {
	if (QueueEmpty(q)) {
		printf("Funtion QueuePop : Pop Node Failed : Empty Queue\n");
	}
	else {
		QNode* PopNode = q->_front;
		q->_front = q->_front->_next;
		free(PopNode);
		if (q->_front == NULL)
			q->_rear = NULL;
	}
}
// 获取队列头部元素 
QDataType QueueFront(Queue* q) {
	if (QueueEmpty(q)) {
		printf("Funtion QueueFront : Get Node Failed: Empty Queue\n");
		return 0;
	}
	return q->_front->_data;
}
// 获取队列队尾元素 
QDataType QueueBack(Queue* q) {
	if (QueueEmpty(q)) {
		printf("Funtion QueueBack : Get Node Failed : Empty Queue\n");
		return 0;
	}
	return q->_rear->_data;
}
// 获取队列中有效元素个数 
int QueueSize(Queue* q) {
	QNode* cur = q->_front;
	int size = 0;
	while (cur) {
		size++;
		cur = cur->_next;
	}
	return size;
}
// 检测队列是否为空，如果为空返回非零结果，如果非空返回0 
int QueueEmpty(Queue* q) {
	return q->_rear == NULL;
}
// 销毁队列 
void QueueDestroy(Queue* q) {
	while (!QueueEmpty(q)) {
		QueuePop(q);
	}
}
```