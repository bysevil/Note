## Qt容器与std容器

qt诞生于91年，98年C++标准才规定使用QLabel风格的头文件，在之前，都是qlabel.h风格的头文件，建议使用标准的头文件，即使旧的文件可用。

在设置时，要注意qt自己的轮子，在qt诞生的91年，c++还没有标准，更没有STL库。所以qt自己造了一套轮子，如QString，QVector,QList,QMap。但是在实际使用时，我们即可以使用STL的容器，也可以使用Qt自己造的，两者大部分情况可以混用。

如setText的参数类型就是QString，但我们传入的是C风格字符串

 QString有时优于std::String 他对字符编码进行了处理，以进行国际化和本土化。

QString和std::string可以互转

```C++
//std::string转Qstring
Qstring Qstr = Qstring::fromStdString(std::string);

//Qstring转std::string
std::string = Qstr.toStdString();
```
 
## QTimer 定时器

定时器对象，启动后会每隔一段时间发送timeout信号

启动定时器，参数是每次发送timeout信号的时间间隔（单位为毫秒）
```C++
timeout->start(1000);
```

停止计时器
```C++
timeout->stop();
```
## Pixmap 图片

Pixmap可以导入外部图片资源
```C++
Qpixmap pixmap(":/1.jpg");// 导入图片文件
pixmap = pixmap.scalec(x,y);//缩放图片
```

## QRect 控件大小位置类

 QRect类，包含了控件的位置（x,y）坐标，大小（width,height）宽高。
 
QRect可以使用qDebug直接打印
```C++
qDebug() << Qrect;
```

可以查看指定值
```C++
QRect.y();
QRect.x();
QRect.width();
QRect.height();
```

修改指定值
```C++
QRect.setY();
QRect.setX();
QRect.setWidth();
QRect.setHeight();
```

> 注意，修改只是修改了Qrect类的值，还需要setGeometry来应用到控件


值得注意的是，如果对QRect的x,y进行修改，将会导致height和wight大小变化。

因为x,y实际调整的是控件左上角的像素点，此时heiget和wight会自动发生调整来使得其他三个角的位置不变。

如果我们实际希望只调整位置不改变大小(如x-5，y-5)，则需要(或是使用move)
```C++
setGeometry(qrect.x(）- 5 ,qrect.y(), qrect.width(), qrect.height());
```

## QIcon 图标

Qicon类类似于QPixmap类，不过其用于设置或获取窗口或控件的图标

初始化QIcon对象,传入图标文件的路径
```C++
QIcon icon("c:/icon/1.jpg");
```

## QFront 字体样式

QFront是控件的字体样式，可以通过QFront获取或设置控件字体样式

family 字体
```C++
font.setFamily("仿宋");
```
pointSize 大小
weight 粗细 （取值\[0,99\]，越大越粗） 
bold 是否加粗 （以下都是bool值）
italic 是否倾斜
underline 是否有下划线
strikeOut 是否有删除线
## Qt::focusPolicy 焦点
Qt::FocusPolicy是一个枚举类型，表示控件成为焦点的方式：

获取控件的foucsPolicy属性
```C++
Qt::FocusPolicy focusPolicy() 
```

- Qt::NoFocus：控件不会成为焦点
- Qt::TabFocus：控件可以通过tab键切换成为焦点
- Qt::ClickFocus：控件可以通过鼠标点击成为焦点
- Qt::strongFoucs：控件既可以通过鼠标，也可以通过tab成为焦点
- Qt::wheelFocus：控件可以通过鼠标滚轮成为焦点（新增，较少使用）

## QDateTime 日期类

QDate 日期类 QTime 时间类 QDateTime 日期时间类

可以将日期转换为String型

```C++
QDate date;
date.to_string();
```

计算时间日期的差值，
```C++
QDateTime dateTime;
int day = dateTime.daysTo(QDateTime); //计算两个日期的差值（单位为天）
int secsonds = dateTime.secsTo(QDateTime); // 计算两个日期的插值(单位为秒)
```
> 注意daysTo函数，只要跨过12点就是隔了一天。如2000/1/1 23:59 到2000/1/2 00:01 就算是已经相隔一天。即使两个时间只相隔1分钟。
## QRegExp 正则表达式

保存正则表达式
```C++
QRegExp regExp("正则表达式");
```

## QCursur 鼠标光标

## QTextCursur 输入框文本光标

```C++
QTextCursur testCursur;
//获取文本光标位置
testCursur->position();
//获取光标选中的文本
testCursur->selectedText();
```

### QKeySequence 按键序列

这个类是一个键盘按键序列,表示一个或多个按键

可以通过字符串来初始化
```C++
QKeySequence("Ctrl+1")
```

或是通过按键枚举
```C++
QkeySequence(Qt::CLRL + Qt::key_K);
```

## QShortcut 快捷键

QShortcut可以绑定一个键盘按键，当按下此按键时触发信号`Qshortcut::activated`

```C++
QShortcut* shortcur = new QShortcut(this);

QShortcut->setKey(QkeySequence);
```

## MosueEvent 鼠标事件

作为处理鼠标事件的参数传入，以处理各种鼠标事件

- real x 事件发生时的鼠标x轴位置 (坐标原点为控件左上角)
- real y 事件发生时的鼠标y轴位置 (坐标原点为控件左上角)
- int botton 鼠标按下的按键，是一个枚举值
	- Qt::LeftButton 左键
	- Qt::rightButton 右键

x() 获取事件发生时的鼠标x轴位置 (坐标原点为控件左上角)
y() 获取事件发生时的鼠标y轴位置 (坐标原点为控件左上角)
globalx(); 获取事件发生时的鼠标x轴位置 (坐标原点为屏幕左上角）
globaly(); 获取事件发生时的鼠标y轴位置 (坐标原点为屏幕左上角）
event->botton() 获取事件发生时的鼠标按下的按键。


