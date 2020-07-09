# Android - 屏幕 - 适配

## 目录

1. 术语和概念
2. Drawable文件夹
3. 刘海屏/凹凸屏/水滴屏
4. 挖孔屏
5. 折叠屏
6. 全面屏
7. 模拟各种分辨率的屏幕
8. 今日头条适配方案
9. 宽高限定符适配
10. 最小宽度适配



3. 密度独立dp（相对像素）
4. 屏幕适配方案
5. 声明屏幕尺寸支持
6. 资源配置限定符
7. 适配平板电脑
8. 在多屏幕上测试应用
9. 屏幕兼容模式
10. “全面屏”18:9适配
11. APP启动图标
12. 屏幕分辨率
13. 



## 术语和概念

1. #### 基准屏幕

   也叫标准屏幕。即屏幕大小：320×480，像素密度160dpi，对应mdpi。在这个基准屏幕里，1dp=1px。一个相对像素就等于一个实际像素。后面说到的屏幕适配就是根据相对于基准屏幕的大小来进行相应的放大或者缩小。

2. #### 屏幕大小

   所谓的大小就是屏幕对角线的距离，单位英寸。也就是常说的4.5英寸、5.5英寸的屏幕。

3. #### 分辨率/像素密度dpi

（1）**分辨率**就是指屏幕宽和高方向上分别有多少个像素。比如1920*1080的屏幕分辨率指的就是在屏幕高度上有1920个像素，在宽度上有1080个像素。

（2）**像素密度**是指每英寸里的像素个数。**单位是dpi**(dot per inch)。

（3）像素密度的计算方式：

​			1）勾股定理计算出对角线上的像素总数；

​			2）像素总数除以屏幕尺寸；

​		**像素密度=√{（长度像素数^2+宽度像素数^2）}/ 屏幕尺寸**

​		比如1920*1080像素的5寸手机的就是469ppi

​				√（1920^2+1080^2）=2202.9071

​				2202.9/5=468.7021（ppi)≈469ppi

（4）上面公式计算出来的ppi，现在手机标注的屏幕参数也是多少ppi。那么ppi和dpi如何转换呢？简单来说，可以通用，你就把ppi当成dpi来用就可以了。因为当在屏幕上显示的时候，这两者可以通用，当要打印出来的时候就使用dpi就可以了。

（5）根据分辨率的不同，也就是dpi的高低，使得在开始时出现了ldpi      mdpi hdpi xhdpi xxhdpi xxxhdpi等针对多种像素密度的文件夹用来存放对应的icon。具体有什么不同，在后面会介绍。



放大像素sp

主要用于字体的显示，他会根据像素密度来放大或者缩 写字体。

#### 1.5 像素px

也就是像素，也是长度单位，但不推荐使用。

#### 1.6 相对像素dp

（1）假设低分辨率手机1英寸有10个像素，高分辨率手机1英寸有100个像素，我们有一个ImageView希望在低分辨率和高分辨率上都能显示为1英寸*1英寸的大小。我们知道不同像素密度的手机，每英寸上的像素个数是不一样的。如果要保证大小一致，那么就需要高分辨率上这个ImageView占用100*100个像素，第分辨率上占用10*10个像素。

（2）我们肯定无法在代码中直接设定ImageView在运行设备上的所占像素大小，因为我们无法得知设备的像素密度是多少。不用担心，Android系统提供了相对像素的概念，帮我们自动转换为运行设备上真实像素个数。



## Drawable文件夹

