---
author: mcleanbyron
ms.assetid: cf0d2709-21a1-4d56-9341-d4897e405f5d
description: "了解如何在您的 app 中抓取 AdControl 錯誤。"
title: "XAML/C# 錯誤處理的逐步解說"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 廣告, 廣告, 錯誤處理, XAML, C#"
ms.openlocfilehash: d6c048397a5f7fd6c9a6cd625a7ff5ce0b6c29bf
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="error-handling-in-xamlc-walkthrough"></a>XAML/C# 錯誤處理的逐步解說

了解如何在您的 App 中抓取 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 錯誤。

這些範例假設您有包含 **AdControl** 的 XAML/C# app。 如需示範如何將 **AdControl** 新增到 app 的逐步指示，請參閱 [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)。 如需示範如何將橫幅廣告新增到使用 C# 和 C++ 的 XAML app 的完整範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

1.  在您的 MainPage.xaml 檔案中，找出 **AdControl** 的定義。 該程式碼看起來像這樣：

  > [!div class="tabbedCodeSnippets"]
  ``` xml
  <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="10865270"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300" />
  ```

2.   在 **Width** 屬性後方，但在結尾標記的前方，指派錯誤事件處理常式的名稱給 [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.erroroccurred.aspx) 事件。 在這個逐步解說中，錯誤事件處理常式的名稱為 **OnAdError**。

  > [!div class="tabbedCodeSnippets"]
  ``` xml
  <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="10865270"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300"
      ErrorOccurred="OnAdError"/>
  ```

2.  若要在執行階段產生錯誤，請建立具有不同應用程式識別碼的第二個 **AdControl**。 因為在 app 中，所有 **AdControl** 物件都必須使用相同的應用程式識別碼，所以建立具有不同應用程式識別碼的其他 **AdControl** 將會擲回錯誤。

    在 MainPage.xaml 中第一個 **AdControl** 的後方定義第二個 **AdControl**，然後將 [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) 屬性設為零 (“0”)。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl
        ApplicationId="0"
        AdUnitId="10865270"
        HorizontalAlignment="Left"
        Height="250"
        Margin="10,265,0,0"
        VerticalAlignment="Top"
        Width="300"
        ErrorOccurred="OnAdError" />
    ```

3.  在 MainPage.xaml.cs 中，將以下 **OnAdError** 事件處理常式新增到 **MainPage** 類別。 這個事件處理常式會寫入資訊到 Visual Studio **Output** 視窗。

  > [!div class="tabbedCodeSnippets"]
  ``` csharp
  private void OnAdError(object sender, AdErrorEventArgs e)
  {
      System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name +
          "): " + e.ErrorMessage + " ErrorCode: " + e.ErrorCode.ToString());
  }
  ```

4.  建置和執行專案。 執行 app 之後，您將會看到下面與 Visual Studio 之 **Output** 視窗中的訊息類似的訊息。

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  AdControl error (): MicrosoftAdvertising.Shared.AdException: all ad requests must use the same application ID within a single application (0, d25517cb-12d4-4699-8bdc-52040c712cab) ErrorCode: ClientConfiguration
  ```

## <a name="related-topics"></a>相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)

 
