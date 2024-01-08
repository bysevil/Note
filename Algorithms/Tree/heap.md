## TOP-K
TOP-K算法是用来求N个数中,最大或者最小的K个数的算法.

找最大:
1. N个数中取出K个数建**小堆**
2. 从剩余的数中依次取出一个数与堆顶比较
3. 如果大于堆顶,则替换堆顶元素,并向下调整使得符合堆结构.

> 堆中的数即为我们找到的最大的K个数
> 
> 因为小堆的堆顶是堆中最小的数,如果有数大于堆顶,则这个数会成为新的最大的K个数之一
>
>此时原本的最小的数(也就是堆顶)表示最大的K个数之一了,所以直接用新的数替换堆顶,然后调整堆即可.


找最小:
1. N个数中取出K个数建**大堆**
2. 从剩余的数中依次取出一个数与堆顶比较
3. 如果小于堆顶,则替换堆顶元素,并向下调整使得符合堆结构

```c
Heap* TopK(int* nums, int n, int k){
	//用前K个数建堆
	Heap* hp = HeapCreate(nums, k);
	//用剩余数依次比较
	for(int i = k; i< n ;i++){
		if(nums[i] > HeapTop(hp)){
			//新数与堆顶交换
			hp->data[0] = nums[i];
			//向下调整堆顶数据
			AdjustDown(hp->data, k, 0); 
		}
	}
	return hp;
}
```