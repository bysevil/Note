main函数接受的是命令行参数

```c
int main(int argc,string argv[]){
	return 0;
}
```

argc 指的是main接受的参数的数目
argv 以字符串形式储存每个参数
> argc的最小值是1，不是0，因为程序最起码接受一个参数argv[0] ，它的值总是程序的名称。