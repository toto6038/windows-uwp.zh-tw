---
author: mijacobs
Description: "App 在 \\[開始\\] 功能表上以磚的形式顯示。 每個 app 都會有一個磚。 當您在 Microsoft Visual Studio 中建立新的通用 Windows 平台 (UWP) app 專案時，它會包含顯示 app 名稱和標誌的預設磚。"
title: "磚"
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
label: Tiles
template: detail.hbs
ms.sourcegitcommit: d3fe62d4de00c42079d62d105acdbb21e296ba5f
ms.openlocfilehash: a9f5d25dfd359364fa8e16666b03c7c105a867dd

---

# 適用於 UWP App 的磚





App 在 \[開始\] 功能表上以磚的形式顯示。 每個 app 都會有一個磚。 當您在 Microsoft Visual Studio 中建立新的通用 Windows 平台 (UWP) app 專案時，它會包含顯示 app 名稱和標誌的預設磚。 Windows 會在第一次安裝 app 時顯示這個磚。 安裝 app 之後，您可以透過通知變更磚的內容。例如，您可以變更磚以傳遞新的資訊 (例如新聞頭條或最新未讀郵件的主旨) 給使用者。

## <span id="Configure_the_default_tile"></span><span id="configure_the_default_tile"></span><span id="CONFIGURE_THE_DEFAULT_TILE"></span>設定預設磚


在 Visual Studio 中建立新的專案時，它會建立一個顯示 app 名稱和標誌的簡單預設磚。

```XML
  <Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="ExampleApp.App">
      <uap:VisualElements
        DisplayName="ExampleApp"
        Square150x150Logo="Assets\Logo.png"
        Square44x44Logo="Assets\SmallLogo.png"
        Description="ExampleApp"
        BackgroundColor="#464646">
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
    </Application>
  </Applications>
```

以下是您應該更新的幾個項目。

-   DisplayName：使用您想要在磚上顯示的名稱來取代此值。
-   ShortName：因為磚上可容納顯示名稱的空間有限，建議您另外指定 ShortName，以確保您的 app 名稱不會被截斷。
-   標誌影像：

    您應該以自己的影像取代這些影像。 您可以選擇為不同的視覺比例提供影像，但不需全部提供。 若要確保您的 app 在各種裝置上有很好的顯示效果，我們建議您提供每個影像的 100%、200% 及 400% 比例版本。

    縮放影像按照以下命名慣例： 
    
    &lt;影像名稱&gt;.scale-&lt;縮放比例&gt;.&lt;影像檔案副檔名&gt;  
    
    例如：SmallLogo.scale-100.png

    參考影像時，您將以 *&lt;影像名稱&gt;*.*&lt;影像檔案副檔名&gt;* 的格式來參考它 (在此範例中為 "SmallLogo.png")。 系統會從您提供的影像中，為裝置自動選取適當的縮放影像。

-   您不需要 (但強烈建議您) 提供適用於寬形磚和大型磚大小的標誌，方便使用者可以將 App 的磚調整成那些尺寸。 若要提供這些額外的影像，您可以建立 `DefaultTile` 元素，並使用 `Wide310x150Logo` 和 `Square310x310Logo` 屬性來指定其他影像：
```    XML
  <Applications>
        <Application Id="App"
          Executable="$targetnametoken$.exe"
          EntryPoint="ExampleApp.App">
          <uap:VisualElements
            DisplayName="ExampleApp"
            Square150x150Logo="Assets\Logo.png"
            Square44x44Logo="Assets\SmallLogo.png"
            Description="ExampleApp"
            BackgroundColor="#464646">
            <uap:DefaultTile
              Wide310x150Logo="Assets\WideLogo.png"
              Square310x310Logo="Assets\LargeLogo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
      </Applications>
```

## <span id="Use_notifications_to_customize_your_tile"></span><span id="use_notifications_to_customize_your_tile"></span><span id="USE_NOTIFICATIONS_TO_CUSTOMIZE_YOUR_TILE"></span>使用通知來自訂磚


安裝 App 後，您可以使用通知來自訂磚。 您可以在第一次啟動 App 或在回應某些事件 (例如推播通知) 時進行這個動作。

1.  建立描述磚的 XML 裝載 (以 [**Windows.Data.Xml.Dom.XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br206173) 的形式)。

    -   Windows 10 引進了新的彈性磚結構描述供您使用。 如需說明，請參閱[彈性磚](tiles-and-notifications-create-adaptive-tiles.md)。 如需結構描述的資訊，請參閱[彈性磚結構描述](tiles-and-notifications-adaptive-tiles-schema.md)。 

    -   您可以使用 Windows 8.1 磚範本來定義您的磚。 如需詳細資訊，請參閱[建立磚與徽章 (Windows 8.1)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868260)。

2.  建立磚通知物件，並將它傳送至您所建立的 [**XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br206173)。 通知物件有數種類型：
    -   可立即更新磚的 [**Windows.UI.NotificationsTileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616) 物件。
    -   可在未來某個時間點更新磚的 [**Windows.UI.Notifications.ScheduledTileNotification**](https://msdn.microsoft.com/library/windows/apps/hh701637) 物件。

3.  使用 [**Windows.UI.Notifications.TileUpdateManager.CreateTileUpdaterForApplication**](https://msdn.microsoft.com/library/windows/apps/br208623) 建立 [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628) 物件。
4.  呼叫 [**TileUpdater.Update**](https://msdn.microsoft.com/library/windows/apps/br208632) 方法，並將它傳遞到您在步驟 2 中所建立的磚通知物件。

 

 







<!--HONumber=Jun16_HO4-->


