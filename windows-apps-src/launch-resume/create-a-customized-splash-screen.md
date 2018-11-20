---
author: TylerMSFT
title: 延長顯示啟動顯示畫面
description: 您可以為 app 建立延長式啟動顯示畫面，讓啟動顯示畫面的顯示時間變長。 這個延長的畫面是模仿您應用程式啟動時所顯示的啟動顯示畫面，但是您可以自訂這個畫面。
ms.assetid: CD3053EB-7F86-4D74-9C5A-950303791AE3
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 80242b95e64f0d642df0284c94455d60825f6daf
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7283522"
---
# <a name="display-a-splash-screen-for-more-time"></a>延長顯示啟動顯示畫面




**重要 API**

-   [**SplashScreen 類別**](https://msdn.microsoft.com/library/windows/apps/br224763)
-   [**Window.SizeChanged 事件**](https://msdn.microsoft.com/library/windows/apps/br209055)
-   [**Application.OnLaunched 方法**](https://msdn.microsoft.com/library/windows/apps/br242335)

您可以為應用程式建立延長式啟動顯示畫面，讓啟動顯示畫面的顯示時間變長。 這個延長的畫面是模仿您應用程式啟動時所顯示的啟動顯示畫面，但是您可以自訂這個畫面。 不論您是想要顯示即時載入資訊，或只是想給 app 額外的時間來準備好初始 UI，都可以利用延長式啟動顯示畫面來定義啟動體驗。

> **注意：** 中的 「 延長式啟動顯示畫面 」 這個主題是指在螢幕停留一段時間的啟動顯示畫面。 這不是指衍生自 [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) 類別的子類別。

 

請遵循下列建議，以確定您的延長式啟動顯示畫面準確模仿預設啟動顯示畫面：

-   您的延長式啟動顯示畫面頁面應該使用 620 x 300 像素影像，此影像會與您應用程式資訊清單中針對啟動顯示畫面指定的影像一致 (您應用程式的啟動顯示畫面影像)。 在 Microsoft Visual Studio2015，啟動顯示畫面設定儲存在您的應用程式資訊清單 （Package.appxmanifest 檔案） 中的 [**視覺資產**] 索引標籤的**啟動顯示畫面**一節。
-   您的延長式啟動顯示畫面所使用的背景色彩，應該與您應用程式資訊清單中針對啟動顯示畫面指定的背景色彩一致 (您應用程式的啟動顯示畫面背景)。
-   您的程式碼應該使用 [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) 類別，將應用程式的啟動顯示畫面影像，放置在與預設啟動顯示畫面相同的螢幕座標上。
-   您的程式碼應該藉由使用 [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) 類別，將項目重新放置在延長式啟動顯示畫面上，以回應視窗調整大小事件 (例如當螢幕旋轉或應用程式被移到螢幕上另一個應用程式旁邊時)。

使用下列步驟建立延長式啟動顯示畫面，有效模擬預設啟動顯示畫面。

## <a name="add-a-blank-page-item-to-your-existing-app"></a>將 **\[空白頁面\]** 項目新增到您的現有 app


這個主題是假設您想要將延長式啟動顯示畫面新增到使用 C#、Visual Basic 或 C++ 的現有通用 Windows 平台 (UWP) app 專案。

-   在視覺 Studio2015 中開啟您的應用程式。
-   從功能表列按或開啟 **\[專案\]**，然後按一下 **\[加入新項目\]**。 將會出現 **\[加入新項目\]** 對話方塊。
-   從這個對話方塊，將一個新的 **\[空白頁面\]** 新增到您的應用程式。 這個主題將延長式啟動顯示畫面頁面命名為 "ExtendedSplash"。

新增一個 **\[空白頁面\]** 項目會產生兩個檔案，一個用於標記 (ExtendedSplash.xaml)，另一個用於程式碼 (ExtendedSplash.xaml.cs)。

## <a name="essential-xaml-for-an-extended-splash-screen"></a>延長式啟動顯示畫面的基本 XAML


請依照這些步驟，將影像與進度控制項新增到您的延長式啟動顯示畫面。

在您的 ExtendedSplash.xaml 檔案中：

-   變更預設 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 元素的 [**Background**](https://msdn.microsoft.com/library/windows/apps/br209396) 屬性，使其符合您在應用程式資訊清單中針對應用程式啟動顯示畫面設定的背景色彩 (在 Package.appxmanifest 檔案的 **\[視覺資產\]** 區段)。 預設的啟動顯示畫面色彩是淺灰色 (十六進位值為 \#464646)。 請注意，這個 **Grid** 元素是當您建立新 **\[空白頁面\]** 時預設提供的元素。 您不需要使用 **Grid**；它只是用來建置延長式啟動顯示畫面的一個便利基礎。
-   將 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 元素新增到 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)。 您將使用這個 **Canvas** 來放置您的延長式啟動顯示畫面影像。
-   將 [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) 元素新增到 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)。 在您的延長式啟動顯示畫面使用 600 x 320 像素影像，亦即與您為預設啟動顯示畫面選擇的影像相同。
-   (選用) 新增進度控制項以向使用者顯示您的應用程式正在載入。 本主題新增了 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) 來代替確定或不確定的 [**ProgressBar**](https://msdn.microsoft.com/library/windows/apps/br227529)。

請在 ExtendedSplash.xaml 中新增下列控制碼來定義 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 與 [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) 元素，以及定義 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) 控制項：

```xml
    <Grid Background="#464646">
        <Canvas>
            <Image x:Name="extendedSplashImage" Source="Assets/SplashScreen.png"/>
            <ProgressRing Name="splashProgressRing" IsActive="True" Width="20" HorizontalAlignment="Center"></ProgressRing>
        </Canvas>
    </Grid>
```

**注意：** 這個程式碼會[**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538)的寬度設定為 20 像素。 您可以將其寬度設定為適合您應用程式的值，不過，如果寬度小於 20 像素，控制項將無法顯示。

 

## <a name="essential-code-for-an-extended-splash-screen-class"></a>延長式啟動顯示畫面類別的基本程式碼


每當視窗大小 (僅 Windows) 或方向變更時，您的延長式啟動顯示畫面都必須予以回應。 您使用的影像位置必須更新，如此才能讓您的延長式啟動顯示畫面不論視窗如何變更都能保持美觀。

使用這些步驟來定義方法，以便正確顯示您的延長式啟動顯示畫面。

1.  **新增必要的命名空間**

    您需要將下列命名空間新增到 ExtendedSplash.xaml.cs，以存取 [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) 類別、[**Window.SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br209055) 事件。

    ```cs
    using Windows.ApplicationModel.Activation;
    using Windows.UI.Core;
    ```

2.  **建立部分類別並宣告類別變數**

    請在 ExtendedSplash.xaml.cs 中納入下列程式碼，以建立部分類別來代表延長式啟動顯示畫面。

    ```cs
    partial class ExtendedSplash : Page
    {
        internal Rect splashImageRect; // Rect to store splash screen image coordinates.
        private SplashScreen splash; // Variable to hold the splash screen object.
        internal bool dismissed = false; // Variable to track splash screen dismissal status.
        internal Frame rootFrame;

       // Define methods and constructor
    }
    ```

    這些類別變數是透過數個方法來使用。 `splashImageRect` 變數會儲存系統顯示應用程式的啟動顯示畫面影像的座標。 `splash` 變數會儲存 [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) 物件，而 `dismissed` 變數會追蹤系統顯示的啟動顯示畫面是否已關閉。

3.  **定義可正確放置影像的類別建構函式**

    下列程式碼為用來接聽視窗調整大小事件的延長式啟動顯示畫面類別定義了建構函式、將影像與 (選用) 進度控制項放置在延長式啟動顯示畫面、建立用於瀏覽的框架，以及呼叫非同步方法來還原已儲存的工作階段狀態。

    ```cs
    public ExtendedSplash(SplashScreen splashscreen, bool loadState)
    {
        InitializeComponent();

        // Listen for window resize events to reposition the extended splash screen image accordingly.
        // This ensures that the extended splash screen formats properly in response to window resizing.
        Window.Current.SizeChanged += new WindowSizeChangedEventHandler(ExtendedSplash_OnResize);

        splash = splashscreen;
        if (splash != null)
        {
            // Register an event handler to be executed when the splash screen has been dismissed.
            splash.Dismissed += new TypedEventHandler<SplashScreen, Object>(DismissedEventHandler);

            // Retrieve the window coordinates of the splash screen image.
            splashImageRect = splash.ImageLocation;
            PositionImage();

            // If applicable, include a method for positioning a progress control.
            PositionRing();
        }

        // Create a Frame to act as the navigation context
        rootFrame = new Frame();            
    }
    ```

    請確定在類別建構函式中登錄您的 [**Window.SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br209055) 處理常式 (範例中的 `ExtendedSplash_OnResize`)，以便讓應用程式在延長式啟動顯示畫面中正確放置影像。

4.  **定義類別方法，以便在您的延長式啟動顯示畫面中放置影像**

    這個程式碼示範如何使用 `splashImageRect` 類別變數，將影像放置在延長式啟動顯示畫面頁面。

    ```cs
    void PositionImage()
    {
        extendedSplashImage.SetValue(Canvas.LeftProperty, splashImageRect.X);
        extendedSplashImage.SetValue(Canvas.TopProperty, splashImageRect.Y);
        extendedSplashImage.Height = splashImageRect.Height;
        extendedSplashImage.Width = splashImageRect.Width;
    }
    ```

5.  **(選用) 定義類別方法以將進度控制項放置在您的延長式啟動顯示畫面**

    如果您選擇將 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) 新增到延長式啟動顯示畫面，請將它放置在啟動顯示畫面影像的相對位置。 請將下列程式碼新增到 ExtendedSplash.xaml.cs，以將 **ProgressRing** 置中放在影像下方 32 像素的位置。

    ```cs
    void PositionRing()
    {
        splashProgressRing.SetValue(Canvas.LeftProperty, splashImageRect.X + (splashImageRect.Width*0.5) - (splashProgressRing.Width*0.5));
        splashProgressRing.SetValue(Canvas.TopProperty, (splashImageRect.Y + splashImageRect.Height + splashImageRect.Height*0.1));
    }
    ```

