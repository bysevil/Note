用来查找数组中占比超过一半以上的数（可称为主要元素）的算法
1. 指定一个候选元素`candidate`,和一个累计值`count`;
	- 如果数组的元素和候选元素相同，累计值加一
	- 如果数组的元素和候选元素不同，累计值减一
2. 当累计值为0时，表明此时已经遍历过的数组元素中，假设的主要元素和其他元素的数目相等。
3. 因为主要元素占比超过数组的一半，所以可以继续假设未遍历过的第一个数是主要元素。
4. 重复上两步直到遍历结束，此时`candidate`是真正的主要元素。
```c
int majorityElement(int* nums, int numsSize){
    int candidate = nums[0], count = 0;
    for(int i = 0; i < numsSize; i++){
        if(count == 0) candidate = nums[i];
        if(nums[i] != candidate) count--;
        else count++;
    }
    return ans;
}
```