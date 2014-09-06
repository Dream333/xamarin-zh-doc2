#CocosSharp 抓捕香蕉游戏开发指南

本教程会告诉您怎么用CocosSharp 去开发一款游戏，从最开始设置一个项目到一个完整的游戏。 您可以学到怎么使用不同的CocosSharp内容，类似：精灵，动作，图层，视差，量子系统，场景以及物理。在这个教程完成后您就完成了您就完成您的第一个CocosSharp游戏。

下载CocosSharp

CocosSharp可以从NuGet下载pcl版本或是平台专属版本，源码可以从github上下载。

游戏概况

我们今天要创作的游戏叫做抓捕香蕉，目的是挪动小猴子去抓捕最多的香蕉拍。下面是完成后的截图。

![GoneBananas](pic/finsih_screen.jpg?raw=true)

我们这次会为安卓和iOS开发这款游戏，但是CocosSharp支持的平台不限于这些，请到官方wiki上查看完整支持列表。

首先我们大家Xamarin studio，新建一个iPhone empty project, 名字设置成GoneBananas.

创建完后，我们需要把NuGet上Cosossharp的包和它的依赖添加到项目中。

添加完之后新建一个安卓Ice Cream Sandwich Application 模板 到这个项目并且也添加nuget包到那个项目。

我们会把主要内容给放到一个shared project里面，这样的话我们的有些逻辑可以在不同的平台之间共享。我们把那个shared project来叫GoneBananasShared。

这几个项目都创建好后，我们就可以开始开发了。

创建应用委托（Application delegate)类

我们首先创建一个CCApplicationDelegate的子类，CCApplicationDelegate和UIApplicationDelegate很相似。这个类管理（handle）应用生命周期事件。

下面简单介绍下一些：

ApplicationDidFinishLaunching：app打开后触发

ApplicationDidEnterBackground：app进入后台，这时候是我们停止播放动画以及音乐的时候。

ApplicationWillEnterForeground：从后台返回，这时候我们应该恢复动画以及音乐

创建一个新类叫做GoneBananasApplicationDelegate 按照下面的实现。
```csharp
using CocosDenshion;

using CocosSharp;

namespace GoneBananas

{

  public class GoneBananasApplicationDelegate : CCApplicationDelegate

  {

    public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)

    {

      application.PreferMultiSampling = false;

      application.ContentRootDirectory = "Content";

      mainWindow.SupportedDisplayOrientations = CCDisplayOrientation.Portrait;

      application.ContentSearchPaths.Add("hd");

      CCSimpleAudioEngine.SharedEngine.PreloadEffect ("Sounds/tap");

      CCScene scene = GameStartLayer.GameStartLayerScene(mainWindow);

      mainWindow.RunWithScene (scene);

    }

    public override void ApplicationDidEnterBackground (CCApplication application)

    {

      // stop all of the animation actions that are running.

      application.Paused = true;

      // if you use SimpleAudioEngine, your music must be paused

      CCSimpleAudioEngine.SharedEngine.PauseBackgroundMusic ();

    }

    public override void ApplicationWillEnterForeground (CCApplication application)

    {

      application.Paused = false;

      // if you use SimpleAudioEngine, your background music track must resume here.

      CCSimpleAudioEngine.SharedEngine.ResumeBackgroundMusic ();

    }

  }

}
```
在这个类里面ApplicationDidFinishLaunching里面我们初始化一个MainWindow，让这个游戏以竖着方式运行。然后加载声音效果，运行原始场景。

内容文件夹（Content folder）

在GoneBananasApplicationDelegate里面我们把application.ContentRootDirectory（内容根目录）给设置成 "Content", 也就是一个文件夹的名字里面包含一些类似字体，图片和声音的文件。 这个文件夹以及里面的子文件夹都可以从github上下载。接下来把这个文件夹分别拷贝到ios和安卓的项目目录底下。（安卓应该把这个文件夹给放到Assets里）。

# CCApplication 

# 类

**这个类主要工作是启动游戏，这块是这个项目中唯一一处要为不同平台写一份代码的地方。**

**iOS： 请把下面的代码加到AppDelegate 里**

```csharp
public partial class AppDelegate : UIApplicationDelegate

{

  public override void FinishedLaunching (UIApplication app)

  {

    var application = new CCApplication ();

    application.ApplicationDelegate = new GoneBananasApplicationDelegate ();

    application.StartGame ();

  }

}
```

安卓：把这段代码给加到Mainactivity

```csharp
public class MainActivity : AndroidGameActivity

{

  protected override void OnCreate(Bundle bundle)

  {

    base.OnCreate(bundle);

    var application = new CCApplication();

    application.ApplicationDelegate = new GoneBananasApplicationDelegate();

    SetContentView(application.AndroidContentView);

    application.StartGame();

  }

}
```

创建第一个场景

在GoneBananasApplicationDelegate类中通过下面代码来创建第一个场景的实例。
```csharp
CCScene scene = GameStartLayer.GameStartLayerScene(mainWindow);
```

**CocosSharp使用场景，通过CCScene类来实现，使用场景可以管理不同部分的游戏逻辑。每个场景随后包含不同的层（layer）来为那个场景展示ui。每个层都要对返回它的父场景负责。像这个游戏我们创建了三个层**

GameStartLayer-一个开始介绍屏幕，点击这个
GameLayer-真正的游戏界面
GameOverLayer-游戏结束界面

添加一个类，加上下面的代码来实现GameStartLayer

```csharp
public class GameStartLayer : CCLayerColor
{
    public GameStartLayer () : base ()
    {
        var touchListener = new CCEventListenerTouchAllAtOnce ();
        touchListener.OnTouchesEnded = (touches, ccevent) => Window.DefaultDirector.ReplaceScene (GameLayer.GameScene (Window));

        AddEventListener (touchListener, this);

        Color = CCColor3B.Black;
        Opacity = 255;
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        Scene.SceneResolutionPolicy = CCSceneResolutionPolicy.ShowAll;

        var label = new CCLabelTtf ("Tap Screen to Go Bananas!", "arial", 22) {
            Position = VisibleBoundsWorldspace.Center,
            Color = CCColor3B.Green,
            HorizontalAlignment = CCTextAlignment.Center,
            VerticalAlignment = CCVerticalTextAlignment.Center,
            AnchorPoint = CCPoint.AnchorMiddle,
            Dimensions = ContentSize
        };

        AddChild (label);
    }

    public static CCScene GameStartLayerScene (CCWindow mainWindow)
    {
        var scene = new CCScene (mainWindow);
        var layer = new GameStartLayer ();

        scene.AddChild (layer);

        return scene;
    }
}
```
我们使用的是CCLayerColor作为基类所以我们不能对这个layer（层）的背景颜色进行更改。这段代码展示了一个label和一个到GameLayer的事件如果玩家点击了屏幕。label用的arial字体在font文件夹里包含在我们直接拷贝进项目Content文件夹中。

下面是运行后的截图
![GoneBananas](pic/GameStart.png?raw=true)

#过度到GameLayer场景
调用Window.DefaultDirector.ReplaceScene，这个方法需要一个scene（场景）来过渡到另外一个场景 。在这个游戏中我们是这样使用的
```csharp
Window.DefaultDirector.ReplaceScene (GameLayer.GameScene (Window));
```
