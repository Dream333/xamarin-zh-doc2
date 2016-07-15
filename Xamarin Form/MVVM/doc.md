#Data Binding（数据绑定）到MVVM
>一个到Model(模型）-View（视图）-ViewModel（视图模型）架构的介绍。

Model-View-ViewModel (MVVM) 架构搭建时是考虑过XAML的。这个模式加强XAML用户界面（视图） 和底部数据（模型）的分离，通过一个视图和模型的中间类（视图模型）。视图和视图模型一般是通过数据绑定来连接起来；数据绑定一般是在XAML里定义。视图的BindingContext一般都是一个ViewModel的事例（instance).

##一个简单的视图模型
在这个是视图模型里的解释，我们先来看一个没有它的程序。在前面的教程中，你看见了如果定义一个新的XML命名空间来允许一个XAML文件引用其他程序集。以下是一个定义XML命名空间来引用System明明空间。
```xml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```
这个程序然后使用x:Static来获得当前日期和时间从静态的Datatime.Now属性中然后把DataTime值设为StackLayout的BindingContext:

```xml
<StackLayout BindingContext="{x:Static sys:DateTime.Now}" …>
```
BindingContext 是一个很特殊的属性；它会被所有子元素继承。这就是说所有StackLayout的自元素都会有一样的BindingContext，并且他们都已包含简单的绑定（bingdings）到父BindingContext的属性。

在这个程序中，两个子元素绑定到了DateTime的属性，但是其他的两个自元素从表面上确实一个绑定路径。这个其实是DateTime本身被绑定并且被StringFormat处理：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="XamlSamples.OneShotDateTimePage"
             Title="One-Shot DateTime Page">

  <StackLayout BindingContext="{x:Static sys:DateTime.Now}"
               HorizontalOptions="Center"
               VerticalOptions="Center">

    <Label Text="{Binding Year, StringFormat='The year is {0}'}" />
    <Label Text="{Binding StringFormat='The month is {0:MMMM}'}" />
    <Label Text="{Binding Day, StringFormat='The day is {0}'}" />
    <Label Text="{Binding StringFormat='The time is {0:T}'}" />

  </StackLayout>
</ContentPage>
```
当然，最大的问题是个页面的日期和时间只会被设置一次，当这个页面编译完后，他们是不会改变的。
![](https://developer.xamarin.com/guides/xamarin-forms/xaml/xaml-basics/data_bindings_to_mvvm/Images/OneShotDateTime-large.png)

一个XAML文件可以显示一个能表达当前时间的钟但是需要一些代码来帮助一下。在MVVM模式下思考，模式和视图模式是全部需要代码实现的。视图是一般用一个引用其他在视图模型定义的属性的XAML文件，通过数据绑定。

一个正确的模型是应该对视图模型不知情的，并且一个正确的视图模型是对视图不知情的。 但是，一般来说，程序员会定制视图模型的数据结构对不同的UI。举个例子，如果一个模型访问一个包含8-bit ASCII码字符的数据库，视图模型将会把这些字符给专程Unicode来让只能读Unicode的UI显示和使用。

在一个简单的MVVM例子（就像这里的），很多时候没有一个模型，这个模式之包含一个视图和一个视图模型用数据绑定而俩节。

这是一个为一个时钟设计的数据模型拥有只有一个属性dateTime。 这个数据模型每一秒更新一次dateTime属性。

```csharp
using System;
using System.ComponentModel;
using Xamarin.Forms;

namespace XamlSamples
{
    class ClockViewModel : INotifyPropertyChanged
    {
        DateTime dateTime;

        public event PropertyChangedEventHandler PropertyChanged;

        public ClockViewModel()
        {
            this.DateTime = DateTime.Now;

            Device.StartTimer(TimeSpan.FromSeconds(1), () =>
                {
                    this.DateTime = DateTime.Now;
                    return true;
                });
        }

        public DateTime DateTime
        {
            set
            {
                if (dateTime != value)
                {
                    dateTime = value;

                    if (PropertyChanged != null)
                    {
                        PropertyChanged(this,
                            new PropertyChangedEventArgs("DateTime"));
                    }
                }
            }
            get
            {
                return dateTime;
            }
        }
    }
}
```
视图模型一般实现INotifyPropertyChanged接口。这些类激发一个INotifyPropertyChanged当他们的一个属性改变。Xamarin.Forms的数据绑定机制为PropertyChanged附上一个句柄，通知XAML如果一个属性被改变，并且更新新的值。

一个钟基于这个视图模型可以这么简单。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ClockPage"
             Title="Clock Page">

  <Label Text="{Binding DateTime,
                        StringFormat='{0:T}'}"
         FontSize="Large"
         HorizontalOptions="Center"
         VerticalOptions="Center">
    <Label.BindingContext>
      <local:ClockViewModel />
    </Label.BindingContext>
  </Label>
</ContentPage>
```
注意ClockViewModel被设成了Label的BindingContext使用属性元素标签。或者，你可以具现化一个ClockViewModel在一个Resource集合里面并且设置这个到BindingContext使用一个StaticResource扩展。或者这个code-behind文件可以具现化视图模型，

这个Binding在Label的text属性正确的格式DateTime属性。
![](https://developer.xamarin.com/guides/xamarin-forms/xaml/xaml-basics/data_bindings_to_mvvm/Images/Clock-large.png)
当然，你也可以直接访问DateTime的单独属性，直接在DateTime后面加一个“."连接单独的属性。

```xml
<Label Text="{Binding DateTime.Second,
                        StringFormat='{0}'}" …>
```


