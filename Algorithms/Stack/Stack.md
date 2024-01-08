## 栈
一种特殊的线性表，其只允许在固定的一端进行插入和删除元素操作。

进行数据插入和删除操作的一端称为栈顶，另一端称为栈底。

栈中的数据元素遵守后进先出LIFO（Last In First Out）的原则。

栈的实现一般可以使用数组或者链表实现，相对而言数组的结构实现更优一些。因为数组在尾上插入数据的代价比较小。

## 常见操作
- 压栈 ：栈的插入操作叫做进栈/压栈/入栈，入数据在栈顶。
- 出栈 ：栈的删除操作叫做出栈。出数据也在栈顶。
- 销毁栈
- 判断栈是否为空
- 获取栈顶元素
- 获取栈有效元素个数

## C语言模拟实现栈
使用数组模拟实现栈

StackArray.h :
```C
#pragma once
#include<stdio.h>
#include<stdlib.h>
#include<assert.h>
// 支持动态增长的栈
typedef int STDataType;
typedef struct Stack
{
	STDataType* _a;
	int _top;		// 栈顶
	int _capacity;  // 容量 
}Stack;
// 初始化栈
void StackInit(Stack* ps);
// 入栈 
void StackPush(Stack* ps, STDataType data);
// 出栈 
void StackPop(Stack* ps);
// 获取栈顶元素 
STDataType StackTop(Stack* ps);
// 获取栈中有效元素个数 
int StackSize(Stack* ps);
// 检测栈是否为空，如果为空返回非零结果，如果不为空返回0 
int StackEmpty(Stack* ps);
// 销毁栈 
void StackDestroy(Stack* ps);
```
StackArray.c :
```C
#include"ArrayStack.h"
// 初始化栈 
void StackInit(Stack* ps) {
	ps->_a = malloc(sizeof(STDataType) * 4);
	if (ps->_a == NULL) {
		perror("Init malloc fail");
		exit(EXIT_FAILURE);
	}
	ps->_capacity = 4;
	ps->_top = 0;
}
// 入栈 
void StackPush(Stack* ps, STDataType data) {
	assert(ps);
	assert(ps->_a);
	if (ps->_top == ps->_capacity) {
		ps->_capacity *= 2;
		STDataType* tmp = realloc(ps->_a, sizeof(STDataType) * ps->_capacity);
		if (tmp == NULL) {
			perror("Push realloc fail");
			exit(EXIT_FAILURE);
		}
		ps->_a = tmp;
	}
	ps->_a[ps->_top] = data;
	ps->_top++;
}
// 出栈 
void StackPop(Stack* ps) {
	assert(ps);
	assert(ps->_a);
	assert(!StackEmpty(ps));
	ps->_top--;
}
// 获取栈顶元素 
STDataType StackTop(Stack* ps) {
	assert(ps);
	assert(ps->_a);
	assert(ps->_top);
	return ps->_a[ps->_top - 1];
}
// 获取栈中有效元素个数 
int StackSize(Stack* ps) {
	assert(ps);
	assert(ps->_a);
	return ps->_top;
}
// 检测栈是否为空，如果为空返回非零结果，如果不为空返回0 
int StackEmpty(Stack* ps) {
	assert(ps);
	assert(ps->_a);
	if (ps->_top == 0) {
		return 1;
	}
	else {
		return 0;
	}
}
// 销毁栈 
void StackDestroy(Stack* ps) {
	assert(ps);
	assert(ps->_a);
	free(ps->_a);
	ps->_top = 0;
	ps->_capacity = 0;
}
```