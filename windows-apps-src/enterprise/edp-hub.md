---
author: mcleblanc
Description: "這是中樞主題，涵蓋企業資料保護 (EDP) 與檔案的關聯、緩衝區、剪貼簿、網路、背景工作以及鎖定時的資料保護的完整開發人員描述。"
MS-HAID: dev\_enterprise.edp\_hub
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "企業資料保護 (EDP)"
translationtype: Human Translation
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: 97bdbce8360fabad63f9fe7e85e5172ccd83f403

---

# 企業資料保護 (EDP)

__注意__：企業資料保護 (EDP) 原則無法套用於 Windows 10 1511 版 (組建 10586) 或更早版本。

這是中樞主題，涵蓋企業資料保護 (EDP) 與檔案的關聯、緩衝區、剪貼簿、網路、背景工作以及鎖定時的資料保護的完整開發人員描述。

如需了解使用者和系統管理員觀點的 EDP 詳細資訊，請參閱[企業資料保護 (EDP) 概觀](https://technet.microsoft.com/library/dn985838(v=vs.85).aspx)。

## 什麼是 EDP？

EDP 是桌上型電腦、膝上型電腦、平板電腦和手機上的一組功能，用來進行行動裝置管理 (MDM)。 EDP 讓企業更能控制企業所管理裝置上的資料 (企業檔案和資料 Blob) 處理方式。

-   企業資料會使用加密進行標記。 這是「受企業保護資料」，或簡稱「受保護資料」
-   只有管理企業明確透過 EDP 原則允許的應用程式才能存取受企業保護資料 (例如在檔案中)。
-   只有明確透過 EDP 原則允許的應用程式才會具有企業虛擬私人網路 (VPN) 存取權。
-   應用程式限制原則也會決定允許的應用程式應該如何處理企業資料。
-   原則限制甚至適用於透過 Windows 剪貼簿或分享協定所交換的企業內容。
-   依需求，管理企業可以撤銷裝置對保護內容的存取，基本上會清除裝置上的企業資料，但保留個人資料。
-   通道應用程式是可下載保護資料的應用程式。 範例包括電子郵件和檔案同步處理應用程式。

EDP 可增強[加密檔案系統 (EFS)](http://technet.microsoft.com/library/cc700811.aspx) 和 [Windows 選擇性抹除](https://technet.microsoft.com/library/dn486874.aspx)，提供更多安全性和彈性選項。 新的 EDP API 可讓您建立保護和撤銷企業內容存取的應用程式、使用保護檔案屬性，以及以原始形式存取已加密資料。 除了引進新的 API 來保護檔案和資料夾之外，還引進 API 來保護緩衝區和串流。 同時引進一組 API，讓應用程式可以識別並指出必須強制執行資料保護原則的企業。

因此，管理企業可以控制對其保護資料的存取、應用程式限制原則會定義應用程式清單，以及那些應用程式的限制。 應用程式預設無法獨立存取受保護資料。 為了進行存取，應用程式必須新增到稱為允許清單的清單中，而允許清單上的應用程式稱為允許應用程式。 在受管理裝置上，Windows 可以限制和 (或) 稽核對剪貼簿上或透過分享協定的受保護資料的存取，因此，會稽核不在允許清單上的應用程式的存取，以及 (或) 需要取得使用者同意，否則將完全予以封鎖。

管理企業會提供裝置的 EDP 原則 (這讓裝置成為「受管理裝置」)。 原則的佈建可以透過使用者在行動裝置管理 (MDM) 中註冊、IT 手動設定或另一個管理和原則傳遞機制 (例如 System Center Configuration Manager (SCCM))。

EDP 檔案保護會運用 Rights Management Service (RMS) 金鑰 (如果已佈建)，因為這些金鑰可以跨裝置漫遊，因而允許受保護資料漫遊。 如果沒有 RMS 金鑰，這些 API 將回復為本機選擇性抹除金鑰，並限制漫遊功能。 透過 Microsoft 所提供的平台特定 RMS 應用程式，以及啟用 RMS 的協力廠商應用程式，可以在舊版 Windows 和協力廠商裝置上存取透過加密漫遊的資料。

總結來說，企業可以管理使用 EDP API 所保護的資料，因此您可以建置應用程式來協助企業保護和管理其資料。 換句話說，您可以建置企業用應用程式。 並且協助您只進行本指南其餘部分的作業。

## 設定您的電腦使用 EDP


為了正確地開發您的應用程式，並測試它在企業中的行為，您會想要使用正確的條件來設定您的電腦或裝置。 這通常牽涉到 IT 系統管理員領域中的幾個工作。

-   在行動裝置管理 (MDM) 中註冊您的開發電腦。
-   透過 [AppLocker 設定服務提供者](https://msdn.microsoft.com/library/windows/hardware/dn920019)，將您的應用程式新增到允許清單。

下一個工作是建置可感知的應用程式，並且可以動態回應在其內執行之企業的受管理身分識別以及作用中的保護原則。 這表示「啟用」您的應用程式。 啟用以遵循原則的應用程式極有可能在允許存取企業資料的清單中。

## 啟用企業的應用程式


應用程式位於允許清單之後，即可讀取受保護的資料。 而且，系統預設會自動保護您應用程式的任何資料輸出。 這項自動保護的原因是管理企業必須透過一或多種方式確保仍然可以自行控制企業資料。 但是，這麼近的控制應用程式只是達成該目的的預設方式。 更好的方式是讓系統足夠信任您，以提供給您更多的能力和彈性。 而且，該許可的代價就是讓您的應用程式變得更為聰明。 這表示更進一步，而不只是處理允許清單；它表示讓您的應用程式並將其宣告為啟用企業。

如果應用程式使用我們將說明的技術來獨立保護企業資料 (不論資料是待用、使用中還是傳送中)，則已啟用您的應用程式。 您的啟用應用程式可辨識企業資料來源和企業資料，並在資料到達您的應用程式時予以保護。 變成啟用，也表示只要企業資料離開您的應用程式，就會感知並遵守 EDP 原則。 這包括不允許內容進入非企業網路端點、先以可攜式加密格式包裝資料再讓它漫遊，而且可能 (根據原則設定) 會先提示使用者再將企業資料貼入不在允許清單上的應用程式。 啟用應用程式之後，您的應用程式會透過宣告限制的 **enterpriseDataPolicy** 功能，以向系統宣告它是啟用的應用程式。 如需使用受限制功能的詳細資訊，請參閱[特殊和受限制的功能](https://msdn.microsoft.com/library/windows/apps/mt270968#special_and_restricted_capabilities)。

在理想的情況下，所有企業資料都是受保護的資料 (待用和傳送中)。 但是，不可避免地，最初產生的企業資料與正在保護的企業資料之間必須有一段簡短期間。 而且，企業資料有時可以存在於企業網路端點上，而不進行加密。 啟用的應用程式可以獨立保護這類資料；允許但不啟用的應用程式需要具有系統所公開的保護。

原因是未啟用的應用程式一律會以企業模式執行。 系統可確保這項作業。 但是，針對啟用的應用程式在任何指定時間使用的資料類型，應用程式可以任意且適當地在企業模式與個人模式之間自由切換。 對啟用的應用程式而言，尊重個人資料以及不要將個人資料標記為企業資料，也十分重要。 只要遵守這些承諾，啟用的應用程式就可以同時處理企業資料與個人資料。 下一節示範如何透過程式碼切換模式。

## 確認身分識別受到管理，以及判斷保護原則強制層級

您的應用程式通常是從外部資源 (例如信箱電子郵件地址、受管理的網域或 URI 主機名稱) 取得企業身分識別。 您可以呼叫 [**ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync**](https://msdn.microsoft.com/library/windows/apps/dn706027)，以取得網路端點主機名稱的受管理身分識別 (如果有的話)。

這個程式碼範例說明已啟用行為的一般結構，包括如何判斷是否實際管理企業身分識別，以及哪些原則目前作用中。

```CSharp
using Windows.Security.EnterpriseData;

...

string identity = "contoso.com";

if (ProtectionPolicyManager.IsIdentityManaged(identity))
{
    EnforcementLevel enforcementLevel = ProtectionPolicyManager.GetEnforcementLevel();

    // During UI activities or network access, call ProtectionPolicyManager APIs
    // (taking the enforcement level into account) to ensure that the system
    // tags data with the identity as appropriate.

    ProtectionPolicyManager.GetForCurrentView().Identity = identity;
    // The app is now in enterprise mode.

    ProtectionPolicyManager.GetForCurrentView().Identity = string.Empty;
    // The app is back in personal mode.
}
else
{
    // No policy enforcement is done on this identity.
}
```

如所示，您先判斷是否為企業的身分識別設定 EDP 原則。 「受管理」這個詞是「受 EDP 原則管理」的縮寫。 設定特定身分識別的 EDP 原則時，該身分識別的 [**ProtectionPolicyManager.IsIdentityManaged**](https://msdn.microsoft.com/library/windows/apps/dn705171) 會傳回 true。 這是搭配使用 EDP API 與該身分識別的提示。 雖然檔案和緩衝區 API 的例外在於它們會如預期運作 (即使是未受管理的身分識別也是一樣)，但是這種情況沒有任何意義。 如果裝置受到管理，則會針對企業身分識別進行管理。 如果身分識別未受管理，則保護該身分識別的資料沒有意義。

下一步是決定和實作原則強制層級。 若要決定原則強制層級，請呼叫 [**GetEnforcementLevel**](https://msdn.microsoft.com/library/windows/apps/mt608406) 方法。 對身分識別強制執行企業原則時，啟用的應用程式必須在 UI 活動或網路存取期間呼叫 [**ProtectionPolicyManager**](https://msdn.microsoft.com/library/windows/apps/dn705170) API，以使用原則強制來協助系統確保系統使用此身分識別來標記資料傳輸 (必要時)。 本指南包含如何解譯強制層級並將它放諸實行的詳細資訊。 此程式碼範例也會示範如何進入企業模式以及返回個人模式，方法分別是將 [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) 的值設定為企業身分識別或空字串。 再次請您注意，進入和離開企業模式，只對使用受管理的身分識別才有意義。

## EDP 功能簡介


**檔案和緩衝區保護。**

-   您的應用程式可以保護、包含和抹除與企業身分識別相關聯的資料。
-   金鑰管理是由 Windows 所處理。 裝置可以使用企業的 RMS 金鑰時，Windows 就會使用該 RMS 金鑰；否則，Windows 會回復為本機選擇性抹除保護。

**裝置原則管理。**

-   您的應用程式可以查詢用來管理裝置的身分識別 (企業或組織)。
-   您的應用程式可以保護使用者免於意外公開資料，方法是關聯身分識別與有問題的資料。
-   您的應用程式可以保護網路上的企業資源，方法是檢查企業擁有的網路端點連線 (伺服器，IP 範圍)，以及關聯資料與受管理的 (即由 MDM 註冊) 身分識別。
-   EDP API 只會使用具有裝置上所定義 EDP 原則的受管理身分識別。 如果身分識別未受管理，則 API 會向應用程式指出這種情況 (必要時)。

以下是切入這些特定功能區域特有的 EDP API 和案例的主題連結清單。

## 檔案

請參閱[使用 EDP 保護檔案](../files/protect-your-enterprise-data-with-edp.md)。

## 串流和緩衝區

請參閱[使用 EDP 保護串流和緩衝區](../files/use-edp-to-protect-streams-and-buffers.md)。

## 剪貼簿、分享以及在應用程式之間交換資料

請參閱[使用 EDP 保護在應用程式之間傳輸的企業資料](../app-to-app/use-edp-to-protect-enterprise-data-transferred-between-apps.md)。

## 網路功能

請參閱[使用 EDP 身分識別標記網路連線](../networking/tagging_network_connections_with_edp_identity.md)。

## 鎖定時的資料保護 (DPL) 和背景工作

組織可以選擇管理安全的「鎖定時的資料保護」(DPL) 原則，在此情況下，鎖定裝置時，會從裝置記憶體暫時移除存取受保護資源所需的加密金鑰。 若要針對這個可能性來準備您的應用程式，請參閱本主題中的[處理裝置鎖定事件並避免在記憶體中保留未受保護的內容](#handle_lock_events)一節。 此外，如果您應用程式的背景工作需要保護檔案，請參閱[保護新檔案中的企業資料 (適用於背景工作)](../files/protect-your-enterprise-data-with-edp.md#protect_data_new_file_bg)。

## UI 原則強制執行


在本節中，我們將採用啟用的郵件應用程式範例，而此郵件應用程式目前顯示一組信箱中的企業信箱，而這組信箱同時包括屬於使用者的企業信箱和個人信箱。 若要確定企業信箱中的企業資料沒有資料外洩，應用程式會呼叫 [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791)，確定作業系統知道應用程式的目前內容 (不論是企業還是個人)。 如果身分識別未受企業原則管理，則 API 會傳回 false。

```CSharp
using Windows.Security.EnterpriseData;

...

public class Mailbox
{
    public bool HasEnterpriseMail { get { /* implementation */ } }
    public string Identity { get { /* implementation */ } }
}

private void SwitchMailbox(Mailbox targetMailbox)
{
    // Code goes here to perform setup for "targetMailbox".

    if (targetMailbox.HasEnterpriseMail)
    {
        bool result = 
            ProtectionPolicyManager.TryApplyProcessUIPolicy(targetMailbox.Identity);

        // Code goes here to process "result", which indicates whether
        // or not policy enforcement is in place for the identity.
    }
    else
    {
        // For personal mailboxes, we clear policy enforcement (in case
        // it is still set from when we processed an enterprise mailbox).
        ProtectionPolicyManager.ClearProcessUIPolicy();
    }
}
```

## 處理裝置鎖定事件並避免在記憶體中保留未受保護的內容


在這個案例中，我們將採用設計成處理企業郵件和個人郵件的啟用的郵件應用程式範例。 在選擇管理安全「鎖定時的資料保護」(DPL) 原則的組織中執行應用程式時，應用程式必須確定鎖定裝置時從記憶體移除所有敏感性資料。 若要這樣做，它會註冊 [**ProtectionPolicyManager.ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787) 和 [**ProtectionPolicyManager.ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786) 事件，以在鎖定和解除鎖定裝置時收到通知 (如果 DPL 作用中)。

暫時移除裝置上所佈建的資料保護金鑰之前，會引發 [**ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787)。 鎖定裝置時移除這些金鑰的原因是要確保鎖定裝置時無法對加密資料進行未經授權存取，也可能是未擁有其擁有者。 金鑰在解除鎖定裝置時再次可用之後，會引發 [**ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786)。

透過處理這兩個事件，應用程式可以使用 [**DataProtectionManager**](https://msdn.microsoft.com/library/windows/apps/dn706017) 來確定保護記憶體中的任何敏感性內容。 它也應該關閉其受保護檔案上開啟的任何檔案串流，確保系統未在記憶體中快取任何敏感性資料。 您可以使用數種方式的其中一種方式，來執行這項作業。 若要關閉從 **StorageFile** 的 Open 方法傳回的檔案串流，您可以對該串流呼叫 **Dispose** 方法。 您可以使用 using 陳述式 (C\# 或 VB) 限定串流的使用。 或者，您可以將 **DataReader** 或 **DataWriter** 物件包裝在串流中，以及搭配使用 **Dispose** 方法或 using 陳述式與該物件。

**注意**  
在未設定 DPL 原則的環境中，會引發 [**ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786) 事件，而不是 [**ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787) 事件。 請在程式碼中注意這種情況，並且小心不假設事件在每個系統上一律都成對出現，您也不是一律都會使用事件來決定裝置的已鎖定/解除鎖定狀態。 您可以在這裡的程式碼範例中看到這種情況，而在程式碼範例中，我們小心不假設有關目前顯示電子郵件的受保護狀態的任何項目，也不要假設有關資料庫檔案串流的開啟狀態的任何項目。

同時，請注意，在未設定 DPL 原則的裝置上從鎖定繼續時，[**ProtectedAccessResumedEventArgs.Identities**](https://msdn.microsoft.com/library/windows/apps/dn705772) 是空集合。

為了簡單起見，此程式碼範例已稍微簡化，並著重於處理企業郵件。 在真實的應用程式中，個人郵件將會寫入不同且未受保護的郵件資料庫檔案中，並不會保護鎖定時的個人電子郵件。

```CSharp
using Windows.Security.Cryptography;
using Windows.Security.EnterpriseData;
using Windows.Storage;
using Windows.Storage.Streams;

...

public class DisplayedMail
{
    public string Body { get; set; }
    public IBuffer ProtectedBody { get; set; }
    public bool IsProtected { get; set; }
}

private IOutputStream mailDatabaseStream = null;
private string currentlyDisplayedMailIdentity = "contoso.com";
private DisplayedMail currentlyDisplayedMail = new DisplayedMail()
    { Body = "Lorem ipsum dolor...", ProtectedBody = null, IsProtected = false };

// Gets the app's protected mail database file, then opens and stores a stream on it.
private async void OpenMailDatabase()
{
    // Only attempt to open the database file stream if we know it's closed.
    if (this.mailDatabaseStream == null)
    {
        StorageFolder appDataStorageFolder = ApplicationData.Current.LocalFolder;
        StorageFile storageFile = await appDataStorageFolder.GetFileAsync("maildb.dat");
        using (IRandomAccessStream randomAccessStream =
            await storageFile.OpenAsync(FileAccessMode.ReadWrite))
        {
            this.mailDatabaseStream = randomAccessStream.GetOutputStreamAt(0);
        }
    }
}

// Called once by the app when starting up.
private void AppSetup()
{
    ProtectionPolicyManager.ProtectedAccessSuspending +=
        this.ProtectionPolicyManager_ProtectedAccessSuspending;
    ProtectionPolicyManager.ProtectedAccessResumed +=
        this.ProtectionPolicyManager_ProtectedAccessResumed;
    this.OpenMailDatabase();
}

// Background work called when the app receives an email.
private async void AppMailReceived(string fauxEmail)
{
    // Only attempt to write to the database file stream if we know it's open.
    if (this.mailDatabaseStream != null)
    {
        IBuffer emailAsBuffer = CryptographicBuffer.ConvertStringToBinary
            (fauxEmail, BinaryStringEncoding.Utf8);
        await this.mailDatabaseStream.WriteAsync(emailAsBuffer);
        await this.mailDatabaseStream.FlushAsync();
    }
    else
    {
        // Code goes here to handle the case where the device is
        // locked and we can't access the protected mail database.
    }
}

// Called by ProtectionPolicyManager when the device is locked if under DPL.
private async void ProtectionPolicyManager_ProtectedAccessSuspending
    (object sender, ProtectedAccessSuspendingEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.currentlyDisplayedMailIdentity))
    {
        // This event is not for our identity. Another will be sent for our identity.
        return;
    }

    // Get suspension deferral.
    Windows.Foundation.Deferral deferral = e.GetDeferral();

    // Protect the displayed mail content.
    if (!this.currentlyDisplayedMail.IsProtected)
    {
        IBuffer mailBodyBuffer = CryptographicBuffer.ConvertStringToBinary
            (this.currentlyDisplayedMail.Body, BinaryStringEncoding.Utf8);
        BufferProtectUnprotectResult result = await DataProtectionManager.ProtectAsync
            (mailBodyBuffer, this.currentlyDisplayedMailIdentity);
        if (result.ProtectionInfo.Status == DataProtectionStatus.Protected)
        {
            // Save the protected version and clear the unprotected version.
            this.currentlyDisplayedMail.ProtectedBody = result.Buffer;
            this.currentlyDisplayedMail.Body = null;
        }
    }

    // Close the mail database file stream to make sure that we have no unprotected
    // content in memory.
    this.mailDatabaseStream.Dispose();
    this.mailDatabaseStream = null;

    // Optionally, code goes here to use e.Deadline to determine whether we have more
    // than 15 seconds left before the suspension deadline. If we do then process any
    // messages queued up for sending while we are still able to access them.

    // All done. Complete deferral.
    deferral.Complete();
}

// Called by ProtectionPolicyManager when the device is unlocked.
private async void ProtectionPolicyManager_ProtectedAccessResumed
    (object sender, ProtectedAccessResumedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.currentlyDisplayedMailIdentity))
    {
        // This event is not for our identity. Another will be sent for our identity.
        return;
    }

    // Unprotect the displayed mail content.
    if (this.currentlyDisplayedMail.IsProtected)
    {
        BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync
            (this.currentlyDisplayedMail.ProtectedBody);
        if (result.ProtectionInfo.Status == DataProtectionStatus.Unprotected)
        {
            // Restore the unprotected version.
            this.currentlyDisplayedMail.Body = CryptographicBuffer.ConvertBinaryToString
                (BinaryStringEncoding.Utf8, result.Buffer);
            this.currentlyDisplayedMail.ProtectedBody = null;
        }
    }

    // Reopen the closed mail database.
    this.OpenMailDatabase();
}
```

## 註冊以在撤銷受保護內容時收到通知


請繪製郵件應用程式已在使用者裝置上設定企業信箱的案例。 在某個時間點 (以及基於數個可能原因之一)，企業希望撤銷對受企業保護的電子郵件以及該裝置上其他資源的存取。 撤銷有數個可能的原因。 最可能是特定企業原則不是在取消註冊時觸發撤銷，因此有一種情況是使用者已經從企業取消註冊其裝置 (也許是正在將裝置當成禮物或銷售裝置、只想要使用不同的裝置，他們要移到另一項工作，或者他們將退休)。 另一種情況是由 IT 系統管理員遠端透過行動裝置管理 (MDM) 傳送取消註冊通知 (可能是裝置回報為遺失時)。

不論是何種情況，企業都會傳送要求來抹除使用者裝置中的所有郵件，因為已經不再需要它們。 裝置上的遠端管理用戶端會收到來自企業遠端管理伺服器的要求，並呼叫 [**ProtectionPolicyManager.RevokeContent**](https://msdn.microsoft.com/library/windows/apps/dn705790) 以撤銷存取該裝置上受該企業身分識別保護之內容所需的金鑰。

如果您的應用程式需要在撤銷時收到通知，則可以註冊使用 [**ProtectionPolicyManager.ProtectedContentRevoked**](https://msdn.microsoft.com/library/windows/apps/dn705788) 事件。 您的應用程式收到該事件時，可以刪除任何與企業信箱相關聯的中繼資料 (不再需要時)。

```CSharp
using Windows.Security.EnterpriseData;

...

private string mailIdentity = "contoso.com";

void MailAppSetup()
{
    ProtectionPolicyManager.ProtectedContentRevoked += ProtectionPolicyManager_ProtectedContentRevoked;
    // Code goes here to set up mailbox for 'mailIdentity'.
}

private void ProtectionPolicyManager_ProtectedContentRevoked(object sender, ProtectedContentRevokedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.mailIdentity))
    {
        // This event is not for our identity.
        return;
    }

    // Code goes here to delete any metadata associated with 'mailIdentity'.
}
```

## 遠端管理用戶端起始受企業保護資料的抹除


使用者在其個人裝置上有數個受到企業身分識別保護的企業檔案。 使用者遺失其裝置。 如果企業在使用者遺失其裝置時收到通知，則會傳送抹除使用者裝置中所有敏感性資料的要求，以防止資料外洩。 裝置上的遠端管理用戶端會收到來自企業遠端管理伺服器的這個要求，並呼叫 [**ProtectionPolicyManager.RevokeContent**](https://msdn.microsoft.com/library/windows/apps/dn705790) 以撤銷存取受企業身分識別保護的內容所需的金鑰。

```CSharp
Windows.Security.EnterpriseData.ProtectionPolicyManager.RevokeContent("contoso.com");
```

 

 






<!--HONumber=Jun16_HO4-->


