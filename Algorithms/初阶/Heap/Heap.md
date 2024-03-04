堆在逻辑上像是一个二叉树

1. 小根堆：每个节点的左右子节点均大于自己，此时根节点是堆中最小的数。
2. 大根堆：每个节点的左右子结点均小于自己，此时根节点是堆中最大的数。

并且一定是完全二叉树。

所以在物理上，我们用数组来实现它。
1. 根节点的下标为$N$时，左子结点的下标为 $N * 2 + 1$，右子结点的下标为$N * 2 + 2$
2. 子节点的下标为$N$时，其父节点的下标为$\frac{N-1}{2}$
## 建立堆

有两种方法，向下调整建堆和向上调整建堆

一般来说，从数组建堆时使用从后向前遍历数组，对每个数进行依次使用向下调整算法。

使用向下调整算法的优点在于，能够直接从倒数第二行开始调整，比向上调整建堆少调整一列（而且这一列极有可能是数据最多的）
```c
// 堆的构建
Heap* HeapCreate(HPDataType nums[], int Size){
    Heap* hp = malloc(sizeof(Heap));
    assert(hp);

    hp->_capacity = Size ? Size : 4;
    hp->_size = 0;

    hp->_data = malloc(sizeof(HPDataType) * HeapCapacity(hp));
    assert(hp->_data);

    for(int i = 0; i < Size; i++){
        HeapPush(hp,nums[i]);
    }

    return hp;
}
```

### 向下调整算法
```c
void ADJustDown(Heap* hp, int parent){
    assert(hp);
    int child = parent * 2 + 1;
    while(child < HeapSize(hp)){
        if(child + 1 < HeapSize(hp) && hp->_data[child] > hp->_data[child+1]){
            child++;
        }
        if(hp->_data[child] < hp->_data[parent]){
            Swap(hp->_data + child, hp->_data + parent);
            parent = child;
            child = parent * 2 + 1;
        }else{
            break;
        }
    }
}
```
### 向上调整算法
```c
void ADJustUp(Heap* hp, int child){
    assert(hp);
    int parent = (child - 1 ) / 2;
    while(parent >= 0){
        if(hp->_data[child] < hp->_data[parent]){
            Swap(hp->_data + child, hp->_data + parent);
            child = parent;
            parent = (child - 1 ) / 2;
        }else{
            break;
        }
    }
}
```
## 基本操作
```c
// 堆的销毁
void HeapDestory(Heap* hp){
    assert(hp);
    free(hp->_data);
    free(hp);
}
```
### 插入数据
堆的插入一般将数据插入尾部，然后使用向上调整算法
```c
// 堆的插入
void HeapPush(Heap* hp, HPDataType x){
    assert(hp);
    
    if(HeapSize(hp) == HeapCapacity(hp)){
        hp->_capacity *= 2;
        hp->_data = realloc(hp->_data,HeapCapacity(hp));
        assert(hp->_data);
    }
    
    hp->_data[HeapSize(hp)] = x;
    hp->_size++;

    ADJustUp(hp,HeapSize(hp) - 1);
}
```
### 删除堆顶数据
一般来说，堆只能删除顶部数据。删除其他数据没有意义。

删除时，只需要将堆尾和堆顶数据交换。此时只需要对新的堆顶再进行一次向下调整算法，堆就又有序了。

> 如果直接删除，不只是所有数据都要前移1位，而且所有数据的父子关系将全部混乱，又要重新堆每个数据进行一次向下调整建堆。
```c
// 堆的删除
HPDataType HeapPop(Heap* hp){
    assert(hp);
    assert(!HeapEmpty(hp));
    Swap(hp->_data,hp->_data + HeapSize(hp) - 1);
    hp->_size--;
    ADJustDown(hp,0);
    return hp->_data[HeapSize(hp)];
}
```
### 其他常见算法
```c
// 取堆顶的数据
HPDataType HeapTop(Heap* hp){
    assert(hp);
    assert(!HeapEmpty(hp));
    return hp->_data[0];
}
// 堆的数据个数
int HeapSize(Heap* hp){
    assert(hp);
    return hp->_size;
}
//堆的大小
int HeapCapacity(Heap* hp){
    assert(hp);
    return hp->_capacity;
}
// 堆的判空
int HeapEmpty(Heap* hp){
    assert(hp);
    return hp->_size == 0;
}

```




