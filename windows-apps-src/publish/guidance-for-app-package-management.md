---
author: jnHs
Description: Learn how your app's packages are made available to your customers, and how to manage specific package scenarios.
title: 應用程式套件管理指導方針
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.author: wdg-dev-content
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a43f3b4c5684d93ea6986c4d1f1e4dae46c1a959
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "5521159"
---
# <a name="guidance-for-app-package-management"></a>應用程式套件管理指導方針

了解您的應用程式套件如何提供給您的客戶，以及如何管理特定套件案例。

-   [作業系統版本和套件發佈](#os-versions-and-package-distribution)
-   [將適用於 Windows 10 的套件新增至先前發佈的 app](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [維護 Windows Phone 8.1 的套件相容性](#maintaining-package-compatibility-for-windows-phone-81)
-   [從市集移除 App](#removing-an-app-from-the-store)
-   [移除先前支援之裝置系列的套件](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>作業系統版本和套件發佈

不同的作業系統上可以執行不同類型的套件。 如果有一個以上的套件可以在客戶的裝置上執行，則 Microsoft Store 將提供最佳絕配。

一般而言，較新的作業系統版本可以針對相同的裝置系列，執行以之前作業系統版本為目標的套件。 不過，客戶只會取得那些套件，如果 app 不包含以其目前的作業系統版本為目標的套件。

例如，windows 10 裝置可以執行所有先前支援的 OS 版本 （每個裝置系列）。 Windows 10 桌面裝置可以執行針對 Windows8.1 或 Windows8; 建置的應用程式Windows 10 行動裝置可以執行應用程式已建置適用於 Windows Phone 8.1、 WindowsPhone8 和 Windows Phone 7.x。 

下列範例針對包含以不同作業系統版本為目標之套件的應用程式，說明各種的案例 (除非您套件的特定限制不允許它們在此處所列的每一個作業系統版本/裝置類型上執行，例如，套件的架構必須適用於裝置)。 

### <a name="example-app-1"></a>範例 app 1

| 套件的目標作業系統 | 將取得此套件的作業系統 |
|-------------------------------------|----------------------------------------------|
| Windows8.1                         | Windows 10 桌上型電腦裝置、 Windows8.1      |
| Windows Phone 8.1                   | Windows 10 行動裝置、 Windows Phone 8.1 |
| WindowsPhone8                     | WindowsPhone8                              |
| Windows Phone 7.1                   | Windows Phone 7.x                            |

在範例 app 1，app 還沒有特別針對 windows 10 裝置建置的通用 Windows 平台 (UWP) 套件，但在 windows 10 上的客戶仍可取得應用程式。 這些客戶會根據其裝置類型取得最佳的套件。

### <a name="example-app-2"></a>範例 app 2

| 套件的目標作業系統  | 將取得此套件的作業系統 |
|--------------------------------------|----------------------------------------------|
| Windows 10 （通用裝置系列） | Windows 10 （所有裝置系列）             |
| Windows8.1                          | Windows8.1                                  |
| Windows Phone 8.1                    | Windows Phone 8.1                            |
| Windows Phone 7.1                    | Windows Phone 7.x、 WindowsPhone8           |

在範例 app 2 中，沒有可在 Windows8 上執行的套件。 在所有其他作業系統版本上執行的客戶可以取得應用程式。 在 Windows 10 上的所有客戶將取得相同的套件。

### <a name="example-app-3"></a>範例 app 3

| 套件的目標作業系統 | 將取得此套件的作業系統                  |
|-------------------------------------|---------------------------------------------------------------|
| Windows 10 （傳統型裝置系列）  | Windows 10 桌面裝置                                    |
| WindowsPhone8                     | Windows 10 行動裝置、 WindowsPhone8，Windows Phone 8.1 |

在範例 app 3，因為沒有以行動裝置系列為目標的 UWP 套件在 windows 10 行動裝置上的客戶將會取得 WindowsPhone8 套件。 如果此 app 後來新增以行動裝置系列 （或通用裝置系列） 為目標的套件，該套件就能在 windows 10 行動裝置，而不是 WindowsPhone8 套件上的客戶。

也請注意，這個範例 app 不包含任何可在 Windows Phone 7.x 上執行的套件。

### <a name="example-app-4"></a>範例 app 4

| 套件的目標作業系統  | 將取得此套件的作業系統 |
|--------------------------------------|----------------------------------------------|
| Windows 10 （通用裝置系列） | Windows 10 （所有裝置系列）             |

在範例 app 4 中，任何執行 windows 10 的裝置可以取得應用程式，但它不會提供給任何之前作業系統版本上的客戶。 通用裝置系列為目標的 UWP 套件，因為它可供任何 windows 10 的裝置 （每個您的[裝置系列可用性選取項目](device-family-availability.md)）。


## <a name="removing-an-app-from-the-store"></a>從 Microsoft Store 移除 App

有時，您可能想要停止為客戶提供某個 app，有效地「取消發佈」該 app。 若要執行此動作，請按一下 [**應用程式概觀**] 頁面上的 [**停止提供應用程式**]。 在您確認想要停止提供該應用程式之後，該應用程式在數小時內便無法在 Microsoft Store 中讓別人看見，而所有的新客戶都將無法取得它 (除非他們有[促銷碼](generate-promotional-codes.md)且使用 Windows 10 裝置)。

> [!IMPORTANT]
> 此選項將會覆寫您已在提交中選取的所有[可見度](choose-visibility-options.md#discoverability)設定。 

這個選項具有以下選項的相同效果：如果您建立提交並選擇 **\[在 Microsoft Store 推出此產品，但不供搜尋\]** 與 **\[停止取得\]** 選項。 不過，它不需要您建立新的提交。

請注意，任何已擁有此 app 的客戶仍可使用它並可再次下載 (甚至可以在您於稍後提交新的套件時取得更新)。

停止提供該 app 之後，您仍能在儀表板中看見它。 如果您決定再次為客戶提供該 app，就可以按一下 [App 概觀] 頁面上的 **\[提供 App\]**。 當您確認之後，該 app 即可在數小時內提供給新的客戶使用 (除非受到您在最新提交中的設定所限制)。

> [!NOTE]
> 如果您想保持應用程式的可用性，但不想繼續提供給使用特定作業系統版本的新客戶，您可以建立新的提交，並針對您想要在其上防止新取得的作業系統版本移除所有套件。 例如，如果您先前已有套件適用於 Windows Phone 8.1 和 windows 10，且您不想持續提供該 app WindowsPhone8.1 的新客戶，所有 WindowsPhone8.1 套件從提交中移除。 發佈更新之後，所有新客戶 WindowsPhone8.1 上的將無法取得應用程式，但已經擁有該的客戶仍能繼續使用它）。 不過，應用程式仍然可以使用適用於 windows 10 上的新客戶。


## <a name="removing-packages-for-a-previously-supported-device-family"></a>移除先前支援之裝置系列的套件

如果您移除所有套件之特定[裝置系列](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)，您的應用程式先前支援，系統將提示您確認這是您意圖，您可以在 \ [**套件**] 頁面上儲存變更之前。

當您發佈移除所有套件，可在您的應用程式先前支援的裝置系列上執行的提交時，新客戶將無法取得該裝置系列上的應用程式。 此後您還是可以發佈其他更新，以再次針對該裝置系列提供套件。

請注意，即使您移除支援特定裝置系列的所有套件，任何已安裝該 app 的現有客戶仍能夠使用它，且他們會取得您之後所提供的任何更新。


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>適用於 windows 10 的套件新增至先前發佈的 app

如果您有應用程式在市集中只包含套件適用於 Windows 8.x 和/或 Windows Phone 8.x，而您想要更新您的應用程式的 windows 10、 建立新的提交和[套件](upload-app-packages.md)步驟期間加入您的 UWP.msixupload 或.appxupload 套件。 您的應用程式通過認證程序之後，也會適用於 windows 10 上的客戶所提供新的下載數的 UWP 套件。

> [!NOTE]
> 一旦 windows 10 客戶取得您的 UWP 套件，您無法讓該客戶回復到使用任何之前作業系統版本的套件。 

請注意，您的 windows 10 套件的版本號碼必須高於您已經使用任何 Windows8、 Windows8.1，和/或 Windows Phone 8.1 套件的。 如需詳細資訊，請參閱[套件版本編號](package-version-numbering.md)。

如需針對 Microsoft Store 封裝 UWP 應用程式的詳細資訊，請參閱[封裝應用程式](../packaging/index.md)。
