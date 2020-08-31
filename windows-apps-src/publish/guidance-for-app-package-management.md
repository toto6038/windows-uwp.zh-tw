---
Description: 了解您的 app 套件是如何提供給您的客戶，以及如何管理特定套件案例。
title: 應用程式套件管理指導方針
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5ecd8cc96196c31615eac032183956de3bee9e4b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171052"
---
# <a name="guidance-for-app-package-management"></a>應用程式套件管理指導方針

了解您的 app 套件是如何提供給您的客戶，以及如何管理特定套件案例。

-   [作業系統版本和套件發佈](#os-versions-and-package-distribution)
-   [將適用於 Windows 10 的套件新增至先前發佈的 app](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [從 Microsoft Store 移除 App](#removing-an-app-from-the-store)
-   [移除先前支援之裝置系列的套件](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>作業系統版本和套件發佈

不同的作業系統上可以執行不同類型的套件。 如果有一個以上的套件可以在客戶的裝置上執行，則 Microsoft Store 將提供最佳絕配。

一般而言，較新的作業系統版本可以針對相同的裝置系列，執行以之前作業系統版本為目標的套件。 Windows 10 裝置可執行所有先前支援的 OS 版本， (每個裝置系列) 。 Windows 10 桌面裝置可以執行針對 Windows 8.1 或 Windows 8 建置的 app；Windows 10 行動裝置可以執行針對 Windows Phone 8.1、Windows Phone 8 和 Windows Phone 7.x 所建置的 app。 不過，只有當應用程式不包含以適用裝置系列為目標的 UWP 套件時，Windows 10 的客戶才會取得這些套件。

> [!IMPORTANT]
> 您無法再上傳使用 Windows Phone 8.x SDK (s) 建立的新 XAP 套件。 已存在於 XAP 套件中的應用程式將會繼續在 Windows 10 行動裝置版裝置上運作。 如需詳細資訊，請參閱這 [篇 blog 文章](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)。


## <a name="removing-an-app-from-the-store"></a>從 Microsoft Store 移除 App

有時，您可能想要停止為客戶提供某個 app，有效地「取消發佈」該 app。 若要這樣做，請按一下**應用程式總覽**頁面中的 [**讓應用程式無法使用**]。 在您確認想要停止提供該應用程式之後，該應用程式在數小時內便無法在 Microsoft Store 中讓別人看見，而所有的新客戶都將無法取得它 (除非他們有[促銷碼](generate-promotional-codes.md)且使用 Windows 10 裝置)。

> [!IMPORTANT]
> 此選項將會覆寫您已在提交中選取的所有[可見度](choose-visibility-options.md#discoverability)設定。 

這個選項具有以下選項的相同效果：如果您建立提交並選擇 **\[在 Microsoft Store 推出此產品，但不供搜尋\]** 與 **\[停止取得\]** 選項。 不過，它不需要您建立新的提交。

請注意，任何已擁有此 app 的客戶仍可使用它並可再次下載 (甚至可以在您於稍後提交新的套件時取得更新)。

應用程式無法使用之後，您仍會在合作夥伴中心中看到它。 如果您決定再次為客戶提供該 app，就可以按一下 [App 概觀] 頁面上的 **\[提供 App\]**。 當您確認之後，該 app 即可在數小時內提供給新的客戶使用 (除非受到您在最新提交中的設定所限制)。

> [!NOTE]
> 如果您想保持應用程式的可用性，但不想繼續提供給使用特定作業系統版本的新客戶，您可以建立新的提交，並針對您想要在其上防止新取得的作業系統版本移除所有套件。 例如，如果您先前已有適用於 Windows Phone 8.1 及 Windows 10 的套件，而您不想持續提供該應用程式給 Windows Phone 8.1 上的客戶，則可從提交中移除所有 Windows Phone 8.1 套件。 發佈更新之後，Windows Phone 8.1 上的新客戶將無法取得應用程式，但已有客戶可以繼續使用該應用程式) 。 不過，應用程式仍然可供 Windows 10 上的新客戶使用。


## <a name="removing-packages-for-a-previously-supported-device-family"></a>移除先前支援之裝置系列的套件

如果您移除應用程式先前支援之特定 [裝置系列](/uwp/extension-sdks/device-families-overview) 的所有套件，系統會提示您確認這是您的目的，然後才能將變更儲存在 [ **套件** ] 頁面上。

當您發佈提交，以移除可在您的應用程式先前支援的裝置系列上執行的所有套件時，新客戶將無法在該裝置系列上取得該應用程式。 此後您還是可以發佈其他更新，以再次針對該裝置系列提供套件。

請注意，即使您移除支援特定裝置系列的所有套件，任何已安裝該 app 的現有客戶仍能夠使用它，且他們會取得您之後所提供的任何更新。


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>將適用於 Windows 10 的套件新增至先前發佈的 app

如果存放區中的應用程式只包含適用于 Windows 8. x 和/或 Windows Phone 8.x 的封裝，而您想要更新 Windows 10 的應用程式，請建立新的提交，並在 [封裝](upload-app-packages.md) 步驟期間新增 msixupload 或 .appxupload 套件 () s。 當您的應用程式通過認證程式之後，UWP 套件也將可供客戶在 Windows 10 上進行新的收購。

> [!NOTE]
> 一旦 Windows 10 客戶取得您的 UWP 套件後，您無法讓該客戶回復到使用任何之前作業系統版本的套件。 

請注意，Windows 10 套件的版本號碼必須高於您所使用的任何 Windows 8、Windows 8.1 及/或 Windows Phone 8.1 套件的版本號碼。 如需詳細資訊，請參閱[套件版本編號](package-version-numbering.md)。

如需針對 Microsoft Store 封裝 UWP 應用程式的詳細資訊，請參閱[封裝應用程式](../packaging/index.md)。