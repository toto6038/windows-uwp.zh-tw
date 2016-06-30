---
author: TylerMSFT
title: "啟動 URI 的預設 app"
description: "了解如何啟動統一資源識別項 (URI) 的預設 app。 URI 可讓您啟動另一個 app 來執行特定工作。 本主題也提供許多內建於 Windows 之 URI 配置的概觀。"
ms.assetid: 7B0D0AF5-D89E-4DB0-9B79-90201D79974F
ms.sourcegitcommit: 9011d2e2e1e51edc89851e815d31e13390c24f96
ms.openlocfilehash: d454317d135e2b2b952c16fb00685e34b489865c

---

# 啟動 URI 的預設 app


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

了解如何啟動統一資源識別項 (URI) 的預設 app。 URI 可讓您啟動另一個 app 來執行特定工作。 本主題也提供許多內建於 Windows 之 URI 配置的概觀。 您也可以啟動自訂 URI。 如需關於登錄自訂 URI 配置和處理 URI 啟用的詳細資訊，請參閱[處理 URI 啟用](handle-uri-activation.md)。

## 如何啟動 URI


URI 配置可讓您按一下超連結以開啟 app。 就像您可以使用 **mailto:** 建立新的電子郵件一樣，您可以使用 **http:** 來開啟預設網頁瀏覽器。 本主題描述一些內建於 Windows 的 URI 配置：

