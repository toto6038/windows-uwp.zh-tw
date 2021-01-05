---
title: Unity 和 UWP 中遺失 .NET API
description: 深入了解在 Unity 中建置 UWP 遊戲時遺失 .NET API 的問題，以及常見問題的因應措施。
ms.assetid: 28A8B061-5AE8-4CDA-B4AB-2EF0151E57C1
ms.date: 02/21/2018
ms.topic: article
keywords: Windows 10, uwp, 遊戲, .net, unity
ms.localizationpriority: medium
ms.openlocfilehash: b687f3ec09a99ae6ccb81e5c205eb454e0af0e04
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860115"
---
# <a name="missing-net-apis-in-unity-and-uwp"></a>Unity 和 UWP 中遺失 .NET API

使用 .NET 建置 UWP 遊戲 時，您可能會發現一些您可能在 Unity 編輯器或獨立電腦遊戲中使用的 API，未出現在 UWP 上。 這是因為 UWP apps 適用的 .NET 包括每個命名空間的完整 .NET Framework 中所提供的類型子集。

此外，部分遊戲引擎使用未與 UWP 適用的 .NET (例如 Unity 的 Mono) 完全相容的不同類型 .NET。 所以當您撰寫遊戲時，所有項目在編輯器中可能運作正常，但是當您移至 UWP 的建置，您可能會收到這類錯誤：**命名空間 'System.Runtime.Serialization' 中沒有類型或命名空間 'Formatters' (是否遺漏了組件參考？)**

幸好 Unity 提供一些這些遺失的 API 作為延伸方法和更換類型，這在[通用 Windows 平台︰.NET 指令碼後端遺失 .NET 類型](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html)中有所描述。 但是，如果您需要的功能不在此處，則 [.net for Windows 8. x 應用程式的總覽](/previous-versions/windows/apps/br230302(v=vs.140)) 將討論您可以如何將程式碼轉換為使用 WinRT 或 .net 來 Windows 執行階段 api。 (這討論 Windows 8，但也適用於 Windows 10 UWP app)。

## <a name="net-standard"></a>.NET Standard

