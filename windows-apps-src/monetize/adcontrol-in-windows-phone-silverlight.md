---
author: mcleanbyron
ms.assetid: c0450f7b-5c81-4d8c-92ef-2b1190d18af7
description: "了解如何在 Windows Phone 8.1 或 Windows Phone 8.0 的 Silverlight 應用程式使用 AdControl 類別來顯示橫幅廣告。"
title: "Windows Phone Silverlight 中的 AdControl"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 廣告, AdControl, Silverlight, Windows Phone"
ms.openlocfilehash: 743b9faccaa120f1904b592fc09a965dc7878e03
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="adcontrol-in-windows-phone-silverlight"></a>Windows Phone Silverlight 中的 AdControl

本文會逐步說明如何在 Windows Phone 8.1 或 Windows Phone 8.0 的 Silverlight 應用程式使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/hh524191.aspx) 類別來顯示橫幅廣告。

> **Windows Phone Silverlight 8.0 的注意事項**&nbsp;&nbsp;使用 Universal Ad Client SDK 或 Microsoft Advertising SDK 較早版本的 **AdControl** 且已經在市集中的現有 Windows Phone 8.0 Silverlight 應用程式仍然支援橫幅廣告。 不過，新的 Windows Phone 8.0 Silverlight 專案不再支援橫幅廣告。 此外，Windows Phone 8.x Silverlight 專案中的某些偵錯和測試案例也有限。 如需詳細資訊，請參閱[在您的應用程式中顯示廣告](display-ads-in-your-app.md#silverlight_support)。

## <a name="add-the-advertising-assemblies-to-your-project"></a>將廣告組件新增到您的專案

一開始，將包含適用於 Windows Phone Silverlight 之 Microsoft Advertising 組件的 NuGet 套件下載並安裝到您的專案。

1.  在 Visual Studio 中，開啟您的專案。

2.  按一下 **\[工具\]**，指向 **\[NuGet 套件管理員\]**，然後按一下 **\[套件管理器主控台\]**。

3.  在 **\[套件管理器主控台\]** 視窗中，輸入其中一個命令。

  * 如果您的專案是針對 Windows Phone 8.0，請輸入這個命令。

      > [!div class="tabbedCodeSnippets"]
      ```syntax
      Install-Package Microsoft.Advertising.WindowsPhone.SL80 -Version 6.2.40501.1
      ```

  * 如果您的專案是針對 Windows Phone 8.1，請輸入這個命令。

      > [!div class="tabbedCodeSnippets"]
      ```syntax
      Install-Package Microsoft.Advertising.WindowsPhone.SL81 -Version 8.1.50112
      ```

    輸入這個指令之後，適用於 Silverlight 的所有必要 Microsoft Advertising 組件都會透過 NuGet 套件下載到您的本機專案，而且對這些組件的參考會自動新增到您的專案。

## <a name="code-your-app"></a>撰寫應用程式程式碼


1.  在 WMAppManifest.xml 檔案中的 **Capabilities** 新增下列功能。

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  <Capability Name="ID_CAP_IDENTITY_USER"/>
  <Capability Name="ID_CAP_MEDIALIB_PHOTO"/>
  <Capability Name="ID_CAP_PHONEDIALER"/>
  ```

  對於此範例，您的 **Capabilities** 看起來如下：

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  <Capabilities>
      <Capability Name="ID_CAP_NETWORKING"/>
      <Capability Name="ID_CAP_MEDIALIB_AUDIO"/>
      <Capability Name="ID_CAP_MEDIALIB_PLAYBACK"/>
      <Capability Name="ID_CAP_SENSORS"/>
      <Capability Name="ID_CAP_WEBBROWSERCOMPONENT"/>
      <Capability Name="ID_CAP_IDENTITY_USER"/>
      <Capability Name="ID_CAP_MEDIALIB_PHOTO"/>
      <Capability Name="ID_CAP_PHONEDIALER"/>
  </Capabilities>
  ```

2.  (選擇性) 儲存您的專案。 按一下 **\[檔案\]** 功能表底下的 **\[全部儲存\]** 圖示，按一下 **\[全部儲存\]**。

3.  將**「網際網路 (用戶端與伺服器)」**功能新增到專案中的 Package.appxmanifest 檔案 在 **\[方案總管\]** 中，按兩下 **\[Package.appxmanifest\]**。

    ![wp81silverlightmarkup\-solutionexplorer\-packageappxmanifest](images/13-b98c2a1a-69c3-4018-be0a-6ce010e703e7.jpg)

    在 **\[編輯器\]** 中，按一下 **\[功能\]** 索引標籤。 選取 **\[網際網路 (用戶端與伺服器)\]** 方塊。

4.  (選擇性) 儲存您的專案。 按一下 **\[檔案\]** 功能表底下的 **\[全部儲存\]** 圖示，按一下 **\[全部儲存\]**。

5.  修改 MainPage.xaml 檔案中的 Silverlight 標記，以包含 **Microsoft.Advertising.Mobile.UI** 命名空間。

  > [!div class="tabbedCodeSnippets"]
  ``` xml
  xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
  ```

  您頁面的標頭會有下列程式碼︰

  > [!div class="tabbedCodeSnippets"]
  ``` xml
  xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
  xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
  x:Class="PhoneApp1.MainPage"
  ```

6.  在 **Grid** 標籤中，加入以下 **AdControl** 程式碼。 將 **ApplicationId** 和 **AdUnitId** 屬性指派給[測試模式屬性](test-mode-values.md)中提供的屬性，並將 **Height** 和 **Width** 屬性調整為其中一個[橫幅廣告支援的廣告大小](supported-ad-sizes-for-banner-ads.md)。

  > **注意**&nbsp;&nbsp;在提交應用程式之前，請以實際值取代測試的 **ApplicationId** 和 **AdUnitId** 值。

  > [!div class="tabbedCodeSnippets"]
  ``` xml
  <Grid x:Name="ContentPanel" Grid.Row="1">
      <UI:AdControl
          ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
          AdUnitId="10865270"
          HorizontalAlignment="Left"
          Height="80"
          VerticalAlignment="Top"
          Width="480"/>
  </Grid>
  ```

7.  建置並執行專案。 確認您的應用程式會顯示廣告如下︰

  ![wp81silverlight\-emulatorwithad](images/13-8db1492f-ae1d-439b-9b78-bed8e22fe996.jpg)

## <a name="release-your-app-with-live-ads-using-dev-center"></a>使用開發人員中心發行包含即時廣告的應用程式

1.  在開發人員中心儀表板中，移至應用程式的 **\[創造營收\]** &gt; **\[利用廣告獲利\]** 頁面，並[建立獨立的 Microsoft Advertising 單位](../publish/monetize-with-ads.md)。 單位類型請指定 **\[橫幅\]**。 記下廣告單位識別碼與應用程式識別碼。

2.  在您的程式碼中，將測試的廣告單位值 (**applicationId** 和 **adUnitId**)，用在開發人員中心產生的實際值取代。

3.  使用開發人員中心儀表板[提交您的應用程式](../publish/app-submissions.md) 到市集。

4.  在開發人員中心儀表板中檢閱您的[廣告效益報表](../publish/advertising-performance-report.md)。


 
