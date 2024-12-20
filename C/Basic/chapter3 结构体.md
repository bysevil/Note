结构体是C语言自定义类型的一种

它是一个集合，可以包含多个不同类型的变量。这些变量称为成员变量（或称字段）。

## 1.结构体的定义及使用
### 1.1 定义
```C
struct name{
	int a;
	char b;
}
//这是一个有两个成员的结构体类型
//结构体类型名为name
```
## 2.使用
如前文所言，结构体是一种自定义的类型，类似于C语言自带的`int`,`char`。

所以我们应当先定义一个结构体类型的变量（或称此结构体的实例）。
```C
struct name obj;
//定义了类型为 struct name 的变量obj
//struct name 是上面我们自定义的结构体类型
struct name* ptr;
//定义了类型为 struct name* 的指针ptr
```
### 2.1 结构体类型的指针
```C
struct name* ptr;
```

这是一个结构体类型的指针，与普通变量指针不同的是，普通变量指针不初始化时是野指针，对其赋值是未定义的行为。

但是结构体指针即使不初始化，对其操作(如改变其成员变量的值)的行为也是合法的。

这与其独特的性质有关
 - 声明结构体指针
	 - 当直接操作这个指针
		 - 编译器申请一个结构体大小的空间
		 - 使得这个结构体类型的指针指向这片空间
		 - 执行相应的操作
	- 使这个指针指向别的结构体变量
		- 指针指向别的结构体变量
		- 编译器**不会**开辟新的结构体大小的空间
### 2.2 结构体访问操作符`.`，`->`
使用`.`可以访问结构体变量的结构体成员。

使用`->`可以访问结构体指针变量的结构体成员

```C
obj.a = 4;//对结构体变量obj的结构体成员变量a赋值
obj.b = 'A';//对结构体变量obj的结构体成员变量b赋值
printf("%d",obj.a);//打印obj.a
printf("%d",obj.b);//打印obj.b

ptr->a = 5; //对结构体指针ptr的结构体成员变量a赋值
ptr->b = 'B'; //对结构体指针ptr的结构体成员变量a赋值
printf("%d",ptr->a);//打印ptr->a
printf("%d",ptr->b);//打印ptr->b
```

> 值得注意的是，不同结构体变量之间并无关系。
> 
> 改变结构体变量`obj`的成员`a`的值不会影响别的结构体变量的成员`a`的值。

### 2.3 初始化结构体变量

结构体变量的初始化与数组类似，顺序初始化每个结构体成员。与数组不同的是，每个成员的类型并不相同

```C
struct name obj = { 1 , 'A'};
//创建结构体类型的变量obj
//并初始化其成员a的值为1
//初始化其成员b的值为2
```

结构体也可以不按顺序进行初始化

```C
struct name obj = {
	.b = 'A',
	.a = 1
}
//与上一段代码作用相同
//但是没有按顺序初始化
```

初始化结构体时并**不是必须**初始化每个成员

```C
struct name obj1 = { 1 };
//创建结构体类型的变量obj1，并只初始化其成员a的值为1
struct name obj2 = { .b = 'A'};
//创建结构体类型的变量obj2，并只初始化其成员b的值为'A'
```

> 结构体指针也可以这样初始化

## 3.特殊的结构体
### 3.1 匿名结构体
我们可以在声明结构体类型时，直接生成了一个此结构体的实例
```c
struct name{
//此处省略了结构体内部，下面的一些例子也一样
}obj;
```
这相当于
```c
struct name{};
struct name obj;
```
事实上,结构体类型名**不是必须**的
```c
struct{}obj;
```
这样，我们创建了一个匿名的结构体类型，并为之创建了结构体类型的实例。

> 我们很难在别的地方再次创建一个此结构体的实例，因为这个结构体是匿名的。
 
 可以同时声明多个实例，如
```c
struct{}obj1,obj2;
```

> 非匿名结构体也可以在定义时声明多个实例 

### 3.2 嵌套结构体
结构体支持嵌套声明
```c
struct name {
	int a;
	char b;
	struct name2{
		short c;
	};
};
```

> `name2`的成员不能与`name`的成员重名，否则会造成重定义错误
>
> 我们可以直接在`name`的作用域中声明`name2`类型的变量，而非只能在`name`内部声明

