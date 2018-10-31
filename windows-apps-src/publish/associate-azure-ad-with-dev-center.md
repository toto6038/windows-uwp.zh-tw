---
author: jnHs
Description: In order to add and manage account users, you must first associate your Partner Center account with your organization's Azure Active Directory.
title: 將 Azure Active Directory 與您的合作夥伴中心帳戶產生關聯
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad, azure 租用戶, 新增租用戶, azure ad 租用戶, 租用戶管理, 租用戶
ms.localizationpriority: medium
ms.openlocfilehash: a76021f53417d30b91db282a194f6dc6ca268c1f
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5835474"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>將 Azure Active Directory 與您的合作夥伴中心帳戶產生關聯

為了要[新增和管理帳戶使用者](add-users-groups-and-azure-ad-applications.md)，您必須先產生您的合作夥伴中心帳戶關聯與您組織的 Azure Active Directory。 

[合作夥伴中心](https://partner.microsoft.com/dashboard)會運用針對多使用者帳戶存取和管理 Azure AD。 如果您的組織已經使用 Office 365 或其他 Microsoft 所提供的商務服務，您就已經具備 Azure AD。 否則，您可以建立新的 Azure AD 租用戶從合作夥伴中心內免費。

> [!TIP]
> 本主題專屬於 Windows 應用程式開發人員計畫，在[合作夥伴中心](https://partner.microsoft.com/dashboard)，但關聯租用戶和管理使用者的運作方式類似 Windows 傳統型應用程式中的帳戶 （請參閱[Windows 傳統型應用程式](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users)詳細資訊） 和 Windows 硬體開發人員計畫 （其中參考**管理員**角色時也適用於具有**系統管理員**角色的硬體帳戶; 如需詳細資訊，請參閱[儀表板管理](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration)） 中。

單一 Azure AD 租用戶可以與多個合作夥伴中心帳戶產生關聯。 您只需要一個即可新增多個帳戶使用者，與您的合作夥伴中心帳戶相關聯的 Azure AD 租用戶，但您也可以新增多個 Azure AD 租用戶至單一的合作夥伴中心帳戶的選項。 **管理員**角色，合作夥伴中心帳戶中具有任何使用者將可以選擇加入和 Azure AD 租用戶移除帳戶。

> [!IMPORTANT]
> 您的合作夥伴中心帳戶關聯至您的 Azure AD 租用戶之後，以便新增和管理帳戶使用者在該租用戶，您將需要相同的租用戶中具有**管理員**角色的使用者身分登入合作夥伴中心。


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>將您的合作夥伴中心帳戶與組織的現有的 Azure AD 租用戶產生關聯

如果您的組織已經使用 Azure AD，請依照下列步驟連結您的合作夥伴中心帳戶。

1.  從[合作夥伴中心](https://partner.microsoft.com/dashboard)，選取齒輪圖示 （靠近儀表板右上角），然後選取 [**開發人員設定**。 在 [**設定**] 功能表中，選取**租用戶**。
2.  選取**將 Azure AD 與您的合作夥伴中心帳戶建立關聯**。
3.  輸入您想要建立關聯之租用戶的 Azure AD 認證。
4.  檢閱 Azure AD 租用戶的組織和網域名稱。 若要完成關聯，請選取 **\[確認\]**。
5.  如果關聯成功，您接著就可以開始新增和管理帳戶使用者，在合作夥伴中心的**使用者**] 區段中。

> [!IMPORTANT]
> 若要建立新使用者，或對 Azure AD 進行其他變更，您將需要使用擁有該租用戶之[全域系統管理員權限](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)的帳戶來登入該 Azure AD。 不過，您不需要全域系統管理員權限即可關聯租用戶，或將使用者已存在於該租用戶中新增到您的合作夥伴中心帳戶。


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>建立全新的 Azure AD 與您的合作夥伴中心帳戶產生關聯

如果您需要將新的 Azure AD 設定為與您的合作夥伴中心帳戶連結，請依照下列步驟。

1.  從[合作夥伴中心](https://partner.microsoft.com/dashboard)中，選取齒輪圖示 （靠近儀表板右上角），然後選取 [**開發人員設定**。 在 [**設定**] 功能表中，選取**租用戶**。
2.  選取 **\[建立新的 Azure AD\]**。
3.  輸入新 Azure AD 的目錄資訊︰
    - **網域名稱**︰我們會連同 “.onmicrosoft.com” 針對您的 Azure AD 網域使用的唯一名稱。 例如，如果您輸入 “example”，您的 Azure AD 網域就是 “example.onmicrosoft.com”。
    - **連絡人電子郵件**：我們可以在需要時針對帳戶相關資訊與您連絡的電子郵件地址。
    - **全域管理員使用者帳戶資訊**：您想要針對新的全域管理員帳戶使用的名字、姓氏、使用者名稱及密碼。
4.  按一下 **\[建立\]** 來確認新網域和帳戶資訊。
5.  使用您新的 Azure AD 全域管理員使用者名稱與密碼來登入，開始[新增和管理其他的帳戶使用者](add-users-groups-and-azure-ad-applications.md)。


## <a name="manage-azure-ad-tenant-associations"></a>管理 Azure AD 租用戶關聯

您有與您的合作夥伴中心帳戶相關聯的 Azure AD 租用戶之後，您可以新增新的租用戶，或從**租用戶**頁面 」 移除現有的租用戶。


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>將多個 Azure AD 租用戶新增至您的合作夥伴中心帳戶

有**管理員**角色，合作夥伴中心帳戶的任何使用者都可以關聯至 Azure AD 租用戶帳戶。

若要關聯新的租用戶，請選取 **\[關聯另一個 Azure AD 租用戶\]**，然後依照上述步驟進行。 請注意，系統會提示您輸入您想要關聯之 Azure AD 租用戶的認證。


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>從您的合作夥伴中心帳戶移除 Azure AD 租用戶

有**管理員**角色，合作夥伴中心帳戶的任何使用者都可以從帳戶移除 Azure AD 租用戶。

> [!IMPORTANT]
> 當您移除租用戶時，從該租用戶新增至合作夥伴中心帳戶的所有使用者將不會再無法登入帳戶。 

若要移除租用戶，**租用戶**頁面上尋找其名稱 （在 [**帳戶設定**），然後選取 [**移除**]。 系統會提示您確認您要移除租用戶。 一旦您這樣做時，該租用戶中的任何使用者無法登入合作夥伴中心帳戶，並將會移除任何您已設定為這些使用者的權限。

> [!TIP]
> 如果您目前登入合作夥伴中心使用相同的租用戶的帳戶，您無法移除租用戶。 若要移除租用戶，您必須登入合作夥伴中心**管理員**帳戶相關聯的另一個租用戶。 如果只有一個租用戶關聯至帳戶，則必須以開啟該帳戶的 Microsoft 帳戶登入後，才能移除該租用戶。


