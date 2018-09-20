---
author: jnHs
Description: In order to add and manage account users, you must first associate your Dev Center account with your organization's Azure Active Directory.
title: 將 Azure Active Directory 與您的開發人員中心帳戶產生關聯
ms.author: wdg-dev-content
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, azure ad, azure 租用戶, 新增租用戶, azure ad 租用戶, 租用戶管理, 租用戶
ms.localizationpriority: medium
ms.openlocfilehash: dd729d76705849c981516109da39bbd27c140286
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2018
ms.locfileid: "4088485"
---
# <a name="associate-azure-active-directory-with-your-dev-center-account"></a>將 Azure Active Directory 與您的開發人員中心帳戶產生關聯

若要[新增和管理帳戶使用者](add-users-groups-and-azure-ad-applications.md)，您必須先將開發人員中心帳戶與組織的 Azure Active Directory 產生關聯。 

Windows 開發人員中心會針對多使用者帳戶存取和管理來運用 Azure AD。 如果您的組織已經使用 Office 365 或其他 Microsoft 所提供的商務服務，您就已經具備 Azure AD。 否則，您可以從開發人員中心免費建立新的 Azure AD 租用戶。

> [!TIP]
> 本主題專屬於 Windows 應用程式開發人員計畫，但建立租用戶關聯和管理使用者的方式同樣適用於 Windows 傳統型應用程式計畫 (如需詳細資訊，請參閱 [Windows 傳統型應用程式計畫](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users)) 和 Windows 硬體開發人員計畫 (其中 **\[管理員\]** 角色的參考資料也適用於具有 **\[系統管理員\]** 角色的硬體帳戶；如需詳細資訊，請參閱[儀表板管理](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration)) 中的帳戶。

一個 Azure AD 租用戶可以與多個開發人員中心帳戶產生關聯。 您只需將一個 Azure AD 租用戶關聯至開發人員中心帳戶，即可新增多個帳戶使用者，但您也可以選擇新增多個 Azure AD 租用戶至單一開發人員中心帳戶。 在開發人員中心帳戶中具有**管理員**角色的所有使用者，都能選擇新增和移除 Azure AD 租用戶。

> [!IMPORTANT]
> 將您的開發人員中心帳戶關聯至 Azure AD 租用戶之後，若要新增和管理該租用戶中的帳戶使用者，您需要以相同租用戶中具有**管理員**角色的使用者來登入開發人員中心。


## <a name="associate-your-dev-center-account-with-your-organizations-existing-azure-ad-tenant"></a>將開發人員中心帳戶與組織現有的 Azure AD 租用戶產生關聯

如果您的組織已經使用 Azure AD，請依照下列步驟連結您的開發人員中心帳戶。

1.  從[Windows 開發人員中心儀表板](https://partner.microsoft.com/dashboard)中，選取齒輪圖示 （靠近儀表板右上角），然後選取 [**帳戶設定**。 在 [**設定**] 功能表中，選取**租用戶**。
2.  選取 **\[將 Azure AD 與您的開發人員中心帳戶建立關聯\]**。
3.  輸入您想要建立關聯之租用戶的 Azure AD 認證。
4.  檢閱 Azure AD 租用戶的組織和網域名稱。 若要完成關聯，請選取 **\[確認\]**。
5.  如果關聯成功，則您已經準備好在開發人員中心的 **\[使用者\]** 區段上新增和管理帳戶使用者。

> [!IMPORTANT]
> 若要建立新使用者，或對 Azure AD 進行其他變更，您將需要使用擁有該租用戶之[全域系統管理員權限](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)的帳戶來登入該 Azure AD。 不過，您不需要全域系統管理員權限即可關聯租用戶，或將該租用戶中已存在的使用者新增到您的開發人員中心帳戶。


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account"></a>建立全新的 Azure AD 以便與您的開發人員中心帳戶產生關聯

如果您需要設定新的 Azure AD 以便與您的開發人員中心帳戶連結，請遵循下列步驟。

1.  從[Windows 開發人員中心儀表板](https://partner.microsoft.com/dashboard)中，選取齒輪圖示 （靠近儀表板右上角），然後選取 [**帳戶設定**。 在 [**設定**] 功能表中，選取**租用戶**。
2.  選取 **\[建立新的 Azure AD\]**。
3.  輸入新 Azure AD 的目錄資訊︰
    - **網域名稱**︰我們會連同 “.onmicrosoft.com” 針對您的 Azure AD 網域使用的唯一名稱。 例如，如果您輸入 “example”，您的 Azure AD 網域就是 “example.onmicrosoft.com”。
    - **連絡人電子郵件**：我們可以在需要時針對帳戶相關資訊與您連絡的電子郵件地址。
    - **全域管理員使用者帳戶資訊**：您想要針對新的全域管理員帳戶使用的名字、姓氏、使用者名稱及密碼。
4.  按一下 **\[建立\]** 來確認新網域和帳戶資訊。
5.  使用您新的 Azure AD 全域管理員使用者名稱與密碼來登入，開始[新增和管理其他的帳戶使用者](add-users-groups-and-azure-ad-applications.md)。


## <a name="manage-azure-ad-tenant-associations"></a>管理 Azure AD 租用戶關聯

將 Azure AD 租用戶關聯至您的開發人員中心帳戶之後，您可以新增新的租用戶或從 **\[租用戶\]** 頁面移除現有租用戶。


### <a name="add-multiple-azure-ad-tenants-to-your-dev-center-account"></a>新增多個 Azure AD 租用戶至開發人員中心帳戶

具有於開發人員中心帳戶之 **\[管理員\]** 角色的所有使用者，都能將 Azure AD 租用戶關聯至帳戶。

若要關聯新的租用戶，請選取 **\[關聯另一個 Azure AD 租用戶\]**，然後依照上述步驟進行。 請注意，系統會提示您輸入您想要關聯之 Azure AD 租用戶的認證。


### <a name="remove-an-azure-ad-tenant-from-your-dev-center-account"></a>從開發人員中心帳戶移除 Azure AD 租用戶

具有於開發人員中心帳戶之 **\[管理員\]** 角色的所有使用者，都能從帳戶移除 Azure AD 租用戶。

> [!IMPORTANT]
> 移除租用戶時，從該租用戶新增至開發人員中心帳戶的所有使用者都將無法再登入帳戶。 

若要移除租用戶，**租用戶**頁面上尋找其名稱 （在 [**帳戶設定**），然後選取 [**移除**]。 系統會提示您確認您要移除租用戶。 一旦這樣做，該租用戶中的任何開發人員中心使用者都將無法登入開發人員中心帳戶，並會移除您為這些使用者設定的任何權限。

> [!TIP]
> 如果您目前使用同一個租用戶的帳戶登入開發人員中心，將無法移除該租用戶。 若要移除租用戶，您必須以與該帳戶關聯之另一個租用戶的 **\[管理員\]** 身分登入開發人員中心。 如果只有一個租用戶關聯至帳戶，則必須以開啟該帳戶的 Microsoft 帳戶登入後，才能移除該租用戶。


