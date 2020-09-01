---
title: 共用命名物件
description: 本主題說明如何在通用 Windows 平臺 (UWP) 應用程式和 Win32 應用程式之間共用命名物件。
ms.date: 04/06/2020
ms.topic: article
keywords: windows 10, uwp
ms.openlocfilehash: 38d08e71c44945a7b22f124d15507c7889f8589d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175612"
---
# <a name="sharing-named-objects"></a>共用命名物件

本主題說明如何在通用 Windows 平臺 (UWP) 應用程式和 Win32 應用程式之間共用命名物件。

## <a name="named-objects-in-packaged-applications"></a>封裝應用程式中的命名物件

[命名物件](/windows/win32/sync/object-names) 提供簡單的方法，讓處理常式共用物件控制碼。 在進程建立了命名物件之後，其他進程可以使用此名稱來呼叫適當的函式，以開啟物件的控制碼。 命名物件通常用於 [執行緒同步](/windows/win32/sync/interprocess-synchronization) 處理和 [進程間的通訊](./interprocess-communication.md)。

根據預設，封裝的應用程式只能存取其所建立的命名物件。 為了與封裝的應用程式共用命名物件，必須在建立物件時設定許可權，而且在開啟物件時必須限定名稱。

## <a name="creating-named-objects"></a>建立命名物件

使用對應的 API 建立命名物件 `Create` ：

* [CreateEvent](/windows/win32/api/synchapi/nf-synchapi-createeventexw)
* [CreateFileMapping](/windows/win32/api/memoryapi/nf-memoryapi-createfilemappingw)
* [CreateMutex](/windows/win32/api/synchapi/nf-synchapi-createmutexexw)
* [CreateSemaphore](/windows/win32/api/synchapi/nf-synchapi-createsemaphoreexw)
* [CreateWaitableTimer](/windows/win32/api/synchapi/nf-synchapi-createwaitabletimerexw)

這些 Api 全都共用一個參數，可 `LPSECURITY_ATTRIBUTES` 讓呼叫者指定 [ (acl) 的存取控制清單 ](/previous-versions/windows/desktop/legacy/aa379560(v=vs.85)) ，以控制哪些進程可以存取該物件。 為了與封裝的應用程式共用命名物件，在建立命名物件時，必須在 Acl 中授與許可權。

 (Sid) 的安全識別碼代表 Acl 中的身分識別。 每個封裝的應用程式都有自己的 SID （以其套件系列名稱為基礎）。 您可以藉由將套件系列名稱傳遞給 [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)，來產生已封裝應用程式的 SID。

> [!NOTE]
> 套件系列名稱可在開發期間透過 Visual Studio 中的套件資訊清單編輯器，透過 [合作夥伴中心](../publish/view-app-identity-details.md) 透過 Microsoft Store 發行的應用程式，或透過已安裝之應用程式的 [add-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell 命令來找到。

[這個範例](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath#examples) 會示範 ACL 命名物件所需的基本模式。 若要與封裝的應用程式共用命名物件，請為每個應用程式建立一個 [EXPLICIT_ACCESS](/windows/win32/api/accctrl/ns-accctrl-explicit_access_w) 結構：

* `grfAccessMode = GRANT_ACCESS`
* `grfAccessPermissions =` 根據物件和預定使用方式的適當許可權
    * [一般存取權限](/windows/win32/secauthz/generic-access-rights)
    * [同步處理物件安全性和存取權限](/windows/win32/sync/synchronization-object-security-and-access-rights)
    * [檔案對應安全性和存取權限](/windows/win32/memory/file-mapping-security-and-access-rights)
* `grfInheritance = NO_INHERITANCE`
* `Trustee.TrusteeForm = TRUSTEE_IS_SID`
* `Trustee.TrusteeType = TRUSTEE_IS_USER`
* `Trustee.ptstrName =`從[DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)取得的 SID

藉由 `LPSECURITY_ATTRIBUTES` `Create` 使用已封裝應用程式的規則來填入呼叫中的參數 `EXPLICIT_ACCESS` ，您可以將存取權授與這些應用程式，以開啟命名物件。

> [!NOTE]
> Win32 應用程式可以存取已封裝應用程式所建立的所有命名物件，只要它們在 [開啟](#opening-named-objects)時限定物件名稱即可。 它們不需要被授與存取權。

## <a name="opening-named-objects"></a>開啟命名物件

命名物件的開啟方式是將名稱傳遞給對應的 `Open` API：

* [OpenEvent](/windows/win32/api/synchapi/nf-synchapi-openeventw)
* [OpenFileMapping](/windows/win32/api/memoryapi/nf-memoryapi-openfilemappingw)
* [OpenMutex](/windows/win32/api/synchapi/nf-synchapi-openmutexw)
* [OpenSemaphore](/windows/win32/api/synchapi/nf-synchapi-opensemaphorew)
* [OpenWaitableTimer](/windows/win32/api/synchapi/nf-synchapi-openwaitabletimerw)

已封裝應用程式所建立的命名物件會建立在應用程式的命名空間內，也就是所謂的命名物件路徑。 開啟已封裝應用程式所建立的命名物件時，物件名稱前面必須加上建立應用程式的命名物件路徑。

[GetAppContainerNamedObjectPath](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath) 會根據其 SID 傳回已封裝應用程式的命名物件路徑。 您可以藉由將套件系列名稱傳遞給 [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)，來產生已封裝應用程式的 SID。

> [!NOTE]
> 套件系列名稱可在開發期間透過 Visual Studio 中的套件資訊清單編輯器，透過 [合作夥伴中心](../publish/view-app-identity-details.md) 透過 Microsoft Store 發行的應用程式，或透過已安裝之應用程式的 [add-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell 命令來找到。

開啟已封裝應用程式所建立的命名物件時，請使用 `<PATH>\<NAME>` 下列格式：

* 取代為 `<PATH>` 建立應用程式的命名物件路徑。
* 取代 `<NAME>` 為物件名稱。

> [!NOTE]
> `<PATH>`只有當封裝的應用程式建立物件時，才需要在物件名稱前面加上。 Win32 應用程式所建立的命名物件不需要限定，不過在 [建立](#creating-named-objects)物件時，仍然必須授與存取權。

## <a name="remarks"></a>備註

封裝應用程式中的命名物件預設會隔離，以保留安全性並確保應用程式生命週期事件（如暫止和終止）的支援。 在應用程式之間共用命名物件會導入緊密系結和版本控制條件約束，並要求每個應用程式都可以在其生命週期中復原。 基於這些理由，建議您只在相同發行者的應用程式之間共用命名物件。