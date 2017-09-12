---
author: mcleanbyron
ms.assetid: c0450f7b-5c81-4d8c-92ef-2b1190d18af7
description: "了解如何在 Windows Phone 8.1 或 Windows Phone 8.0 的 Silverlight 應用程式使用 AdControl 類別來顯示橫幅廣告。"
title: "Windows Phone Silverlight 中的 AdControl"
ms.author: mcleans
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 廣告, AdControl, Silverlight, Windows Phone"
ms.openlocfilehash: f1582639757abfb6de156bf88ce8af71ba3eaacd
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/21/2017
---
# <a name="adcontrol-in-windows-phone-silverlight"></a>Windows Phone Silverlight 中的 AdControl

本文會逐步說明如何在 Windows Phone 8.1 或 Windows Phone 8.0 的 Silverlight 應用程式使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/hh524191.aspx) 類別來顯示橫幅廣告。

<span id="silverlight_support"/>
## <a name="advertising-support-for-windows-phone-8x-silverlight-projects"></a>Windows Phone 8.x Silverlight 專案的廣告支援

Windows Phone 8.x Silverlight 專案中已不再支援某些開發人員案例。 如需詳細資訊，請參閱下表。

|  平台版本  |  現有專案    |   新專案  |
|-----------------|----------------|--------------|
| Windows Phone 8.0 Silverlight     |  如果您現有的 Windows Phone 8.0 Silverlight 專案是使用之前所推出的 Universal Ad Client SDK 或 Microsoft Advertising SDK 中的 **AdControl** 或 **AdMediatorControl**，且該應用程式已在 Windows 市集發行，則您可以修改並重新建置該專案，並在裝置上偵錯或測試變更。 不支援在模擬器中偵錯或測試專案。  |  不支援。  |
| Windows Phone 8.1 Silverlight    |  如果您現有的 Windows Phone 8.1 Silverlight 專案是使用先前 SDK 中的 **AdControl** 或 **AdMediatorControl**，您可以修改並重新建置該專案。 不過，若要偵錯或測試應用程式，您必須在模擬器中執行該應用程式，並針對「應用程式識別碼」和「廣告單位識別碼」使用[測試值](test-mode-values.md)。 不支援在裝置上偵錯或測試應用程式。  |   您可以在新的 Windows Phone 8.1 Silverlight 專案中新增 **AdControl** or **AdMediatorControl**。 不過，若要偵錯或測試應用程式，您必須在模擬器中執行該應用程式，並針對「應用程式識別碼」和「廣告單位識別碼」使用[測試值](test-mode-values.md)。 不支援在裝置上偵錯或測試應用程式。 |

## <a name="add-the-advertising-assemblies-to-your-project"></a>將廣告組件新增到您的專案

一開始，將包含適用於 Windows Phone Silverlight 之 Microsoft Advertising 組件的 NuGet 套件下載並安裝到您的專案。

1.  在 Visual Studio 中，開啟您的專案。

2.  按一下 **\[工具\]**，指向 **\[NuGet 套件管理員\]**，然後按一下 **\[套件管理器主控台\]**。

3.  在 **\[套件管理器主控台\]** 視窗中，輸入其中一個命令。

  * 如果您的專案是針對 Windows Phone 8.0，請輸入這個命令。

      ```syntax
      Install-Package Microsoft.Advertising.WindowsPhone.SL80 -Version 6.2.40501.1
      ```

  * 如果您的專案是針對 Windows Phone 8.1，請輸入這個命令。

      ```syntax
      Install-Package Microsoft.Advertising.WindowsPhone.SL81 -Version 8.1.50112
      ```

    輸入這個指令之後，適用於 Silverlight 的所有必要 Microsoft Advertising 組件都會透過 NuGet 套件下載到您的本機專案，而且對這些組件的參考會自動新增到您的專案。

## <a name="code-your-app"></a>撰寫應用程式程式碼


1.  在 WMAppManifest.xml 檔案中的 **Capabilities** 新增下列功能。

  ``` syntax
  <Capability Name="ID_CAP_IDENTITY_USER"/>
  <Capability Name="ID_CAP_MEDIALIB_PHOTO"/>
  <Capability Name="ID_CAP_PHONEDIALER"/>
  ```

  對於此範例，您的 **Capabilities** 看起來如下：

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

  ``` xml
  xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
  ```

  您頁面的標頭會有下列程式碼︰

  ``` xml
  xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
  xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
  x:Class="PhoneApp1.MainPage"
  ```

6.  在 **Grid** 標籤中，加入以下 **AdControl** 程式碼。 將 **ApplicationId** 和 **AdUnitId** 屬性指派給[測試模式屬性](test-mode-values.md)中提供的屬性，並將 **Height** 和 **Width** 屬性調整為其中一個[橫幅廣告支援的廣告大小](supported-ad-sizes-for-banner-ads.md)。

  > **注意**&nbsp;&nbsp;在提交應用程式之前，請以實際值取代測試的 **ApplicationId** 和 **AdUnitId** 值。

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

1.  在開發人員中心儀表板中，移至應用程式的 **\[創造營收\]** &gt; **\[利用廣告獲利\]** 頁面，並[建立獨立的廣告單元](../publish/monetize-with-ads.md)。 單位類型請指定 **\[橫幅\]**。 記下廣告單位識別碼與應用程式識別碼。

2.  在您的程式碼中，將測試的廣告單位值 (**applicationId** 和 **adUnitId**)，用在開發人員中心產生的實際值取代。

3.  使用開發人員中心儀表板[提交您的應用程式](../publish/app-submissions.md) 到市集。

4.  在開發人員中心儀表板中檢閱您的[廣告效益報表](../publish/advertising-performance-report.md)。


 
