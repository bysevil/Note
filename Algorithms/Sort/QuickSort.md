快速排序在大多数情况下都是速度最快的算法。

1. 对范围内数据排序 
	1. 找一个基准值key
	2. 使得key左边的数都小于key，key右边的数都大于key
2. 对key的左右两边执行同样的算法
3. 重复，直到key的左右两边无法再排序（只有一个数或者没有数）

## 不使用数组的第一个数当key
我们一般使用数组的第一个数当key，但是你可以使用数组中的任意一个数当key，不过要额外进行处理。

> 一般来说这没有意义，但是有的算法题会针对快速排序的最差情况来设计测试用例，此时就需要使用一个数组中的随机数当key并使用三路划分进行优化,来避免超时

> 针对性OJ题和使用快排接答在文章最后

处理思路为，选出一个随机值，在这个随机值和数组的第一个数与数组的最后一个数中选出大小居中的数作为key，并与数组的第一个数交换
```c
void MidKey(int* nums, int left, int right){
	int mid = left + rand() % (right - left);
	if(nums[left] < nums[mid]){
		if(nums[mid] < nums[right])// left < mid < right
			swap(nums+left,nums+mid);
		if(nums[right] > nums[left])
			swap(nums+left,nums+mid);// left < right < mid	
	}
	if(nums[left] > nums[mid]){
		if(nums[mid] > nums[right])
			swap(nums+left,nums+mid)// right < mid < left
		if(nums[left] > nums[right])
			swap(nums+left,nums+right)// mid < right < left
	}
}
```
## 快排实现
### 递归实现
```c
void QuickSort(int* a, int begin, int end){
	if (begin >= end)
		return;

	int key = PartSort(a, begin, end - 1);

	QuickSort(a, begin, key);
	QuickSort(a, key + 1, end);
}
```
### 非递归实现
非递归利用栈的思想
1. 将要排序的数组的左右边界入栈
	1. 左右边界出栈，对其中数据排序
	2. 排序结束后，判断key的左右是否还需要排序
	3. 如果需要排序，将key左边和右边的左右边界入栈
2. 重复上面3步，直到栈为空，此时数组有序
```c
void QuickSortNonR(int* nums, int left, int right){
    Stack* s = StackInit();
    StackPush(s,left);
    StackPush(s,right);

    while(!StackEmpty(s)){
        int end = StackPop(s);
        int begin = StackPop(s);

        int key = PartSort(nums,begin,end-1);

        if(begin < key - 1){
            StackPush(s,begin);
            StackPush(s,key);
        }
        if(key + 1 < end - 1){
            StackPush(s,key + 1);
            StackPush(s,end);
        }
    }
}
```
## 排序主体
### hoare方法
1. 找数组中的第一个数当key
2. 使用两个指针left和right
	1. 右边指针向左走，找到小于key的数时停止
	2. 左边指针向右走，找到大于key的数时停止
	3. 交换左右指针的值
3. 重复上面3步，直到左右指针相遇
4. 将key放在指针left处，将left原本的值放在数组的第一位。
5. 此时key左边都小于key，右边都大于key，key位于交汇处

> 如果使用数组的第一个数当key，则必须保证右边指针先走，这样能保证左右指针相遇时处的值小于等于key，可以放心的交换到左边
> 如果选数组最后一个数当key，则必须左指针先走
> 如果随机使用一个数当key，则必须进行额外的判断

```c
int PartSort(int* nums, int left, int right){
	int key = left;
	while (left < right){
		while (left < right && nums[right] >= nums[key])
		    right--;
		while (left < right && nums[left] <= nums[key])
			left++;
		Swap(nums + left, nums + right);
	}
	Swap(nums + key, nums + left);
    return left;
}
```
### 挖坑法

挖坑法顾名思义，就是挖一个坑，将数填进来。

1. 选择数组的第一个数当key，key所在的位置为坑
2. 使用两个指针，left和right
	1. 右边指针向左走，找比key小的数，找到后与key交换，此时这个位置成为新的坑。
	2. 左边指针向右走，找比key大的数，找到后key交换，此时这个位置成为新的坑
