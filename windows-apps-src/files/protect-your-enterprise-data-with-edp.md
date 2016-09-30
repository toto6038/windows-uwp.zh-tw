---
author: TylerMSFT
Description: "本主題說明達成一些最常見的檔案相關企業資料保護 (EDP) 案例所需的程式設計工作範例。"
MS-HAID: dev\_files.protect\_your\_enterprise\_data\_with\_edp
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "使用企業資料保護 (EDP) 來保護檔案"
translationtype: Human Translation
ms.sourcegitcommit: 9b9e9ecb70f3a0bb92038ae94f45ddcee3357dbd
ms.openlocfilehash: a31fc65599f43be5b302b568774a51ab77065300

---

# 使用企業資料保護 (EDP) 來保護檔案

> [!NOTE]
> 企業資料保護 (EDP) 原則無法套用於 Windows 10 1511 版 (組建 10586) 或更早版本。

本主題說明達成一些最常見的檔案相關企業資料保護 (EDP) 案例所需的程式設計工作範例。 如需 EDP 如何與檔案、串流、剪貼簿、網路、背景工作及鎖定時的資料保護產生關係的完整開發人員說明，請參閱[企業資料保護 (EDP)](../enterprise/edp-hub.md)。

> [!NOTE]
> [企業資料保護 (EDP) 範例](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)涵蓋許多與本主題所示範的案例相同的案例。

## 先決條件

-   **設定 EDP**

    請參閱[設定您的電腦使用 EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP)。

