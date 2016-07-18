# Xamarin.iOS 导航模式

> xamarin 有很多种导航模式，使用自己最需要的和适用的导航方式是最重要的。

## 导航模式简介
* `UINavigationController`

这个导航模式应该是我们最熟悉的一个了，在系统自带的设置就是这种导航方式。
![](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_deepdive/Images/01.png)

* `UITabBarController`

这种导航模式使用底部导航栏，让一些视图以平行结构呈现出。系统自带的News是一个很好的例子。
![](http://blogs-images.forbes.com/bradmoon/files/2016/06/iOS-10-News.jpg)

* `UISplitViewController`
这个导航模式一般在iPad， 和iPhone Plus 上用的较多。屏幕分开成2部分，一个Master（类似导航页）和相对应的Detail页面。

![](http://www.programmingforiphone.com/content/files/2014/11/image.jpg)

# Navigation Controller

我们先来看一下怎么使用最常用的Navigation Controller.

## 原理
Navigation Controller 使用的是一个Stack（伐）来存储用户的ViewController（视图控制器），并且是视图的父控制器。 每新显示一个视图，这个Stack上会放上一个新的ViewController，注意底部的ViewController还是会在内存里，所以用户点击返回以后，系统把Stack最前面的ViewController给扔掉，所以前面的ViewController的状态还是会被保存，但是点击返回的ViewController的状态就不会被保留。因此，Navigation Controller 很适合显示多层关系，允许用户挖掘出更多的细节。

## 使用代码实现
本教程会在后部分加如使用storyboard来操纵Navigation Controller。

### 创建 Navigation Controller
一般来讲，我们是在Appdelegate 文件里，创建一个Navigation Controller 作为程序的根View Controller，并且把第一个子View Controller给传入进传入进去。

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
Window = new UIWindow (UIScreen.MainScreen.Bounds);
var navigationController = new UINavigationController(new rootViewController());
Window.RootViewController = navigationController; Window.MakeKeyAndVisible(); 
return true;
}
```

### 往前导航，显示一个新View Controller

每个UIViewController都有一个NavigationController属性,直接引用到当前的Navigation Controller。 如果当前界面没有使用UINavigation Controller， 这个值将会是null。
Navigation Controller 有一个PushViewController。传入一个要显示的View Controller和一个布尔值，表达需不需要动画。

```csharp
public virtual Void PushViewController (UIViewController viewController, Boolean animated)
```

我们可以利用这个机会来给新的View Controller 传值。

```csharp
...
var pushedViewController = new customViewController(whatevervalueyouwanttopass);//在这里可以传值。
this.NavigationController.PushViewController(pushedViewController, animated:true);
```

### 返回
注意，一旦返回后，被返回的Navigation Controller的状态会失去。并且，如果Navigation Controller 的Stack只有一个元素是不能返回的。不仅可以使用系统的返回键，我们也可以使用代码定义。

```csharp
this.NavigationController.PopViewController(animated:true);
```

或者可以直接回到最底层

```csharp
this.NavigationController.PopToRootViewController(animated:true);
```
或者回到一个指定的View Controller

```
this.NavigationController.PopToViewController(
viewController,animated:true);
```

###其他方法
Navigation Controller还有其他的一些相对高级的一些的方法在下面列出。

```csharp
public class UINavigationController {
UIViewController TopViewController { get; } UIViewController VisibleViewController { get; } UIViewController[] ViewControllers { get; set; }
UIViewController[] PopToRootViewController(bool animated); 
void SetViewControllers(UIViewController[] controllers, bool animated);
}
```

###使用NavigationItem来操控导航栏

```csharp
NavigationItem.Prompt = "这个是一个Prompt";
NavigationItem.Title = "这个是一个Title";
NavigationItem.RightBarButtonItem = new UIBarButtonItem(UIBarButtonSystemItem.Done);
NavigationItem.LeftBarButtonItem = new UIBarButtonItem(UIBarButtonSystemItem.Cancel);

```
具体效果请看下图，注意，如果设置了左侧的按钮，系统的返回键就消失了。
![](https://raw.githubusercontent.com/jiujiu1123/mono-xamarin-_chinese_doc/master/Xamarin.iOS/Application%20Fundamentals/iOSNavigation_Own/Pic/NavigationItem.png)
BarItem也是可以自定义，放自己的图片以及文字。

这些属性可以藏住导航栏

方法名|解释
-----|-----
HidesBarsOnSwipe(Boolean)|导航栏向上滑的时候是否藏住
HidesBarsOnTap(Boolean)|导航栏被点击的时候是否藏住
HidesBarsWhenKeyboardAppears(Boolean)|键盘出来是否藏住导航栏
HidesBarsWhenVerticallyCompact(Boolean)|在Size Class是Vertical Compact（iPhone竖屏时）
NavigationBarHidden（Boolean）|是否藏住导航栏。
