# 第二章 - APP结构

现代用户界面是从各种个样的视觉对象来创建出。取决与不同操作系统，视觉对象可能有不同的名字-控件（controls），元素（element），视图（views）以及部件（widgets），但是它们都是专注于显示，交互，或者两个都负责。

在Xamarin.Forms，屏幕上现实的对象全部都称为*visual elements*（视觉元素）。它们主要分为三个种类。

* Page(页面）
* Layout（布局）
* View（视图）

这些不是抽象的概念！Xamarin.Forms的应用程序接口(API)定义一些类命名为 `VisualElement`,`Page`,`Layout`和`View`。这些类和它们的继承类成为Xamarin.Forms用户界面的支撑。`VisualElement`是一个在Xamarin.Forms里非常重要的一个类。 一个`VisualElement`对象是所有在屏幕占据空间的元素。

一个Xamarin.Forms应用是由一个或更多页组成。一个页面一般占据全部（或至少很大一部分）屏幕。有些应用只有一个页面，同时，其他应用能在不同页面中导航。在很多这本书靠前的部分，你只会看见一个页面类型叫做`ContentPage`。

在每个页面中，视觉元素是使用父子等级来组织的。一个`ContentPage`的子元素一般是某种布局来组织里面的元素。有些布局只有一个子元素，但是很多布局能布置很多子元素。 这些子元素可以是其他布局或者视图。有些布局可以组织子元素到一个堆，在二元格（Grid）中，或者以一种更自由的模式。但是在本章中，我们的页面将只包含一个子元素。

`View`这个词，在Xamarin.Forms里代表我们熟悉的显示或交互对象：text（文本）, bitmaps（位图）, buttons（按钮）, text-entry fields（输入框）, sliders（滑动条）, switches（开关）, progress bars（进度条）, date and time pickers（日期和时间选择器）, 或者其他你设计的视图。这些一般来说是叫做控件在其他编程环境里。这本书称呼他们为视图或者元素。在这章，你会遇见`Label`(标签）来显示文本。

##说Hello
使用微软Visual Studio或者Xamarin Studio，我们创建一个新的Xamarin.Forms程序使标准模板。这个流程会创建一个解决方案包含六个项目：5个平台项目-iOS, Android, the Universal Windows Platform (UWP), Windows 8.1, and Windows Phone 8.1-和一个共同分享的项目包含这个应用的绝大代码。

在Visual Studio里，选择**文件>新建>项目**。在新项目窗口左侧，选择**Visual C#**然后**Cross-Platform**。在这个窗口的中部，你会看见一些可使用解决方案模板，包括三个Xamarin.Forms的：

* **Blank App (Xamarin.Forms Portable)**
* **Blank App (Xamarin.Forms Shared)**
* **Class Library(Xamarin.Forms)**

现在怎么办呢？我们肯定是要创建一个Blank App解决方案但是什么样的呢？

Xamarin Studio有一个相似的问题。

* **Use Portable Class Libary**
* **Use Shared Libary**

在这个上下文中，'Portable’这个词指的是一个Portable Class Libary（PCL）。所有共同的应用代码成为一个被所有单独引用的动态链接库（DLL）。

’Shared‘这个词在这个上下文指的是一个Shared Asset Project（SAP）包含松散的代码文件（和其他文件），分享到所有平台项目里，基本上成为项目的一部分。

目前我们先使用Portable。给创建的项目一个名字-例如-**Hello**-然后选择一个文件路径在这个窗口里。

如果你在使用Visuak Studio，六个项目会被创建：一个共有项目（PCL项目）和其他五个应用项目，像一个叫做Hello的解决方案，有：

* 一个被其他五个引用的PCL项目叫做**Hello**
* 一个给安卓的应用叫做**Hello.Droid**
* 一个给iOS的应用叫做**Hello.iOS**
* 一个给Universal Windows Platform of Windows 10 和 Windows Mobile 10的应用叫做**Hello.UWP**
* 一个给Windows8.1的应用叫做**Hello.Windows**
* 一个给Windows Phone 8.1的应用叫做**Hello.WinPhone**

如果你在Mac上运行Xamarin Studio， Windows 和Windows Phone 项目不会被创建。