若要了解為何一些 API 可能無法運作，請務必了解不同的 .NET 類型，以及 UWP 如何實作 .NET。 [.NET Standard](/dotnet/standard/net-standard) 是 .NET API 的正式規格，可跨平台和統一不同的 .NET 類型。 每個實作的 .NET 支援特定版本的 .NET Standard。 您可以在 [.NET 實作支援](/dotnet/standard/net-standard#net-implementation-support)看到標準和實作的表格。

每個版本的 UWP SDK 符合不同層級的 .NET Standard。 例如，16299 SDK (Fall Creators Update) .NET Standard 2.0。

如果您想要知道特定 .NET API 在您的目標 UWP 版本中是否受支援，您可以檢查 [.NET Standard API 參考](/dotnet/api/index?view=netstandard-2.0&preserve-view=true)，然後選取該版本的 UWP 支援的 .NET Standard 版本。

## <a name="scripting-backend-configuration"></a>指令碼後端設定

如果您建置 UWP 時遇到問題，第一件事您應該做的是檢查 **\[播放程式設定\]**(**\[檔案\] > \[組建設定\]**，選取 **\[通用 Windows 平台\]**，然後 **\[播放程式設定\]**)。 在 **\[其他設定\] > \[設定\]** 下，前三個下拉選項 (**\[指令碼執行階段版本\]**、**\[指令碼後端\]** 和 **\[Api 相容性層級\]**) 是所有要考慮的重要設定。

**\[指令碼執行階段版本\]** 是 Unity 指令碼後端用來讓您取得您所選擇大致上對等版本的 .NET Framework 支援。 不過，請記住，並非該版本的 .NET framework 的所有 API 都支援，只有您的 UWP 目標那些 .NET Standard 版本。

通常若推出 .NET 新版本，會有更多的 API 新增至 .NET Standard，讓您可以跨獨立和 UWP 使用相同的程式碼。 例如，[System.Runtime.Serialization.Json](/dotnet/api/system.runtime.serialization.json) 命名空間已導入 .NET Standard 2.0。 如果您設定 **\[指令碼執行階段版本\]** 為 **\[.NET 3.5 同等\]**(其目標是較舊版本的 .NET Standard)，嘗試使用 API 時，您將會收到錯誤，請將其切換為 **\[.NET 4.6 同等\]**(支援 .NET Standard 2.0)，API 將可運作。

**\[指令碼後端\]** 可以是 **\[.NET\]** 或 **\[IL2CPP\]**。 本主題中，我們假設您已選擇 **\[.NET\]**，因為那是此處所討論的問題所在。 如需詳細資訊，請參閱[指令碼後端](https://docs.unity3d.com/Manual/windowsstore-scriptingbackends.html)。

最後，您應該設定 **\[Api 相容性層級\]** 為您要讓您的遊戲在上面執行的 .NET 版本。 這應該符合 **\[指令碼執行階段版本\]**。

一般而言，對於 **\[指令碼執行階段版本\]** 和 **\[Api 相容性層級\]**，您應該選取已推出的最新版本，以便與 .NET Framework 有更多相容性，如此可讓您使用更多的 .NET API。

![設定︰指令碼執行階段版本；指令碼後端；Api 相容性層級](images/missing-dot-net-apis-in-unity-1.png)

## <a name="platform-dependent-compilation"></a>平台相關編譯

如果您為多個平台 (包括 UWP) 建置 Unity 遊戲，您會想要使用平台相關編譯，以確保 UWP 用的程式碼只在遊戲建置為 UWP 時執行。 如此一來，您可以對獨立的桌面和其他平台使用完整的 .NET Framework 和 UWP 適用的 WinRT API，而不會收到建置錯誤。

使用下列指示詞僅在執行為 UWP app 時編譯程式碼：

```csharp
#if NETFX_CORE
    // Your UWP code here
#else
    // Your standard code here
#endif
```

> [!NOTE]
> `NETFX_CORE` 只是要檢查您是否要針對 .NET 腳本後端編譯 c # 程式碼。 如果您要使用不同的腳本後端（例如 IL2CPP），請 [`ENABLE_WINMD_SUPPORT`](https://docs.unity3d.com/Manual/windowsstore-code-snippets.html) 改用。

如需平台相關編譯指示詞的完整清單，請參閱[平台相關編譯](https://docs.unity3d.com/Manual/PlatformDependentCompilation.html)。

## <a name="common-issues-and-workarounds"></a>常見問題和因應措施

下列案例描述 UWP 子集遺失 .NET API 時可能發生的常見問題及其因應措施。

### <a name="data-serialization-using-binaryformatter"></a>使用 BinaryFormatter 資料序列化

遊戲通常會將儲存資料序列化，使得玩家無法輕易操縱它。 不過，[BinaryFormatter](/dotnet/api/system.runtime.serialization.formatters.binary.binaryformatter) 會將物件序列化為二進位，並不適用於較舊版本的 .NET Standard (2.0 之前)。 請考慮改為使用 [XmlSerializer](/dotnet/api/system.xml.serialization.xmlserializer) 或 [DataContractJsonSerializer](/dotnet/api/system.runtime.serialization.json.datacontractjsonserializer)。

```csharp
private void Save()
{
    SaveData data = new SaveData(); // User-defined object to serialize

    DataContractJsonSerializer serializer = 
      new DataContractJsonSerializer(typeof(SaveData));

    FileStream stream = 
      new FileStream(Application.persistentDataPath, FileMode.CreateNew);

    serializer.WriteObject(stream, data);
    stream.Dispose();
}
```

### <a name="io-operations"></a>I/O 作業

[System.IO](/dotnet/api/system.io) 命名空間中的某些類型，例如 [FileStream](/dotnet/api/system.io.filestream)，不適用於較舊版本的 .NET Standard。 不過，Unity 提供 [Directory](/dotnet/api/system.io.directory)、[File](/dotnet/api/system.io.file) 和 **FileStream** 類型，您可以在您的遊戲中使用它們。

或者，您可以使用 [Windows.Storage](/uwp/api/Windows.Storage) API，這僅適用於 UWP app。 不過，這些 API 會限制 App 寫入其特定的儲存空間，並且不提供免費存取整個檔案系統的權限。 如需詳細資訊，請參閱[檔案、資料夾和媒體櫃](../files/index.md)。

一個重要事項是 [Close](/dotnet/api/system.io.stream.close) 方法只有在 .NET Standard 2.0 及更新版本中可用 (雖然 Unity 提供延伸方法)。 改為使用 [Dispose](/dotnet/api/system.io.stream.dispose)。

### <a name="threading"></a>執行緒

[System.Threading](/dotnet/api/system.threading) 命名空間中的某些類型，例如 [ThreadPool](/dotnet/api/system.threading.threadpool)，不適用於較舊版本的 .NET Standard。 在這些情況中，您可以改為使用 [Windows.System.Threading](/uwp/api/windows.system.threading) 命名空間。

以下是如何可處理 Unity 遊戲中的執行緒，使用平台相關編譯來為 UWP 和非 UWP 平台 做準備︰

```csharp
private void UsingThreads()
{
#if NETFX_CORE
    Windows.System.Threading.ThreadPool.RunAsync(workItem => SomeMethod());
#else
    System.Threading.ThreadPool.QueueUserWorkItem(workItem => SomeMethod());
#endif
}
```

### <a name="security"></a>安全性

部分 **系統安全性。** _ 當您建立 UWP 的 Unity 遊戲時，無法使用 [system.security.cryptography.x509certificates.x509certificate2](/dotnet/api/system.security.cryptography.x509certificates?view=netstandard-2.0&preserve-view=true)命名空間（例如，）。 在這些情況下，請使用 _*Windows 安全性。* *_ Api，其中涵蓋許多相同的功能。

下列範例只會取得憑證存放區中具指定名稱的憑證：

```cs
private async void GetCertificatesAsync(string certStoreName)
    {
#if NETFX_CORE
        IReadOnlyList<Certificate> certs = await CertificateStores.FindAllAsync();
        IEnumerable<Certificate> myCerts = 
            certs.Where((certificate) => certificate.StoreName == certStoreName);
#else
        X509Store store = new X509Store(certStoreName, StoreLocation.CurrentUser);
        store.Open(OpenFlags.OpenExistingOnly);
        X509Certificate2Collection certs = store.Certificates;
#endif
    }
```

如需使用 WinRT 安全性 API 的詳細資訊，請參閱 [安全性](../security/index.md)。

### <a name="networking"></a>網路功能

當您建立 UWP 的 Unity 遊戲時，也無法使用某些 _* *system &period; net.tcp*_ 命名空間，例如 [system .net. Mail](/dotnet/api/system.net.mail?view=netstandard-2.0&preserve-view=true)。 針對大部分的 api，請使用對應的 _*Windows.* *_ 和 _*windows* *_ . 網路。WinRT Api 來取得類似的功能。 如需詳細資訊，請參閱[網路和 web 服務](../networking/index.md)。

如果是 _ * System .Net. Mail * *，請使用 [ApplicationModel](/uwp/api/windows.applicationmodel.email) 命名空間。 如需詳細資訊，請參閱[傳送電子郵件](../contacts-and-calendar/sending-email.md)。

## <a name="see-also"></a>請參閱

* [通用 Windows 平台︰.NET 指令碼後端遺失 .NET 類型](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html)
* [用於 UWP App 的 .NET 概觀](/previous-versions/windows/apps/br230302(v=vs.140))
* [Unity UWP 移植指南](https://unity3d.com/partners/microsoft/porting-guides)
