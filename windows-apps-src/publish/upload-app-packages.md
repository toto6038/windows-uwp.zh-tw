---
author: jnHs
Description: 套件 頁面是為您要提交的應用程式上傳所有套件檔案 (.xap、.appx、.appxupload 和/或 .appxbundle) 的所在之處。 您可以在此步驟中，為您的應用程式對象上傳適用於任何作業系統的套件。
title: 上傳應用程式套件
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
---

# 上傳應用程式套件


\[套件\] 頁面是為您要提交的 app 上傳所有套件檔案 (.xap、.appx、.appxupload 和/或 .appxbundle) 所在之處。 您可以在此步驟中，為您的應用程式對象上傳適用於任何作業系統的套件。 當客戶下載您的 app 時，市集將會查看您所有 app 的套件，並自動為每個客戶提供最適合他們的裝置的套件。

如需有關套件內容及其建構方式的詳細資訊，請參閱[應用程式套件需求](app-package-requirements.md)。 您也會想要了解[版本號碼可能會影響哪個套件傳遞給特定客戶](package-version-numbering.md)，以及[如何將套件發佈到不同的作業系統](guidance-for-app-package-management.md)。

## 將套件上傳到您的提交


若要上傳套件，請將套件拖曳到欄位內，或按一下以瀏覽您的檔案。 \[套件\] 頁面可讓您上傳 .xap、.appx、.appxupload 和/或 .appxbundle 檔案。

當您建立新的提交時，您將會在 [套件](package-flights.md)頁面上看到一個下拉式清單，其中包含從其中一個套件正式發行前小眾測試版複製套件的選項。 選取含有您要納入之套件的套件正式發行前小眾測試版。 然後，您可選取其任一或所有套件，以包含在此提交中。

> **重要事項**：針對 Windows 10，您應一律在此處上傳 .appxupload 檔案，而非 .appx 或 .appxbundle。 如需針對市集封裝 UWP 應用程式詳細資訊，請參閱[封裝適用於 Windows 10 的通用 Windows 應用程式](../packaging/packaging-uwp-apps.md)。

如果我們在進行驗證時偵測到您套件的問題時，您必須移除套件、修正問題，然後再重新上傳。 如需詳細資訊，請參閱[解決套件上傳錯誤](resolve-package-upload-errors.md)。

您也會看到警告，讓您知道可能會造成問題的相關資訊，但不會阻止您繼續提交。

## 套件詳細資料


成功上傳套件之後，我們將會依照目標作業系統分組，列出這些套件。 系統將會顯示套件的名稱、版本及架構。 如需有關如每個套件支援的語言、app 功能，以及檔案大小的詳細資訊，可以按一下 **詳細資料**。

如果您使用 [Windows 廣告流量分配](../monetize/use-ad-mediation-to-maximize-revenue.md)，您也會看到可以為每個套件設定廣告流量分配的連結。

如果您需要移除您提交的某個套件，可以按一下每個套件的 \[詳細資料\] 區段底部的 \[移除\] 連結。

## 移除重複的套件


如果我們偵測到您的一或多個套件重複，我們將顯示一個警告，建議您從此提交中移除重複的套件。 當您先前已上傳套件，而現在又提供支援相同客戶群的更新版本套件時，通常會發生這個狀況。 在此情況下，客戶將不會取得重複的套件，因為您有更好 (更高版本) 的套件可以支援這些客戶。

當我們偵測到您有重複的套件時，我們將提供一個自動從此提交移除所有重複套件的選項。 如果您想要，也可以從此提交個別移除套件。

## 包含 Visual Studio Application Insights 的套件


我們建議您在套件中使用 [Visual Studio Application Insights](http://go.microsoft.com/fwlink/?LinkId=615086) \(或在建置您的套件時選取 \[在 Windows 開發人員中心顯示遙測\] 方塊啟用它\) 讓我們可以提供您[應用程式使用量遙測詳細資料](usage-report.md)。 如果您沒有在 Microsoft Visual Studio 中設定 Application Insights，當我們偵測到套件含有它時，將會顯示一個訊息，確認在您提交套件時同意啟用有關您開發人員帳戶的 app 使用方式遙測。 您可以在 \[帳戶設定\] 中，隨時停用 app 使用方式遙測。

 

 






<!--HONumber=May16_HO2-->


