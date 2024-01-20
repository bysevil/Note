建堆：

1. 每次堆顶的数据为堆中的最大值，此时将堆顶数据替换至堆尾部
2. 假设堆长度-1
3. 对新的堆顶使用向下调整算法，使得再次成为有效堆
4. 重复上面几步，直到数据有序，此时数组为升序

> 也就是升序建大堆，降序建小堆

```c
void AdjustDown(int* nums, int parent, int numsSize){
    int child = parent * 2 + 1;
    while(child < numsSize){
        if(child+1 < numsSize && nums[child] < nums[child+1])
            child++;
        if(nums[child] > nums[parent]){
            swap(nums+child,nums+parent);
            parent = child;
            child = parent * 2 + 1;
        }
        else
            break;
    }
}

void HeapSort(int* nums, int numsSize){
    for(int i = numsSize-1; i >= 0; i--)
        AdjustDown(nums,i,numsSize);
    
    for(int i = numsSize - 1; i > 0; i--){
        swap(nums,nums+i);
        AdjustDown(nums,0,i);
    }
}
```

时间复杂度：$O(N*logN)$

空间复杂度：$O(N)$

稳定性：不稳定