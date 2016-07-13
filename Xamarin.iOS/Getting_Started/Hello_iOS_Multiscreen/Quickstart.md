#Hello, iOS 多屏应用
>在这个2个部分的教程中，我们扩展前面创建的Phoneword程序，加入第二个页面。并且， 我们会介绍模型（Model）、视图（View）和控制器（Controller)设计模式，实现我们第一个iOS导航，和形成一个更深的iOS应用结构和功能的理解。

使用这个连接来下载Phoneword程序：

##Hello.iOS 多屏应用 快速入门

在这个教程的攻略部分，我们为Phoneword加上第二个页面来记录使用这个应用打电话的历史。最终的程序会有一个第二个页面来显示通话记录。见下方图。

![完成应用](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/00.png)

在这个教程的深入研究里面，我们会分析这个应用然后讨论这个的架构，导航和其他新的iOS概念我们可能会遇见。

##必备条件
这个教程需要要求你下载前面的代码。

##攻略 - 使用Segues导航

1. 使用Xamarin Studio来打开Phoneword代码。![phoneword代码](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/01New.png)

2. 我们先来修改用户界面。使用*Solution Pad*来打开*Main.storyboard* 。![storyboard](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/02New.png)

3. 接下来我们从*Toolbox*里面脱出一个*Navigation Controller(导航控制器)*。你需要放小才能看见全部的设计空间。![navigation](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/03New.png)

4. 把*Sourceless Segue*(那个灰色的箭头现在在Phoneword场景左侧）给拖到Navigation Controller的左侧， 改变程序起点。![change Segue](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/04New.png)

5. 点击 *Root View Controller* 的底部黑条， 然后按*Delete*键，从设计空间中删除。然后，把*Phoneword*场景拖到*Navigation Controller*旁边。![delete scene](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/05New.png)

6. 接下来，我们把*Navigation Controller*的*Root View Controller*设为 *Phoneword*的*View Controller*. 选择*Navigation Controller*，按下CTRL键,一个蓝线应该会出现。然后，继续按着CTRL键, 鼠标从*Navigation Controller*拖动到*Phoneword*场景然后松开。这个叫做”Ctrl-dragging".![delete scene](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/06.png)

7. 在跳出的框里，选择*Root*:
![root](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/07New.png)
这个*ViewController*现在是*Navigation Controller*的*Root View Controller*, 也就是根视图控制器。
![change root](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/08.png)