嵌套一个匿名结构体试试
```c
struct name {
	int a;
	char b;
	struct{
		short c;
	}o;
};

```
我们可以这样访问成员C
```c
struct name obj;
obj.o.c = 10;
```

> 直接通过`obj.c`访问成员c是非法的

### 3.3 自引用结构体
结构体是支持自引用的。但并非直接引用
```c
struct name{
	int a;
	struct name obj;
};//这种嵌套是错误的
```

> 上述代码的错误主要在于内存，我们在创建一个结构体实例时，会为结构体申请内存，如果存在自引用，则又要为自引用的实例创建内存，但是自引用的实例申请的内存中又存在自引用的自引用实例。这样就陷入了无穷的嵌套中去。

正确的方式为：
```c
struct name{
	int a;
	struct name* ptr;
}
```

> 也就是说，只要涉及自引用，便不可以使用匿名结构体。

## 4.对结构体类型重命名
使用`typedef`对结构体类型进行重命名
```c
typedef struct name{}newname;//对非匿名结构体重命名
typedef struct{}newname;//对匿名结构体重命名
```
这样我们再使用时就可以
```c
newname obj1;
newname* ptr;
```
但是注意，自引用时依旧需要使用原名
```c
typedef struct name{
	newname obj1;//错误
	struct name obj2;//正确
}newname;
```
>
## 5.结构体所占的内存
有如下结构体
```c
struct tag{
	char a;
	int b;
}obj;
```
很容易想到，`obj`所占内存大小为5字节，即一个`int（4字节）`加上一个`char（1字节）`。

但并非如此，在VS中`obj`实际所占内存为`8`字节;这实际是因为内存对齐的存在

### 5.1 内存对齐

1. 结构体的第一个成员从0地址处开始。  
2. 其他成员变量起始地址对齐到对齐数的整数倍。  
	- 对⻬数 = min(编译器默认对⻬数,该成员变量大小)  
	- VS中默认的对齐数为8  
	- Linux中没有默认对⻬数  
3. 结构体总大小为最大对⻬数的整数倍。

> 注意，只是对齐。变量实际所占大小未变

### 5.2 修改默认对齐数
```c
#pragma pack(1)
```
修改默认对齐数为1(即不进行内存对齐)
### 5.3 在内存对齐下节省内存
```c
struct name1{
	char a;
	int b;
	char c;
}obj1;

struct name2{
	char a;
	char b;
	int c;
}obj2;
```
如果你对比`obj1`和`obj2`所占的内存，你会发现`obj1`占了`12`个字节，而`obj2`只占了`8`个字节。

这与内存对齐有关

- 对于obj1
	- `char a` 占`1`个字节，对齐数为`1`。
		- 为结构体申请`1`字节空间。（此时结构体大小为`1`）
	- `int b` 占`4`字节,对齐数为`4`。
		- 向`4`的倍数对齐，结构体大小变为`4`。变量`a`后的`3`字节空间浪费。
		- 为变量`b`申请`4`字节空间。（此时结构体大小为`8`字节）
	- `char c`占`1`个字节，对齐数为`1`
		- 向1的倍数对齐，因为已经对齐，不额外申请空间
		- 为变量`c`申请`1`字节空间。（此时结构体大小为`9`字节）
	- 此时结构体的最大对齐数为`4`
		- 因为结构体的空间为最大对齐数的整数倍
		- 向4的倍数对齐，结构体大小变为`12`.变量`c`后的`3`字节浪费 
	- 结构体总大小为`12`
	- 对于`obj2`
		- `char a`占一个字节，对齐数为`1`
			- 为结构体申请`1`字节空间.（此时结构体大小为`1`）
		- `char b`占一个字节，对齐数为`1`.
			- 向`1`的倍数对齐，因为已经对齐，不额外申请空间
			- 为变量`b`申请`1`字节空间。（此时结构体大小为`2`）
		- `int c`占`4`个字节，对齐数为`4`
			- 向`4`的倍数对齐，结构体大小变为`4`。变量`b`后的`2`字节浪费
			- 为变量`c`申请`4`字节空间。（此时结构体大小变为8）。
		- 此时结构体最大对齐数为`4`。
			- 结构体总大小为最大对齐数整数倍。
			- 向4的倍数对齐，因为已经对齐。不额外申请空间。
		- 结构体总大小为`8`

