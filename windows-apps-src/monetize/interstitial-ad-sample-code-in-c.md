---
author: mcleanbyron
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: "了解如何使用 C# 啟動插入式廣告。"
title: "使用 C 的插入式廣告範例程式碼#"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: e83730c60eada273aee4f3bca4ff28cf6feccbf8


---

# 使用 C 的插入式廣告範例程式碼\# #  


\[ 針對 Windows 10 上的 UWP App 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本主題示範如何使用 C# 啟動插入式廣告。 如需示範如何設定專案以使用此程式碼的逐步指示，請參閱[插入式廣告](interstitial-ads.md)。 如需示範如何將插入式廣告影片新增到使用 C# 之 XAML App 的完整範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。


## 程式碼範例

此程式碼範例示範實作插入式廣告的運作 MainPage.xaml.cs 程式碼檔案。 此程式碼假設 MainPage.xaml 檔案擁有運作中的按鈕，該按鈕具有能夠觸發且由名為 **button_Click** 方法所處理的 **Click** 事件。 此程式碼會在按鈕上的 **Click** 事件被觸發時啟動插入式廣告。

在將 App 提交至市集之前，請以具有實際值的 **AppID** 和 **AdUnitId** 變數取代文字。 如需詳細資訊，請參閱[在您的 App 中設定廣告單元](set-up-ad-units-in-your-app.md)。

``` syntax
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
using Microsoft.Advertising.WinRT.UI;


namespace BasicCSharpInterstitialUWP
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        InterstitialAd MyVideoAd = new InterstitialAd();

        public MainPage()
        {
            this.InitializeComponent();
        }

        private void button_Click(object sender, RoutedEventArgs e)
        {
            // Define ApplicationId and AdUnitId.
            // Test values are shown here. Replace the test values with live values before submitting the app to the Store.
            var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
            var MyAdUnitId = "11389925";

            MyVideoAd.AdReady += MyVideoAd_AdReady;
            MyVideoAd.ErrorOccurred += MyVideoAd_ErrorOccurred;
            MyVideoAd.Completed += MyVideoAd_Completed;
            MyVideoAd.Cancelled += MyVideoAd_Cancelled;

            // Pre-fetch an ad 30-60 seconds before you need it.
            MyVideoAd.RequestAd(AdType.Video, MyAppId, MyAdUnitId);
        }

        void MyVideoAd_AdReady(object sender, object e)
        {
            if ((InterstitialAdState.Ready) == (MyVideoAd.State))
            {
                MyVideoAd.Show();
            }
        }

        void MyVideoAd_ErrorOccurred(object sender, AdErrorEventArgs e)
        {
            // Add your code here.
        }

        void MyVideoAd_Completed(object sender, object e)
        {
            // Add your code here.
        }

        void MyVideoAd_Cancelled(object sender, object e)
        {
            // Add your code here.  
        }
    }
}
```

 
## 相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)
 



<!--HONumber=Jun16_HO4-->


