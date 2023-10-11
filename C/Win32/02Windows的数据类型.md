Windows 数据类型并不是内建的数据类型类型，而都是从C类型重定义得到的。

```c++
typedef int                 INT;       /* 整形 */
typedef unsigned int        UINT;      /* 无符号整形 */
typedef unsigned int        *PUINT;    /* 无符号整形指针 */
typedef int                 BOOL;      /* 布尔类型 */
typedef unsigned char       BYTE;      /* 字节 */
typedef unsigned short      WORD;      /* WORD (无符号短整型) */
typedef unsigned long       DWORD;     /* DOUBLE WORD (无符号长整形)*/
typedef float               FLOAT;     /* 浮点型 */
typedef FLOAT               *PFLOAT;   /* 指向float类型指针 */
typedef BOOL near           *PBOOL;    /* 指向布尔类型指针 */
typedef BOOL far            *LPBOOL;
typedef BYTE near           *PBYTE;    /* 指向字节类型指针 */
typedef BYTE far            *LPBYTE;
typedef int near            *PINT;     /* 整形指针 */
typedef int far             *LPINT;
typedef WORD near           *PWORD;    /* 指向WORD类型的指针 */
typedef WORD far            *LPWORD;
typedef long far            *LPLONG;   /* 指向长整形的指针 */
typedef DWORD near          *PDWORD;   /* 指向DWORD类型的指针 */
typedef DWORD far           *LPDWORD;
typedef void far            *LPVOID;   /* 指向void类型的指针 */
typedef CONST void far      *LPCVOID;  /* 指向void类型的常指针 */
```
Windows 数据类型名命名的规律

- 无符号类型：一般是以“U”开头，比如“INT”对应的“UINT”。
- 指针类型：其指向的数据类型前加“LP”或“P”，比如指向 DWORD 的指针类型为“LPDWORD”和“PDWORD”。
- 句柄类型：以“H”开头。比如，HWND 是window（WND简写）也就是窗口的句柄，菜单(MENU)类型对应的句柄类型为 “HMENU” 等等。