6.  **在類別內部，定義 Dismissed 事件的處理常式**

    在 ExtendedSplash.xaml.cs 中將 `dismissed` 類別變數設定為 true，以在 [**SplashScreen.Dismissed**](https://msdn.microsoft.com/library/windows/apps/br224764) 事件發生時予以回應。 如果您的應用程式有安裝程式作業，請將它們新增到這個事件處理常式。

    ```cs
    // Include code to be executed when the system has transitioned from the splash screen to the extended splash screen (application's first view).
    void DismissedEventHandler(SplashScreen sender, object e)
    {
        dismissed = true;

        // Complete app setup operations here...
    }
    ```

    在應用程式安裝完成後，請離開延長式啟動顯示畫面。 下列程式碼定義了一個叫做 `DismissExtendedSplash` 的方法，這個方法會瀏覽至您應用程式的 MainPage.xaml 檔案中定義的 `MainPage`。

    ```cs
    async void DismissExtendedSplash()
      {
         await Windows.ApplicationModel.Core.CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal,() =>            {
              rootFrame = new Frame();
              rootFrame.Content = new MainPage(); Window.Current.Content = rootFrame;
            });
      }
      ```

7.  **在類別內部，定義 Window.SizeChanged 事件的處理常式**

    請準備您的延長式啟動顯示畫面，讓它在使用者調整視窗大小時重新放置元素。 這個程式碼會藉由擷取新座標並重新放置影像，在 [**Window.SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br209055) 事件發生時予以回應。 如果您已將進度控制項新增到延長式啟動顯示畫面，也請將它重新放置在這個事件處理常式內。

    ```cs
    void ExtendedSplash_OnResize(Object sender, WindowSizeChangedEventArgs e)
    {
        // Safely update the extended splash screen image coordinates. This function will be executed when a user resizes the window.
        if (splash != null)
        {
            // Update the coordinates of the splash screen image.
            splashImageRect = splash.ImageLocation;
            PositionImage();

            // If applicable, include a method for positioning a progress control.
            // PositionRing();
        }
    }
    ```

    **注意：** 您嘗試取得影像位置之前，請確定類別變數 (`splash`) 包含有效的[**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763)物件，如範例所示。

     

8.  **(選擇性) 新增類別方法以還原已儲存的工作階段狀態**

    您在步驟 4：[修改啟動啟用處理常式](#modify-the-launch-activation-handler)中新增到 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法的程式碼，會導致您的應用程式在啟動時顯示延長式啟動顯示畫面。 若要將延長式啟動顯示畫面類別中與 app 啟動相關的所有方法合併，您可以考慮將下列非同步方法新增到您的 ExtendedSplash.xaml.cs 檔案來還原 app 狀態。

    ```cs
    async void RestoreStateAsync(bool loadState)
    {
        if (loadState)
        {
             // code to load your app's state here
        }
    }
    ```

    當您修改 App.xaml.cs 中的啟動啟用處理常式時，如果 app 之前的 [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) 為 **Terminated**，則也需要將 `loadstate` 設定為 true。 如果是這種情況，`RestoreStateAsync` 方法會將 app 還原到它之前的狀態。 如需 app 啟動、暫停及終止的概觀，請參閱 [App 週期](app-lifecycle.md)。

## <a name="modify-the-launch-activation-handler"></a>修改啟動啟用處理常式


啟動應用程式時，系統會將啟動顯示畫面資訊傳遞給應用程式的啟動啟用事件處理常式。 您可以使用這項資訊，將影像正確放置在延長式啟動顯示畫面頁面上。 您可以從傳遞給 app [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 處理常式的啟用事件引數中取得這項啟動顯示畫面資訊 (請參閱下列程式碼中的 `args` 變數)。

如果您尚未覆寫 app 的 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 處理常式，請參閱 [App 週期](app-lifecycle.md)，以了解如何處理啟用事件。

在 App.xaml.cs 中，新增下列程式碼來建立和顯示延長式啟動顯示畫面。

```cs
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
    if (args.PreviousExecutionState != ApplicationExecutionState.Running)
    {
        bool loadState = (args.PreviousExecutionState == ApplicationExecutionState.Terminated);
        ExtendedSplash extendedSplash = new ExtendedSplash(args.SplashScreen, loadState);
        Window.Current.Content = extendedSplash;
    }

    Window.Current.Activate();
}
```

## <a name="complete-code"></a>完整程式碼


> **注意：** 上一個步驟中顯示的程式碼片段稍微不同下列程式碼。
-   ExtendedSplash.xaml 包含一個 `DismissSplash` 按鈕。 按一下這個按鈕時，事件處理常式 `DismissSplashButton_Click` 會呼叫 `DismissExtendedSplash` 方法。 在您的應用程式中，請在應用程式完成資源載入或 UI 初始化時呼叫 `DismissExtendedSplash`。
-   這個 app 也使用一個 UWP app 專案範本，而該範本使用 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 瀏覽。 因此，在 App.xaml.cs 中，啟動啟用處理常式 ([**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)) 定義了 `rootFrame`，並使用它來設定應用程式視窗的內容。

ExtendedSplash.xaml：這個範例包含一個 `DismissSplash` 按鈕，因為它沒有要載入的應用程式資源。 在您的應用程式中，請在應用程式完成資源載入或其初始 UI 準備時，自動關閉延長式啟動顯示畫面。

```xml
<Page
    x:Class="SplashScreenExample.ExtendedSplash"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SplashScreenExample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="#464646">
        <Canvas>
            <Image x:Name="extendedSplashImage" Source="Assets/SplashScreen.png"/>
            <ProgressRing Name="splashProgressRing" IsActive="True" Width="20" HorizontalAlignment="Center"/>
        </Canvas>
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Bottom">
            <Button x:Name="DismissSplash" Content="Dismiss extended splash screen" HorizontalAlignment="Center" Click="DismissSplashButton_Click" />
        </StackPanel>
    </Grid>
</Page>
```

ExtendedSplash.xaml.cs：請注意，`DismissExtendedSplash` 方法是從 `DismissSplash` 按鈕的按一下事件處理常式呼叫的。 在您的應用程式中將不需要 `DismissSplash` 按鈕。 請在應用程式完成資源載入，而您想要瀏覽至它的主頁面時，改為呼叫 `DismissExtendedSplash`。

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

using Windows.ApplicationModel.Activation;
using SplashScreenExample.Common;
using Windows.UI.Core;

// The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?LinkID=234238

namespace SplashScreenExample
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    partial class ExtendedSplash : Page
    {
        internal Rect splashImageRect; // Rect to store splash screen image coordinates.
        private SplashScreen splash; // Variable to hold the splash screen object.
        internal bool dismissed = false; // Variable to track splash screen dismissal status.
        internal Frame rootFrame;

        public ExtendedSplash(SplashScreen splashscreen, bool loadState)
        {
            InitializeComponent();

            // Listen for window resize events to reposition the extended splash screen image accordingly.
            // This is important to ensure that the extended splash screen is formatted properly in response to snapping, unsnapping, rotation, etc...
            Window.Current.SizeChanged += new WindowSizeChangedEventHandler(ExtendedSplash_OnResize);

            splash = splashscreen;

            if (splash != null)
            {
                // Register an event handler to be executed when the splash screen has been dismissed.
                splash.Dismissed += new TypedEventHandler<SplashScreen, Object>(DismissedEventHandler);

                // Retrieve the window coordinates of the splash screen image.
                splashImageRect = splash.ImageLocation;
                PositionImage();

                // Optional: Add a progress ring to your splash screen to show users that content is loading
                PositionRing();
            }

            // Create a Frame to act as the navigation context
            rootFrame = new Frame();

            // Restore the saved session state if necessary
            await RestoreStateAsync(loadState);
        }

        async void RestoreStateAsync(bool loadState)
        {
            if (loadState)
            {
                // TODO: write code to load state
            }
        }

        // Position the extended splash screen image in the same location as the system splash screen image.
        void PositionImage()
        {
            extendedSplashImage.SetValue(Canvas.LeftProperty, splashImageRect.X);
            extendedSplashImage.SetValue(Canvas.TopProperty, splashImageRect.Y);
            extendedSplashImage.Height = splashImageRect.Height;
            extendedSplashImage.Width = splashImageRect.Width;

        }

        void PositionRing()
        {
            splashProgressRing.SetValue(Canvas.LeftProperty, splashImageRect.X + (splashImageRect.Width*0.5) - (splashProgressRing.Width*0.5));
            splashProgressRing.SetValue(Canvas.TopProperty, (splashImageRect.Y + splashImageRect.Height + splashImageRect.Height*0.1));
        }

        void ExtendedSplash_OnResize(Object sender, WindowSizeChangedEventArgs e)
        {
            // Safely update the extended splash screen image coordinates. This function will be fired in response to snapping, unsnapping, rotation, etc...
            if (splash != null)
            {
                // Update the coordinates of the splash screen image.
                splashImageRect = splash.ImageLocation;
                PositionImage();
                PositionRing();
            }
        }

        // Include code to be executed when the system has transitioned from the splash screen to the extended splash screen (application's first view).
        void DismissedEventHandler(SplashScreen sender, object e)
        {
            dismissed = true;

            // Complete app setup operations here...
        }

        void DismissExtendedSplash()
        {
            // Navigate to mainpage
            rootFrame.Navigate(typeof(MainPage));
            // Place the frame in the current Window
            Window.Current.Content = rootFrame;
        }

        void DismissSplashButton_Click(object sender, RoutedEventArgs e)
        {
            DismissExtendedSplash();
        }
    }
}
```

App.xaml.cs： 這個專案被建立使用 UWP 應用程式的**空白應用程式 (XAML)** 專案範本中視覺 Studio2015。 `OnNavigationFailed` 與 `OnSuspending` 事件處理常式都是自動產生的，而且不需變更就可以實作延長式啟動顯示畫面。 這個主題只會修改 `OnLaunched`。

如果您沒有將專案範本用於您的 app，請參閱步驟 4：[修改啟動啟用處理常式](#modify-the-launch-activation-handler)，當中有一個沒有使用 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 瀏覽的已修改 `OnLaunched` 範例。

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

// The Blank Application template is documented at http://go.microsoft.com/fwlink/p/?LinkID=234227

namespace SplashScreenExample
{
    /// <summary>
    /// Provides application-specific behavior to supplement the default Application class.
    /// </summary>
    sealed partial class App : Application
    {
        /// <summary>
        /// Initializes the singleton application object.  This is the first line of authored code
        /// executed, and as such is the logical equivalent of main() or WinMain().
        /// </summary>
        public App()
        {
            Microsoft.ApplicationInsights.WindowsAppInitializer.InitializeAsync(
            Microsoft.ApplicationInsights.WindowsCollectors.Metadata |
            Microsoft.ApplicationInsights.WindowsCollectors.Session);
            this.InitializeComponent();
            this.Suspending += OnSuspending;
        }

        /// <summary>
        /// Invoked when the application is launched normally by the end user.  Other entry points
        /// will be used such as when the application is launched to open a specific file.
        /// </summary>
        /// <param name="e">Details about the launch request and process.</param>
        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
#if DEBUG
            if (System.Diagnostics.Debugger.IsAttached)
            {
                this.DebugSettings.EnableFrameRateCounter = true;
            }
#endif

            Frame rootFrame = Window.Current.Content as Frame;

            // Do not repeat app initialization when the Window already has content,
            // just ensure that the window is active
            if (rootFrame == null)
            {
                // Create a Frame to act as the navigation context and navigate to the first page
                rootFrame = new Frame();
                // Set the default language
                rootFrame.Language = Windows.Globalization.ApplicationLanguages.Languages[0];

                rootFrame.NavigationFailed += OnNavigationFailed;

                //  Display an extended splash screen if app was not previously running.
                if (e.PreviousExecutionState != ApplicationExecutionState.Running)
                {
                    bool loadState = (e.PreviousExecutionState == ApplicationExecutionState.Terminated);
                    ExtendedSplash extendedSplash = new ExtendedSplash(e.SplashScreen, loadState);
                    rootFrame.Content = extendedSplash;
                    Window.Current.Content = rootFrame;
                }
            }

            if (rootFrame.Content == null)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(typeof(MainPage), e.Arguments);
            }
            // Ensure the current window is active
            Window.Current.Activate();
        }

        /// <summary>
        /// Invoked when Navigation to a certain page fails
        /// </summary>
        /// <param name="sender">The Frame which failed navigation</param>
        /// <param name="e">Details about the navigation failure</param>
        void OnNavigationFailed(object sender, NavigationFailedEventArgs e)
        {
            throw new Exception("Failed to load Page " + e.SourcePageType.FullName);
        }

        /// <summary>
        /// Invoked when application execution is being suspended.  Application state is saved
        /// without knowing whether the application will be terminated or resumed with the contents
        /// of memory still intact.
        /// </summary>
        /// <param name="sender">The source of the suspend request.</param>
        /// <param name="e">Details about the suspend request.</param>
        private void OnSuspending(object sender, SuspendingEventArgs e)
        {
            var deferral = e.SuspendingOperation.GetDeferral();
            // TODO: Save applicaiton state and stop any background activity
            deferral.Complete();
        }
    }
}
```

## <a name="related-topics"></a>相關主題


* [App 週期](app-lifecycle.md)

**參考**

* [**Windows.ApplicationModel.Activation 命名空間**](https://msdn.microsoft.com/library/windows/apps/br224766)
* [**Windows.ApplicationModel.Activation.SplashScreen 類別**](https://msdn.microsoft.com/library/windows/apps/br224763)
* [**Windows.ApplicationModel.Activation.SplashScreen.ImageLocation 屬性**](https://msdn.microsoft.com/library/windows/apps/br224765)
* [**Windows.ApplicationModel.Core.CoreApplicationView.Activated 事件**](https://msdn.microsoft.com/library/windows/apps/br225018)

 

 
