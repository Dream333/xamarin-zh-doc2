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

8. 双击Phoneword 页面的导航栏，把名字给改成“Phoneword".![change name](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/09.png)

9. 从Toolbox里面拖动一个Button(按钮)，放在Call Button底下，把它改改成和Call Button 一样的宽度。![change name](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/10New.png)
10. 在Properties Pad里，把Name给成CallHistoryButton然后把Title改成”Call History“。![history button](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/11New.png)
11. 然后，从Toolbox里面，拖出一个 Table View Controller 到设计空间里面。
![table view controller](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/12New.png)
12. 点击Table View Controller 底部黑条来选择。在Properies Pad里面，改变 Table View Controller 的class 到 CallHistoryController然后按下Enter。
![name class](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/13New.png)
iOS 设计器将会生成一个后台的类叫做CallHistoryController来管理这个页面的内容视图等级。 CallHistoryController.cs 文件会在Solution Pad内出现。
![name class](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/14New.png)
13. 双击CallHistoryController.cs, 换成下面的代码。
```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace Phoneword_iOS
{
    public partial class CallHistoryController : UITableViewController
    {
        public List<String> PhoneNumbers { get; set; }

        static NSString callHistoryCellId = new NSString ("CallHistoryCell");

        public CallHistoryController (IntPtr handle) : base (handle)
        {
            TableView.RegisterClassForCellReuse (typeof(UITableViewCell), callHistoryCellId);
            TableView.Source = new CallHistoryDataSource (this);
            PhoneNumbers = new List<string> ();
        }

        class CallHistoryDataSource : UITableViewSource
        {
            CallHistoryController controller;

            public CallHistoryDataSource (CallHistoryController controller)
            {
                this.controller = controller;
            }

            // Returns the number of rows in each section of the table
            public override nint RowsInSection (UITableView tableView, nint section)
            {
                return controller.PhoneNumbers.Count;
            }
            //
            // Returns a table cell for the row indicated by row property of the NSIndexPath
            // This method is called multiple times to populate each row of the table.
            // The method automatically uses cells that have scrolled off the screen or creates new ones as necessary.
            //
            public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
            {
                var cell = tableView.DequeueReusableCell (CallHistoryController.callHistoryCellId);

                int row = indexPath.Row;
                cell.TextLabel.Text = controller.PhoneNumbers [row];
                return cell;
            }
        }
    }
}
```

14. 创建一个Segue(跳转)连接Phoneword 场景和 Call History
场景。 选择上Call Button， Ctrl拖动到Call History场景。![Callbutton](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/15.png)
在跳出窗口里面选择Show。
![segue](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/16New.png)
iOS设计器会创建一个Segue在两个场景之间。
![segue](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/17New.png)
15. 接下来，我们选中Table View Controller. 然后把Title改成Call History。![CallHistory](https://developer.xamarin.com/guides/ios/getting_started/hello,_iOS_multiscreen/hello,_iOS_multiscreen_quickstart/Images/18New.png
)
16. 如果我们现在运行程序，Call History Button 会打开 Call History 页面， 但是Table View将会是空的，因为我们还没有加入代码来记录电话号码。 那我们来加入那个功能吧。

首先我们需要一个地方来存储打过的号码。 我们将会把它们以一个List<string>存到PhoneNumbers里。我们需要把

```csharp
using System.Collections.Generic;
```
加到 View Controller的指令集中。

17. 接下来改变ViewController的以下代码。
```csharp
namespace Phoneword_iOS
{
    public partial class ViewController : UIViewController
    {
        // translatedNumber was moved here from ViewDidLoad ()
        string translatedNumber = "";

        public List<String> PhoneNumbers { get; set; }

        public ViewController (IntPtr handle) : base (handle)
        {
            //initialize list of phone numbers called for Call History screen
            PhoneNumbers = new List<String> ();

        }
        // ViewDidLoad, etc.
    }
}
```
注意，我们把translatedNumber给变成了一个类级别的变量。

18. 接下来，我们修改 CallButton 代码来把打过的号码加到List里面
```csharp
CallButton.TouchUpInside += (object sender, EventArgs e) => {

                //Store the phone number that we're dialing in PhoneNumbers
                PhoneNumbers.Add (translatedNumber);

                // Use URL handler with tel: prefix to invoke Apple's Phone app...
                var url = new NSUrl ("tel:" + translatedNumber);

                // otherwise show an alert dialog
                if (!UIApplication.SharedApplication.OpenUrl (url)) {
                    var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                    alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                    PresentViewController (alert, true, null);
                }
            };
```
19. 最终把这段代码加到ViewController里面。
```csharp
ublic override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
  {
     base.PrepareForSegue (segue, sender);

     // set the View Controller that’s powering the screen we’re
     // transitioning to

     var callHistoryContoller = segue.DestinationViewController as CallHistoryController;

     //set the Table View Controller’s list of phone numbers to the
     // list of dialed phone numbers

     if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
     }
   }
```
20. 保存和编译，然后运行。你成功了！
