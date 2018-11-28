---
Description: Learn how to use notification mirroring on your toast notifications.
title: 通知鏡像
label: Notification mirroring
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: windows 10, uwp, 快顯通知, 雲端的控制中心, 通知鏡像, 通知, 跨裝置
ms.localizationpriority: medium
ms.openlocfilehash: dc870601159a80bc6d03a287fd19f082e968e09e
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7969058"
---
# <a name="notification-mirroring"></a>通知鏡像

通知鏡像由雲端的控制中心 (Action Center in the Cloud) 提供，可讓您在電腦上看到您的手機通知。

> [!IMPORTANT]
> **需要年度更新版**：您必須執行組建 14393 或更高版本，才能看到通知鏡像運作。 如果您想讓您的應用程式退出通知鏡像，您必須以 SDK 14393 為目標來存取鏡像 API。

使用通知鏡像和 Cortana，使用者可以從手邊的電腦接收其手機通知 (Windows Mobile 和 Android) 並對其採取操作。 身為開發人員，您不需要執行任何動作就能啟用通知鏡像，鏡像會自動運作！ 按一下鏡像快顯通知上的按鈕，例如快速回覆訊息，就會路由回到手機、叫用背景工作或啟動前景應用程式。

<img alt="Notification mirroring diagram" src="images/toast-mirroring.gif" width="350"/>

開發人員從通知鏡像取得兩個優點： 鏡像的通知中更多的使用者持續使用您的服務，同時也能協助使用者探索您的 Microsoft Store 傳統型應用程式 ！ 您的使用者可能根本不知道您有適用於 Windows 10 桌面的絕佳 UWP app。 當使用者從他們的手機收到鏡像的通知時，使用者可以按一下快顯通知前往 Microsoft Store，他們可以在其中安裝您的 UWP 傳統型應用程式。

鏡像適用於 Windows Phone 和 Android。 使用者必須在他們的手機和桌面同時登入 Cortana，通知鏡像才能運作。


## <a name="what-if-the-app-is-installed-on-both-devices"></a>如果兩部裝置上都已安裝應用程式，會怎麼樣？

如果使用者已在其電腦上安裝您的應用程式，我們會自動將鏡像手機通知設為靜音，以免使用者看到重複的通知。 鏡像通知會根據下列條件自動設為靜音...

1. 電腦上的應用程式擁有**相同的顯示名稱或相同的 PFN** (套件系列名稱)
2. 電腦應用程式已傳送快顯通知

如果電腦應用程式尚未傳送快顯通知，我們仍會顯示手機通知，因為使用者有可能尚未啟動電腦應用程式。


## <a name="how-to-opt-out-of-mirroring"></a>如何退出鏡像

UWP app 開發人員、企業和使用者可以選擇停用通知鏡像。

> [!NOTE]
> 停用鏡像也會同時停用[通用關閉](universal-dismiss.md)。


### <a name="as-a-developer-opt-out-an-individual-notification"></a>身為開發人員，可退出個別通知

您偶爾可能會收到您不想鏡像到其他裝置的裝置專屬通知。 您可以透過設定快顯通知上的 **Mirroring** 屬性，防止鏡像特定通知。 目前，此鏡像屬性只能在本機通知上設定 (傳送 WNS 推播通知時不能指定此項目)。

**已知問題**：透過 `ToastNotificationHistory.GetHistory()` API 擷取鏡像屬性一律會傳回預設值 (**Allowed**)，而非您所指定的選項。 請不要擔心，所有項目都能運作 - 它只是擷取已損壞的值。

```csharp
var toast = new ToastNotification(xml)
{
    // Disable mirroring of this notification
    Mirroring = NotificationMirroring.Disabled
};
  
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


### <a name="as-a-developer-opt-out-completely"></a>身為開發人員，可以完全退出

某些開發人員可能會選擇將其應用程式完全退出通知鏡像。 縱使我們相信所有應用程式都能受益於鏡像，但退出方式也很容易。只需呼叫一次下列方法，您的應用程式就會退出。例如，您可將此呼叫放在 `App.xaml.cs` 中的應用程式建構函式內...

```csharp
public App()
{
    this.InitializeComponent();
    this.Suspending += OnSuspending;
 
    // Disable notification mirroring for entire app
    ToastNotificationManager.ConfigureNotificationMirroring(NotificationMirroring.Disabled);
}
```


### <a name="as-an-enterprise-how-do-i-opt-out"></a>身為企業，我如何退出？

企業可以選擇完全停用通知鏡像。 若要這樣做，只需編輯群組原則以關閉通知鏡像。


### <a name="as-a-user-how-do-i-opt-out"></a>身為使用者，我如何退出？

使用者可以退出個別應用程式，也可以透過停用功能來完全退出。 您可能不想要特定應用程式的通知鏡像到您的桌面，所以您只要停用該特定應用程式即可。 您可以在手機和電腦上的 Cortana 設定中找到這些選項。