-   [ms-settings: URI 配置](#settings)啟動 Windows 設定 app
-   [ms-store: URI 配置](#store)會啟動 Windows 市集 app
-   [http: URI 配置](#browser)會啟動預設網頁瀏覽器
-   [mailto: URI 配置](#email)會啟動預設電子郵件 app
-   [bingmaps:、ms-drive-to: 和 ms-walk-to: URI 配置](#maps)會啟動 Windows 地圖 app

例如，下列 URI 會開啟預設瀏覽器，並顯示 Bing 網站。

`http://bing.com`

您也可以啟動自訂 URI 配置。 如果未安裝可處理該 URI 的 app，您可以建議使用者安裝某個 app。 如需詳細資訊，請參閱[建議 app](#recommend)。

您的 app 通常無法選取已啟動的 app。 使用者決定要啟動哪個 app。 您可以登錄多個 app 來處理相同的 URI 配置。 但對保留的 URI 配置則例外。 若登錄保留的 URI 配置，將會被忽略。 如需保留 URI 配置的完整清單，請參閱[處理 URI 啟用](handle-uri-activation.md)。 如果有多個 app 可能已登錄相同的 URI 配置，您的 app 可能會建議啟動特定 app。 如需詳細資訊，請參閱[建議 app](#recommend)。

### 呼叫 LaunchUriAsync

使用 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 方法來啟動 URI。 呼叫此方法時，您的 app 必須是前景 app，也就是說，使用者必須看得到您的 app。 這項需求可讓使用者握有控制權。 為了滿足這項需求，請務必將所有 URI 啟動直接繫結到您的應用程式 UI。 使用者一律必須採取某些動作，才能起始 URI 啟動。 如果您嘗試啟動 URI，但您的 app 不在前景，則啟動將會失敗，並會叫用您的錯誤回呼。

首先，建立 [**System.Uri**](https://msdn.microsoft.com/en-us/library/windows/apps/system.uri.aspx) 物件來代表 URI，然後將它傳送到 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 方法。 使用傳回結果查看呼叫是否成功，如下列範例所示。

```cs
private async void launchURI_Click(object sender, RoutedEventArgs e)
{
   // The URI to launch
   var uriBing = new Uri(@"http://www.bing.com");

   // Launch the URI
   var success = await Windows.System.Launcher.LaunchUriAsync(uriBing);

   if (success)
   {
      // URI launched
   }
   else
   {
      // URI launch failed
   }
}
```

在某些情況下，作業系統會提示使用者確認是否真的要切換 app。

![警告對話方塊會顯示在 app 變暗的背景上。 對話方塊會詢問使用者是否想要切換 app，並在右下方顯示 \[是\] 和 \[否\] 按鈕。 \[否\] 按鈕會醒目顯示。](images/warningdialog.png)

如果您一律要顯示此提示，請使用 [**Windows.System.LauncherOptions.TreatAsUntrusted**](https://msdn.microsoft.com/library/windows/apps/hh701442) 屬性指示作業系統顯示警告。

```cs
// The URI to launch
var uriBing = new Uri(@"http://www.bing.com");

// Set the option to show a warning
var promptOptions = new Windows.System.LauncherOptions();
promptOptions.TreatAsUntrusted = true;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriBing, promptOptions);
```

### 建議 app

在某些情況下，使用者可能尚未安裝可處理您要啟動之 URI 的 app。 依照預設，作業系統處理這些情況的方法是提供連結，讓使用者在市集上搜尋適當的應用程式。 如果您想提供使用者在此情況下應取得何種 app 的特定建議，可以在啟動 URI 時一併傳送該建議。

在多個 app 已登錄要處理 URI 配置時，建議十分有用。 在建議特定 app 時，如果該 app 已安裝，Windows 將會加以開啟。

若要提供建議，請呼叫 [**Windows.System.Launcher.LaunchUriAsync(Uri, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701484) 方法，並將 [**LauncherOptions.preferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482) 設為您想建議使用者使用的市集應用程式套件系列名稱。 作業系統會使用此資訊，並搭配從市集取得建議 app 的特定選項，以取代在市集中搜尋 app 的一般選項。

```cs
// Set the recommended app
var options = new Windows.System.LauncherOptions();
options.PreferredApplicationPackageFamilyName = "Contoso.URIApp_8wknc82po1e";
options.PreferredApplicationDisplayName = "Contoso URI Ap";

// Launch the URI and pass in the recommended app
// in case the user has no apps installed to handle the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

### 設定其餘檢視喜好設定

呼叫 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 的來源 app 可要求在 URI 啟動後停留在畫面上。 根據預設，Windows 會嘗試將所有可用空間平均分享給來源 app 與用來處理 URI 的目標 app。 來源 app 可以使用 [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) 屬性，告知作業系統要讓 app 視窗佔用較多或較少可用空間。 您也可以使用 **DesiredRemainingView**，指示來源 app 在 URI 啟動後不需要停留在畫面上，且可由目標 app 完全取代。 這個屬性只會指定發出呼叫的 app 的慣用視窗大小。 它不會指定其他可能也同時在螢幕上之 app 的行為。

**注意** Windows 在判斷來源 app 的最終視窗大小時，會考量多種不同因素，例如來源 app 的喜好設定、螢幕上的 app 數目、螢幕方向等。 設定 [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) 並無法保證來源 app 的特定視窗行為。

 

```cs
// Set the desired remaining view.
var options = new Windows.System.LauncherOptions();
options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

## Windows 地圖 app URI 配置


您的 app 可以使用 **bingmaps:**、**ms-drive-to:** 和 **ms-walk-to:** URI 配置，針對特定的地圖、方向和搜尋結果[啟動 Windows 地圖 app](launch-maps-app.md)。 例如，下列 URI 會開啟 Windows 地圖 app，並顯示以紐約市為中心的地圖。

`bingmaps:?cp=40.726966~-74.006076`

![Windows 地圖 app 的範例。](images/mapnyc.png)

如需詳細資訊，請參閱[啟動 Windows 地圖 app](launch-maps-app.md)。 若要在您自己的 app 中使用地圖控制項，請參閱[顯示地圖的 2D、3D 和 Streetside 檢視](https://msdn.microsoft.com/library/windows/apps/mt219695)。

## Windows 設定 app URI 配置


您的 app 可以使用 **ms-settings:** URI 配置來[啟動 Windows 設定 app](launch-settings-app.md)。 啟動設定 app 是撰寫隱私權感知 app 的重要部分。 如果您的 app 無法存取敏感資源，建議讓使用者能夠方便地連結到該資源的隱私權設定。 例如，下列 URI 會開啟設定 app，並顯示相機隱私權設定。

`ms-settings:privacy-webcam`

![相機隱私權設定。](images/privacyawarenesssettingsapp.png)

如需詳細資訊，請參閱[啟動 Windows 設定 app](launch-settings-app.md) 和[隱私權感知 app 的指導方針](https://msdn.microsoft.com/library/windows/apps/hh768223)。

## Windows 市集 app URI 配置


您的 app 可以使用 **ms-windows-store:** URI 配置來[啟動 Windows 市集 app](launch-store-app.md)。 開啟產品詳細資料頁面、產品檢閱頁面及搜尋頁面等等。例如，下列 URI 會開啟 Windows 市集 app，並啟動 \[市集\] 的首頁。

`ms-windows-store://home/`

如需詳細資訊，請參閱[啟動 Windows 市集 app](launch-store-app.md)。

## 撥號 app URI 配置


您的 app 可以使用 **ms-call:** URI 配置來啟動撥號 app。

| URI 配置       | 結果                               |
|------------------|---------------------------------------|
| ms-call:settings | 啟動撥號 app 設定頁面。 |

 

## 聊天 app URI 配置


您的 app 可以使用 **ms-chat:** URI 配置來啟動訊息中心 app。

| URI 配置                               | 結果                                                                                                                                                                                |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ms-chat:                                 | 啟動訊息中心 app。                                                                                                                                                            |
| ms-chat:?ContactID={contacted}           | 可讓訊息中心應用程式以特定連絡人的資訊啟動。                                                                                               |
| ms-chat:?Body={body}                     | 可讓訊息中心應用程式以做為訊息內容的字串啟動。                                                                                    |
| ms-chat:?Addresses={address}&amp;Body={body} | 可讓訊息中心應用程式以特定地址的資訊，以及做為訊息內容的字串啟動。 注意：地址可以串連。 |
| ms-chat:?TransportId={transportId}       | 可讓訊息中心應用程式以特定傳輸識別碼啟動。                                                                                                        |

 

## 電子郵件 URI 配置


您的 app 可以使用 **mailto:** URI 配置來啟動預設郵件 app。

| URI 配置               | 結果                                                                                                                                                     |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| mailto:                  | 啟動預設電子郵件 app。                                                                                                                             |
| mailto:\[email address\] | 啟動電子郵件 app，並以指定於 \[收件者\] 行上的電子郵件地址建立新訊息。 請注意，在使用者點選「傳送」之後，才會傳送電子郵件。 |

 

## HTTP URI 配置


您的 app 可以使用 **http:** URI 配置來啟動預設網頁瀏覽器。

| URI 配置 | 結果                           |
|------------|-----------------------------------|
| http:      | 啟動預設網頁瀏覽器。 |

 

## 附近號碼 app URI 配置


您的 app 可以使用 **ms-yellowpage:** URI 配置來啟動附近號碼 app。

| URI 配置                                            | 結果                                                                               |
|-------------------------------------------------------|---------------------------------------------------------------------------------------|
| ms-yellowpage:?input=\[keyword\]&amp;method=\[String|T9\] | 啟動已安裝且支援這個新 URI 的參考資訊 (POI) 搜尋 app。 |

 

## 連絡人 app URI 配置


您的 app 可以使用 **ms-people:** URI 配置來啟動連絡人 app。

如需詳細資訊，請參閱[啟動連絡人 app](launch-people-apps.md)。

 

 



<!--HONumber=Jun16_HO4-->