-   **致力於建置企業開明 app**

    可自主確保企業資料受企業管控的 app，稱之為企業開明 app。 開明應用程式比非開明應用程式的功能更強、更有智慧、更有彈性且更值得信任。 您的應用程式會透過宣告限制的 **enterpriseDataPolicy** 功能，以向系統宣告它是開明應用程式。 但是，比起設定功能，還有更多開明的行為。 若要深入了解，請參閱[企業開明 app](../enterprise/edp-hub.md#enterprise-enlightened-apps)。

-   **了解通用 Windows 平台 (UWP) app 的非同步程式設計**

    若要了解如何使用 C# 或 Visual Basic 撰寫非同步的應用程式，請參閱[在 C# 或 Visual Basic 中呼叫非同步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)。 若要了解如何使用 C++ 撰寫非同步的應用程式，請參閱 [C++ 的非同步程式設計](https://msdn.microsoft.com/library/windows/apps/mt187334)。

## 您的本機資料夾路徑，以及在檔案總管中檢視受保護的檔案


您可以建立一個 app 來裝載本主題中的程式碼範例，以嘗試使用它們。 程式碼範例將檔案寫入您的 app 本機資料夾，而該資料夾在磁碟上的確切位置，是您的 app 特有的。 若要判斷您的應用程式本機資料夾的位置，可以新增下列程式碼。

```CSharp
// Put a breakpoint on the line after the line of code below. Use the value of localFolderPath
// in File Explorer to find the output file that the later code examples create.
string localFolderPath = ApplicationData.Current.LocalFolder.Path;
```

擁有路徑之後，您就能使用檔案總管輕鬆地找到您的應用程式建立的檔案。 如此一來，您就能確認它們受到保護，並且是以正確的身分識別保護。

在 [檔案總管] 中，勾選 [變更資料夾和搜尋選項] ****，在 [檢視]**** 索引標籤上，選取 [使用色彩顯示加密的檔案]****。 另請使用 [檔案總管] 的 [檢視]****&gt;[新增欄]**** 命令來新增 [已加密到]**** 欄，這樣您就可以看到保護檔案的企業身分識別。

## 保護新檔案中的企業資料 (適用於互動式應用程式)


企業資料可能以多種方法輸入您的 app，包括從特定的網路端點、檔案、剪貼簿或分享協定。 您的 app 也可以建立新的企業資料。 無論您的開明 app 如核取得企業資料，您的 app 在將資料保存到新檔案時，必須以受管理的企業身分識別小心保護資料。

基本步驟是使用一般儲存 API 來建立檔案、使用 EDP API 以企業身分識別保護檔案，然後 (同樣使用一般儲存 API) 來寫入檔案。 寫入之前，請務必小心保護檔案 (如下列範例所示)。 您可以使用 [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) 方法來保護檔案。 而且，一如往常，只有在某個身分識別受管理時，以該身分識別保護才有意義。 如需上述原因的詳細資訊，以及您的應用程式如何判斷它執行時所用的企業身分識別，請參閱[確認身分識別是受管理的](../enterprise/edp-hub.md#confirming-an-identity-is-managed)。

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void SaveEnterpriseDataToFile(string enterpriseData, string identity)
{
    // You should only protect to an identity that is managed.
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFile storageFile = storageFolder.CreateFileAsync("sample.txt",
        CreationCollisionOption.ReplaceExisting);

    // It's important to protect a file *before* writing enterprise data to it.
    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(storageFile, identity);

    // If the file is now protected, to the intended identity, then we can write to it.
    if (fileProtectionInfo.Identity == identity &&
        fileProtectionInfo.Status == FileProtectionStatus.Protected)
        await Windows.Storage.FileIO.WriteTextAsync(storageFile, enterpriseData);
}
```

## 保護新檔案中的企業資料 (適用於背景工作)


我們在上一節使用的 [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) API 僅適用於互動式 app。 針對背景工作，您的程式碼可以在鎖定畫面時執行。 而組織可能使用安全的「鎖定時的資料保護」(DPL) 原則，在此情況下，當裝置被鎖定時，會從裝置記憶體暫時移除存取受保護資源所需的加密金鑰。 這可防止在遺失裝置時發生資料外洩。 這個功能也會在受保護檔案的控制代碼被關閉時，移除與受保護檔案關聯的金鑰。 不過，在鎖定期間 (裝置被鎖定與解除鎖定之間的時間) 可以建立新的受保護檔案並存取它們，同時又讓檔案控制代碼保持開啟。 **StorageFolder.CreateFileAsync** 會在檔案建立時關閉控制代碼，因此不能使用這個演算法。

1.  使用 **StorageFolder.CreateFileAsync** 來建立新檔案。
2.  使用 **FileProtectionManager.ProtectAsync** 來進行加密。
3.  開啟檔案，然後寫入它。

由於步驟 1 牽涉到關閉檔案控制代碼 (即使步驟 1 沒有關閉控制代碼，步驟 2 也會這麼做)，因此步驟 3 不可行，因為用來存取該檔案的加密金鑰已經無法使用。

若要解決這種情況，您可以使用 [**FileProtectionManager.CreateProtectedAndOpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn705153) 來建立受保護的檔案，並將 **IRandomAccessStream** 傳回給它。 這可讓應用程式對檔案進行多次寫入，同時又讓檔案控制代碼保持開啟。

範例程式碼示範一個會在收到新郵件時寫出新檔案的極簡單郵件 app。 郵件 app 必須在裝置被鎖定時同步郵件。 當這個 app 收到新郵件時，它會使用 [**CreateProtectedAndOpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn705153) 來建立新檔案、抓取輸出串流，然後將郵件本文寫入該檔案。

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;
using Windows.Storage.Streams;

...

private async void SaveEnterpriseDataToFile(string enterpriseData, string identity)
{
    // You should only protect to an identity that is managed.
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

    ProtectedFileCreateResult protectedFileCreateResult =
        await FileProtectionManager.CreateProtectedAndOpenAsync(storageFolder,
            "sample.txt", identity, CreationCollisionOption.ReplaceExisting);

    // It's important to successfully protect a file *before* writing enterprise data to it.
    if (protectedFileCreateResult.ProtectionInfo.Identity == identity &&
        protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        using (IOutputStream outputStream =
            protectedFileCreateResult.Stream.GetOutputStreamAt(0))
        {
            using (DataWriter writer = new DataWriter(outputStream))
            {
                writer.WriteString(enterpriseData);
                await writer.StoreAsync();
                await writer.FlushAsync();
            }
        }
    }
    else if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.AccessSuspended)
    {
        // Perform any special processing for the access suspended case.
    }
}
```

## 保護用來包含企業資料的資料夾


如果您希望保護某個資料夾內的所有項目，您也可以這麼做。 您可以使用 [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) 來保護空白資料夾。 如此一來，後續在資料夾內建立的任何檔案或資料夾也會受到保護。 您可以保護現有的資料夾，或可建立新的資料夾來保護 (下列範例會建立新的資料夾)。 但是，在任一種情況下要成功保護資料夾，都必須先清空資料夾。 如果未清空，[**FileProtectionInfo.Status**](https://msdn.microsoft.com/library/windows/apps/dn705150) 將包含 [**FileProtectionStatus.NotProtectable**](https://msdn.microsoft.com/library/windows/apps/dn279147) 的值。

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void CreateANewFolderAndProtectItAsync(string folderName, string identity)
{
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFolder newStorageFolder =
        await storageFolder.CreateFolderAsync(folderName);

    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(newStorageFolder, identity);

    if (fileProtectionInfo.Identity != identity ||
        fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // Protection failed.
    }
}
```

## 從某個檔案複製保護到另一個檔案


在這個案例中，已經有一個您已知以適當的企業身分識別保護的檔案。 您可以很輕鬆地將保護複寫到另一個檔案。 您甚至不需知道身分識別是什麼，只需知道是正確的身分識別。

若要對受保護的檔案進行簡單的複製，您可以呼叫 [**StorageFile.CopyAsync**](https://msdn.microsoft.com/library/windows/apps/br227190)。 產生的檔案複本與原始檔案會具有相同的保護。

若要先保護某個現有的未受保護檔案，再將企業資料寫入該檔案，除了呼叫 [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) (我們在前述案例中已經看到，而且您必須將受管理的身分識別傳遞給它) 以外，也可以呼叫 [**FileProtectionManager.CopyProtectionAsync**](https://msdn.microsoft.com/library/windows/apps/dn705152)，如程式碼範例所示。

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void CopyProtectionFromOneFileToAnother
    (StorageFile sourceStorageFile, StorageFile targetStorageFile)
{
    bool copyResult = await
        FileProtectionManager.CopyProtectionAsync(sourceStorageFile, targetStorageFile);

    if (!copyResult)
    {
        // Copying failed. To diagnose, you could check the file's status.
        // (call FileProtectionManager.GetProtectionInfoAsync and
        // check FileProtectionInfo.Status).
    }
}
```

## 處理存取您保護的檔案時被拒的情況


在這個案例中，您的應用程式嘗試存取一個檔案 (您的應用程式先前保護的檔案)，但是存取被拒。 您必須檢查檔案的狀態，以了解發生什麼錯誤。 在這個程式碼範例中，app 會呼叫 [**FileProtectionManager.GetProtectionInfoAsync**](https://msdn.microsoft.com/library/windows/apps/dn705154) API 來查詢狀態，並判斷原因是否是因為遠端管理導致檔案的存取權現在被撤銷。

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async System.Threading.Tasks.Task<bool> IsFileProtectionStatusRevoked
    (StorageFile storageFile)
{
    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.GetProtectionInfoAsync(storageFile);

    if (fileProtectionInfo.Status == FileProtectionStatus.Revoked)
    {
        // Inform the user that their data has been revoked.
    }
    return fileProtectionInfo.Status == FileProtectionStatus.Revoked;
}
```

## 根據檔案的受保護身分識別來啟用 UI 原則強制執行


當您的 app 即將在其 UI 上顯示受保護檔案 (例如 PDF) 的內容時，它必須根據檔案保護對象的身分識別，啟用 UI 原則強制執行。 您應該查詢檔案的保護資訊，然後從抓取到的身分識別啟用系統的 UI 原則強制執行。

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void EnableUIPolicyFromFile(StorageFile storageFile)
{
    FileProtectionInfo fileProtectionInfo = 
        await FileProtectionManager.GetProtectionInfoAsync(storageFile);

    if (fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // No policy enforcement required, unless the file is inaccessible
        // (Revoked, ProtectedToOtherIdentity) in which case that should
        // be handled in an app-specific way.
        return;
    }

    ProtectionPolicyManager.TryApplyProcessUIPolicy(fileProtectionInfo.Identity);
}
```

> 本文章適用於撰寫通用 Windows 平台 (UWP) App 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## 相關主題

- [企業資料保護 (EDP) 範例](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

- [**Windows.Security.EnterpriseData 命名空間**](https://msdn.microsoft.com/library/windows/apps/dn279153)








<!--HONumber=Jul16_HO1-->


