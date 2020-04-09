---
title: 共用命名物件
description: 本主題說明如何在通用 Windows 平臺（UWP）應用程式與 Win32 應用程式之間共用已命名的物件。
ms.date: 04/06/2020
ms.topic: article
keywords: windows 10, uwp
ms.openlocfilehash: 95bbecd85b1dfa6f6e12766c082f3338de549677
ms.sourcegitcommit: 2d375e1c34473158134475af401532cc55fc50f4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80888570"
---
# <a name="sharing-named-objects"></a>共用命名物件

本主題說明如何在通用 Windows 平臺（UWP）應用程式與 Win32 應用程式之間共用已命名的物件。

## <a name="named-objects-in-packaged-applications"></a>封裝應用程式中的命名物件

[命名物件](/windows/win32/sync/object-names)提供簡單的方法，讓處理常式共用物件控制碼。 在進程建立了命名物件之後，其他進程就可以使用此名稱來呼叫適當的函式，以開啟物件的控制碼。 命名物件通常用於[執行緒同步](/windows/win32/sync/interprocess-synchronization)處理和[處理序間通訊](/windows/uwp/communication/interprocess-communication)。

根據預設，封裝的應用程式只能存取其所建立的已命名物件。 若要與已封裝的應用程式共用已命名的物件，必須在建立物件時設定許可權，而且在開啟物件時必須限定名稱。

## <a name="creating-named-objects"></a>建立命名物件

使用對應的 `Create` API 建立命名物件：

* [CreateEvent](/windows/win32/api/synchapi/nf-synchapi-createeventexw)
* [CreateFileMapping](/windows/win32/api/memoryapi/nf-memoryapi-createfilemappingw)
* [CreateMutex](/windows/win32/api/synchapi/nf-synchapi-createmutexexw)
* [CreateSemaphore](/windows/win32/api/synchapi/nf-synchapi-createsemaphoreexw)
* [CreateWaitableTimer](/windows/win32/api/synchapi/nf-synchapi-createwaitabletimerexw)

所有這些 Api 共用一個 `LPSECURITY_ATTRIBUTES` 參數，可讓呼叫者指定[存取控制清單（acl）](/previous-versions/windows/desktop/legacy/aa379560(v=vs.85)) ，以控制哪些處理常式可以存取物件。 為了與已封裝的應用程式共用已命名的物件，建立命名物件時，必須在 Acl 中授與許可權。

安全識別碼（Sid）代表 Acl 內的身分識別。 每個封裝的應用程式都有自己的 SID （根據其套件系列名稱）。 您可以將套件系列名稱傳遞至[DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)，以產生已封裝應用程式的 SID。

> [!NOTE]
> 套件系列名稱可以透過在開發期間 Visual Studio 中的套件資訊清單編輯器來尋找，或透過[合作夥伴中心](/windows/uwp/publish/view-app-identity-details)，針對已安裝的應用程式透過 Microsoft Store 或 add-appxpackage PowerShell 命令[取得](/powershell/module/appx/get-appxpackage?view=win10-ps)。

[這個範例](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath#examples)會示範 ACL 命名物件所需的基本模式。 若要與已封裝的應用程式共用已命名的物件，請為每個應用程式建立一個[EXPLICIT_ACCESS](/windows/win32/api/accctrl/ns-accctrl-explicit_access_w)結構：

* `grfAccessMode = GRANT_ACCESS`
* 根據物件和預期的使用方式 `grfAccessPermissions =` 適當的許可權
    * [一般存取權限](/windows/win32/secauthz/generic-access-rights)
    * [同步處理物件安全性和存取權限](/windows/win32/sync/synchronization-object-security-and-access-rights)
    * [檔案對應安全性和存取權限](/windows/win32/memory/file-mapping-security-and-access-rights)
* `grfInheritance = NO_INHERITANCE`
* `Trustee.TrusteeForm = TRUSTEE_IS_SID`
* `Trustee.TrusteeType = TRUSTEE_IS_USER`
* `Trustee.ptstrName =` 從[DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)取得的 SID

藉由使用封裝應用程式的 `EXPLICIT_ACCESS` 規則填入 `Create` 呼叫中的 `LPSECURITY_ATTRIBUTES` 參數，您可以授與這些應用程式的存取權，以開啟已命名的物件。

> [!NOTE]
> Win32 應用程式可以存取封裝的應用程式所建立的所有已命名物件，只要它們在[開啟](#opening-named-objects)時就會限定物件名稱。 它們不需要被授與存取權。

## <a name="opening-named-objects"></a>開啟命名物件

藉由將名稱傳遞給對應的 `Open` API，即可開啟命名物件：

* [OpenEvent](/windows/win32/api/synchapi/nf-synchapi-openeventw)
* [OpenFileMapping](/windows/win32/api/memoryapi/nf-memoryapi-openfilemappingw)
* [OpenMutex](/windows/win32/api/synchapi/nf-synchapi-openmutexw)
* [OpenSemaphore](/windows/win32/api/synchapi/nf-synchapi-opensemaphorew)
* [OpenWaitableTimer](/windows/win32/api/synchapi/nf-synchapi-openwaitabletimerw)

封裝應用程式所建立的命名物件是在應用程式的命名空間內建立的，也稱為「命名物件路徑」。 開啟封裝應用程式所建立的命名物件時，物件名稱前面必須加上建立應用程式的命名物件路徑。

[GetAppContainerNamedObjectPath](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath)會根據其 SID 傳回已封裝應用程式的已命名物件路徑。 您可以將套件系列名稱傳遞至[DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)，以產生已封裝應用程式的 SID。

> [!NOTE]
> 套件系列名稱可以透過在開發期間 Visual Studio 中的套件資訊清單編輯器來尋找，或透過[合作夥伴中心](/windows/uwp/publish/view-app-identity-details)，針對已安裝的應用程式透過 Microsoft Store 或 add-appxpackage PowerShell 命令[取得](/powershell/module/appx/get-appxpackage?view=win10-ps)。

開啟封裝應用程式所建立的命名物件時，請使用 `<PATH>\<NAME>`的格式：

* 將 `<PATH>` 取代為建立應用程式的命名物件路徑。
* 以物件名稱取代 `<NAME>`。

> [!NOTE]
> 只有在已封裝的應用程式建立物件時，才需要在物件名稱前面加上 `<PATH>`。 雖然在[建立](#creating-named-objects)物件時仍然必須授與存取權，但 Win32 應用程式所建立的命名物件不需要限定。

## <a name="remarks"></a>備註

封裝應用程式中的命名物件預設會進行隔離，以保留安全性並確保應用程式生命週期事件的支援，例如暫停和終止。 跨應用程式共用命名物件會引進緊密的系結和版本控制條件約束，而且需要每個應用程式都能在其他人的生命週期中復原。 基於這些理由，建議您只在相同發行者的應用程式之間共用已命名的物件。
