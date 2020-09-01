---
title: 處理序之間通訊 (IPC)
description: 本主題說明在通用 Windows 平臺 (UWP) 應用程式和 Win32 應用程式之間執行處理序間通訊 (IPC) 的各種方式。
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp
ms.openlocfilehash: 0aa3c62100ecbb30e136c52cee3a6862cf15bef2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175622"
---
# <a name="interprocess-communication-ipc"></a>處理序之間通訊 (IPC)

本主題說明在通用 Windows 平臺 (UWP) 應用程式和 Win32 應用程式之間執行處理序間通訊 (IPC) 的各種方式。

## <a name="app-services"></a>應用程式服務

應用程式服務可讓應用程式公開服務，以在背景中 ([**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet)) ，來公開接受並傳回基本屬性包的服務。 如果已 [序列化](https://stackoverflow.com/questions/46367985/how-to-make-a-class-that-can-be-added-to-the-windows-foundation-collections-valu)，就可以傳遞豐富的物件。

應用程式服務可以在背景工作中執行，或在前景應用程式內[以進程](../launch-resume/convert-app-service-in-process.md)[的形式執行](../launch-resume/how-to-create-and-consume-an-app-service.md)。

應用程式服務最適合用來共用少量的資料，而不需要近乎即時的延遲時間。

## <a name="com"></a>COM

[COM](/windows/win32/com/component-object-model--com--portal) 是一種分散式物件導向系統，可用來建立可互動和通訊的二進位軟體元件。 如果您是開發人員，您可以使用 COM 來為應用程式建立可重複使用的軟體元件和自動化層。 COM 元件可以在進程中或進程外執行，而且可以透過 [用戶端和伺服器](/windows/win32/com/com-clients-and-servers) 模型進行通訊。 跨進程 COM 伺服器一直都是用來做為 [物件間通訊](/windows/win32/com/inter-object-communication)的一種方法。

具有 [runFullTrust](../packaging/app-capability-declarations.md#restricted-capabilities) 功能的已封裝應用程式可以透過 [套件資訊清單](/uwp/schemas/appxpackage/uapmanifestschema/element-com-extension)，為 IPC 註冊跨進程的 COM 伺服器。 這就是所謂的 [封裝 COM](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge/)。

## <a name="filesystem"></a>Filesystem

### <a name="broadfilesystemaccess"></a>BroadFileSystemAccess

封裝的應用程式可以藉由宣告 [broadFileSystemAccess](../files/file-access-permissions.md#accessing-additional-locations) 限制的功能，使用廣泛的檔案系統來執行 IPC。 這項功能會將 [Windows 的儲存體](/uwp/api/Windows.Storage) Api 和 [xxxFromApp](/previous-versions/windows/desktop/legacy/mt846585(v=vs.85)) Win32 api 存取權授與廣泛的檔案系統。

根據預設，已封裝應用程式的檔案系統的 IPC 會限制為本節所述的其他機制。

### <a name="publishercachefolder"></a>PublisherCacheFolder

[PublisherCacheFolder](/uwp/api/windows.storage.applicationdata.getpublishercachefolder)可讓封裝的應用程式在其資訊清單中宣告可由相同發行者與其他封裝共用的資料夾。

共用儲存體資料夾具有下列需求和限制：

* 共用儲存體資料夾中的資料不會進行備份或漫遊。
* 使用者可以清除共用儲存體資料夾的內容。
* 您無法使用共用儲存體資料夾，在不同發行者的應用程式之間共用資料。
* 您無法使用共用的儲存體資料夾，在不同的使用者之間共用資料。
* 共用儲存體資料夾沒有版本管理。

如果您發行多個應用程式，而且您正在尋找在兩者之間共用資料的簡單機制，則 PublisherCacheFolder 是以檔案系統為基礎的簡單選項。

### <a name="sharedaccessstoragemanager"></a>SharedAccessStorageManager

[SharedAccessStorageManager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) 可與應用程式服務、通訊協定啟用 (例如 LaunchUriForResultsAsync) 等）搭配使用，以透過權杖共用 StorageFiles。

## <a name="fulltrustprocesslauncher"></a>FullTrustProcessLauncher

有了 [runFullTrust](../packaging/app-capability-declarations.md#restricted-capabilities) 功能，封裝的應用程式就可以啟動相同套件內的 [完全信任](/uwp/api/Windows.ApplicationModel.FullTrustProcessLauncher) 程式。

對於套件限制是負擔的案例，或缺少 IPC 選項的情況，應用程式可以使用完全信任的程式做為與系統介面的 proxy，然後透過應用程式服務或其他部分支援的 IPC 機制，透過完全信任程式本身進行 IPC。

## <a name="launchuriforresultsasync"></a>LaunchUriForResultsAsync

[LaunchUriForResultsAsync](../launch-resume/how-to-launch-an-app-for-results.md) 可用於簡單的 ([ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet)) 資料與其他可執行 [ProtocolForResults](../launch-resume/how-to-launch-an-app-for-results.md#step-2-override-applicationonactivated-in-the-app-that-youll-launch-for-results) 啟用合約的封裝應用程式交換。 不同于通常會在背景中執行的應用程式服務，目標應用程式會在前景中啟動。

您可以透過 ValueSet 將 [SharedStorageAccessManager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) token 傳遞至應用程式，藉以共用檔案。

## <a name="loopback"></a>回送

回送是與接聽 localhost (回送位址) 的網路伺服器通訊的程式。

為了維護安全性和網路隔離，已封裝的應用程式預設會封鎖 IPC 的回送連線。 您可以使用 [功能](/previous-versions/windows/apps/hh770532(v=win.10)) 和 [資訊清單屬性](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)，在信任的已封裝應用程式之間啟用回送連接。

* 參與回送連線的所有已封裝應用程式都必須 `privateNetworkClientServer` 在其 [套件資訊清單](/uwp/schemas/appxpackage/uapmanifestschema/element-capability)中宣告該功能。
* 有兩個封裝的應用程式可以透過回送進行通訊，方法是在其套件指令[清單中宣告](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)
    * 每個應用程式都必須在其 [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)中列出另一個應用程式。 用戶端會宣告伺服器的「輸出」規則，而伺服器會為其支援的用戶端宣告「in」規則。

> [!NOTE]
> 在開發期間，您可以透過 Visual Studio 中的套件資訊清單編輯器，在開發期間，透過 [合作夥伴中心](../publish/view-app-identity-details.md) 針對透過 Microsoft Store 發行的應用程式，或透過已安裝之應用程式的 [add-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell 命令，找到在這些規則中識別應用程式所需的套件系列名稱。

未封裝的應用程式和服務沒有套件身分識別，因此無法在 [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)中宣告。 您可以透過 [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10))將封裝的應用程式設定為透過回送與未封裝的應用程式和服務進行連線，不過這只適用于您有機器本機存取權的側載或偵測案例，而且您擁有系統管理員許可權。

* 參與回送連線的所有已封裝應用程式都必須 `privateNetworkClientServer` 在其 [套件資訊清單](/uwp/schemas/appxpackage/uapmanifestschema/element-capability)中宣告該功能。
* 如果封裝的應用程式要連接到未封裝的應用程式或服務，請執行 `CheckNetIsolation.exe LoopbackExempt -a -n=<PACKAGEFAMILYNAME>` 以新增已封裝應用程式的回送豁免。
* 如果未封裝的應用程式或服務正在連接到封裝的應用程式，請執行 `CheckNetIsolation.exe LoopbackExempt -is -n=<PACKAGEFAMILYNAME>` 以啟用封裝的應用程式來接收輸入回送連接。
    * 當封裝的應用程式正在接聽連接時， [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10))必須持續執行。
    * `-is`旗標是在 Windows 10 1607 版中引進的 (10.0;組建 14393) 。

> [!NOTE]
> CheckNetIsolation.exe旗標所需的套件系列名稱 `-n` 可在開發期間透過 Visual Studio 中的套件資訊清單編輯器[合作夥伴中心](../publish/view-app-identity-details.md)、透過 Microsoft Store 發佈的應用程式，或透過已安裝之應用程式的[add-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell 命令來找到。 [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10))

[CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) 也適用于偵測 [網路隔離問題](/previous-versions/windows/apps/hh780593(v=win.10)#debug-network-isolation-issues)。

## <a name="pipes"></a>管道

[管道](/windows/win32/ipc/pipes) 可讓管道伺服器和一或多個管道用戶端之間進行簡易通訊。

[匿名管道](/windows/win32/ipc/anonymous-pipes) 和 [具名管道](/windows/win32/ipc/named-pipes) 支援下列條件約束：

* 根據預設，封裝應用程式中的具名管道只在相同套件內的處理常式之間受到支援，除非進程是完全信任的。
* 具名管道可以依照 [共用命名物件](./sharing-named-objects.md)的指導方針，在套件間共用。
* 封裝應用程式中的具名管道必須使用 `\\.\pipe\LOCAL\` 管道名稱的語法。

## <a name="registry"></a>登錄

通常不[建議使用 IPC](/windows/win32/sysinfo/registry-functions)的登錄使用方式，但現有的程式碼支援此功能。 封裝的應用程式只能存取他們有權存取的登錄機碼。

[以 MSIX 封裝的桌面應用程式](/windows/msix/desktop/desktop-to-uwp-root) 會利用登錄 [虛擬化](/windows/msix/desktop/desktop-to-uwp-behind-the-scenes#registry) ，因此全域登錄寫入會包含在 MSIX 封裝內的私用 hive 中。 這可啟用原始程式碼的相容性，同時將全域登錄影響降到最低，並可用於相同套件中的進程之間的 IPC。 如果您必須使用登錄，則建議使用此模型，而不是操作全域登錄。

## <a name="rpc"></a>RPC

您可以使用[rpc](/windows/win32/rpc/rpc-start-page)將封裝的應用程式連接到 Win32 RPC 端點，但前提是封裝的應用程式具有可符合 RPC 端點上 acl 的正確功能。

自訂功能可讓 Oem 和 Ihv [定義任意功能](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#reserving-a-custom-capability)、 [使用它們來 ACL 其 RPC 端點](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#allowing-access-to-an-rpc-endpoint-to-a-uwp-app-using-the-custom-capability)，然後將 [這些功能授與授權的用戶端應用程式](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file)。 如需完整的範例應用程式，請參閱 [CustomCapability](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomCapability) 範例。

RPC 端點也可以碰到至特定封裝的應用程式，以將端點的存取限制為僅限這些應用程式，而不需要管理自訂功能的額外負荷。 您可以使用 [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername) API 從套件系列名稱衍生 sid，然後使用 [CustomCapability](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CustomCapability/Service/Server/RpcServer.cpp) 範例所示的 sid 來 ACL RPC 端點。

## <a name="shared-memory"></a>共用記憶體

檔案[對應](/windows/win32/memory/sharing-files-and-memory)可以用來在兩個或多個進程之間共用檔案或記憶體，但有下列限制：

* 依預設，只有在相同套件內的程式之間才支援封裝應用程式中的檔案對應，除非進程是完全信任的。
* 您可以遵循 [共用命名物件](./sharing-named-objects.md)的指導方針，在套件間共用檔案對應。

建議使用共用記憶體來有效率地共用和操作大量的資料。