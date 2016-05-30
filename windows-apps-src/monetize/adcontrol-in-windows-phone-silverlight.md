---
author: mcleanbyron
ms.assetid: c0450f7b-5c81-4d8c-92ef-2b1190d18af7
description: 了解如何在 Windows Phone 8.1 或 Windows Phone 8.0 的 Silverlight app 使用 AdControl 類別來顯示橫幅廣告。
title: Windows Phone Silverlight 中的 AdControl

---

# Windows Phone Silverlight 中的 AdControl


\[ 針對 Windows 10 上的 UWP App 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本文會逐步說明如何在 Windows Phone 8.1 或 Windows Phone 8.0 的 Silverlight app 使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/hh524191.aspx) 類別來顯示橫幅廣告。

## 先決條件

*  使用 Visual Studio 2015 或 Visual Studio 2013 安裝 [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk)。


## 新增廣告組件參考

適用於 Windows Phone Silverlight 專案的 Microsoft Advertising 組件不會隨 Microsoft Store Engagement 和 Monetization SDK 安裝在本機。 在您可以開始更新程式碼之前，您必須先搭配使用「已連接服務」**** 與 Microsoft Store Engagement 和 Monetization SDK 中支援的廣告流量分配，以下載組件並在專案中加入其參考。

1.  在 Visual Studio 中，依序按一下 [專案]****、[加入已連接服務]****。

2.  在 [加入已連接服務]**** 對話方塊中，按一下 [Ad Mediator]****，然後按一下 [設定]****。

3.  按一下 [選取廣告聯播網]****，並只選取 [Microsoft Advertising]****。

    此時，適用於 Silverlight 的所有必要 Microsoft Advertising 組件都會透過 NuGet 封裝下載到您的本機專案，而且對這些組件的參考會自動新增到您的專案。 廣告流量分配組件的參考也會新增到專案。 在稍後的步驟中會移除廣告流量分配組件，因為此案例中不需要它。

4.  在 [選取廣告聯播網]**** 對話方塊中，按一下 [確定]****。 於下面隨即顯示的 [擷取狀態]**** 頁面中再按一次 [確定]****，最後按一下 [加入]**** 來關閉 [Ad Mediator]**** 對話方塊。

5.  在 [方案總管]**** 中，展開 [參考]**** 節點。 以滑鼠右鍵按一下 [Microsoft.AdMediator.WindowsPhone81SL.MicrosoftAdvertising]****，然後按一下 [移除]****。 這個案例中不需要此組件參考。

## 撰寫 App 程式碼


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

2.  (選擇性) 儲存您的專案。 按一下 [檔案]**** 功能表底下的 [全部儲存]**** 圖示，按一下 [全部儲存]****。

3.  將「網際網路 (用戶端與伺服器)」**** 功能新增到專案中的 Package.appxmanifest 檔案 在 [方案總管]**** 中，按兩下 [Package.appxmanifest]****。

    ![wp81silverlightmarkup\-solutionexplorer\-packageappxmanifest](images/13-b98c2a1a-69c3-4018-be0a-6ce010e703e7.jpg)

    在 [編輯器]**** 中，按一下 [功能]**** 索引標籤。 選取 [網際網路 (用戶端與伺服器)]**** 方塊。

4.  (選擇性) 儲存您的專案。 按一下 [檔案]**** 功能表底下的 [全部儲存]**** 圖示，按一下 [全部儲存]****。

5.  修改 MainPage.xaml 檔案中的 Silverlight 標記，以包含 **Microsoft.Advertising.Mobile.UI** 命名空間。

    ``` syntax
    xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
    ```

    您頁面的標頭會有下列程式碼︰

    ``` syntax
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
        x:Class="PhoneApp1.MainPage"
    ```

6.  在 **Grid** 標籤中，加入以下 **AdControl** 程式碼。 將 **ApplicationId** 和 **AdUnitId** 屬性指派給[測試模式屬性](test-mode-values.md)中提供的屬性，並將 **Height** 和 **Width** 屬性調整為其中一個[橫幅廣告支援的廣告大小](supported-ad-sizes-for-banner-ads.md)。

    > **注意**  
    在提交 App 之前，請以實際值取代測試的 **ApplicationId** 和 **AdUnitId** 值。

    ``` syntax
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

7.  建置並執行專案。 確認您的 App 會顯示廣告如下︰

    ![wp81silverlight\-emulatorwithad](images/13-8db1492f-ae1d-439b-9b78-bed8e22fe996.jpg)

## 使用開發人員中心發行包含即時廣告的 App


1.  在開發人員中心儀表板中，移至 App 的 [營利]****&gt;[利用廣告營利]**** 頁面，並[建立獨立的 Microsoft Advertising 單位](../publish/monetize-with-ads.md)。 單位類型請選取 [橫幅]****。 記下廣告單位識別碼與應用程式識別碼。

2.  在您的程式碼中，將測試的廣告單位值 (**applicationId** 和 **adUnitId**)，用在開發人員中心產生的實際值取代。

3.  使用開發人員中心儀表板[提交您的 App](../publish/app-submissions.md) 到市集。

4.  在開發人員中心儀表板上檢閱[廣告績效報告](../publish/advertising-performance-report.md)。


 


<!--HONumber=May16_HO2-->