3. 重复上面两步，直到左右指针相遇
4. 此时key的左边都小于key，右边都大于key，key一直在坑中

> 使用第一个数当坑，则必须先动右指针

```c
int PartSort(int* nums, int left, int right){
	int key = left;
    while(left < right){
        while(left < right && nums[right] >= nums[key])
            right--;
        Swap(nums+right,nums+key);
        key = right;
        while(left < right && nums[left] <= nums[key])
            left++;
        Swap(nums+left,nums+key);
        key = left;
    }
    return key;
}
```

### 快慢指针法
1. 找数组的第一个数当key
2. 使用两个指针cur和prev
3. cur向后遍历，找比key小的数，找到后和prev + 1位置的数交换，prev向后移动
4. cur遍历完整个数组后，prev和key交换
5. 此时key的左边都小于key，右边都大于key，key位于prev处

```c
int PartSort3(int* nums, int left, int right){
    int key = left;
    int prev = left;
    int cur = left + 1;
    while(cur <= right){
        if(nums[cur] < nums[key] && ++prev < cur)
            Swap(nums+prev,nums+cur);
        cur++;
    }
    Swap(nums+prev,nums+key);
    return prev;
}
```
## 三路划分优化最差情况
快速排序在排序单值数组，如：{2,2,2,2,2}时会到达最坏情况，此时可以使用三路划分来进行优化

1. 使用数组的第一个数当key
2. 使用三个指针left，right和cur
3. cur遍历数组
	1. 找到小于key的数，和left交换，left++,cur++
	2. 找到大于key的数，和right交换，right--
	3. 找到等于key的数，cur++
4. 当cur和right交汇时结束
5. 此时left和right中间的数都为key，left左边都小于key，right右边都大于key（不包含left和right）

```c
void QuickSort(int* nums,int begin,int end){
    if(begin >= end - 1)
        return;
    int key = nums[begin];
    int left = begin, right = end - 1;
    int cur = left + 1;
    while(cur <= right){
        if(nums[cur] < key){
            swap(nums+cur, nums+left);
            left++;
            cur++;
        }
        else if(nums[cur] > key){
            swap(nums+cur,nums+right);
            right--;
        }else{
            cur++;
        }
    }
    QuickSort(nums,begin,left);
    QuickSort(nums,right+1,end);
} 

```
## 数组排序OJ 
使用随机key和三路划分通过[针对性OJ题](https://leetcode.cn/problems/sort-an-array/)
```
void swap(int* a, int* b){
    int tmp = *a;
    *a = *b;
    *b = tmp;
}
void MidKey(int* nums, int left, int right){
	int mid = left + rand() % (right - left);
	if(nums[left] < nums[mid]){
		if(nums[mid] < nums[right])//left < mid < right
			swap(nums+left,nums+mid);
		if(nums[right] > nums[left])
			swap(nums+left,nums+mid);//left < right < mid
			
	}
	if(nums[left] > nums[mid]){
		if(nums[mid] > nums[right])
			swap(nums+left,nums+mid);//right < mid < left
		if(nums[left] > nums[right])
			swap(nums+left,nums+right);//mid < right < left
	}
}

void QuickSort(int* nums,int begin,int end){
    if(begin >= end - 1)
        return;
    MidKey(nums,begin,end-1);

    int key = nums[begin];
    int left = begin, right = end - 1;
    int cur = left + 1;
    while(cur <= right){
        if(nums[cur] < key){
            swap(nums+cur, nums+left);
            left++;
            cur++;
        }
        else if(nums[cur] > key){
            swap(nums+cur,nums+right);
            right--;
        }else{
            cur++;
        }
    }
    QuickSort(nums,begin,left);
    QuickSort(nums,right+1,end);
} 

int* sortArray(int* nums, int numsSize, int* returnSize) {
    QuickSort(nums,0,numsSize);
    *returnSize = numsSize;
    return nums;
}
```