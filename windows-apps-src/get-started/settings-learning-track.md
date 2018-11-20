---
author: TylerMSFT
title: 在 UWP 中儲存和載入設定
description: 了解如何在通用 Windows 平台應用程式中儲存和載入應用程式設定。
ms.author: twhitney
ms.date: 05/07/2018
ms.topic: article
keywords: 開始設定, uwp, windows 10, 了解曲目, 設定, 儲存設定, 載入設定
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 63af8aa4ab4d12a3a4aa69bd7d870106e21844f5
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2018
ms.locfileid: "7421864"
---
# <a name="save-and-load-settings-in-a-uwp-app"></a>在 UWP 中儲存和載入設定

本主題涵蓋您需要了解在通用 Windows 平台 (UWP) 應用程式中開始設定載入與儲存、設定的資訊。 介紹了主要 API，並提供連結有助於您了解更多資訊。

使用設定以記住應用程式的使用者可自訂層面。 例如，新聞閱讀程式可使用應用程式設定儲存要顯示哪一則新聞來源，以及要用於閱讀文章所使用的字型。

我們將初窺程式碼來儲存和載入應用程式設定，包括本機和漫遊設定。

## <a name="what-do-you-need-to-know"></a>您需要知道的事項

使用應用程式設定來儲存設定資料，例如使用者的喜好設定和應用程式狀態。  裝置特定的設定會儲存在本機。 在任何已安裝您的應用程式的裝置所套用的設定會儲存在漫遊資料存放區。 設定在裝置間漫遊，使用者使用相同的 Microsoft 帳戶登入這些裝置，且安裝了相同版本的應用程式。

設定可以使用下列的資料類型：整數、加倍、浮點數、字元、字串、點、DateTimes，及更多。 您也可以儲存 [ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) 類別的執行個體，有多個應視為一個單位的設定時，很實用。 例如，您的應用程式 \[讀取\] 窗格中顯示文字的字型名稱與點大小，應該以單一單位來儲存/還原。 這可以防止一個設定因另一個之前的漫遊延遲而與另一個設定不同步。

以下是您必須知道的主要 API 來儲存或載入應用程式設定：

- [Windows.Storage.ApplicationData.Current.LocalSettings](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData#Windows_Storage_ApplicationData_LocalSettings) 從本機應用程式資料存放區取得應用程式設定容器。 在此，儲存設定不適合在裝置間漫遊，因為它們代表特定於此裝置的狀態，或太大。
- [Windows.Storage.ApplicationData.Current.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings#Windows_Storage_ApplicationData_RoamingSettings) 從漫遊應用程式資料存放區取得應用程式設定容器。 此資料在裝置間漫遊。
- [Windows.Storage.ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) 是一種容器，其表示應用程式設定為索引鍵/值組。 使用這個類別來建立和擷取設定值。
- [Windows.Storage.ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) 代表應以單位序列化的多個應用程式設定。 當一個設定不應該獨立更新時，這非常有用。

## <a name="save-app-settings"></a>儲存應用程式設定

針對本簡介，我們將重點放在兩個簡單案例：裝置間儲存和載入本機應用程式設定，和漫遊複合的字型/字型大小設定。

 ```csharp
// Save a setting locally on the device
ApplicationDataContainer localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
localSettings.Values["test setting"] = "a device specific setting";

// Save a composite setting that will be roamed between devices
ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
Windows.Storage.ApplicationDataCompositeValue composite = new Windows.Storage.ApplicationDataCompositeValue();
composite["Font"] = "Calibri";
composite["FontSize"] = 11;
roamingSettings.Values["RoamingFontInfo"] = composite;
 ```

透過使用 `Windows.Storage.ApplicationData.Current.LocalSettings` 優先取得本機設定資料存放區的 **ApplicationDataContainer**，將設定儲存至本機裝置。 您指派給此執行個體的索引鍵/值字典組會儲存在本機裝置設定資料存放區中。

使用類似的模式儲存漫遊設定。 使用 `Windows.Storage.ApplicationData.Current.RoamingSettings`，優先取得漫遊設定資料存放區的 **ApplicationDataContainer**。 然後將索引鍵/值組指派給執行個體。  這些索引鍵/值組將會自動在裝置間漫遊。

上述的程式碼片段中，**ApplicationDataCompositeValue** 儲存多個索引鍵/值組。 當您有多個應該不會彼此同步處理的設定時，複合值會很實用。 當您儲存 **ApplicationDataCompositeValue** 時，以一個單位儲存與載入這些值，或自動完成。 這個相關的方式設定不會取得同步，因為它們會以一個單位而非個別來進行漫遊。

## <a name="load-app-settings"></a>載入應用程式設定

```csharp
// load a setting that is local to the device
ApplicationDataContainer localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
String localValue = localSettings.Values["test setting"] as string;

// load a composite setting that roams between devices
ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
Windows.Storage.ApplicationDataCompositeValue composite = (ApplicationDataCompositeValue)roamingSettings.Values["RoamingFontInfo"];
if (composite != null)
{
    String fontName = composite["Font"] as string;
    int fontSize = (int)composite["FontSize"];
}
```

透過使用 `Windows.Storage.ApplicationData.Current.LocalSettings` 優先取得本機設定資料存放區的 **ApplicationDataContainer** 執行個體，從本機裝置載入設定。 然後用來擷取索引鍵/值組。

透過下列類似的模式載入漫遊設定。 使用 `Windows.Storage.ApplicationData.Current.RoamingSettings`，從漫遊設定資料存放區優先取得 **ApplicationDataContainer** 執行個體。 從該執行個體存取索引鍵/值組。 如果尚未漫遊資料至裝置，但您正從此裝置存取設定，則會收到 null **ApplicationDataContainer**。 這便是在上述範例程式碼中有 `if (composite != null)` 檢查的原因。

## <a name="useful-apis-and-docs"></a>實用的 API 和文件

以下是 API 的快速摘要，以及其他實用的文件，有助於您開始儲存與載入應用程式設定。

### <a name="useful-apis"></a>實用的 API

| API | 描述 |
|------|---------------|
| [ApplicationData.LocalSettings](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.temporaryfolder) | 從本機應用程式資料存放區中取得應用程式設定容器。 |
| [ApplicationData.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings) | 從漫遊應用程式資料存放區中取得應用程式設定容器。 |
| [ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) | 應用程式設定的容器，支援建立、移除、列舉，與周遊容器階層。 |
| [Windows.UI.ApplicationSettings 命名空間](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings) | 提供您會用來定義應用程式設定的類別，其顯示在 Windows 殼層的設定窗格中。 |

### <a name="useful-docs"></a>實用的文件

| 主題 | 描述 |
|-------|----------------|
| [應用程式設定的指導方針](https://docs.microsoft.com/windows/uwp/design/app-settings/guidelines-for-app-settings) | 描述建立和顯示應用程式設定的最佳做法。 |
| [儲存和擷取設定及其他應用程式資料](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data#create-and-read-a-local-file) | 逐步解說儲存和擷取的設定，包括漫遊設定。 |

## <a name="useful-code-samples"></a>實用的程式碼範例

| 程式碼範例 | 描述 |
|-----------------|---------------|
| [應用程式資料範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) | 案例 2-4 著重於設定 |
