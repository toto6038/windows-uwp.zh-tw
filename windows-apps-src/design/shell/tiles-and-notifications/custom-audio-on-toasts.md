---
author: andrewleader
Description: Learn how to use custom audio on your toast notifications.
title: 自訂快顯通知的音效
label: Custom audio on toasts
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 快顯通知, 自訂音效, 通知, 音訊, 音效
ms.localizationpriority: medium
ms.openlocfilehash: 24be148340366163fcab00ec75f7f26fac6c2d80
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "4503647"
---
# <a name="custom-audio-on-toasts"></a>自訂快顯通知的音效

快顯通知可以使用自訂音效，讓您的應用程式傳達品牌的獨特音效。 例如，訊息中心應用程式可以在其快顯通知上使用自己的訊息中心音效，讓使用者不用聽到一般的通知音效，即可立即知道他們收到此應用程式的通知。

## <a name="install-uwp-community-toolkit-nuget-package"></a>安裝 UWP Community Toolkit NuGet 套件

若要透過程式碼建立通知，我們強烈建議使用 UWP Community Toolkit Notifications 程式庫，此程式庫提供通知 XML 內容的物件模型。 您可以手動建構通知 XML，但這很容易出錯並造成混亂。 UWP Community Toolkit 中的 Notifications 程式庫由在 Microsoft 擁有通知的團隊負責建置和維護。

安裝來自 NuGet 的 [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) (本文中使用 1.0.0 版)。


## <a name="add-namespace-declarations"></a>加入命名空間宣告

`Windows.UI.Notifications` 包含磚和快顯通知 API。 `Microsoft.Toolkit.Uwp.Notifications` 包含 Notifications 程式庫。

```csharp
using Microsoft.Toolkit.Uwp.Notifications;
using Windows.UI.Notifications;
```


## <a name="construct-the-notification"></a>建構通知

快顯通知內容包含文字與影像，也包含按鈕與輸入。 請參閱[傳送本機快顯通知](send-local-toast.md)以查看完整的程式碼片段。

```csharp
ToastContent toastContent = new ToastContent()
{
    Visual = new ToastVisual()
    {
        ... (omitted)
    }
};
```


## <a name="add-the-custom-audio"></a>新增自訂音效

Windows Mobile 一律支援快顯通知中的自訂音效。 不過，桌上型電腦直到版本 1511 (組建 10586) 才加入對自訂音效的支援。 如果您傳送包含自訂音效的快顯通知至版本 1511 之前的電腦裝置，快顯通知會在幕後進行。 因此，對於版本 1511 之前的桌上型電腦，您不應在您的快顯通知中加入自訂音效，讓通知至少使用預設的通知音效。

**已知問題**：如果您使用桌上型電腦版本 1511，則您的應用程式必須透過 Microsoft Store 安裝，自訂快顯通知音效才能運作。 這表示您在將自訂音效提交至 Microsoft Store 之前，無法在桌上型電腦本機上測試自訂音效，但只要從 Microsoft Store 安裝，音效就能正常運作。 我們已在年度更新版修正這個問題，因此來自本機部署應用程式的自訂音效可以正常運作。

```csharp
?
bool supportsCustomAudio = true;
 
// If we're running on Desktop before Version 1511, do NOT include custom audio
// since it was not supported until Version 1511, and would result in a silent toast.
if (AnalyticsInfo.VersionInfo.DeviceFamily.Equals("Windows.Desktop")
    && !ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 2))
{
    supportsCustomAudio = false;
}
 
if (supportsCustomAudio)
{
    toastContent.Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/Audio/CustomToastAudio.m4a")
    };
}
```

支援的音訊檔案類型包括...

- .aac
- .flac
- .m4a
- .mp3
- .wav
- .wma


## <a name="send-the-notification"></a>傳送通知

現在，您的快顯通知內容已完成，傳送通知非常簡單。

```csharp
// Create the toast notification from the previous toast content
ToastNotification notification = new ToastNotification(toastContent.GetXml());
             
// And then send the toast
ToastNotificationManager.CreateToastNotifier().Show(notification);
```


## <a name="related-topics"></a>相關主題

- [GitHub 上的完整程式碼](https://github.com/WindowsNotifications/quickstart-toast-with-custom-audio)
- [傳送本機快顯通知](send-local-toast.md)
- [快顯通知內容文件](adaptive-interactive-toasts.md)