# WPF Transparent window can be resized（By dragging the edge of the window）

```
Follows the CC 4.0 BY-SA agreement
By：絮大王 (Xudawang)
(Translated by Google)
```

<br/>

[（中文版本请看这里）](README[CN].md)

<br/>

**With this article we can learn**：

1. Freely adjust the size of WPF transparent window by dragging the edge of the window.

![](Image/00.gif)

<br/>

2. Drag above the transparent window to move the window.

![](Image/19.gif)

<br/>

3. We can prevent the window from maximizing automatically when it is dragged to the edge of the screen.

4. Change the size of the window proportionally.

![](Image/20.gif)

<br/>

5. You can also change the size of controls in the window proportionally

![](Image/23.gif)

<br/>

<br/>

<br/>

<br/>

## Reference

> **AllowsTransparency="True" 怎么放大缩小窗体**
>
> Author：[不灬赖](https://blog.csdn.net/LJianDong)
>
> Link：https://blog.csdn.net/LJianDong/article/details/99844199

<br/>

> **C# WPF 如何禁止窗口拖到屏幕边缘自动最大化**
>
> Author：[Bird鸟人](https://blog.csdn.net/wcc27857285)
>
> Link：https://blog.csdn.net/wcc27857285/article/details/78223901

<br/>

> **WPF窗口比例如何保持恒定？**
>
> Author：[a7066163](https://my.csdn.net/a7066163)、[Hauk](https://my.csdn.net/haukwong)
>
> Link：https://bbs.csdn.net/topics/390257164

<br/>

<br/>

<br/>

<br/>

## Table of Contents

- Create a "transparent window"
- Resize the "transparent window" by dragging
- Move "transparent window" by dragging
- Disable window maximize
- Change the size of the window proportionally
- Proportionally change the size of controls in the window
- Complete code

<br/>

<br/>

<br/>

<br/>

## Create a "transparent window"

"Transparent window" refers to a window whose Window.AllowsTransparency property is true.

But if `Window.AllowsTransparency ="True"`, then `Window.WindowStyle ="None"` must be used.

If `Window.WindowStyle ="None"`, we can no longer change the size of the window by dragging the edge of the window; nor can we move the position of the window by dragging above the window.

Then what should we do?

Let's go step by step, first we need to create a transparent window as a demonstration.

<br/>

**Step 1**: We create a new WPF project.

![](Image/01.png)

<br/>

​			Now the window looks like this ↓

![](Image/02.png)

<br/>

**Step 2**: Set the background of the window to transparent.

![](Image/03.png)

![](Image/04.png)

<br/>

**Step 3**: However, we can see that the background of the window is now black and has not become transparent. Why?

​			  Because we didn't allow windows to be transparent.

​			  We set `AllowsTransparency ="True"` in the Window control to allow the window to become transparent!

​			  But it will report an error when running this way, because if `AllowsTransparency ="True"`, then the WindowStyle property must be None!

​			  We set `WindowStyle = "None"` in the Window control, so that we get a completely transparent window!

![](Image/05.png)

<br/>

​			  Because the window is completely transparent, we cannot see the window at all! (^ _ ^)

![](Image/06.png)

<br/>

**Step 4**: Let's add something to the window (otherwise we can't see anything).

​			  Here we add a white background to the window, give the background a rounded corner, and give the background a nice shadow.

![](Image/07.png)

![](Image/08.png)

<br/>

​			For easy observation, we put the window in a white area.

![](Image/09.png)

<br/>

**Step 5**: We put another button in the window.

​			 The transparent window is OK!

![](Image/10.png)

![](Image/11.png)

<br/>

**Code (MainWindow.xaml)**：

```xaml
<Window x:Class="ResizeTransparentWindow.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:ResizeTransparentWindow"
        mc:Ignorable="d"
        Title="MainWindow" Height="600" Width="800"
        

        Background="Transparent"
        AllowsTransparency="True"
        WindowStyle="None">



    <!--内容（Content）-->
    <Grid>
    
        <!--背景（Background）-->
        <Border Margin="10" Background="White" CornerRadius="25">
            <Border.Effect>
                <DropShadowEffect Direction="0" ShadowDepth="0" BlurRadius="20" 
                                  Opacity="0.25" Color="#FF5B5B5B"></DropShadowEffect>
            </Border.Effect>
        </Border>
    
        <!--按钮（Button）-->
        <Button Width="400" Height="200">
            <TextBlock Width="400" Height="100" TextAlignment="Center"
                       FontSize="70" FontFamily="Source Han Sans CN Medium"
                       Text="Hello"/>
        </Button>
    
    </Grid>

</Window>
```

<br/>

<br/>

<br/>

<br/>

## Resize the "transparent window" by dragging

Now we have no way to resize the transparent window by dragging the edges of the window.

How to do it?

Don't worry, we can use the WindowChrome class. This way we can scale the window while the window is transparent!

```
If you want to learn more about the WindowChrome class, you can check the official Microsoft documentation:
https://docs.microsoft.com/en-us/dotnet/api/system.windows.shell.windowchrome?view=netframework-4.8
```

<br/>

**Step 1**: In the Window control, create a WindowChrome.

![](Image/12.png)

<br/>

**Step 2**: At this point, we can see that after we move the mouse to the edge of the window and drag, we can already change the size of the window!

![](Image/13.gif)

<br/>

**Step 3**: To be more perfect, we can also set the `CaptionHeight property` and` ResizeBorderThickness property` of WindowChrome.

		CaptionHeight property: Set the height of the title bar.
	    ResizeBorderThickness property: Set the width of the "Resize Window" border (if your window has shadows, you can adjust this value a bit larger)
![](Image/14.png)

<br/>

<br/>

**Code (MainWindow.xaml)**：

```xaml
<Window x:Class="ResizeTransparentWindow.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:ResizeTransparentWindow"
        mc:Ignorable="d"
        Title="MainWindow" Height="600" Width="800"
        

        Background="Transparent"
        AllowsTransparency="True"
        WindowStyle="None">


    <!--WindowChrome-->
    <WindowChrome.WindowChrome>
        <WindowChrome CaptionHeight="0" ResizeBorderThickness="20"/>
    </WindowChrome.WindowChrome>


    <!--内容（Content）-->
    <Grid>
    
        <!--背景（Background）-->
        <Border Margin="10" Background="White" CornerRadius="25">
            <Border.Effect>
                <DropShadowEffect Direction="0" ShadowDepth="0" BlurRadius="20" 
                                  Opacity="0.25" Color="#FF5B5B5B"></DropShadowEffect>
            </Border.Effect>
        </Border>
    
        <!--按钮（Button）-->
        <Button Width="400" Height="200">
            <TextBlock Width="400" Height="100" TextAlignment="Center"
                       FontSize="70" FontFamily="Source Han Sans CN Medium"
                       Text="Hello"/>
        </Button>
    
    </Grid>

</Window>
```

<br/>

<br/>

<br/>

<br/>

## Move "transparent window" by dragging

Because we set `WindowStyle ="None"` of the Window control, we have no way to move the window by dragging.

How to do it?

This is especially simple!

We just need to call Window's `DragMove()` method when the mouse is pressed, and that's it!

<br/>

**Step 1**: We create a Border and name it TopBorder.

​			  We hope that when we hold this Border, we can drag the window.

![](Image/15.png)

![](Image/16.png)

<br/>

**Step 2**:Register the MouseLeftButtonDown event of TopBorder.

​			  (When we press the left mouse button on the TopBorder control, this MouseLeftButtonDown event will be triggered)

![](Image/17.png)

<br/>

**Step 3**: We write in MainWindow.cs ↓

```
The DragMove() method of the Window class is used to move the mouse to drag the window after the left mouse button is pressed.
```

![](Image/18.png)

<br/>

**Step 4**: Now we can drag the window at will!

![](Image/19.gif)

<br/>

<br/>

**Code (MainWindow.xaml)**：

```Xaml
<Window x:Class="ResizeTransparentWindow.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:ResizeTransparentWindow"
        mc:Ignorable="d"
        Title="MainWindow" Height="600" Width="800"
        

        Background="Transparent"
        AllowsTransparency="True"
        WindowStyle="None">


    <!--WindowChrome-->
    <WindowChrome.WindowChrome>
        <WindowChrome CaptionHeight="0" ResizeBorderThickness="20"/>
    </WindowChrome.WindowChrome>


    <!--内容（Content）-->
    <Grid>
    
        <!--背景（Background）-->
        <Border Margin="10" Background="White" CornerRadius="25">
            <Border.Effect>
                <DropShadowEffect Direction="0" ShadowDepth="0" BlurRadius="20" 
                                  Opacity="0.25" Color="#FF5B5B5B"></DropShadowEffect>
            </Border.Effect>
        </Border>
    
        <!--按钮（Button）-->
        <Button Width="400" Height="200">
            <TextBlock Width="400" Height="100" TextAlignment="Center"
                       FontSize="70" FontFamily="Source Han Sans CN Medium"
                       Text="Hello"/>
        </Button>
    
        <!--拖拽窗口的区域（Border：For dragging windows）-->
        <Border Name="TopBorder" Width="780" Height="100"
                VerticalAlignment="Top" Margin="0,10,0,0"
                Background="White" Opacity="0" Cursor="Hand"
                
                MouseLeftButtonDown="TopBorder_OnMouseLeftButtonDown"/>
    
    </Grid>

</Window>
```

<br/>

**Code (MainWindow.cs)**：

```c#
using System.Windows;
using System.Windows.Input;


namespace ResizeTransparentWindow
{
    /// <summary>
    /// MainWindow.xaml 的交互逻辑
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }



        /// <summary>
        /// 当用鼠标左键点击TopBorder控件时，触发此方法
        /// (This method is triggered when the TopBorder control is clicked with the left mouse button)
        /// </summary>
        private void TopBorder_OnMouseLeftButtonDown(object sender, MouseButtonEventArgs e)
        {
            /* 当点击拖拽区域的时候，让窗口跟着移动
             (When clicking the drag area, make the window follow) */
            DragMove();
        }
    
    }

}
```

<br/>

<br/>

<br/>

<br/>

## Disable window maximize

Now when our window is dragged to the top of the screen, it will automatically maximize.

Can we not maximize the window?

of course can!

[Bird鸟人](https://blog.csdn.net/wcc27857285) has implemented this function in a very clever way.

```
Link：https://blog.csdn.net/wcc27857285/article/details/78223901
```

<br/>

> **Thinking**:
>
> Because dragging to the edge of the screen automatically maximizes, a necessary condition is that the mouse is pressed down, and then dragged.
>
> We can use this time to set the ResizeMode to NoResize, and then set the ResizeMode back to CanResize after releasing the mouse.

<br/>

<br/>

**Code (MainWindow.cs)**：

```c#
using System.Windows;
using System.Windows.Input;

namespace ResizeTransparentWindow
{
    /// <summary>
    /// MainWindow.xaml 的交互逻辑
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }



        /// <summary>
        /// 当用鼠标左键点击TopBorder控件时，触发此方法
        /// (This method is triggered when the TopBorder control is clicked with the left mouse button)
        /// </summary>
        private void TopBorder_OnMouseLeftButtonDown(object sender, MouseButtonEventArgs e)
        {
    
            /* 如何在Window.ResizeMode属性为CanResize的时候，阻止窗口拖动到屏幕边缘自动最大化。
               (When the Window.ResizeMode property is CanResize, 
               when the window is dragged to the edge of the screen, 
               it prevents the window from automatically maximizing.)*/
            if (e.ChangedButton == MouseButton.Left)
            {
                if (Mouse.LeftButton == MouseButtonState.Pressed)
                {
                    var windowMode = this.ResizeMode;
                    if (this.ResizeMode != ResizeMode.NoResize)
                    {
                        this.ResizeMode = ResizeMode.NoResize;
                    }
    
                    this.UpdateLayout();


                    /* 当点击拖拽区域的时候，让窗口跟着移动
                    (When clicking the drag area, make the window follow) */
                    DragMove();


                    if (this.ResizeMode != windowMode)
                    {
                        this.ResizeMode = windowMode;
                    }
    
                    this.UpdateLayout();
                }
            }
        }
    
    }

}
```

<br/>

<br/>

<br/>

<br/>

## Change the size of the window proportionally

Can we zoom in / out the window proportionally when dragging the mouse to resize the window?

For example, can you always keep the window ratio to 4:3?

Of course it can!

Here reference [a7066163](https://my.csdn.net/a7066163)、[Hauk](https://my.csdn.net/haukwong) article .

Using this method, you can not only scale the window proportionally, but also avoid flicker!

```
Link：https://bbs.csdn.net/topics/390257164
```

<br/>

> **Thinking**:
>
> ```
> [First, Hauk gave his thoughts]:
> "Show window contents when dragging" When unchecked: SizeChanged event is triggered only once when dragging
> "Show window contents when dragging" When checked: One drag only triggers N SizeChanged events(n=newsize-oldsize)
> 
> The trigger is too frequent so the interface update is a bit wrong.
> You can use the timer to delay the update until the user drags it.           
> ```
>
> ```
> [Then a7066163 made changes based on Hauk's idea]:
> Although in the case of slow mouse release, the window will return to its original size, so I only deal with the mouse release event.
> However, it seems that the mouse event with its own border does not belong to the window event. In the end, I directly captured the WM_EXITSIZEMOVE message and reloaded the window message processing function.
> Through the double guarantee of timer and WM_EXITSIZEMOVE message processing, the effect of real-time constant window proportion can be achieved.
> ```

<br/>

<br/>

**效果**：

![](Image/20.gif)

<br/>

**Code (MainWindow.cs)**：

```c#
using System;
using System.Windows;
using System.Windows.Input;
using System.Windows.Interop;

namespace ResizeTransparentWindow
{
    /// <summary>
    /// MainWindow.xaml 的交互逻辑
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }



        //最后的宽度（Last Width）
        private int LastWidth;
    
        //最后的高度（Last Height）
        private int LastHeight;
    
        //这个属性是指 窗口的宽度和高度的比例（宽度/高度）(4:3)
        //This property refers to the aspect ratio (width / height) of the window (4: 3)
        private float AspectRatio = 4.0f / 3.0f;



        /// <summary>
        /// 捕获窗口拖拉消息
        /// (Capturing window drag messages)
        /// </summary>
        protected override void OnSourceInitialized(EventArgs e)
        {
            base.OnSourceInitialized(e);
            HwndSource source = HwndSource.FromVisual(this) as HwndSource;
            if (source != null)
            {
                source.AddHook(new HwndSourceHook(WinProc));
            }
        }
    
        public const Int32 WM_EXITSIZEMOVE = 0x0232;
    
        /// <summary>
        /// 重载窗口消息处理函数
        /// (Overload window message processing function)
        /// </summary>
        private IntPtr WinProc(IntPtr hwnd, Int32 msg, IntPtr wParam, IntPtr lParam, ref Boolean handled)
        {
            IntPtr result = IntPtr.Zero;
            switch (msg)
            {
                //处理窗口消息 (Handle window messages)
                case WM_EXITSIZEMOVE:
                {
                    //上下拖拉窗口 (Drag window vertically)
                    if (this.Height != LastHeight)
                    {
                        this.Width = this.Height * AspectRatio;
                    }
                    // 左右拖拉窗口 (Drag window horizontally)
                    else if (this.Width != LastWidth)
                    {
                        this.Height = this.Width / AspectRatio;
                    }
    
                    LastWidth = (int)this.Width;
                    LastHeight = (int)this.Height;
                    break;
                }
            }
    
            return result;
        }
    }

}
```

<br/>

<br/>

<br/>

<br/>

## Proportionally change the size of controls in the window

So when we change the window size, can we change the size of the controls in the window proportionally?

Of course!

Here we use the Viewbox control to achieve this function!

<br/>

**Step 1**: We just need to put all the controls into the Viewbox control.

![](Image/21.png)

<br/>

**Step 2**: Then, set the Stretch property of the Viewbox to UniformToFill

![](Image/22.png)

<br/>

**Step 3**: Now, when we zoom the window, the content in the window will automatically be scaled automatically!

![](Image/23.gif)

<br/>

<br/>

**Code (MainWindow.xaml)**：

```xaml
<Window x:Class="ResizeTransparentWindow.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:ResizeTransparentWindow"
        mc:Ignorable="d"
        Title="MainWindow" Height="600" Width="800"
        

        Background="Transparent"
        AllowsTransparency="True"
        WindowStyle="None">


    <!--WindowChrome-->
    <WindowChrome.WindowChrome>
        <WindowChrome CaptionHeight="0" ResizeBorderThickness="20"/>
    </WindowChrome.WindowChrome>


    <!--Viewbox-->
    <Viewbox Stretch="UniformToFill">
    
        <!--内容（Content）-->
        <Grid Width="800" Height="600">
    
            <!--背景（Background）-->
            <Border Margin="10" Background="White" CornerRadius="25">
                <Border.Effect>
                    <DropShadowEffect Direction="0" ShadowDepth="0" BlurRadius="20" 
                                      Opacity="0.25" Color="#FF5B5B5B">		
                    </DropShadowEffect>
                </Border.Effect>
            </Border>
    
            <!--按钮（Button）-->
            <Button Width="400" Height="200">
                <TextBlock Width="400" Height="100" TextAlignment="Center"
                           FontSize="70" FontFamily="Source Han Sans CN Medium"
                           Text="Hello"/>
            </Button>
    
        </Grid>
    
    </Viewbox>

</Window>
```

<br/>

<br/>

<br/>

<br/>

## Complete code

**MainWindow.xaml**：

```Xaml
<Window x:Class="ResizeTransparentWindow.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:ResizeTransparentWindow"
        mc:Ignorable="d"
        Title="MainWindow" Height="600" Width="800"
        

        Background="Transparent"
        AllowsTransparency="True"
        WindowStyle="None">


    <!--WindowChrome-->
    <WindowChrome.WindowChrome>
        <WindowChrome CaptionHeight="0" ResizeBorderThickness="20"/>
    </WindowChrome.WindowChrome>


    <!--Viewbox-->
    <Viewbox Stretch="UniformToFill">
    
        <!--内容（Content）-->
        <Grid Width="800" Height="600">
    
            <!--背景（Background）-->
            <Border Margin="10" Background="White" CornerRadius="25">
                <Border.Effect>
                    <DropShadowEffect Direction="0" ShadowDepth="0" BlurRadius="20" 
                                      Opacity="0.25" Color="#FF5B5B5B"></DropShadowEffect>
                </Border.Effect>
            </Border>
    
            <!--按钮（Button）-->
            <Button Width="400" Height="200">
                <TextBlock Width="400" Height="100" TextAlignment="Center"
                           FontSize="70" FontFamily="Source Han Sans CN Medium"
                           Text="Hello"/>
            </Button>
    
            <!--拖拽窗口的区域（Border：For dragging windows）-->
            <Border Name="TopBorder" Width="780" Height="100"
                    VerticalAlignment="Top" Margin="0,10,0,0"
                    Background="White" Opacity="0" Cursor="Hand"
                
                    MouseLeftButtonDown="TopBorder_OnMouseLeftButtonDown"/>
    
        </Grid>
    
    </Viewbox>

</Window>
```

<br/>

<br/>

**MainWindow.cs**：

```c#
using System;
using System.Windows;
using System.Windows.Input;
using System.Windows.Interop;

namespace ResizeTransparentWindow
{
    /// <summary>
    /// MainWindow.xaml 的交互逻辑
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }



        #region [拖动窗口 (Drag Move Window)]
    
        /// <summary>
        /// 当用鼠标左键点击TopBorder控件时，触发此方法
        /// (This method is triggered when the TopBorder control is clicked with the left mouse button)
        /// </summary>
        private void TopBorder_OnMouseLeftButtonDown(object sender, MouseButtonEventArgs e)
        {
            /* 当点击拖拽区域的时候，让窗口跟着移动
               (When clicking the drag area, make the window follow) */
            DragMove();
        }
    
        #endregion


        #region [等比例调整窗口尺寸（Proportional Resize window）]
    
        //最后的宽度（Last Width）
        private int LastWidth;
    
        //最后的高度（Last Height）
        private int LastHeight;
    
        //这个属性是指 窗口的宽度和高度的比例（宽度/高度）(4:3)
        //This property refers to the aspect ratio (width / height) of the window (4: 3)
        private float AspectRatio = 4.0f / 3.0f;



        /// <summary>
        /// 捕获窗口拖拉消息
        /// (Capturing window drag messages)
        /// </summary>
        protected override void OnSourceInitialized(EventArgs e)
        {
            base.OnSourceInitialized(e);
            HwndSource source = HwndSource.FromVisual(this) as HwndSource;
            if (source != null)
            {
                source.AddHook(new HwndSourceHook(WinProc));
            }
        }
    
        public const Int32 WM_EXITSIZEMOVE = 0x0232;
    
        /// <summary>
        /// 重载窗口消息处理函数
        /// (Overload window message processing function)
        /// </summary>
        private IntPtr WinProc(IntPtr hwnd, Int32 msg, IntPtr wParam, IntPtr lParam, ref Boolean handled)
        {
            IntPtr result = IntPtr.Zero;
            switch (msg)
            {
                //处理窗口消息 (Handle window messages)
                case WM_EXITSIZEMOVE:
                {
                    //上下拖拉窗口 (Drag window vertically)
                    if (this.Height != LastHeight)
                    {
                        this.Width = this.Height * AspectRatio;
                    }
                    // 左右拖拉窗口 (Drag window horizontally)
                    else if (this.Width != LastWidth)
                    {
                        this.Height = this.Width / AspectRatio;
                    }
    
                    LastWidth = (int) this.Width;
                    LastHeight = (int) this.Height;
                    break;
                }
            }
    
            return result;
        }


        #endregion



    }

}
```

