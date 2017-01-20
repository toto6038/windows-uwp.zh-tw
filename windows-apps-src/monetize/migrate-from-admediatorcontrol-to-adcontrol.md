---
author: mcleanbyron
ms.assetid: 9621641A-7462-425D-84CC-101877A738DA
description: "了解如何從 UWP App 的 AdMediatorControl AdControl 中進行移轉。"
title: "針對 UWP App 從 AdMediatorControl 移轉到 AdControl"
translationtype: Human Translation
ms.sourcegitcommit: 2b5dbf872dd7aad48373f6a6df3dffbcbaee8090
ms.openlocfilehash: 6e7e833327dce4b49e44b7485908c8a507b217ef

---

# <a name="migrate-from-admediatorcontrol-to-adcontrol-for-uwp-apps"></a>針對 UWP App 從 AdMediatorControl 移轉到 AdControl

先前的 Advertising SDK 會從 Microsoft 啟用的通用 Windows 平台 (UWP) App 發行，以使用 **AdMediatorControl** 類別來顯示橫幅廣告，讓開發人員能夠藉由顯示來自我們合作夥伴網路 (AOL 和 AppNexus) 以及 AdDuplex 的橫幅廣告，以獲得最佳廣告收益。 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) 不再支援 **AdMediatorControl** 類別。 如果您現有的應用程式會使用來自舊版 SDK 的 **AdMediatorControl** 類別，而您想要將它移轉到使用 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) 的 UWP App，請遵循本文中的指示，更新您的程式碼來使用 **AdControl** 類別而不是 **AdMediatorControl** 類別。 您可以選擇性地使用加權或排名方法，設定 App 以利用 AdDuplex 傳達廣告。

>**注意**&nbsp;&nbsp;本文所提供的程式碼範例僅供說明之用。 您可能需要對程式碼範例進行調整，才能在 App 中使用。

## <a name="prerequisites"></a>先決條件