1. #### drawable文件夹的分类标准

   我们在新建Module的时候，res文件夹下就会默认生产一些drawable文件夹。这些文件夹都以dpi结尾。当我们的APP运行在不同设备上的时候，设备就会根据当前屏幕像素密度属于哪一个范围，然后选择对应dpi下的drawable文件夹中的资源文件。

   下图列出了不同dpi对应的Drawable文件夹，以及对应的比例关系：

   | **系统dpi范围** | **代表设备分辨率** | **对应drawable文件夹** | **基准比例** | **宽高比** | **宽高比** |
   | --------------- | ------------------ | ---------------------- | ------------ | ---------- | ---------- |
   | <120            | 240*320            | drawable-ldpi          | 0.75         | 0.75       | 3/4        |
   | 120～160        | 320*480            | drawable-mdpi          | 1            | 0.6666     | 2/3        |
   | 160~240         | 480*800            | drawable-hdpi          | 1.5          | 0.6        | 3/5        |
   | 240~320         | 720*1280           | drawable-xhdpi         | 2            | 0.5625     | 9/16       |
   | 320~480         | 1080*1920          | drawable-xxhdpi        | 3            | 0.5625     | 9/16       |
   | 480~640         | 2160*3840          | drawable-xxxhdpi       | 4            | 0.5625     | 9/16       |

2. #### 系统是如何选择对应文件夹？

   下面以一部hdpi手机上加载一张drawable下的图片的选择过程说起：

   （1）先去drawable-hdpi下寻找，如果找到了就不缩放直接加载。

   （2）如果drawable-hdpi下没有，那么就向更高分辨率的文件夹下寻找：drawable-xhdpi文件夹、drawable-xxhdpi 文件夹、 drawable-xxxhdpi 文件夹、drawable-nodpi     文件夹中寻找。如果找到了，就根据所在文件夹与drawable-hdpi的比例关系，进行缩小，然后展示到屏幕上。

   （3）如果更高密度的文件夹里都没有找到，就往更低密度的文件夹drawable-ldpi 文件夹下查找。如果找到图片资源就进行缩放处理(放大)，显示在屏幕上

   （4）如果都没找到，最终会在默认的drawable文件夹中寻找，如果默认的drawable文件夹中也没有那就会报错。

3. #### Drawable的缩放规则

4. - 如果drawable的dpi与设备dpi一致，那么不进行缩放。
   - 如果drawable的dpi与设备dpi不一致，就根据比例关系，进行缩放。
   - 如果图片所在目录为      drawable-nodpi，则无论设备 dpi 为多少，保留原图片大小，不进行缩放。

5. 

## 刘海屏/凹凸屏/水滴屏

1. #### 刘海屏和水滴屏

   * 刘海屏是指在屏幕顶部被挖出一个空缺，用来安放前置摄像头以及传感器，因看上去像人的刘海而得名。刘海屏也叫凹凸屏，英文Cutout。典型的代表iPhoneX。
   * 水滴屏相对于刘海屏来说，额头更小，只放置了一个前置摄像头。就像一个水滴💧一样，所以称为水滴屏。

   无论是刘海屏还是水滴屏，我们统一称之为凹凸屏。

2. #### 模拟凹凸屏

   Google在开发者选项中提供了4种模拟凹凸屏的方式：

   * 

3. #### Android P ( 9.0 )的对凹凸屏的规则

   **如果不适配会怎么样？**

   当我们的APP没有适配 P 的凹凸屏时，会有如下两种情况：

   * 全屏页面在通知栏的位置变成黑色，不显示任务内容，包括原通知栏上的电池电量时钟等信息。
   * 如果有状态栏的App，则不受刘海屏的影响，和以前一样。

   主要的影响就是原本全屏的页面，凹凸点的位置多出了一条黑边，导致不能全屏展示。

4. #### 凹凸屏要不要适配？

   首先要说的一点，不适配在大多数情况下也能行。只要你能说服产品、说服设计、说服老板，只要他们同意，咱们干嘛费这个劲？

   唯一的影响就是在Android 9 上，全屏页面会出现黑边。而且某些厂商的手机，在这种页面会弹出一个“全屏展示”的按钮，用户点击后，会强制把你的应用全屏，有可能导致某些内容被刘海挡住。

