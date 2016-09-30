---
author: awkoren
Description: "本主題說明達成一些最常見的資料傳輸相關企業資料保護 EDP 案例所需的編碼工作範例。"
MS-HAID: dev\_app\_to\_app.use\_edp\_to\_protect\_enterprise\_data\_transferred\_between\_apps
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "使用 EDP 保護在 app 之間傳輸的企業資料"
translationtype: Human Translation
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: 77533d4aca3cc84e0a021a0faac57f5afbbefdd7

---

# 使用 EDP 保護在 app 之間傳輸的企業資料

__注意__：企業資料保護 (EDP) 原則無法套用於 Windows 10 1511 版 (組建 10586) 或更早版本。

本主題說明達成一些最常見的資料傳輸相關企業資料保護 EDP 案例所需的編碼工作範例。 如需 EDP 如何與檔案、串流、剪貼簿、網路、背景工作及鎖定時的資料保護產生關係的完整開發人員說明，請參閱[企業資料保護 (EDP)](../enterprise/edp-hub.md)。

**注意**：[企業資料保護 (EDP) 範例](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)涵蓋許多與本主題所示範的案例相同的案例。

## 先決條件


-   **設定 EDP**

    請參閱[設定您的電腦使用 EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP)。

-   **致力於建置企業開明 app**

    可自主確保企業資料受企業管控的 app，稱之為企業開明 app。 開明應用程式比非開明應用程式的功能更強、更有智慧、更有彈性且更值得信任。 您的應用程式會透過宣告限制的 **enterpriseDataPolicy** 功能，以向系統宣告它是開明應用程式。 但是，比起設定功能，還有更多開明的行為。 若要深入了解，請參閱[企業開明 app](../enterprise/edp-hub.md#enterprise-enlightened-apps)。

-   **了解通用 Windows 平台 (UWP) app 的非同步程式設計**

    若要了解如何使用 C# 或 Visual Basic 撰寫非同步的應用程式，請參閱[在 C# 或 Visual Basic 中呼叫非同步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)。 若要了解如何使用 C++ 撰寫非同步的應用程式，請參閱 [C++ 的非同步程式設計](https://msdn.microsoft.com/library/windows/apps/mt187334)。

## 簡單的剪貼簿來源


在這個案例中，您的 App 是一個同時處理個人和企業檔案的記事本型 App。 在這裡，您的 App 完全不需變更其複製貼上邏輯；它只需在每次使用者開啟並顯示企業文件中的內容時呼叫 [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791) 即可。 當內容顯示在應用程式的 UI 中之後，使用者可能會接著將它複製並貼到不同的應用程式中，這就是設定 UI 原則有其重要性的原因。 這樣可讓作業系統將目前設定的原則套用到涉及受保護資料的貼上操作。 同樣地，儘快清除已不再需要的 UI 原則也相當重要，如此一來，使用者才能夠再度隨意複製貼上個人資料。 如果 **TryApplyProcessUIPolicy** 的身分識別引數未受企業原則管理，它就會傳回 false。

```CSharp
using Windows.Security.EnterpriseData;

...

private void OnFileLoaded(FileProtectionInfo fileProtectionInfo, string contentsOfFile)
{
    if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy
            (fileProtectionInfo.Identity);
        if (isIdentityManaged)
        {
            // UI policy is now in effect for the file's identity.
        }
        else
        {
            // Enterprise policy is not in effect, because the file's identity
            // is not managed. In this case, we have a file protected to an
            // unmanaged identity, which is not a valid situation.
            // We still have to call ClearProcessUIPolicy if we want to clear the policy.
            ProtectionPolicyManager.ClearProcessUIPolicy();
        }
    }
    else
    {
        // In case we applied the policy for the previous file, now we need to clear it.
        ProtectionPolicyManager.ClearProcessUIPolicy();
    }
    // Code goes here to display "contentsOfFile" in your UI. Ready for copy-paste...
}
```

## 簡單的剪貼簿目標


在這個案例中，您的 App 是一個同時處理個人和企業帳戶的電子郵件 App。 使用者嘗試將資料貼到使用他們個人帳戶的電子郵件回覆中。 在此情況下，您的 App 必須先呼叫 [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636)，才能抓取內容。 如果我們已有存取權，則該方法會立即傳回。 但是，如果我們沒有存取權，方法就會導致顯示一個要求使用者同意的對話方塊，然後在獲得同意時將資料套件「解除鎖定」。 這為了要讓使用者有機會取消不小心進行的貼上操作。

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private async void OnPasteSimple()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        // In case we don't already have acccess, request consent from the user
        // for us to access the clipboard data.
        await dataPackageView.RequestAccessAsync();

        try
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            // Code goes here to insert the text into the email...
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the clipboard.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

## 剪貼簿目標是一個中性的空白文件


在這個案例中，您的應用程式是一個文書處理應用程式。 建立新文件之後，只要該文件保持維空白，您的應用程式就會將該文件視為中性文件 — 既非企業也非個人文件。 接著，您的使用者會將企業資料貼到這個中性內容的文件中。 因為文件是在中性內容中，所以我們實際上只要將文件切換到企業內容並避免一併呼叫 [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636) (因為在該情況下沒有必要顯示對話方塊) 即可。 因此，如果資料受到保護，我們就只要切換到企業內容並貼上資料即可。

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private async void OnPasteWithApplyPolicy()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        // If the data has no enterprise identity, then we already have access.
        if (!string.IsNullOrEmpty(dataPackageView.Properties.EnterpriseId))
        {
            ProtectionPolicyEvaluationResult policyResult =
                await dataPackageView.RequestAccessAsync(dataPackageView.Properties.EnterpriseId);
            if (this.isNewEmptyDocument &&
                policyResult == ProtectionPolicyEvaluationResult.Allowed)
            {
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog.
                bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy(dataPackageView.Properties.EnterpriseId);
                // You can use "isIdentityManaged", but it's not necessary.
            }
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.
                await dataPackageView.RequestAccessAsync();
            }
        }

        try
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            // Code goes here to insert the text into the email...
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the clipboard.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