* 目前正在使用 AdMediatorControl 且已在 Windows 市集發行的 UWP App。
* 一部已安裝 Visual Studio 2015 和 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) 的開發電腦。
* 如果您想要利用 AdDuplex 傳達廣告，也必須要在開發電腦上安裝 [AdDuplex Windows 10 SDK](https://visualstudiogallery.msdn.microsoft.com/6930860a-e64b-4b46-9d72-62d7fddda077)。

  >**注意**&nbsp;&nbsp;除了從上方連結執行 AdDuplex SDK 安裝程式之外，您也可以在 Visual Studio 2015 中安裝適用於您 UWP App 專案的 AdDuplex 程式庫。 在 Visual Studio 2015 中開啟您的 UWP App 專案，按一下 [專案]**** > [管理 NuGet 套件]****、搜尋名為 **AdDuplexWin10** 的 NuGet 套件，然後安裝該套件。

## <a name="step-1-retrieve-your-application-ids-and-ad-unit-ids"></a>步驟 1︰擷取您的應用程式識別碼和廣告單位識別碼

當您移轉程式碼以使用 **AdControl** 類別時，您必須知道應用程式識別碼和廣告單位識別碼。 取得最新識別碼的最好方法是從您的流量分配設定檔中擷取它們。

1. 登入 Windows 開發人員中心儀表板，然後按一下目前使用 **AdMediatorControl** 的 App。
2. 依序按一下 [創造營收]**** 和 [利用廣告獲利]****。
3. 在 [Windows 廣告流量分配]**** 區段中，按一下 [下載流量分配設定]**** 連結，並在文字編輯器 (例如 [記事本]) 中開啟 AdMediator.config 檔案。
4. 在檔案中，找出含有子元素 ```<Name>MicrosoftAdvertising</Name>``` 的 ```<AdAdapterInfo>``` 元素。 這個區段包含 Microsoft 付費廣告的設定。
5. 在這個 ```<AdAdapterInfo>``` 元素中，找出包含 ```<Key>``` 元素且值為 **WApplicationId** 和 **WAdUnitId** 的 ```<Property>```。 在下列範例中，```<Value>``` 元素的值即為範例。

  > [!div class="tabbedCodeSnippets"]
  ```xml
  <Metadata>
      <Property>
          <Key>WApplicationId</Key>
          <Value>364d4938-c0f5-4c3d-8aae-090206211dc9</Value>
      </Property>
      <Property>
          <Key>WAdUnitId</Key>
          <Value>301568</Value>
      </Property>
  </Metadata>
  ```

6. 在這些 ```<Value>``` 元素中複製這兩個值，以供稍後使用。 這些值所包含的應用程式識別碼和廣告單位識別碼適用於 Microsoft 付費廣告之非行動裝置版的廣告單位。
5. 在同一個 ```<AdAdapterInfo>``` 元素中，找出包含 ```<Key>``` 元素且值為 **MApplicationId** 和 **MAdUnitId** 的 ```<Property>``` 元素。 在下列範例中，```<Value>``` 元素的值即為範例。

  > [!div class="tabbedCodeSnippets"]
  ```xml
  <Metadata>
      <Property>
          <Key>MApplicationId</Key>
          <Value>e2cf8388-7018-4a11-8ab0de90f2a7a401</Value>
      </Property>
      <Property>
          <Key>MAdUnitId</Key>
          <Value>301056</Value>
      </Property>
  </Metadata>
  ```

6. 在 ```<Value>``` 元素中複製這兩個值，以供稍後使用。 這些值所包含的應用程式識別碼和廣告單位識別碼適用於 Microsoft 付費廣告之行動裝置版的廣告單位。
7. 如果您使用[自家廣告](../publish/about-house-ads.md)，請找出含有子元素 ```<Name>MicrosoftAdvertisingHouse</Name>``` 的 ```<AdAdapterInfo>``` 元素。 在此元素中，找出值為 **MAdUnitId** 和 **WAdUnitId** 的 ```<Key>``` 元素，然後儲存對應 ```<Value>``` 元素的值，以供稍後使用。 這些分別是適用於 Microsoft 自家廣告的行動裝置和非行動裝置版的廣告單位識別碼。
8. 如果您使用 AdDuplex，請找出含有子元素 ```<Name>AdDuplex</Name>``` 的 ```<AdAdapterInfo>``` 元素。 在此元素中，找出值為 **AppKey** 和 **AdUnitId** 的 ```<Key>``` 元素，然後儲存對應 ```<Value>``` 元素的值，以供稍後使用。 這分別是您的 AdDuplex App 金鑰和廣告單位識別碼。

## <a name="step-2-update-your-app-code"></a>步驟 2︰更新您的 App 程式碼

現在，您擁有應用程式識別碼和廣告單位識別碼，您已經準備好更新 App 中的程式碼來使用 **AdControl** 而不是 **AdMediatorControl**。

### <a name="microsoft-paid-ads-only"></a>只使用 Microsoft 付費廣告

如果您只會在廣告流量分配設定中使用 Microsoft 付費廣告，請依照下列步驟進行。

  >**注意**&nbsp;&nbsp;這些步驟假設您想要顯示廣告的 App 頁面包含名為 **myAdGrid** 的空方格，例如︰```<Grid x:Name="myAdGrid"/>```。 在這些步驟中，您將完全使用程式碼來建立並設定廣告控制項，而不使用 XAML。

1. 在 Visual Studio 中，開啟您的 UWP App 專案。
2.  在 [方案總管]**** 視窗中的 [參考]**** 上按一下滑鼠右鍵，然後選取 [加入參考…]****。
在 [參考管理員]**** 中，展開 [通用 Windows]****、按一下 [擴充功能]****，然後選取 [適用於 XAML 的 Microsoft Advertising SDK (Version 10.0)]**** 旁邊的核取方塊。
3. 在 [參考管理員]**** 中，按一下 [確定]。
4. 從您的 XAML 移除 **AdMediatorControl** 宣告，並移除任何使用這個 **AdMediatorControl** 物件的程式碼，包括任何相關的事件處理常式。
5. 針對您想要顯示廣告的 App **Page**，開啟程式碼檔案。
6. 請將下列陳述式新增到程式碼檔案頂端 (如果不存在)。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[TrialVersion](./code/AdvertisingSamples/MigrateToAdControl/cs/MainPage.xaml.cs#Snippet1)]

7. 將下列常數宣告新增到您的 **Page** 類別。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[TrialVersion](./code/AdvertisingSamples/MigrateToAdControl/cs/MainPage.xaml.cs#Snippet2)]

7. 針對這其中的每個常數宣告，依照底下所述方式來取代值：

  * **AD_WIDTH** 和 **AD_HEIGHT**︰為這些項目指派其中一個[橫幅廣告支援的廣告大小]( https://msdn.microsoft.com/windows/uwp/monetize/supported-ad-sizes-for-banner-ads)。
  * **WAPPLICATIONID** 和 **WADUNITID**︰針對您稍早從流量分配設定檔中擷取的 Microsoft 付費廣告，為這些項目指派 **WApplicationId** 和 **WAdUnitId** 值 (這些值適用於付費廣告之非行動裝置版的廣告單位)。
  * **MAPPLICATIONID** 和 **MADUNITID**︰針對您稍早從流量分配設定檔中擷取的 Microsoft 付費廣告，為這些項目指派 **MApplicationId** 和 **MAdUnitId** 值 (這些值適用於付費廣告之行動裝置版的廣告單位)。

8. 將下列變數宣告新增到您的 **Page** 類別。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/MainPage.xaml.cs#Snippet3)]

5. 將下列程式碼新增到您的 **Page** 類別建構函式 (放在對 **InitializeComponent()** 方法的呼叫之後)。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/MainPage.xaml.cs#Snippet4)]

### <a name="microsoft-paid-ads-house-ads-and-adduplex"></a>Microsoft 付費廣告、自家廣告及 AdDuplex

如果您使用 Microsoft 自家廣告或 AdDuplex 以及 Microsoft 付費廣告，而且想要繼續利用 AdDuplex 傳達廣告，請依照本節中的步驟進行。 程式碼範例同時支援 AdDuplex 和 Microsoft 自家廣告。 如果您使用 AdDuplex 而不是 Microsoft 自家廣告 (反之亦然)，請移除您的案例不適用的程式碼。

  >**注意**&nbsp;&nbsp;這些步驟假設您想要顯示廣告的 App 頁面包含名為 **myAdGrid** 的空方格，例如︰```<Grid x:Name="myAdGrid"/>```。 在這些步驟中，您將完全使用程式碼來建立並設定廣告控制項，而不使用 XAML。

1. 在 Visual Studio 中，開啟您的 UWP App 專案。
2.  在 [方案總管]**** 視窗中的 [參考]**** 上按一下滑鼠右鍵，然後選取 [加入參考…]****。
在 [參考管理員]**** 中，展開 [通用 Windows]****、按一下 [擴充功能]****，然後選取 [適用於 XAML 的 Microsoft Advertising SDK (Version 10.0)]**** 旁邊的核取方塊。
3. 在 [參考管理員]**** 中，按一下 [確定]。
4. 從您的 XAML 移除 **AdMediatorControl** 宣告，並移除任何使用這個 **AdMediatorControl** 物件的程式碼，包括任何相關的事件處理常式。
5. 針對您想要顯示廣告的 App **Page**，開啟程式碼檔案。
6. 請將下列陳述式新增到程式碼檔案頂端 (如果它們尚未存在)。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/ExamplePage1.xaml.cs#Snippet1)]

7. 將下列常數宣告新增到您的 **Page** 類別。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/ExamplePage1.xaml.cs#Snippet2)]

4. 針對這其中的常數宣告，依照底下所述方式來取代值：

  * **AD_WIDTH** 和 **AD_HEIGHT**︰為這些項目指派其中一個[橫幅廣告支援的廣告大小]( https://msdn.microsoft.com/windows/uwp/monetize/supported-ad-sizes-for-banner-ads)。
  * **HOUSE_AD_WEIGHT**︰為此項指派 0 到 100 的整數，以指定相較於 Microsoft 付費廣告，您想要套用到 Microsoft 自家廣告的權數值 (其中 0 表示永遠不顯示自家廣告，100 則是一律顯示自家廣告)。
  * **WAPPLICATIONID** 和 **WADUNITID_PAID**︰針對您稍早從流量分配設定檔中擷取的 Microsoft 付費廣告，為這些項目指派 **WApplicationId** 和 **WAdUnitId** 值 (這些值適用於付費廣告之非行動裝置版的廣告單位)。
  * **WADUNITID_HOUSE**︰針對您稍早從流量分配設定檔中擷取的自家廣告，為此項指派 **WAdUnitId** (這個值適用於自家廣告之非行動裝置版的廣告單位)。
  * **MAPPLICATIONID** 和 **MADUNITID_PAID**︰針對您稍早從流量分配設定檔中擷取的 Microsoft 付費廣告，為這些項目指派 **MApplicationId** 和 **MAdUnitId** 值 (這些值適用於付費廣告之行動裝置版的廣告單位)。
  * **MADUNITID_HOUSE**︰針對您稍早從流量分配設定檔中擷取的自家廣告，為此項指派 **MAdUnitId** (這個值適用於自家廣告之行動裝置版的廣告單位)。
  * **ADDUPLEX_APPKEY** 和 **ADDUPLEX_ADUNIT**︰為這些項目指派 AdDuplex App 金鑰以及您稍早從流量分配設定檔中擷取的廣告單位識別碼值。

  >**注意**&nbsp;&nbsp;請勿變更上一個範例中顯示的 **AD_REFRESH_SECONDS** 和 **MAX_ERRORS_PER_REFRESH** 值。

5. 將下列變數宣告新增到您的 **Page** 類別。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/ExamplePage1.xaml.cs#Snippet3)]

5. 將下列程式碼新增到您的 **Page** 類別建構函式 (放在對 **InitializeComponent()** 方法的呼叫之後)。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/ExamplePage1.xaml.cs#Snippet4)]

6. 最後，將下列方法新增到您的 **Page** 類別。 這些方法會將 Microsoft **AdControl** 和 AdDuplex **AdControl** 物件具現化，它們會使用隨機數字產生器搭配使用指定的權數值，以定期計時器間隔重新整理這些控制項中的橫幅廣告。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/ExamplePage1.xaml.cs#Snippet5)]



<!--HONumber=Dec16_HO2-->


