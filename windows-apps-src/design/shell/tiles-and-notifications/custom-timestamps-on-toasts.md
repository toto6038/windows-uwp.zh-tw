---
author: andrewleader
Description: Learn how to use custom timestamps on your toast notifications.
title: 快顯通知上的自訂時間戳記
label: Custom timestamps on toasts
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
keywords: windows 10, uwp, 快顯通知, 自訂時間戳記, 時間戳記, 通知, 控制中心
ms.localizationpriority: medium
ms.openlocfilehash: 290825fa079052b79fb2feaec8af928f8da93f95
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5827241"
---
# <a name="custom-timestamps-on-toasts"></a>快顯通知上的自訂時間戳記

根據預設，快顯通知上的時間戳記 (顯示在控制中心) 會設定為傳送通知的時間。

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

您可選擇以您自己的自訂日期和時間來覆寫時間戳記，讓時間戳記代表訊息/資訊/內容的實際建立時間，而不是傳送通知的時間。 這也可確保您的通知以正確的順序出現在控制中心 (依照時間排序)。 我們建議大部分應用程式指定自訂時間戳記。

> [!IMPORTANT]
> **需要 Desktop Creators Update 和 Notifications 程式庫 1.4.0**：您必須執行組建 15063 或更新版本，才能看見自訂時間戳記。 您必須使用版本 1.4.0 或更高版本的 [UWP Community Toolkit Notifications NuGet 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，以便在快顯通知內容上指派時間戳記。

若要使用自訂時間戳記，只需在 **ToastContent** 上指派 **DisplayTimestamp** 屬性。

```csharp
ToastContent toastContent = new ToastContent()
{
    DisplayTimestamp = new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc),
    ...
};
```

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```

如果您使用 XML，日期必須使用 [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) 的格式。

> [!NOTE]
> 您最多只能在秒數使用 3 個小數位數 (但提供這麼細微的值沒有任何實際價值)。 如果提供更多位數，承載將會無效，而您會收到「新通知」通知。


## <a name="usage-guidance"></a>用法指導方針

一般而言，我們建議大部分應用程式指定自訂時間戳記。 這樣可確保無論是否存在網路延遲、飛航模式或定期背景工作的固定間隔，通知的時間戳記都能準確代表訊息/資訊/內容的產生時間。

例如，新聞應用程式可能會每隔 15 分鐘執行一次背景工作，檢查是否有新文章並顯示通知。 在自訂時間戳記前，時間戳記對應到快顯通知的產生時間 (因此一律是 15 分鐘間隔)。 不過，現在應用程式可以將時間戳記設定為文章的實際發佈時間。 同樣地，如果電子郵件應用程式和社交網路應用程式將類似的定期提取模式用於通知，便也能受惠於此功能。

此外，提供自訂時間戳記還能確保即使使用者中斷網際網路連線，時間戳記也能正確無誤。 例如，當使用者開啟電腦而您的背景工作開始執行，您最終可以確保您通知的時間戳記代表傳送訊息的時間，而不是使用者開啟電腦的時間。


## <a name="default-timestamp"></a>預設時間戳記

如果您不提供自訂時間戳記，我們會使用傳送通知的時間。

如果您透過 WNS 傳送推播通知，我們會使用 WNS 伺服器收到通知的時間 (因此延遲傳送通知到裝置不會影響時間戳記)。

如果您傳送本機通知，我們會使用通知平台收到通知的時間 (這應該是立即發生)。


## <a name="related-topics"></a>相關主題

- [傳送本機快顯通知](send-local-toast.md)
- [快顯通知內容文件](adaptive-interactive-toasts.md)