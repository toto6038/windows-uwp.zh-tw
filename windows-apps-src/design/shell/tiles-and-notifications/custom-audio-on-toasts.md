---
description: 瞭解如何在您的快顯通知中使用自訂音訊，讓您的應用程式表達品牌獨特的音效。
title: 自訂快顯通知的音效
label: Custom audio on toasts
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: windows 10, uwp, 快顯通知, 自訂音效, 通知, 音訊, 音效
ms.localizationpriority: medium
ms.openlocfilehash: 905292155dfc43a82c464edb651b2d176aeab960
ms.sourcegitcommit: 5e718720d1032a7089dea46a7c5aefa6cda3385f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2021
ms.locfileid: "103226122"
---
# <a name="custom-audio-on-toasts"></a>自訂快顯通知的音效

快顯通知可以使用自訂音效，讓您的應用程式傳達品牌的獨特音效。 例如，訊息中心應用程式可以在其快顯通知上使用自己的訊息中心音效，讓使用者不用聽到一般的通知音效，即可立即知道他們收到此應用程式的通知。

## <a name="install-uwp-community-toolkit-nuget-package"></a>安裝 UWP Community Toolkit NuGet 套件

若要透過程式碼建立通知，我們強烈建議使用 UWP Community Toolkit Notifications 程式庫，此程式庫提供通知 XML 內容的物件模型。 您可以手動建構通知 XML，但這很容易出錯並造成混亂。 UWP Community Toolkit 中的 Notifications 程式庫由在 Microsoft 擁有通知的團隊負責建置和維護。

從 NuGet 安裝[Microsoft 工具組。](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)


## <a name="add-namespace-declarations"></a>新增命名空間宣告

```csharp
using Microsoft.Toolkit.Uwp.Notifications;
```


## <a name="add-the-custom-audio"></a>新增自訂音效

Windows Mobile 一律支援快顯通知中的自訂音效。 不過，桌上型電腦直到版本 1511 (組建 10586) 才加入對自訂音效的支援。 如果您傳送包含自訂音效的快顯通知至版本 1511 之前的電腦裝置，快顯通知會在幕後進行。 因此，對於版本 1511 之前的桌上型電腦，您不應在您的快顯通知中加入自訂音效，讓通知至少使用預設的通知音效。

**已知問題**：如果您使用桌上型電腦版本 1511，則您的應用程式必須透過 Microsoft Store 安裝，自訂快顯通知音效才能運作。 這表示您在將自訂音效提交至 Microsoft Store 之前，無法在桌上型電腦本機上測試自訂音效，但只要從 Microsoft Store 安裝，音效就能正常運作。 我們已在年度更新版修正這個問題，因此來自本機部署應用程式的自訂音效可以正常運作。

```csharp
var contentBuilder = new ToastContentBuilder()
    .AddText("New message");

    
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
    contentBuilder.AddAudio(new Uri("ms-appx:///Assets/Audio/CustomToastAudio.m4a"));
}

// Send the toast
contentBuilder.Show();
```

支援的音訊檔案類型包括...

- .aac
- .flac
- .m4a
- .mp3
- .wav
- .wma


## <a name="send-the-notification"></a>傳送通知

傳送包含音訊的通知與傳送一般通知 (直接呼叫 Show 方法) 一樣。 若要深入瞭解，請參閱 [傳送本機](send-local-toast.md) 快顯通知。


## <a name="related-topics"></a>相關主題

- [GitHub 上的完整程式碼](https://github.com/WindowsNotifications/quickstart-toast-with-custom-audio)
- [傳送本機快顯通知](send-local-toast.md)
- [快顯通知內容文件](adaptive-interactive-toasts.md)
