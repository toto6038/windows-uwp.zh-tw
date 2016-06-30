---
author: mcleanbyron
ms.assetid: 86D9D3CF-8FDC-4B67-881B-DF33A1BEE8BF
description: "使用廣告流量分配之前，您將需要為每個想要在應用程式中使用的廣告網路設定帳戶。"
title: "選取和管理廣告網路"
translationtype: Human Translation
ms.sourcegitcommit: ec7ce299545de8e5c167e1934fb9a0b4f4370948
ms.openlocfilehash: 49c9b8e60da9239c948265fb22563013019da259

---

# 選取和管理廣告網路


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

使用廣告流量分配之前，您將需要為每個想要在應用程式中使用的廣告網路設定帳戶。 建議您在將廣告流量分配者控制項新增到 Microsoft Visual Studio 專案之前先執行這個動作。 同時也建議您盡可能設定最多廣告網路帳戶，以便讓您有最大的彈性來將廣告流量分配效能最佳化。

## 支援的廣告網路和專案類型

目前支援廣告流量分配的廣告網路和專案類型如下。

|  廣告網路    | 使用 C# 或 Visual Basic 與 XAML 的通用 Windows 平台 (UWP) 應用程式 | 使用 C# 或 Visual Basic 與 XAML 的 Windows 8.1 app | 使用 C# 或 Visual Basic 與 XAML 的 Windows Phone 8.1 app | Windows Phone 8.x Silverlight 應用程式 |               
|-------------------------------------------------------------------------|----------------------------------------------------|----------------------------------------------------------|-----------------------------------|---------------|
| [Microsoft](#microsoft)                         | **支援**                                      | **支援**                                            | **支援**                     | **支援** |
| [AdDuplex](#adduplex)                                                   | **支援**                                      | **支援**                                            | **支援**                     | **支援** |
| [Smaato](#smaato)                                                       | 不支援                                      | **支援**                                            | **支援**                     | **支援** |
| [AdMob (Google)](#admob)                                                | 不支援                                      | 不支援                                            | 不支援                     | **支援** |
| [Inneractive](#inneractive)                                             | 不支援                                      | 不支援                                            | 不支援                     | **支援** |
| [Vserv VMAX](#vserv)                                                    | 不支援                                      | 不支援                                            | **支援**                     | 不支援 | 

廣告流量分配目前不支援某些專案類型 (例如，C++ 搭配 XAML 以及 JavaScript 搭配 HTML)。 針對這些專案類型，您可以顯示來自 Microsoft 的廣告，而不需使用廣告流量分配。 如需詳細資訊，請參閱 [XAML 和 .NET 中的 AdControl](https://msdn.microsoft.com/library/mt313186.aspx) 和 [HTML 5 和 JavaScript 中的 AdControl](https://msdn.microsoft.com/library/mt313130.aspx)。

## 適用於每個網路的特定參數和詳細資料


以下是您需要針對每個廣告網路使用的特定詳細資料，包括如何設定帳戶，以及如何將 app 上架的方式。 我們也會列出當您[送出 app 並設定廣告流量分配](submit-your-app-and-configure-ad-mediation.md) (和/或在 \[已連接服務\] 中為了進行[測試廣告流量分配實作](test-your-ad-mediation-implementation.md)) 時所需提供的參數。 如需使用特定廣告網路的詳細資訊，請瀏覽其網站。

除了所需的參數，每個廣告網路也都有其他選擇性參數，讓您可在 app 中透過程式碼進行設定。 如需範例，請參閱本主題稍後的[選擇性廣告網路參數](#optionalparameters) 。 如需選擇性參數的完整清單，請參閱每個廣告網路提供的說明文件。 廣告流量分配將會忽略或覆寫這些其中部分參數，而以下各節將列出這些參數。 例如，與目前位置相關的參數通常會由客戶位置的相關資訊來覆寫，而客戶的位置是由 app 本身內部的位置功能所判斷。

請注意，當您[新增廣告流量分配者控制項](add-and-use-the-ad-mediator-control.md)並指定要使用的廣告網路時，Visual Studio 會嘗試以程式設計的方式針對某些廣告網路擷取必要的組件。 如果有任何無法自動抓取的組件，您就可以使用以下所示的每個廣告網路連結來安裝它們。

### Microsoft

| 網站                        | 若要設定廣告網路參數，請使用 [Windows 開發人員中心儀表板](https://dev.windows.com/overview)上的[利用廣告營利](https://msdn.microsoft.com/library/windows/apps/mt170658)頁面。   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK 位置                   | [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk)。                                                                                                                                                                                                                         |
| 讓應用程式上架              | 將廣告流量分配控制項新增到您的 App，並將 App 提交至 Windows 開發人員中心儀表板。                                                                                                                                                                                                            |
| 必要參數            | ApplicationId 和 AdUnitId：當您送出 app 套件時，這些參數會根據您的 app 內容自動為您填入。 不過，當您[送出 app 並設定廣告流量分配](submit-your-app-and-configure-ad-mediation.md)時，可以選擇性地編輯這些參數。 <br> <br> Height 和 Width (只有針對 Windows Phone 8 Silverlight 和 Windows Phone 8.1 Silverlight 時是必要項)。                                                                                                                                                                                                           |
| 覆寫/忽略的參數 | Latitude (覆寫)  <br><br> Longitude (覆寫) <br><br> AutoRefreshIntervalInSeconds (忽略) <br><br> IsAutoRefreshEnabled (忽略) <br><br> IsAutoCollapsedEnabled (忽略) <br><br> IsEngaged (忽略) <br><br> IsSuspended (忽略) |

 

### AdDuplex

| 網站                        | [http://adduplex.com](http://go.microsoft.com/fwlink/p/?LinkId=518028)      |
|--------------------------------|-----------------------------------------------------------------------------|
| SDK 位置                   | 首先，嘗試讓廣告流量分配者控制項透過「已連接服務」抓取組件，如[新增和使用廣告流量分配者控制項](add-and-use-the-ad-mediator-control.md)中所述。 如果您需要手動下載它們，請移至 AdDuplex 網站上的[開始使用](http://go.microsoft.com/fwlink/p/?LinkId=518029) (Getting Started) 區段。 |
| 讓應用程式上架              | 在 AdDuplex 入口網站中，建立新的應用程式。                                                                                                                                                                                                                                                                                                        |
| 必要參數            | AppKey <br><br> AdUnitId <br><br> Size (只有針對 Windows 8.1 XAML 時是必要項)  |
| 覆寫/忽略的參數 | Latitude (覆寫) <br><br> Longitude (覆寫) <br><br> AutoRefreshIntervalInSeconds (忽略) <br><br> IsAutoRefreshEnabled (忽略) <br><br> IsAutoCollapsedEnabled (忽略) <br><br> IsEngaged (忽略) <br><br> IsSuspended (忽略) |
 

### Smaato

| 網站                        | [http://www.smaato.com/](http://go.microsoft.com/fwlink/p/?LinkId=518030)                                                                                                                                                                                                        |
|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK 位置                   | 首先，嘗試讓廣告流量分配者控制項透過「已連接服務」抓取組件，如[新增和使用廣告流量分配者控制項](add-and-use-the-ad-mediator-control.md)中所述。 如果您需要手動下載它們，請移至 Smaato 入口網站中的 \[下載\] \(Downloads\) 索引標籤。 |
| 讓應用程式上架              | 在 Smaato 入口網站中，移至 \[我的空間\] \(My Spaces\)，然後產生新的 Adspace。                                                                                                                                                                                                                |
| 必要參數            | Pub <br> <br> Adspace <br> <br> Height 和 Width (只有針對 Windows 8.1 XAML 時是必要項)  |
| 覆寫/忽略的參數 | Gps (覆寫)                                                                                                                                                                                                                                                                |

 

### AdMob (Google)

| 網站                        | [http://apps.admob.com](http://go.microsoft.com/fwlink/p/?LinkId=518031)                                               |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------|
| SDK 位置                   | 從 [Google Mobile Ads SDK 網站](http://go.microsoft.com/fwlink/p/?LinkId=518032)取得 Windows Phone 8 SDK。 |
| 讓應用程式上架              | 在 AdMob 入口網站中，選取 \[讓新應用程式成為賺錢工具\] \(Monetize new app\)。                                                                          |
| 必要參數            | AdUnitID <br> <br> Format                                                                                              |
| 覆寫/忽略的參數 | Location (覆寫)  <br><br> ForceTesting (忽略) <br><br> Refresh Rate (忽略)                                |
 

### Inneractive

| 網站             | [http://inner-active.com](http://go.microsoft.com/fwlink/p/?LinkId=518035)                                                                                                                                                                                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK 位置        | 首先，嘗試讓廣告流量分配者控制項透過「已連接服務」抓取組件，如[新增和使用廣告流量分配者控制項](add-and-use-the-ad-mediator-control.md)中所述。 如果您需要手動下載它們，請登入您的帳戶，然後移至 \[儀表板\] \(Dashboard\) 中的 \[SDK\] 索引標籤，以下載 Windows Phone 8 SDK。 |
| 讓應用程式上架   | 在 Inneractive 入口網站中，建立新的應用程式。                                                                                                                                                                                                                                                                                           |
| 必要參數 | AppID <br> <br> AdType (IaAdType_Banner 或 IaAdType_Text)                                                                               |
 

### Vserv VMAX

| 網站             | [http://www.vmax.com](http://www.vmax.com)                                                                                                                                                                                                                                                                         |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK 位置        | 首先，嘗試讓廣告流量分配者控制項透過「已連接服務」抓取組件，如[新增和使用廣告流量分配者控制項](add-and-use-the-ad-mediator-control.md)中所述。 如果您需要手動下載，請移至 VMAX 網站上的 [SDK](http://go.microsoft.com/fwlink/?LinkId=627078) 區段。 |
| 讓應用程式上架   | 從 VMAX 網站取得適用於您 app 的 Adspot 識別碼 (Adspot 識別碼在 VMAX API 中稱為 ZoneId)。                                                                                                                                                                                                                     |
| 必要參數 | ZoneId                                                                                                                                                                                                                                                                                                                         |12345

 

## 選擇性廣告網路參數


除了所需的參數，每個廣告網路也都有其他選擇性參數，讓您可在 app 中透過程式碼進行設定。 如需選擇性參數的完整清單，請參閱每個廣告網路提供的說明文件。 若要在程式碼中設定這些選擇性參數，請使用 **AdMediatorControl** 物件的 **AdSdkOptionalParameters** 屬性。

下列範例示範如何將 Microsoft Advertising 的 [CountryOrRegion](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.countryorregion.aspx) 屬性設定為使用者的雙字母國家或地區碼。

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["CountryOrRegion"] = "IN";
```

下列範例示範如何設定 Smaato 的 **Width** 和 **Height** 參數。

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 300;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 250;
```

## 相關主題

* [新增和使用廣告流量分配控制項](add-and-use-the-ad-mediator-control.md)
* [測試您的廣告流量分配實作](test-your-ad-mediation-implementation.md)
* [送出您的 App 並設定廣告流量分配](submit-your-app-and-configure-ad-mediation.md)
* [疑難排解廣告流量分配](troubleshoot-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


