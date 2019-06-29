---
Description: 了解您的 app 套件是如何提供給您的客戶，以及如何管理特定套件案例。
title: 應用程式套件管理指導方針
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f514177ad5de7774e6926165435fd3b2d7b5e1f7
ms.sourcegitcommit: 4aef8c01ba9321401d5729a1ec6d46452ee76faf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/29/2019
ms.locfileid: "67468940"
---
# <a name="guidance-for-app-package-management"></a>應用程式套件管理指導方針

了解您的 app 套件是如何提供給您的客戶，以及如何管理特定套件案例。

-   [OS 版本與套件發佈](#os-versions-and-package-distribution)
-   [適用於 Windows 10 中加入先前已發行的應用程式的套件](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [適用於 Windows Phone 8.1 的維護封裝相容性](#maintaining-package-compatibility-for-windows-phone-81)
-   [從存放區中移除應用程式](#removing-an-app-from-the-store)
-   [移除先前支援的裝置系列的套件](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>作業系統版本和套件發佈

不同的作業系統上可以執行不同類型的套件。 如果有一個以上的套件可以在客戶的裝置上執行，則 Microsoft Store 將提供最佳絕配。

一般而言，較新的作業系統版本可以針對相同的裝置系列，執行以之前作業系統版本為目標的套件。 Windows 10 裝置可以執行所有先前支援的 OS 版本 （每個裝置系列）。 Windows 10 desktop 裝置可執行針對 Windows 8.1 或 Windows 8; 所建置的應用程式Windows 10 行動裝置可以執行應用程式建置的 Windows Phone 8.1、 Windows Phone 8 和甚至是 Windows Phone 7.x。 不過，在 Windows 10 上的客戶只會收到這些套件，如果應用程式不會包含以適用的裝置系列為目標的 UWP 套件。

> [!IMPORTANT]
> 自 2018 年 10 月 31 日起，新建立的產品不能包含封裝目標 Windows 8.x/Windows Phone 8.x 或更早版本。 如需詳細資訊，請參閱此[部落格文章](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)。


## <a name="removing-an-app-from-the-store"></a>從市集移除 App

有時，您可能想要停止為客戶提供某個 app，有效地「取消發佈」該 app。 若要執行此動作，請按一下 [**應用程式概觀**] 頁面上的 [**停止提供應用程式**]。 在您確認想要停止提供該應用程式之後，該應用程式在數小時內便無法在 Microsoft Store 中讓別人看見，而所有的新客戶都將無法取得它 (除非他們有[促銷碼](generate-promotional-codes.md)且使用 Windows 10 裝置)。

> [!IMPORTANT]
> 此選項將會覆寫您已在提交中選取的所有[可見度](choose-visibility-options.md#discoverability)設定。 

這個選項具有以下選項的相同效果：如果您建立提交並選擇 **\[在 Microsoft Store 推出此產品，但不供搜尋\]** 與 **\[停止取得\]** 選項。 不過，它不需要您建立新的提交。

請注意，任何已擁有此 app 的客戶仍可使用它並可再次下載 (甚至可以在您於稍後提交新的套件時取得更新)。

完成之後應用程式無法使用，您還是會看到它在合作夥伴中心。 如果您決定再次為客戶提供該 app，就可以按一下 [App 概觀] 頁面上的 [提供 App]  。 當您確認之後，該 app 即可在數小時內提供給新的客戶使用 (除非受到您在最新提交中的設定所限制)。

> [!NOTE]
> 如果您想保持應用程式的可用性，但不想繼續提供給使用特定作業系統版本的新客戶，您可以建立新的提交，並針對您想要在其上防止新取得的作業系統版本移除所有套件。 例如，如果您先前已有套件的 Windows Phone 8.1 和 Windows 10，且您不想要保留給 Windows Phone 8.1 上的新客戶提供應用程式，移除所有 Windows Phone 8.1 封裝提交。 更新發行之後，Windows Phone 8.1 上的任何新客戶將不能夠取得應用程式，但已經有它的客戶可以繼續使用它）。 不過，應用程式仍會適用於 Windows 10 上的新客戶。


## <a name="removing-packages-for-a-previously-supported-device-family"></a>移除先前支援之裝置系列的套件

如果您移除所有封裝一段[裝置系列](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)，您的應用程式先前支援，系統會提示您確認這是您的意圖，您可以在儲存變更之前**封裝**頁面。

當您發行中移除所有可以在您的應用程式先前支援的裝置系列執行的套件在送出時，新客戶將無法取得該裝置系列上的應用程式。 此後您還是可以發佈其他更新，以再次針對該裝置系列提供套件。

請注意，即使您移除支援特定裝置系列的所有套件，任何已安裝該 app 的現有客戶仍能夠使用它，且他們會取得您之後所提供的任何更新。


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>適用於 Windows 10 中加入先前已發行的應用程式的套件

如果您有應用程式只包含 Windows 套件存放區中 8.x 和/或 Windows Phone 8.x 和您想要更新您的應用程式適用於 Windows 10，建立新的送出並新增期間您 UWP.msixupload 或.appxupload 套件[封裝](upload-app-packages.md)步驟。 您的應用程式通過認證程序之後，也會適用於 Windows 10 上的客戶的新收購 UWP 套件。

> [!NOTE]
> 一旦客戶在 Windows 10 上的取得您的 UWP 套件，您無法復原該客戶回到使用任何先前的作業系統版本的套件。 

請注意，您的 Windows 10 套件的版本號碼必須高於任何您已使用的 Windows 8/Windows 8.1 或 Windows Phone 8.1 套件。 如需詳細資訊，請參閱[套件版本編號](package-version-numbering.md)。

如需針對 Microsoft Store 封裝 UWP 應用程式的詳細資訊，請參閱[封裝應用程式](../packaging/index.md)。
