#Asset Catalog图片集

Asset(资源) Catalog(目录) 包含所有图片版本，支持各种个样的设备和放大因素（scale factor)。与其依赖与图片的名字(@2x.@3x)， Image Sets(图像合计）使用一个JSON文件来具体说明哪个图片属于哪个设备或者分辨率。

去创建一个新图像集并加入图片，做以下的步骤。

1. 在Solution Explorer 里，双击Assets.xcassets文件来开始编辑。

![](https://developer.xamarin.com/guides/ios/application_fundamentals/working_with_images/displaying-an-image/Images/ImageSet01.png)

2. 双击 Assets List 然后选择 New Image Set：

![](https://developer.xamarin.com/guides/ios/application_fundamentals/working_with_images/displaying-an-image/Images/ImageSet03.png)

3. 选择新的Image Set，编辑器会出现。

![](https://developer.xamarin.com/guides/ios/application_fundamentals/working_with_images/displaying-an-image/Images/ImageSet03.png)

4. 在这里我们可以把想要的图片为不同分辨率和设备设计的拖入到需要的位置。

5. 在啊Asset List(左栏)，双击新创建的Image Set的名字，来编辑名字。

![](https://developer.xamarin.com/guides/ios/application_fundamentals/working_with_images/displaying-an-image/Images/ImageSet04.png)

在iOS 8 以后， 特殊的矢量(Vector) 类型加到了Image Sets 里面， 允许我们加入PDF 矢量文件到图片集里，而不用加入位图为每个平台单独生成的。 使用这个方式，你可以直接供应一个矢量文件在1x的分辨率，其他的（2x 3x)会自动在编译时生成，加到应用bundle里。

举个例子，如果你加入一个MoneyIcon.pdf 文件作为一个150*150px的矢量图，一下位图资源会编译时包含在最终的APP bundle里面。

* MonkeyIcon@1x.png - 150px x 150px resolution.
* MonkeyIcon@2x.png - 300px x 300px resolution.
* MonkeyIcon@3x.png - 450px x 450px resolution.

但是以下是一些使用PDF矢量需要考虑的地方。

* 这个不是全部的矢量支持因为PDF会在编译时光栅化到一个位图，并且唯一是那个位图会在最终的应用里。
* 只要放到了Asset Catalog里以后，就不可以动态矢量改变大小了，因为如果你改变图像大小（用代码或是Auto Layout）, 这个图片会变型或者失去清晰度就像其他图片一样。
* Asset Catalog只能在iOS7以上程序使用；如果你需要支持iOS 6或更低，你不能使用Asset Catalogs.

当使用一个Image Set在iOS 设计器里，我们可以简单选择这个Set 的名字从下拉单子里。

![](https://developer.xamarin.com/guides/ios/application_fundamentals/working_with_images/displaying-an-image/Images/ImageSet06.png)


当使用代码访问你的Image Set, 你需要使用UIImage的FromBundle静态方法。

`MonkeyImage.Image = UIImage.FromBundle ("PurpleMonkey");`

>如果你的图片显示不出来，确定你使用了正确的名字（Image Set的，不是父Asset Catalog名）Png文件可以不用使用.png文件后缀，但是其他类似jpg必须得使用 (PurpleMonkey.jpg)