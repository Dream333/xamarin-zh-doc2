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

当你创建一个新Xamarin.Forms解决方案，Xamarin.Forms库（和其他支持库）会从Nuget软件包管理器自动下载。Visual Studio和Xamarin Studio存储这些库在一个教Packages的文件夹。 但是一个指定的Xamarin.Forms版本是按照解决方案模板指定而下载的，新的版本可能会有。

可以直接在Studio里更新。

下面讲描述一些关于编译的设置，大部分可以直接跳过，所以先不翻译。

When you create a new Xamarin.Forms solution, the Xamarin.Forms libraries (and various support libraries) are automatically downloaded from the NuGet package manager. Visual Studio and Xamarin Studio store these libraries in a directory named packages in the solution directory. However, the par- ticular version of the Xamarin.Forms library that is downloaded is specified within the solution tem- plate, and a newer version might be available.
In Visual Studio, in the Solution Explorer at the far right of the screen, right-click the solution name and select Manage NuGet Packages for Solution. The dialog that appears contains selectable items at the upper left that let you see what NuGet packages are installed in the solution and let you install others. You can also select the Update item to update the Xamarin.Forms library.
In Xamarin.Studio, you can select the tool icon to the right of the solution name in the Solution list and select Update NuGet Packages.Before continuing, check to be sure that the project configurations are okay. In Visual Studio, select the Build > Configuration Manager menu item. In the Configuration Manager dialog, you’ll see the PCL project and the five application projects. Make sure the Build box is checked for all the projects and the Deploy box is checked for all the application projects (unless the box is grayed out). Take note of the Platform column: If the Hello project is listed, it should be flagged as Any CPU. The Hello.Droid project should also be flagged as Any CPU. (For those two project types, Any CPU is the only option.) For the Hello.iOS project, choose either iPhone or iPhoneSimulator depending on how you’ll be testing the program.
For the Hello.UWP project, the project configuration must be x86 for deploying to the Windows desktop or an on-screen emulator, and ARM for deploying to a phone.For the Hello.WinPhone project, you can select x86 if you’ll be using an on-screen emulator, ARM if you’ll be deploying to a real phone, or Any CPU for deploying to either. Regardless of your choice, Visual Studio generates the same code.
If a project doesn’t seem to be compiling or deploying in Visual Studio, recheck the settings in the Configuration Manager dialog. Sometimes a different configuration becomes active and might not include the PCL project.
In Xamarin Studio on the Mac, you can switch between deploying to the iPhone and iPhone simula- tor through the Project > Active Configuration menu item.
In Visual Studio, you’ll probably want to display the iOS and Android toolbars. These toolbars let you choose among emulators and devices and allow you to manage the emulators. From the main menu, make sure the View > Toolbars > iOS and View > Toolbars > Android items are checked.
Because the solution contains anywhere from two to six projects, you must designate which pro- gram starts up when you elect to run or debug an application.
In the Solution Explorer of Visual Studio, right-click any of the five application projects and select the Set As StartUp Project item from the menu. You can then select to deploy to either an emulator or a real device. To build and run the program, select the menu item Debug > Start Debugging.
In the Solution list in Xamarin Studio, click the little tool icon that appears to the right of a selected project and select Set As Startup Project from the menu. You can then pick Run > Start Debugging from the main menu.

如果全部都没出问题，从模板创建出的骨架程序会运行，你可以看见这个小信息。
![](/Users/peterzhong/Documents/mono-xamarin-_chinese_doc/Xamarin Form/Creating Mobile Apps With Xamarin.Forms Book/Chapter2/pic/0024fig01.jpg)
你可以看见，每个平台有自己的颜色结构。iOS和Windows 10 移动应用显示黑色字体在白色背景上，同时，安卓设备显示白色字体在黑色背景。通常来说，Windows8.1 和 Windows Phone 8.1 和安卓一样，显示白色字体在黑色背景上。

在默认选项里，所有平台都为手机方向做了调整。把手机转成横屏，你会看见文字调整到新的中心。

这个应用不仅可以在设备或者虚拟机上运行，也可以部署到其他设备。它和其他应用在设备或虚拟机上一起显示，也可以从那里直接运行。 如果你不喜欢应用图标或名称，你可以单独在每个平台上改变。

##了解Form的文件

显然，这个被Xamarin.Forms模板创建的程序是非常简单的，所以这是一个学习生成代码的相互联系和工作方式的好机会。

我们先来看看负责显示我们在屏幕上看见的文字的代码。这是一个App类在**Hello**项目里。在一个Visual Studio创建的文件里，App类是定义在 App.cs里，但是在Xamarin Studio里，这个文件会是Hello.CS。如果项目模板从本书出版时没有改变太多，它估计会长的像这个。

```csharp

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

public class App: Application {
 public App() {
  // The root page of your application
  MainPage = new ContentPage {
   Content = new StackLayout {
    VerticalOptions = LayoutOptions.Center,
     Children = {
      new Label {
       HorizontalTextAlignment = TextAlignment.Center,
        Text = "Welcome to Xamarin Forms!"
      };
     }
   }
  }
 }
 protected override void OnStart() {
  // Handle when your app starts
 }
 protected override void OnSleep() {
  // Handle when your app sleeps
 }
 protected override void OnResume() {
     
 }
  // Handle when your app resumes
}

```

注意命名空间的名字和项目名字是一样的。这个App类是定义为Public， 并且从Xamarin.Forms的`Application`类继承。这个构造函数真的只有一个责任：把一个`Page`类的对象给设置为`Application`类的`MainPage`属性。