5. #### 如何适配？

   适配方案也很简单：

   * 不要出现黑边，让全屏的内容显示到凹凸区域内，实现真正的全屏；
   * 将可能被挡住的内容下移，空出凹凸区域；

   那对于开发来说，要做的就是让全屏页面的内容，显示到凹凸区域内。大致分为两种方案：Android 9 适配、厂商适配。

6. #### Android 9 适配

   Android 9.0系统中提供了3种layoutInDisplayCutoutMode属性来允许应用自主决定该如何对刘海屏设备进行适配。

   * **LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT**：默认的属性。这种属性下允许应用程序的内容在竖屏模式下自动延伸到刘海区域，而在横屏/全屏模式下则不会延伸到刘海区域，取而代之的是刘海区域变成一个黑条。

   * **LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES**：这种属性表示，不管手机处于横屏还是竖屏模式，都会允许应用程序的内容延伸到刘海区域。

   * **LAYOUT_IN_DISPLAY_CUTOUT_MODE_NEVER**：这种属性表示，永远不允许应用程序的内容延伸到刘海区域。

   **设置方法**

   ```java
   if (Build.VERSION.SDK_INT >= 28) {
     WindowManager.LayoutParams params = getWindow().getAttributes();
     params.layoutInDisplayCutoutMode = WindowManager.LayoutParams.LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES;
     getWindow().setAttributes(params);
   }
   ```

   

   **获取安全区域**

   凹凸屏刘海的位置是个危险区域，当我们设置为占满刘海区域，但是刚好刘海位置有一个按钮，就会导致按钮被盖住。

   添加如下代码：

   ```java
   if (Build.VERSION.SDK_INT >= 28) {
     rootLayout.setOnApplyWindowInsetsListener(new View.OnApplyWindowInsetsListener() {
         @Override
         public WindowInsets onApplyWindowInsets(View view, WindowInsets windowInsets) {
           DisplayCutout displayCutout = windowInsets.getDisplayCutout();
           if (displayCutout != null) {
             // 通过下面代码拿到刘海位置的坐标信息
             int left = displayCutout.getSafeInsetLeft();
             int top = displayCutout.getSafeInsetTop();
             int right = displayCutout.getSafeInsetRight();
             int bottom = displayCutout.getSafeInsetBottom();
             // 拿到坐标信息后，对于被遮盖的按钮，进行位置修正
             FrameLayout.LayoutParams topParams = (FrameLayout.LayoutParams) topButton.getLayoutParams();
             topParams.setMargins(left, top, right, bottom);
             FrameLayout.LayoutParams sideParams = (FrameLayout.LayoutParams) sideButton.getLayoutParams();
             sideParams.setMargins(left, top, right, bottom);
           }
           return windowInsets.consumeSystemWindowInsets();
         }
     });
   }
   ```

   



