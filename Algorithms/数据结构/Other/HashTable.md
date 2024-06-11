# 位哈希
用一个数据的二进制位的每一位作为哈希表的值，位的位置为哈希表的下标。

缺陷：只能创建布尔值的哈希表，对取值范围要求更高
## 面试题01.01.判定字符是否唯一
[面试题 01.01. 判定字符是否唯一 - 力扣（LeetCode）](https://leetcode.cn/problems/is-unique-lcci/)
![[../../attachment/7bd77fe4a83125f418771e02226ebe8d.png]]
```c
bool isUnique(char* astr){
    int hashtab = 0; // 创建哈希表

    while(*astr){ // 遍历字符串
        int num = *astr - 'a'; // 获取字符在哈希表中的下标
        if(hashtab & (1 << num)) { // 如果此下标下已经有值
	        return false; // 则字符不唯一，返回false
	    }
        else { // 如果此下标下无值
	        hashtab += 1 << num; // 标记值
	    }
        astr++;
    }

    return true; // 遍历完字符串依旧未找到不唯一的字符，返回true
}
```
## 2351. 第一个出现两次的字母
[2351. 第一个出现两次的字母 - 力扣（LeetCode）](https://leetcode.cn/problems/first-letter-to-appear-twice/)
![[../../attachment/Pasted image 20230826194500.png]]
```c
char repeatedCharacter(char * s){
    int hashtab = 0;//创建哈希表

    while(*s){//遍历字符串

        if(hashtab & 1 << *s - 'a'){//如果此下标有值，则找到第一个出现两次的字符
            return *s;;//返回这个字符
        }else{//如果此下标无值
            flag += 1 << *s - 'a';//标记为有值
        }
        s++;
    }
    
}
```
# 哈希表的应用
[桶排序](桶排序.md)