## 具有明確企業身分識別的剪貼簿來源


在這個案例中，您的應用程式是一個文書處理應用程式。 它使用背景執行緒來認可使用者的複製操作。 使用者會從企業檔案複製一些資料，並立即切換到個人檔案，這時，應用程式的全域內容就會變成個人內容。 此時，由於全域狀態已變更且不應使用，應用程式的背景執行緒必須明確告知剪貼簿傳入的資料是企業資料。 您可以藉由設定 [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861) 屬性來達到此目的。

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private void CopyToClipboard(string stringToCopy, string identity)
{
    // Copy the string to the clipboard.
    var dataPackage = new DataPackage();
    dataPackage.SetText(stringToCopy);
    dataPackage.Properties.EnterpriseId = identity;
    Clipboard.SetContent(dataPackage);
}
```

## 以企業身分識別標記特定視窗


在這個案例中，您的 App 是一個以不同視窗 (其中有些是企業的，其他則是個人的) 處理多個文件的文書處理 App。 應用程式想要確保任何要被貼到個人文件視窗中的資料都會受到正確審查 (也就是遭到拒絕，或者如果是企業資料，則是獲得同意)，而任何從企業文件視窗傳出的資料都會被正確標示。 您可以藉由設定 [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) 屬性來達到此目的。

```CSharp
using Windows.Security.EnterpriseData;

...

private void TagCurrentViewWithEnterpriseId(string identity)
{
    ProtectionPolicyManager.GetForCurrentView().Identity = identity;
}
```

## 以企業身分識別標記傳出拖曳物件


您的 App 有一個已開啟的個人視窗，內含一些可拖曳的企業內容。 使用者開始拖曳此內容的部分，而您的 App 需要確保將該內容標示為企業內容。 這個案例並未涉及任何新的 API。 針對這個案例，您的 App 將會設定 [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861) 屬性 (請參閱上方的[具有明確企業身分識別的剪貼簿來源案例](#clipboard_source_explicit_id))。

## 向收到的拖曳物件查詢其企業身分識別


您的 App 有一個已開啟的新空白文件 (此文件只要是空白的，就會被假設為中性文件)，而使用者將一些內容拖放到此文件中。 應用程式現在必須判斷物件的企業身分識別，以便相應地變更文件的狀態。 針對這個案例，您的應用程式將會藉由讀取 [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861)，從資料套件取得 **EnterpriseId** 屬性 (請參閱上方的[具有明確企業身分識別的剪貼簿來源案例](#clipboard_source_explicit_id))。

## 您的應用程式做為分享協定來源


當您在應用程式中支援分享協定時，若要設定分享來源，請在 [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/br205873) 中設定企業身分識別內容，如這個程式碼範例所示。

**注意**：這個程式碼範例需要您已經在保護原則管理員物件上為您目前的檢視設定身分識別 (請參閱[以企業身分識別標記特定視窗](#tag_window_with_id))，否則 [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) 屬性將會包含空字串。



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private void OnShareSourceOperation(object sender, RoutedEventArgs e)
{
    // Register the current page as a share source (or you could do this earlier in your app).
    DataTransferManager.GetForCurrentView().DataRequested += OnDataRequested;
    DataTransferManager.ShowShareUI();
}

private void OnDataRequested(DataTransferManager sender, DataRequestedEventArgs args)
{
    if (!string.IsNullOrEmpty(this.shareSourceContent))
    {
        var protectionPolicyManager = ProtectionPolicyManager.GetForCurrentView();
        DataPackage requestData = args.Request.Data;
        requestData.Properties.Title = this.shareSourceTitle;
        requestData.Properties.EnterpriseId = protectionPolicyManager.Identity;
        requestData.SetText(this.shareSourceContent);
    }
}
```