[Android P版本 （六）刘海屏适配 与WindowInsets，DisplayCutout使用](https://blog.csdn.net/yi_master/article/details/80309757)

[简洁明了的刘海屏适配方案](https://www.jianshu.com/p/c9e710a9fa35)

[Android 9.0系统新特性，对刘海屏设备进行适配](https://blog.csdn.net/sinyu890807/article/details/103112795)





# 声明屏幕尺寸支持

1. 在清单AndroidManifest中加入限定标签，如下：

<manifest ... >

<supports-screens android:requiresSmallestWidthDp="600" />

...

</manifest>

1. 全部参数说明：

![android: resizeable= ["true" "false"]  smallScreens= ["true" I "false"]  android:  I "false"]  android:  • largeScreens= ["true" I "false"]  android.  xlargeScreens= ["true" I "false"]  android:  anyDensity= ["true" I "false"]  android:  and ro id: requi resSma I lestWidthDp=" integer"  and roid: compat ib leWidthLimitDp=" integer"  and roid: la rgestWidthLimitDp=" integer" /> ](file:////Users/zhangguiyou/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/78BA411F-ACB9-2D46-B33E-1E8B47E379D4.png)

- android:smallScreens：是否支持小屏；
- android:normalScreens：是否支持正常屏幕；
- android:largeScreens：
- android:xlargeScreens
- android:anyDensity：是否适配所有像素密度的设备。在1.6（API4）以后，默认是true。
- android:requiresSmallestWidthDp：指定需要的最小      smallestWidth，单位是dp（在设置的时候无需带dp，因为只有这一个单位）。无论屏幕的方向如何，这个值是屏幕两个尺寸中最短的那个。
- android:compatibleWidthLimitDp：指定最大“最小宽度”。当超过这个尺寸的时候，将屏幕兼容模式作为用户的选项。用户可以进入屏幕兼容模式，也可以继续在当前模式下运行。
- android:largestWidthLimitDp：指定最大“最小宽度”，当超过这个尺寸，强制进如屏幕兼容模式，用户无法停止。



# 资源限定符和替代资源

1. 在res新建文件夹的时候遵循这个格式： <resources_name>-<qualifier>

其中<resources_name>是你的文件夹名，<qualifier>就是上面说到的各种密度mdpi、hdpi、xhdpi等。比如drawable-xhdpi

1. 限定符的取值如下：

| 尺寸   | small   | 适用于*小*尺寸屏幕的资源。                                   |
| ------ | ------- | ------------------------------------------------------------ |
|        | normal  | 适用于*正常*尺寸屏幕的资源。（这是基线尺寸。）               |
|        | large   | 适用于*大*尺寸屏幕的资源。                                   |
|        | xlarge  | 适用于*超大*尺寸屏幕的资源。                                 |
| 密度   | ldpi    | 适用于低密度   (*ldpi*) 屏幕 (~120dpi) 的资源。              |
|        | mdpi    | 适用于中密度   (*mdpi*) 屏幕 (~160dpi) 的资源。（这是基线 密度。） |
|        | hdpi    | 适用于高密度   (*hdpi*) 屏幕 (~240dpi) 的资源。              |
|        | xhdpi   | 适用于超高密度   (*xhdpi*) 屏幕 (~320dpi) 的资源。           |
|        | xxhdpi  | 适用于超超高密度   (*xxhdpi*) 屏幕 (~480dpi) 的资源。        |
|        | xxxhdpi | 适用于超超超高密度   (*xxxhdpi*) 屏幕 (~640dpi) 的资源。此限定符仅适用于   启动器图标，请参阅上面的[注](https://developer.android.com/guide/practices/screens_support.html?hl=zh-cn#xxxhdpi-note)。 |
|        | nodpi   | 适用于所有密度的资源。这些是密度独立的资源。不管当前屏幕的密度如何，系统都不会   缩放以此限定符标记的资源。 |
|        | tvdpi   | 适用于密度介于   mdpi 和 hdpi 之间屏幕（约为 213dpi）的资源。它并不是 “主要”密度组，主要用于电视，而大多数应用都不 需要它 —   对于大多数应用而言，提供 mdpi 和 hdpi 资源便已足够，系统将根据需要对其进行 缩放。如果发现必须提供 tvdpi 资源，应以   1.33*mdpi 的系数 调整其大小。例如，mdpi 屏幕的 100px x 100px 图像应该相当于 tvdpi 的 133px x 133px。 |
| 方向   | land    | 适用于横屏（长宽比）的资源。                                 |
|        | port    | 适用于竖屏（高宽比）的资源。                                 |
| 纵横比 | long    | 适用于纵横比明显高于或宽于（分别在竖屏   或横屏时）基线屏幕配置的屏幕的资源。 |
|        | notlong | 适用于使用纵横比类似于基线屏幕   配置的屏幕的资源。          |

1. 替代布局

为了更精确的适配各种屏幕，我们就可以在上面定义的限定文件夹中放入不同屏幕布局。

1. 替代可绘制对象

2. 1. 和替代可绘制布局类似，需要放到对应的限定文件夹中。
   2. 如果您使用 XML       文件定义形状、颜色或其他[可绘制对象资源](https://developer.android.com/guide/topics/resources/drawable-resource.html)，应该       将一个副本放在默认可绘制对象目录中 (drawable/)。



# 适配平板电脑

平板电脑的适配主要是使用尺寸限定符，和上面的资源限定符差不多。

主要有三类限定符：（直接从官网上复制下来）

| smallestWidth | sw<N>dp       示例：   sw600dp   sw720dp      | 屏幕的基本尺寸，由可用屏幕区域的最小尺寸指定。 具体来说，设备的 smallestWidth   是屏幕可用高度和宽度的最小尺寸（您也可以将其视为屏幕的“最小可能宽度”）。无论屏幕的当前方向如何，您均可使用此限定符确保应用 UI   的可用宽度至少为 <N>dp。   例如，如果布局要求屏幕区域的最小尺寸始终至少为 600 dp，则可使用此限定符创建布局资源 res/layout-sw600dp/。仅当可用屏幕的最小尺寸至少为 600dp 时，系统才会使用这些资源，而不考虑 600dp   所代表的边是用户所认为的高度还是宽度。smallestWidth 是设备的固定屏幕尺寸特性；**设备的 smallestWidth 不会随屏幕方向的变化而改变**。   设备的 smallestWidth 将屏幕装饰元素和系统 UI 考虑在内。例如，如果设备的屏幕上有一些永久性 UI   元素占据沿 smallestWidth 轴的空间，则系统会声明 smallestWidth 小于实际屏幕尺寸，因为这些屏幕像素不适用于您的 UI。   这可替代通用化的屏幕尺寸限定符（小、正常、大、超大），   可让您为 UI 可用的有效尺寸定义不连续的数值。 使用 smallestWidth 定义一般屏幕尺寸很有用，因为宽度 通常是设计布局时的驱动因素。UI   经常会垂直滚动，但 对其水平需要的最小空间具有非常硬性的限制。可用的宽度也是 确定是否对手机使用单窗格布局或是对平板电脑使用多窗格布局   的关键因素。因此，您可能最关注每部 设备上的最小可能宽度。 |
| ------------- | --------------------------------------------- | ------------------------------------------------------------ |
| 可用屏幕宽度  | w<N>dp       示例：   w720dp   w1024dp        | 指定资源应该使用的最小可用宽度（dp 单位） — 由 <N> 值定义。**当屏幕的方向在横屏与竖屏之间切换时，系统对应的 宽度值将会变化**，以 反映 UI 可用的当前实际宽度。   这对于确定是否使用多窗格布局往往很有用，因为即使是在   平板电脑设备上，您也通常不希望竖屏像横屏一样 使用多窗格布局。因此，您可以使用此功能指定布局需要的最小宽度，而 无需同时使用屏幕尺寸和方向限定符。 |
| 可用屏幕高度  | h<N>dp       示例：   h720dp   h1024dp   等等 | 指定资源应该使用的最小屏幕高度（dp 单位） — 由 <N> 值定义。**当屏幕的方向在横屏与竖屏之间切换时，系统 对应的高度值将会变化**，以 反映 UI 可用的当前实际高度。   使用此方式定义 布局需要的高度很有用，它与使用 w<N>dp 定义   所需宽度的方式相同，无需同时使用屏幕尺寸和方向限定符。 但大多数应用不需要此限定符，考虑到 UI 经常垂直滚动， 因此高度更弹性，而宽度更刚性。 |





# 在多屏幕上测试应用

1. Android      SDK中已经包含了模拟各种尺寸和密度的模拟器。无需为了测试而购买不同的设备。也就是说，你完全可以新建多个AVD模拟器来模拟各种情景。
2. 在使用模拟器的时候，需要先设置你当前电脑屏幕的密度，这样才能更准确。



# 屏幕兼容模式



# 全面屏18:9适配

1. 正常的屏幕都是16比9，但是在2017年一大堆厂商发布了18比9的屏幕。姑且不论什么叫全面屏，只说下常见的18比9，2160*1080的屏幕。
2. 这种屏幕最大的问题就是全屏展示图片时可能会拉伸，比如启动页的背景图。
3. 解决办法就是一种是按照16比9来设计，但是两边预留一定空间不要放置重要信息。另一种是加入限定符，drawable-xxhdpi-2160x1080的文件夹。



# APP启动图标

[官方教程](https://developer.android.com/guide/practices/ui_guidelines/icon_design_adaptive.html)

1. Android 8.0可以根据不同的设备类型去生产各种形状的自适应的启动图标。比如可以显示为原型、圆角矩形、方形等。
2. 实现原理。这里可以参照官网的介绍，其实就是在launch图标上面加上了一层面具。当盖上原型的面具，就显示为原型启动图标。圆角矩形的面具mask就是圆角矩形的启动图标。

1. 默认的APP Luncher图标大小

我们在Android Studio新建一个工程，默认的图标大小如下，（单位是像素，当在屏幕上显示时效果等同于48dp*48dp）

- mdpi：48*48
- hdpi：72*72
- xhdpi：96*96
- xxhdpi：144*144
- xxxhdpi：192*192

1. 在Android7.0 API25以前的版本，启动图标是48dp*48dp的大小（注意这里的单位是dp相对像素，和icon的单位像素是有区别的）。也就是说在mdpi里面要放一个48*48像素的图标，其他文件夹下按照比例放大即可。

2. 在Android8.0      API26，启动图标需要分为两层：foreground和background，分别对应背景图和前景图。背景图就是背景色，前景一般才是自己的icon。

3. 定义方法：

4. 1. 在res/mipmap-anydpi/下创建一个ic_launcher.xml文件。
   2. 代码如下，分别定义前景和背景

<adaptive-icon>

<background android:drawable="@color/ic_background"/>

<foreground android:drawable="@mipmap/ic_foreground"/>

</adaptive-icon>

1. 然后在Manifest文件的application中定义icon为刚才的ic_launcher。



# 屏幕分辨率

截止至2020年5月27日

| **机型**    | **屏幕尺寸** | **分辨率** | **宽高比**    | **dpi** | **ppi** |
| ----------- | ------------ | ---------- | ------------- | ------- | ------- |
| 华为mate30  | 6.52         | 2340x1080  | 19.5/9   13/6 |         |         |
| mate20      | 6.53         | 2244x1080  |               |         | 381     |
| 华为P30 Pro | 6.47         | 2340x1080  | 19.5/9        |         | 398     |
| 华为P40     | 6.58         | 2640x1200  | 19.8/9   11/5 |         | 441     |
| 华为nova7   | 6.57         | 2340x1080  | 19.5/9        |         |         |
| 荣耀30s     | 6.5          | 2400x1080  | 20/9          |         | 405     |
| 小米10      | 6.67         | 2340x1080  | 19.5/9        |         | 386     |
| 红米k30     | 6.67         | 2400x1080  | 20/9          |         | 395     |
| 一加8pro    | 6.78         | 3168x1440  | 19.8/9        |         | 513     |
| 一加7       | 6.55         | 2400x1080  | 20/9          |         | 402     |
| iphone 11   | 6.1          | 1792x828   | 19.5/9        |         | 326     |
| IQOO Neo3   | 6.57         | 2408x1080  | 20/9          |         | 400     |







# 今日头条适配方案

[一种极低成本的Android屏幕适配方式](https://mp.weixin.qq.com/s/d9QCoBP6kV9VSWvVldVVwA)（头条官方公众号）

[Android  屏幕适配从未如斯简单（8月10日最终更新版）](https://blankj.com/2018/07/30/easy-adapt-screen/)

1. 背景

上面所讲到的适配方案，可以保证让单个view在不同设备上大小显示的一致。但是这还远远不够。因为整体的显示效果还是会因设备而异了，不能保证和设计图一模一样。主要是布局的宽高比、view之间的间距等，都会因为设备不同而不一样。

1. 要解决的问题

假设设计图是一个640*360（4:3）的大小。设计图看起来很完美。那么考虑以下问题：

- 比 4:3      要矮胖的屏幕，怎么显示？
- 比 4:3      要修长的怎么显示？

或者说更具体一些。一张 4:3 的照片（代表原设计图），在矮胖的屏幕和修长屏幕上上是centerInside还是centerCrop？

1. 基准维度选择

2. 1. 可以上下滑动的页面

以宽度为基准不变，去适配高度

1. 不能上下滑的页面

以高度为基准不变，去适配宽度



# 宽高限定符适配（屏幕限定符适配）

1. 使用方法

2. 1. 添加一堆不同宽高的文件夹

在res下穷举出几乎所有的分辨率，如下图

![img](file:////Users/zhangguiyou/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/E96ED22C-EE60-924A-BEB2-1FA38C45768E.png)

1. 添加x和y轴上各种长度对应的px值

每个values-***x***下添加lay_x.xml和lay_y.xml。内容如下

![img](file:////Users/zhangguiyou/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/3A9A7FAD-EC9E-9E42-88F4-DADCBB9CC836.png)

在lay_x下添加如上图：<dimen name="x1">0.66px</dimen>的内容。

在lay_y下添加如：<dimen name="y1">0.66px</dimen>的内容。

（具体要添加多少，每个的值怎么算，后面会讲）

1. 设定一个基准屏幕

一般是以设计图为准。比如设计图的宽度为1280x720，那么就以此为基准屏幕。找到values-1280x720文件夹。在lay_x.xml文件夹中，创建720个dimen item：

<dimen name="x1">1px</dimen>

<dimen name="x2">2px</dimen>

...

<dimen name="x720">720px</dimen>

Y轴同理。

1. 换算出其它分辨率下的值

第一个例子，1100x1080文件夹下的值怎么算。以宽度x轴为例

1. 该文件夹下和基准屏幕放置一样多的dimen。也是从x1到x720。
2. 最大值x720的值就是当前屏幕最大的值，也就是

<dimen name="x720">1080px</dimen>

1. 1100x1080其余的值就按照比例 ratio = 1080/720，进行换算。

公式：**目标值** **= 基准值 \* 当前屏幕最大值 / 基准屏幕最大值**

如：x719的值就是 719 * 1080 / 720 = 1078.5 

<dimen name="x719">1078px</dimen>

1. 布局使用设定的dimen值

比如设计图上有一张40*40的图片。那么放置一个ImageView的时候，宽高如下

宽设为android:layout_width="@dimen/x40"，

高设为：android:layout_height="@dimen/y40"

1. 适配原理

2. 1. 不需要我们自己考虑dpi了，系统会根据设备当前的分辨率自动选择合适的dimen值。

3. 优缺点

4. 1. 侵入性比较高，需要开发人员手动修改控件尺寸，不容易把握。
   2. 有地方都建议统一使用px’，包括字体、自定义控件等，否则就会不兼容。比如图很小，但是字很大。
   3. 如果字也使用了px，那么字体大小就脱离了系统字体大小的限制。
   4. 换算过程存在小数点，四舍五入后有误差。



# 最小宽度适配



### 参考链接

[Android P版本 （六）刘海屏适配 与WindowInsets，DisplayCutout使用](https://blog.csdn.net/yi_master/article/details/80309757)

[简洁明了的刘海屏适配方案](https://www.jianshu.com/p/c9e710a9fa35)

[Android 9.0系统新特性，对刘海屏设备进行适配](https://blog.csdn.net/sinyu890807/article/details/103112795)