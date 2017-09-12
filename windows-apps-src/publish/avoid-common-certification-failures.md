---
author: jnHs
Description: "檢閱此清單有助於避免發生經常讓 app 無法通過認證的問題，或是 app 發行後可能在抽樣檢查中發現的問題。"
title: "避免常見的認證失敗"
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 92e1b35c8eba7f1f3d3a32f0c994d3353a065419
ms.sourcegitcommit: fadde8afee46238443ec1cb71846d36c91db9fb9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2017
---
# <a name="avoid-common-certification-failures"></a>避免常見的認證失敗


檢閱此清單有助於避免發生經常讓 app 無法通過認證的問題，或是 app 發行後可能在抽樣檢查中發現的問題。

> [!NOTE]
> 請務必檢閱 [Windows 市集原則](https://msdn.microsoft.com/library/windows/apps/dn764944)，以確認您的應用程式符合此處所列的所有需求。

-   只有在完成上述檢閱之後，才提教您的 app。 您可以使用應用程式的介紹來提及即將推出的新功能，但務必確定應用程式中沒有包含未完成的區段或建構中的網頁連結，或是讓客戶覺得您的應用程式有未完成的任何其他項目。

-   在您送出 app 之前，請[使用 Windows 應用程式認證套件測試 app](../debug-test-perf/windows-app-certification-kit.md)。

-   在數種不同的設定上測試您的應用程式，盡可能確保它的穩定性。

-   請確定在沒有網路連線時您的應用程式不會當機。 即使需要連線才能實際使用您的應用程式，仍需要確保在沒有連線的情況下應用程式仍能適當執行。

-   [提供任何所需的資訊](notes-for-certification.md)來使用應用程式，例如測試帳戶的使用者名稱和密碼 (若您的應用程式需要使用者登入服務)，或存取隱藏或鎖定功能的任何所需步驟。

-   您的應用程式需要時包含[隱私權原則](create-app-store-listings.md#privacy-policy)。例如，如果您的應用程式以任何方式存取任何類型的個人資訊或若法律要求時。 為協助判斷您的應用程式是否需要隱私權原則，請檢閱[應用程式開發人員合約](https://msdn.microsoft.com/library/windows/apps/hh694058)和 [Windows 市集原則](https://msdn.microsoft.com/library/windows/apps/dn764944)。

-   確定應用程式描述可清楚說明應用程式的功能。 如需說明，請參閱[撰寫一份出色的應用程式介紹](write-a-great-app-description.md)的指導方針。

-   在[年齡分級](age-ratings.md)區段中，針對所有的問題提供完整且準確的回應。

-   請勿[將您的 app 宣告為無障礙應用程式](app-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines)，除非您特別針對協助工具案例建置 app 並進行測試。

-   如果您的 app 使用來自 [**Windows.ApplicationModel.Store**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) 命名空間的商務 API，請務必測試 app，確認它可以處理一般的例外狀況。 另外，請確定您的應用程式使用 [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 類別，而不是 [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 類別 (這僅供測試使用)  (請注意，如果您 App 的目標是 Windows 10 版本 1607 或更新版本，建議您使用 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空間 (而不是 Windows.ApplicationModel.Store 命名空間) 的成員)。


 

 




