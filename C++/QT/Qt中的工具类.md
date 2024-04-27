
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

## QDate 日期


可以将日期转换为String型号

```C++
QDate date;
date.to_string();
```