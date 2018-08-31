---
author: jnHs
Description: The Packages page is where you upload all of the package files (.appxupload, .appx, .appxbundle, and/or .xap) for the app that you're submitting.
title: 上傳應用程式套件
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
ms.author: wdg-dev-content
ms.date: 5/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，套件上, 傳，套件上傳
ms.localizationpriority: medium
ms.openlocfilehash: 6013a238cff8db3b85dd98af58cccaf344a72f51
ms.sourcegitcommit: 1e5590dd10d606a910da6deb67b6a98f33235959
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2018
ms.locfileid: "3232980"
---
# <a name="upload-app-packages"></a>上傳應用程式套件

**\[套件\]** 頁面是為您要提交的 App 上傳所有套件檔案 (.appx、.appxupload、.appxbundle 和/或 .xap) 的所在之處。 您可以在此步驟中，為您的 App 對象上傳適用於任何作業系統的套件。 當客戶下載您的 App 時，[市集] 會自動提供每位客戶最適合他們的裝置的套件。 在您上傳套件之後，您會看到一個資料表以排序方式指出[哪個套件將提供給特定的 Windows 10 裝置系列](#device-family-availability) (以及舊版 OS，如果適用的話)。

如需有關套件內容及其建構方式的詳細資訊，請參閱[應用程式套件需求](app-package-requirements.md)。 您也會想要深入了解[如何的版本號碼可能會影響哪個套件傳遞給特定的客戶](package-version-numbering.md)，以及[如何將套件發佈到不同的作業系統](guidance-for-app-package-management.md)。

## <a name="uploading-packages-to-your-submission"></a>將套件上傳到您的提交

若要上傳套件，請將套件拖曳到欄位內，或按一下以瀏覽您的檔案。 **\[套件\]** 頁面可讓您上傳 .xap、.appx、.appxupload 和/或 .appxbundle 檔案。

> [!IMPORTANT]
> 對於 Windows 10，我們建議您上傳 .appxupload 檔案，而非 .appx 或 .appxbundle。  如需針對市集封裝 UWP 應用程式的詳細資訊，請參閱[使用 Visual Studio 封裝 UWP app](../packaging/packaging-uwp-apps.md)。

當您建立新的提交時，您將會在 [套件](package-flights.md)頁面上看到一個下拉式清單，其中包含從其中一個套件正式發行前小眾測試版複製套件的選項。 選取含有您要納入之套件的套件正式發行前小眾測試版。 然後，您可選取其任一或所有套件，以包含在此提交中。

如果我們錯誤的套件驗證時偵測到它，我們會顯示訊息，可讓您了解什麼是錯誤。 您將需要移除套件、 修正這個問題，然後再試一次上傳。 您也會看到警告，讓您知道可能會造成問題的相關資訊，但不會阻止您繼續提交。


## <a name="device-family-availability"></a>裝置系列可用性

在您的套件成功上傳之後，**\[裝置系列可用性\]** 區段會顯示一個資料表以排序方式指出哪個套件將提供給特定的 Windows 10 裝置系列 (以及舊版 OS，如果適用的話)。 本區段也可讓您選擇是否要針對特定 Windows 10 裝置系列的客戶提供提交。

如需詳細資訊，請參閱[裝置系列可用性](device-family-availability.md)。


## <a name="package-details"></a>套件詳細資料

上傳的套件會列出在這裡，依照目標作業系統分組。 系統將會顯示套件的名稱、版本及架構。 如需有關如每個套件支援的語言、App 功能，以及檔案大小的詳細資訊，可以按一下 **\[顯示詳細資料\]**。

如果您需要移除您提交的某個套件，可以按一下每個套件的 **\[詳細資料\]** 區段底部的 **\[移除\]** 連結。


## <a name="removing-redundant-packages"></a>移除重複的套件

如果我們偵測到您的一或多個套件重複，我們將顯示一個警告，建議您從此提交中移除重複的套件。 當您先前已上傳套件，而現在又提供支援相同客戶群的更新版本套件時，通常會發生這個狀況。 在此情況下，客戶將不會取得重複的套件，因為您有更好 (更高版本) 的套件可以支援這些客戶。

當我們偵測到您有重複的套件時，我們將提供一個自動從此提交移除所有重複套件的選項。 如果需要，您也可以從此提交個別移除套件。


## <a name="gradual-package-rollout"></a>漸進式套件推出

如果您的提交是先前已發佈之 App 的更新，您會看到 **\[發佈此提交後漸進式推出更新 (僅限 Windows 10 客戶)\]** 核取方塊。 這可讓您選擇可從提交取得套件之客戶的百分比，這樣您就能監視意見反應與分析資料，以確保您在更廣泛地推出更新之前更有信心。 您隨時可以增加百分比 (或停止更新)，而不必建立新的提交。 

如需詳細資訊，請參閱[漸進式套件推出](gradual-package-rollout.md)。


## <a name="mandatory-update"></a>強制更新

如果您的提交是先前已發佈之 App 的更新，您會看到 **\[進行這項強制更新\]** 核取方塊。 這可讓您設定強制更新的日期與時間，假設您已經使用 Windows.Services.Store API 來讓您的 App 以程式設計方式檢查套件更新，以及下載和安裝已更新套件。 您的 App 必須以 Windows 10 版本 1607 或更新版本為目標，才能使用此選項。

如需詳細資訊，請參閱[下載與安裝 App 的套件更新](../packaging/self-install-package-updates.md)。

 




