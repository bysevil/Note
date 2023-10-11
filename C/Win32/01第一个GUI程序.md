编写Windows程序，首先要包含 windows.h 头文件。windows.h 还包含了其他一些Windows头文件，例如：

- windef.h：基本类型定义
- winbase.h：内核函数
- wingdi.h：用户接口函数
- winuser.h： 图形设备接口函数

这些头文件定义了Windows的所有数据类型、函数原型、数据结构和符号常量，也就是说，所有的Windows API都在这些头文件中声明。

在C语言中，程序都是“黑屏”的，称为控制台程序(Console Application)。而带界面的Windows程序(Windows Application)，也称为GUI程序(GUI Application)。

控制台程序以 main() 为入口函数，Windows程序以 WinMain() 为入口函数，动态链接库(DLL)以 DllMain() 为入口函数，不同的入口函数决定了不同类型的程序。
## WinMain函数原型

```c++
int WINAPI WinMain(
    HINSTANCE hInstance,  // 当前窗口句柄
    HINSTANCE hPrevInstance,  // 前一个窗口句柄，Win32下为NULL（Win16留下的废物，目前已弃用）
    LPSTR lpCmdLine,  // 命令行参数
    int nCmdShow  // 窗口显示方式
);
```

## 第一个GUI程序
```c++
#include <windows.h>

int WINAPI WinMain(
    HINSTANCE hInstance,
    HINSTANCE hPrevInstance,
    LPSTR lpCmdLine,
    int nCmdShow
){
    // 调用API 函数MessageBox
    int nSelect = MessageBox(NULL, TEXT("窗口内容"), TEXT("窗口标题"), MB_OKCANCEL);
    if(nSelect == IDOK){
        MessageBox(NULL, TEXT("你点击了“确定”按钮"), TEXT("提示"), MB_OK);
    }else{
        MessageBox(NULL, TEXT("你点击了“取消”按钮"), TEXT("提示"), MB_OK);
    }

    return 0;
}
```
## MessageBox()函数
这个函数用来弹出一个指定风格的对话框
```c++
int WINAPI MessageBox( HWND hWnd, LPCTSTR lpText, LPCTSTR lpCaption, UINT uType );
```
- hWnd：该消息框的父窗口句柄，如果此参数为NULL，则该消息框没有拥有父窗口。大家不用急于理解这个参数，后续会详细讲解。
- lpText：消息框的内容。LPCTSTR 是自定义数据类型，等价于 const char *。
- lpCaption：消息框的标题。
- uType：对话框的按钮样式和图标。
### uType参数
#### uType支持的按钮样式
|按钮|含义|
|---|---|
|MB_OK|默认值，有一个“确认”按钮在里面|
|MB_YESNO|有“是”和“否”两个按钮在里面|
|MB_ABORTRETRYIGNORE|有“中止”，“重试”和“跳过”三个按钮在里面|
|MB_YESNOCANCEL|有“是”，“否”和“取消”三个按钮在里面|
|MB_RETRYCANCEL|有“重试”和“取消”两个按钮在里面|
|MB_OKCANCEL|有“确定”和“取消”两个按钮在里面|
这些按钮都是宏定义：
```c++
#define MB_OK                 0x00000000L
#define MB_OKCANCEL           0x00000001L
#define MB_ABORTRETRYIGNORE   0x00000002L
#define MB_YESNOCANCEL        0x00000003L
#define MB_YESNO              0x00000004L
#define MB_RETRYCANCEL        0x00000005L
```
也就是说，你可以使用数字来表示按钮
#### uType支持的图标样式
|图标|含义|
|---|---|
|MB_ICONEXCLAMATION|一个惊叹号出现在消息框：![](http://localhost:8000/c9846dd6-bfb6-4bd0-ad01-cac77e4e9a94/OEBPS/Images/bae3e2a8ed52a69de81edcc0ccb330d7.jpg)|
|MB_ICONWARNING|一个惊叹号出现在消息框（同上）|
|MB_ICONINFORMATION|一个圆圈中小写字母i组成的图标出现在消息框：![](http://localhost:8000/c9846dd6-bfb6-4bd0-ad01-cac77e4e9a94/OEBPS/Images/507cd6f9028c09b866d3b817a4e4ef75.jpg)|
|MB_ICONASTERISK|一个圆圈中小写字母i组成的图标出现在消息框（同上）|
|MB_ICONQUESTION|一个问题标记图标出现在消息框：![](http://localhost:8000/c9846dd6-bfb6-4bd0-ad01-cac77e4e9a94/OEBPS/Images/0f445858227e65105884b5e2cfdbb231.jpg)|
|MB_ICONSTOP|一个停止消息图标出现在消息框：![](http://localhost:8000/c9846dd6-bfb6-4bd0-ad01-cac77e4e9a94/OEBPS/Images/143c1e3866b5aa3e53395b5d0efaf49f.jpg)|
|MB_ICONERROR|一个停止消息图标出现在消息框（同上）|
|MB_ICONHAND|一个停止消息图标出现在消息框（同上）|
这些图标同样是宏定义
```c++
#define MB_ICONHAND           0x00000010L
#define MB_ICONQUESTION       0x00000020L
#define MB_ICONEXCLAMATION    0x00000030L
#define MB_ICONASTERISK       0x00000040L
```
#### 同时定义窗口和图标
可以使用运算符`|`
```c++
MessageBox(
    NULL,
    TEXT("窗口内容"),
    TEXT("窗口标题"),
    MB_OKCANCEL | MB_ICONINFORMATION
);
```
按钮都是用十六进制的第1位（二进制前4位）来表示，图标都是使用十六进制第2位（二进制第5~8位）来表示。Windows 通过检测第1位的值来确定按钮的样式，检测第2位的值来确定图标样式。所以你也可以
```c++
MessageBox(
    NULL,
    TEXT("窗口内容"),
    TEXT("窗口标题"),
    0x11
);
```
来同时定义窗口与图标
### MessageBox()函数的返回值
MessageBox() 返回被按下的按钮，以数字表示，这些数字都被定义成了宏，如下所示：  
|返回值|含义|
|---|---|
|IDOK|用户按下了“确认”按钮|
|IDCANCEL|用户按下了“取消”按钮|
|IDABORT|用户按下了“中止”按钮|
|IDRETRY|用户按下了“重试”按钮|
|IDIGNORE|用户按下了“忽略”按钮|
|IDYES|用户按下了“是”按钮|
|IDNO|用户按下了“否”按钮|
对应的宏定义为
```c++
#define IDOK       1
#define IDCANCEL   2
#define IDABORT    3
#define IDRETRY    4
#define IDIGNORE   5
#define IDYES      6
#define IDNO       7
```