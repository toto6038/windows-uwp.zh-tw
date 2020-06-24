---
title: 處理序之間通訊 (IPC)
description: 本主題說明在通用 Windows 平臺（UWP）應用程式與 Win32 應用程式之間執行處理序間通訊（IPC）的各種方式。
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp
ms.openlocfilehash: 5db029db3ffb538802f39aa616c96dbe75601eac
ms.sourcegitcommit: bf7d4f6739aeeaac735aae3dd0dcbda63a8c5e69
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85256378"
---
# <a name="interprocess-communication-ipc"></a>處理序之間通訊 (IPC)

本主題說明在通用 Windows 平臺（UWP）應用程式與 Win32 應用程式之間執行處理序間通訊（IPC）的各種方式。

## <a name="app-services"></a>應用程式服務

App service 可讓應用程式在背景中公開接受和傳回基本類型（[**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet)）之屬性包的服務。 如果已[序列化](https://stackoverflow.com/questions/46367985/how-to-make-a-class-that-can-be-added-to-the-windows-foundation-collections-valu)豐富的物件，就可以傳遞它們。

應用程式服務可能會以背景工作[的](/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)形式執行，或在前景應用程式內的[進程中](/windows/uwp/launch-resume/convert-app-service-in-process)執行。

應用程式服務最適合用來共用少量的資料，而不需要近乎即時的延遲時間。

## <a name="com"></a>COM

[COM](/windows/win32/com/component-object-model--com--portal)是分散式物件導向系統，用於建立可以互動和通訊的二進位軟體元件。 身為開發人員，您可以使用 COM 來建立應用程式的可重複使用軟體元件和自動化層級。 COM 元件可能正在處理或進程外，而且可以透過[用戶端和伺服器](/windows/win32/com/com-clients-and-servers)模型進行通訊。 跨進程 COM 伺服器已長期用來做為[物件間通訊](/windows/win32/com/inter-object-communication)的手段。

具有[runFullTrust](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)功能的已封裝應用程式可以透過[套件資訊清單](/uwp/schemas/appxpackage/uapmanifestschema/element-com-extension)，為 IPC 註冊跨進程的 COM 伺服器。 這就是所謂的[封裝 COM](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge/)。

## <a name="filesystem"></a>Filesystem

### <a name="broadfilesystemaccess"></a>BroadFileSystemAccess

封裝的應用程式可以藉由宣告[broadFileSystemAccess](/windows/uwp/files/file-access-permissions#accessing-additional-locations)限制的功能，使用廣泛的檔案系統來執行 IPC。 這項功能會授與[Windows 存放區](/uwp/api/Windows.Storage)api，並[xxxFromApp](/previous-versions/windows/desktop/legacy/mt846585(v=vs.85)) Win32 api 存取廣泛的檔案系統。

根據預設，已封裝應用程式的檔案系統的 IPC 僅限於本節所述的其他機制。

### <a name="publishercachefolder"></a>PublisherCacheFolder

[PublisherCacheFolder](/uwp/api/windows.storage.applicationdata.getpublishercachefolder)可讓封裝的應用程式在其資訊清單中宣告資料夾，讓同一個發行者可以與其他封裝共用。

共用儲存體資料夾具有下列需求和限制：

* 共用儲存體資料夾中的資料不會備份或漫遊。
* 使用者可以清除共用儲存體資料夾的內容。
* 您無法使用共用的儲存體資料夾，在來自不同發行者的應用程式之間共用資料。
* 您無法使用共用的儲存體資料夾，在不同的使用者之間共用資料。
* 共用儲存體資料夾沒有版本管理。

如果您發行多個應用程式，而且您正在尋找一個簡單的機制來共用它們之間的資料，則 PublisherCacheFolder 是以 filesystem 為基礎的簡單選項。

### <a name="sharedaccessstoragemanager"></a>SharedAccessStorageManager

[SharedAccessStorageManager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager)會與應用程式服務、通訊協定啟用（例如 LaunchUriForResultsAsync）等搭配使用，以透過權杖共用 StorageFiles。

## <a name="fulltrustprocesslauncher"></a>FullTrustProcessLauncher

有了[runFullTrust](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)功能，封裝的應用程式就可以在同一個封裝內[啟動完全信任的進程](/uwp/api/Windows.ApplicationModel.FullTrustProcessLauncher)。

對於套件限制為負擔的情況，或缺少 IPC 選項，應用程式可以使用完全信任進程做為 proxy 來與系統進行互動，然後透過應用程式服務或一些其他支援的 IPC 機制，透過完全信任進程本身進行 IPC。

## <a name="launchuriforresultsasync"></a>LaunchUriForResultsAsync

[LaunchUriForResultsAsync](/windows/uwp/launch-resume/how-to-launch-an-app-for-results)是用於簡單（[ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet)）資料交換，與其他可執行[ProtocolForResults](/windows/uwp/launch-resume/how-to-launch-an-app-for-results#step-2-override-applicationonactivated-in-the-app-that-youll-launch-for-results)啟用合約的封裝應用程式。 不同于通常在背景中執行的應用程式服務，目標應用程式會在前景中啟動。

透過 ValueSet 將[SharedStorageAccessManager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) token 傳遞至應用程式，即可共用檔案。

## <a name="loopback"></a>回送

回送處理是與在 localhost （回送位址）上接聽的網路伺服器通訊的程式。

為了維護安全性和網路隔離，已封裝的應用程式預設會封鎖 IPC 的回送連接。 您可以使用[功能](/previous-versions/windows/apps/hh770532(v=win.10))和[資訊清單屬性](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)，在受信任的封裝應用程式之間啟用回送連接。

* 所有參與回送連接的封裝應用程式，都必須 `privateNetworkClientServer` 在其[封裝資訊清單](/uwp/schemas/appxpackage/uapmanifestschema/element-capability)中宣告功能。
* 兩個已封裝的應用程式可以透過回送來進行通訊，方法是在其封裝指令[清單內](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)
    * 每個應用程式都必須在其[LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)中列出另一個。 用戶端會為伺服器宣告「out」規則，而伺服器會針對其支援的用戶端宣告「in」規則。

> [!NOTE]
> 在開發期間，您可以透過 [套件資訊清單編輯器]，Visual Studio 在 add-appxpackage 中找到識別應用程式所需的套件系列名稱，或透過 Microsoft Store 或已安裝之應用程式的[Get-AppxPackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell 命令，從[合作夥伴中心](/windows/uwp/publish/view-app-identity-details)取得。

未封裝的應用程式和服務沒有套件身分識別，因此無法在[LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)中宣告。 您可以設定封裝的應用程式，透過[CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10))透過回送與未封裝的應用程式和服務進行連線，但這只適用于您具有機器本機存取權的側載或偵測案例，而且您具有系統管理員許可權。

* 參與回送連線的所有已封裝應用程式都必須 `privateNetworkClientServer` 在其[封裝資訊清單](/uwp/schemas/appxpackage/uapmanifestschema/element-capability)中宣告功能。
* 如果封裝的應用程式要連接到未封裝的應用程式或服務，請執行 `CheckNetIsolation.exe LoopbackExempt -a -n=<PACKAGEFAMILYNAME>` 來為封裝的應用程式新增回送豁免。
* 如果未封裝的應用程式或服務正在連接到已封裝的應用程式，請執行， `CheckNetIsolation.exe LoopbackExempt -is -n=<PACKAGEFAMILYNAME>` 讓封裝的應用程式能夠接收輸入回送連接。
    * 當封裝的應用程式正在接聽連接時， [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10))必須持續執行。
    * 此 `-is` 旗標是在 Windows 10 版本1607（10.0;）中引進的組建14393）。

> [!NOTE]
> CheckNetIsolation.exe旗標所需的套件系列名稱 `-n` ，可以在開發期間透過 [套件資訊清單編輯器] 中的 Visual Studio 找到，或透過 [[合作夥伴中心](/windows/uwp/publish/view-app-identity-details)]，針對已安裝的應用程式透過 Microsoft Store 或 Add-appxpackage PowerShell 命令[取得](/powershell/module/appx/get-appxpackage?view=win10-ps)。 [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10))

[CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10))也很適合用來進行[網路隔離問題的調試](/previous-versions/windows/apps/hh780593(v=win.10)#debug-network-isolation-issues)程式。

## <a name="pipes"></a>管道

[管道](/windows/win32/ipc/pipes)可讓管道伺服器與一或多個管道用戶端之間進行簡單的通訊。

[匿名管道](/windows/win32/ipc/anonymous-pipes)和[具名管道](/windows/win32/ipc/named-pipes)支援下列條件約束：

* 根據預設，只有在相同封裝內的進程之間才支援已封裝應用程式中的具名管道，除非進程是完全信任的。
* 具名管道可以透過[共用命名物件](/windows/uwp/communication/sharing-named-objects)的指導方針，在封裝之間共用。
* 封裝應用程式中的具名管道必須使用 `\\.\pipe\LOCAL\` 管道名稱的語法。

## <a name="registry"></a>登錄

通常不[建議使用 IPC](/windows/win32/sysinfo/registry-functions)的登錄，但現有程式碼支援它。 封裝的應用程式只能存取他們有權存取的登錄機碼。

[封裝為 MSIX 的桌面應用程式](/windows/msix/desktop/desktop-to-uwp-root)會利用登錄[虛擬化](/windows/msix/desktop/desktop-to-uwp-behind-the-scenes#registry)，因此全域登錄寫入會包含在 MSIX 套件內的私用 hive 中。 這可讓原始程式碼相容，同時將全域登錄的影響降至最低，並可用於相同封裝中的進程之間的 IPC。 如果您必須使用登錄，此模型是慣用的，而不是操作全域登錄。

## <a name="rpc"></a>RPC

[Rpc](/windows/win32/rpc/rpc-start-page)可以用來將已封裝的應用程式連線到 Win32 RPC 端點，但前提是封裝的應用程式具有正確的功能，可符合 RPC 端點上的 acl。

自訂功能可讓 Oem 和 Ihv[定義任意功能](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#reserving-a-custom-capability)、將[其與 RPC 端點進行 ACL](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#allowing-access-to-an-rpc-endpoint-to-a-uwp-app-using-the-custom-capability)，然後將[這些功能授與授權的用戶端應用程式](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file)。 如需完整的範例應用程式，請參閱[CustomCapability](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomCapability)範例。

RPC 端點也可以碰到至特定封裝的應用程式，將端點的存取限制為只有那些應用程式，而不需要自訂功能的管理額外負荷。 您可以使用[DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername) API 從套件系列名稱中衍生 SID，然後以 SID 作為 RPC 端點的 ACL，如[CustomCapability](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CustomCapability/Service/Server/RpcServer.cpp)範例所示。

## <a name="shared-memory"></a>共用記憶體

檔案[對應](/windows/win32/memory/sharing-files-and-memory)可以用來在兩個或多個進程之間共用檔案或記憶體，並具有下列條件約束：

* 根據預設，只有在相同封裝內的進程之間才支援封裝應用程式中的檔案對應，除非程式是完全信任的。
* 您可以遵循[共用命名物件](/windows/uwp/communication/sharing-named-objects)的指導方針，在封裝之間共用檔案對應。

建議使用共用記憶體來有效率地共用及操作大量資料。
