---
author: jnHs
Description: "套件 頁面是為您要提交的應用程式上傳所有套件檔案 (.xap、.appx、.appxupload 和/或 .appxbundle) 的所在之處。 您可以在此步驟中，為您的應用程式對象上傳適用於任何作業系統的套件。"
title: "上傳應用程式套件"
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 10523df0297013573dad62d32901c597ba850175
ms.lasthandoff: 02/07/2017

---

# <a name="upload-app-packages"></a>上傳應用程式套件


[套件]**** 頁面是為您要提交的 App 上傳所有套件檔案 (.appx、.appxupload、.appxbundle 和/或 .xap) 的所在之處。 您可以在此步驟中，為您的 App 對象上傳適用於任何作業系統的套件。 當客戶下載您的 App 時，[市集] 會自動提供每位客戶最適合他們的裝置的套件。 在您上傳套件之後，您會看到一個資料表以排序方式指出[哪個套件將提供給特定的 Windows 10 裝置系列](#device-family-availability) (以及舊版 OS，如果適用的話)。

如需有關套件內容及其建構方式的詳細資訊，請參閱[應用程式套件需求](app-package-requirements.md)。 您也會想要了解[版本號碼可能會影響哪個套件傳遞給特定客戶](package-version-numbering.md)，以及[如何將套件發佈到不同的作業系統](guidance-for-app-package-management.md)。

## <a name="uploading-packages-to-your-submission"></a>將套件上傳到您的提交

若要上傳套件，請將套件拖曳到欄位內，或按一下以瀏覽您的檔案。 **\[套件\]** 頁面可讓您上傳 .xap、.appx、.appxupload 和/或 .appxbundle 檔案。

當您建立新的提交時，您將會在 [套件](package-flights.md)頁面上看到一個下拉式清單，其中包含從其中一個套件正式發行前小眾測試版複製套件的選項。 選取含有您要納入之套件的套件正式發行前小眾測試版。 然後，您可選取其任一或所有套件，以包含在此提交中。

> **重要** 針對 Windows 10，您應一律在此處上傳 .appxupload 檔案，而非 .appx 或 .appxbundle。 如需針對市集封裝 UWP 應用程式詳細資訊，請參閱[封裝適用於 Windows 10 的通用 Windows 應用程式](../packaging/packaging-uwp-apps.md)。

如果我們在進行驗證時偵測到您套件的問題時，您必須移除套件、修正問題，然後再重新上傳。 如需詳細資訊，請參閱[解決套件上傳錯誤](resolve-package-upload-errors.md)。

您也會看到警告，讓您知道可能會造成問題的相關資訊，但不會阻止您繼續提交。

## <a name="device-family-availability"></a>裝置系列可用性

在您的套件成功上傳之後，[裝置系列可用性]**** 區段會顯示一個資料表以排序方式指出哪個套件將提供給特定的 Windows 10 裝置系列 (以及舊版 OS，如果適用的話)。 本區段可讓您選擇是否要針對特定 Windows 10 裝置系列上的使用者提供提交。

> **注意** 如果您尚未上傳套件，[裝置系列可用性]**** 區段將顯示 Windows 10 裝置系列與核取方塊，以指出提交是否會提供給那些裝置系列上的客戶。 在您上傳您的套件之後，資料表才會顯示。

您也將看到一個核取方塊，讓您可以指出是否要讓 Microsoft 將 App 提供給未來的 Windows 10 裝置系列。 我們建議您保持選取此方塊，讓您的 App 在新裝置系列引入時可供更多潛在客戶使用。

### <a name="choosing-which-device-families-to-support"></a>選擇支援的裝置系列

如果您不希望將您的提交提供給該裝置類型上的使用者，您可以取消選取任何 Windows 10 裝置系列的方塊。 如果未選取裝置系列的方塊，該裝置類型上的新客戶就無法取得 App (雖然已經有 App 的客戶仍然可以使用，且取得您所提交的任何更新)。 

> **注意** **Windows 8/8.1** 和 **Windows Phone 8.x 及更早版本**沒有核取方塊。 如果您的提交包含了可以在這些 OS 版本上運行的套件，則會提供這些套件給客戶。 若要停止提供您的 App 給舊版 OS 上的客戶，您必須從您的提交移除對應的套件。

如果您的 App 支援行動裝置與電腦裝置系列，建議您維持選取 **Windows 10 行動裝置版**和 **Windows 10 Desktop** 的方塊，除非您有特定原因必須限制可以取得您的 App 的 Windows 10 裝置類型。 例如，您可能已建立 Windows 通用套件，但是您仍然需要測試 App 在行動裝置上的一些問題。 若要防止新客戶在 Windows 10 行動裝置上下載 App，您可以取消選取這裡的 [Windows 10 行動裝置版]**** 核取方塊。 然後，如果您之後決定要提供給 Windows 10 行動裝置的客戶，您可以建立新的提交並且勾選 [Windows 10 行動裝置版]**** 方塊。

如果您的 App 不是遊戲 (或者如果是遊戲，而您已經完成[概念核准](../gaming/concept-approval.md)程序)，且您的提交包含使用 Windows 10 SDK 版本 14393 或更新版本編譯的中性和/或 x64 UWP 套件，您可以選取 **\[Windows 10 Xbox\]** 方塊來提供 App 給 Xbox 上的客戶。 

> **重要** 為了讓您的 App 在 Xbox 裝置上啟動，您必須包含使用 Windows SDK 版本 14393 或更新版本編譯的中性或 x64 套件。 不過，如果您選取 \[Windows 10 Xbox\]，您可用於 Xbox 的最高版本套件 (也就是以 Xbox 或通用裝置系列為目標的中性或 x64 套件) 將一律提供給 Xbox 上的客戶，即使套件是以舊版 SDK 編譯。 基於這個原因，確保適用於 Xbox 的最高版本套件是以 Windows SDK 版本 14393 或更高版本進行編譯非常重要。 如果不是，您會看到錯誤訊息，指出 Xbox 客戶不能啟動您的 App。 
> 
> 若要解決這個錯誤，您可以執行下列其中一項：
> -    以使用 Windows SDK 版本 14393 或更高版本編譯的套件取代適用的套件。
> -    如果您已經有支援 Xbox 且以 Windows SDK 版本 14393 或更高版本編譯的套件，請增加其版本號碼，這樣才能是提交中最高版本的套件。
> -    取消選取 [Windows 10 Xbox]**** 方塊。
>     
> 如果您仍然無法解決問題，請連絡支援服務。

如果您已測試您的 App 以確保它可以在 Microsoft HoloLens 上適當地執行，您也可以勾選 [Windows 10 全像攝影版]**** 方塊將此 App 提供給 HoloLens 客戶。 如需建置、測試及發佈全像攝影 App 的詳細資訊，請參閱 [Windows 全像攝影開發概觀](http://dev.windows.com/holographic/development_overview)。

> **重要** 若要完全防止特定 Windows 10 裝置系列取得您的 App，您需要更新您的 appx 資訊清單中的 [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) 元素，讓目標僅限於要支援的裝置系列 (也就是 Windows.Mobile 或 Windows.Desktop)，而不是讓它做為 Microsoft Visual Studio 預設包含在 appx 資訊清單中的 Windows.Universal 值 (適用於通用裝置系列)。

請務必注意您在這裡所做的選擇僅會套用至新擷取。 已經有您的 App 的任何人都能繼續使用並取得您提交的任何更新，即使您在這裡移除該裝置系列也一樣。 甚至會套用至在升級至 Windows 10 之前就取得您的 App 的客戶。 例如，如果您有具有 Windows Phone 8.1 套件的已發佈 App，您稍後將 Windows 10 (UWP) 套件新增至目標為通用裝置系列的相同 App，具有您的 Windows Phone 8.1 套件的 Windows 10 行動裝置客戶就會取得這個 Windows 10 (UWP) 套件的更新，即使您已經取消選取 [Windows 10 行動裝置版]**** 方塊 (因為這不是新購買而是更新)。 但是，如果您未提供目標為通用或行動裝置系列的任何 Windows 10 (UWP) 套件，則您的 Windows 10 行動裝置客戶仍然保持為 Windows Phone 8.1 套件。

如需裝置系列的詳細資訊，請參閱[通用 Windows 平台 (UWP) App 指南](https://msdn.microsoft.com/library/windows/apps/dn894631)和 [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903)。

### <a name="understanding-ranking"></a>了解排行

除了讓您指出那些 Windows 10 裝置系列可以下載您的提交，本區段也顯示您的哪些特定套件可以提供特定裝置系列使用。 如果您有多個套件可在特定裝置系列上執行，資料表會根據套件的版本號碼，指出提供套件的順序。 如需有關市集如何根據版本號碼排序套件的詳細資訊，請參閱[套件版本編號](package-version-numbering.md)。 

例如，假設您有兩個套件︰Package_A.appxupload 與 Package_B.appxupload。 針對特定的裝置系列，如果 Package_A.Appxupload 排序為 1 而 Package_B.Appxupload 排序為 2，則表示當該裝置類型上的使用者取得您的 App 時，市集會先嘗試傳遞 Package_A.Appxupload。 如果客戶的裝置無法執行 Package_A.appxupload，則市集會提供 Package_B.appxupload。 如果客戶的裝置無法執行任何適用於該裝置系列的套件—例如，如果您的 App 支援的 **MinVersion** 高於客戶裝置上的版本—客戶就無法在該裝置上下載 App。

> **注意** 在決定要提供給指定客戶哪一個套件時，不會考量 .xap 套件中的版本號碼。 基於這個原因，如果您有多個同樣順位的 .xap 套件，您會看到星號而非號碼，而客戶可能會收到任一套件。 若要將客戶從一個 .xap 套件更新為較新的套件，請務必在新的提交中移除較舊的 .xap。



## <a name="package-details"></a>套件詳細資料

成功上傳套件之後，我們將會依照目標作業系統分組，列出這些套件。 系統將會顯示套件的名稱、版本及架構。 如需有關如每個套件支援的語言、App 功能，以及檔案大小的詳細資訊，可以按一下 [顯示詳細資料]****。

如果您使用 [Windows 廣告流量分配](../monetize/use-ad-mediation-to-maximize-revenue.md)，您也會看到可以為每個套件設定廣告流量分配的連結。

如果您需要移除您提交的某個套件，可以按一下每個套件的 **\[詳細資料\]** 區段底部的 **\[移除\]** 連結。

## <a name="removing-redundant-packages"></a>移除重複的套件

如果我們偵測到您的一或多個套件重複，我們將顯示一個警告，建議您從此提交中移除重複的套件。 當您先前已上傳套件，而現在又提供支援相同客戶群的更新版本套件時，通常會發生這個狀況。 在此情況下，客戶將不會取得重複的套件，因為您有更好 (更高版本) 的套件可以支援這些客戶。

當我們偵測到您有重複的套件時，我們將提供一個自動從此提交移除所有重複套件的選項。 如果需要，您也可以從此提交個別移除套件。

## <a name="gradual-package-rollout"></a>漸進式套件推出

如果您的提交是先前已發佈之 App 的更新，您會看到 [發佈此提交後漸進式推出更新 (僅限 Windows 10 客戶)]**** 核取方塊。 這可讓您選擇可從提交取得套件之客戶的百分比，這樣您就能監視意見反應與分析資料，以確保您在更廣泛地推出更新之前更有信心。 您隨時可以增加百分比 (或停止更新)，而不必建立新的提交。 

如需詳細資訊，請參閱[漸進式套件推出](gradual-package-rollout.md)。

## <a name="mandatory-update"></a>強制更新

如果您的提交是先前已發佈之 App 的更新，您會看到 [進行這項強制更新]**** 核取方塊。 這可讓您設定強制更新的日期與時間，假設您已經使用 Windows.Services.Store API 來讓您的 App 以程式設計方式檢查套件更新，以及下載和安裝已更新套件。 您的 App 必須以 Windows 10 版本 1607 或更新版本為目標，才能使用此選項。

如需詳細資訊，請參閱[下載與安裝 App 的套件更新](../packaging/self-install-package-updates.md)。

 