## 您的應用程式做為分享協定目標


這個程式碼範例會繼續遵守我們的原則，亦即只要我們正在處理的檔案是空白的， 我們就能依據傳入的資料自由切換成適當的內容，而儘可能避免彈出同意 UI。 因此，當您的 App 是以分享協定目標的身分來接收資料時，如果沒有現有的內容，它應該呼叫 [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791)，否則應該呼叫 [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636)。 程式碼範例將示範如何進行。

```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.ApplicationModel.DataTransfer.ShareTarget;

...

string identity = "microsoft.com";

protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    ShareOperation shareOperation = args.ShareOperation;
    if (shareOperation.Data.Contains(StandardDataFormats.Text))
    {
        // If the data has no enterprise identity, then we already have access.
        if (!string.IsNullOrEmpty(shareOperation.Data.Properties.EnterpriseId))
        {
            ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult =
                await ProtectionPolicyManager.RequestAccessAsync
                    (shareOperation.Data.Properties.EnterpriseId, identity);
            if (this.isNewEmptyDocument && protectionPolicyEvaluationResult ==
                ProtectionPolicyEvaluationResult.Allowed)
            {
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog.
                bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy
                    (shareOperation.Data.Properties.EnterpriseId);
                // You can use "isIdentityManaged", but it';s not necessary.
            }
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.
                protectionPolicyEvaluationResult = await shareOperation.Data.RequestAccessAsync();
            }
        }

        try
        {
            // Get the text from the share operation...
            App.shareTargetContent = await shareOperation.Data.GetTextAsync();
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the share operation.
        }
    }
    else
    {
        // The value in the share operation is not in the text format.
    }
}
```

## 被動查詢複製貼上原則


在這個案例中，您的 App 只有在資料位於剪貼簿上時，才會啟用貼上 UI。 針對這項功能，您可以使用 [**ProtectionPolicyManager.CheckAccess**](https://msdn.microsoft.com/library/windows/apps/dn705783) 方法，此方法可允許被動式原則檢查。

**注意**：這個程式碼範例需要您已經在保護原則管理員物件上為您目前的檢視設定身分識別 (請參閱[以企業身分識別標記特定視窗](#tag_window_with_id))，否則 [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) 屬性將會包含空字串。



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private bool IsClipboardPeekAllowedAsync()
{
    ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = ProtectionPolicyEvaluationResult.Blocked;
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);
    }

    // Since this is a convenience feature to allow a peek of clipboard content,
    // if state is Blocked or ConsentRequired, do not show peek if this helper
    // method returns false.
    return (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed);
}
```

## 要求複製貼上操作的存取權


這個案例說明如何檢查貼上操作的存取權。

**注意**：這個程式碼範例需要您已經在保護原則管理員物件上為您目前的檢視設定身分識別 (請參閱[以企業身分識別標記特定視窗](#tag_window_with_id))，否則 [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) 屬性將會包含空字串。



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private async void OnPasteWithRequestAccess()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);

        if (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed)
        {
            try
            {
                string contentsOfClipboard = await dataPackageView.GetTextAsync();
                // Code goes here to insert the text into the app...
                this.fileContentsTextBox.Text = contentsOfClipboard;
            }
            catch (Exception)
            {
                // Code goes here to handle the exception retrieving text from the clipboard.
            }
        }
        else
        {
            // Paste from the enterprise context is not allowed by policy.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

**注意**：本文適用於撰寫通用 Windows 平台 (UWP) app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。



## 相關主題


[企業資料保護 (EDP) 範例](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData 命名空間**](https://msdn.microsoft.com/library/windows/apps/dn279153)








<!--HONumber=Jun16_HO4-->