也就是说，我们可以通过修改变量的声明顺序以达到节省空间的效果。

### 5.4 为什么存在内存对齐
1. 平台原因(移植原因)：

不是所有的硬件平台都能访问任意地址上的任意数据的；某些硬件平台只能在某些地址处取某些特定类型的数据，否则抛出硬件异常。

2. 性能原因：

数据结构(尤其是栈)应该尽可能地在自然边界上对⻬。

原因在于，为了访问未对⻬的内存，处理器需要作两次内存访问；而对⻬的内存访问仅需要一次访问。

假设一个处理器总是从内存中取8个字节，则地址必须是8的倍数。

如果我们能保证将所有的double类型的数据的地址都对⻬成8的倍数，那么就可以用一个内存操作来读或者写值了。

否则，我们可能需要执行两次内存访问，因为对象可能被分放在两个8字节内存块中。

总体来说：结构体的内存对⻬是拿空间来换取时间的做法
## 6.位段
结构体除了能使用字段成员，也能使用位段成员。与字段成员不同，位段成员的大小是以比特位为单位，而非以字节为单位。

```c
strcut name{
	int _a:8;
};
```
上面生成了一个位段`_a`虽然声明位int型，但它实际只占8个比特位，也就是一个字节。

> 位段的成员必须是`int`、`unsigned int` 或`signed int`，在C99中位段成员的类型也可以选择其他类型。
>
> 虽然`int _a`只占1个字节，但是结构体占4字节空间，因为int型的对齐数是4.
> 
> 位段成员名并非必须使用`_`开头，只是笔者为了与字段成员区分

大多数时候位段的几个成员共有同一个字节，这样有些成员的起始位置并不是某个字节的起始位置，那么这些位置处是没有地址的。

内存中每个字节分配一个地址，一个字节内部的bit位是没有地址的。

所以不能对位段的成员使用&操作符

这样就不能使用scanf直接给位段的成员输入值，只能是先输入放在一个变量中，然后赋值给位段的成员

### 6.1 位段的内存分配

虽然上面的代码是声明了一个8位的位段，但是实际上你可以声明任意位的位段。不用以字节（也就是8位）为单位。
> 单个位段的最大值与及其变量类型有关，如char型位段，最大只能占8位。也与机器有关，如32位机器最多只支持32位的位段。

但是位段的空间是按照需要以4个字节（int）或者1个字节（char）的方式来开辟的。

```c
struct name{
	int _a:1;
};
```

上面的代码声明的`_a`位段只占一个位，但是在内存中开辟了一个字节的空间，剩下的7位浪费。

```c
struct name{
	int _a:1;
	int _b:4;
}
```
上面的代码声明的两个位段只占5个位的空间。但是依旧开辟了一个字节的空间。剩下的3位浪费
> 位段
### 6.2 位段的跨平台性
1. `int`位段被当成有符号数还是无符号数是不确定的。
2. 单个位段变量所能占的最大位不确定。（16位机器最大16，32位机器最大32，写成27，在16位机器会出问题。
1. 位段中的成员在内存中从左向右分配，还是从右向左分配标准尚未定义。
	- 如有：`char _a:1`和`char _b:4`。编译器开辟一个字节空间。
	- 如果从左向右分配，则`_a`占第1个位，`_b`占第2到第5个位。
	- 如果从右向左分配，则`_a`占第8个位，`_b`占第4到第7个位
2. 当一个结构包含两个位段，第二个位段成员比较大，无法容纳于第一个位段剩余的位时，是舍弃剩余的位还是利用，这是不确定的。
	- 如有：`int _a:4`和`int _b:12`，编译器开辟4字节空间。
		- 如果利用`_a`剩余的位，第一个字节第1到4位分配给`_a`，第一个字节剩下的4位和第二个字节的所有位分配给`_b`。剩下16位舍弃。
		- 如果舍弃剩余的位，则第一个字节1到4位分配给`_a`，剩下4位舍弃。第2个字节所有位分配给`_b`和第三个字节前4位分配给`_b`，剩下12位舍弃。

总结：跟结构相比，位段可以达到同样的效果，并且可以很好的节省空间，但是有跨平台的问题存在。