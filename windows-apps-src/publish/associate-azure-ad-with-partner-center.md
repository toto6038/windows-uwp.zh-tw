---
Description: 若要新增及管理帳戶的使用者，您必須先使您的合作夥伴中心帳戶與您組織的 Azure Active Directory。
title: 將 Azure Active Directory 與您的合作夥伴中心帳戶產生關聯
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad, azure 租用戶, 新增租用戶, azure ad 租用戶, 租用戶管理, 租用戶
ms.localizationpriority: medium
ms.openlocfilehash: 5d830fb3a984faecd93519b4e682ade59077d2d6
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2019
ms.locfileid: "63787223"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>將 Azure Active Directory 與您的合作夥伴中心帳戶產生關聯

若要[新增及管理帳戶使用者](add-users-groups-and-azure-ad-applications.md)，您必須先使您的合作夥伴中心帳戶與您組織的 Azure Active Directory。 

[合作夥伴中心](https://partner.microsoft.com/dashboard)運用多使用者帳戶的存取和管理 Azure AD。 如果您的組織已經使用 Office 365 或其他 Microsoft 所提供的商務服務，您就已經具備 Azure AD。 否則，您可以建立新的 Azure AD 租用戶從合作夥伴中心內，不另收費。

> [!TIP]
> 本主題是 Windows 應用程式開發人員計劃中的特定[合作夥伴中心](https://partner.microsoft.com/dashboard)，但將租用戶產生關聯，以及管理使用者的運作方式類似 Windows 桌面應用程式中的帳戶 (請參閱[Windows桌面應用程式](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users)如需詳細資訊) 和 Windows 硬體開發人員計劃中 (其中參考**Manager**角色也適用於硬體帳戶**系統管理員**角色，請參閱[儀表板管理](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration)如需詳細資訊)。

單一 Azure AD 租用戶可以與多個合作夥伴中心帳戶相關聯。 您只需要有 Azure AD 租用戶與您的合作夥伴中心帳戶建立關聯，以便新增多個帳戶的使用者，但您也可以選擇將多個 Azure AD 租用戶新增至單一的合作夥伴中心帳戶。 任何擁有使用者**Manager**合作夥伴中心帳戶中的角色必須新增和移除帳戶的 Azure AD 租用戶的選項。

> [!IMPORTANT]
> 合作夥伴中心帳戶建立關聯的 Azure AD 租用戶之後，若要新增及管理該租用戶中的帳戶使用者您必須在相同的租用戶的人員使用者身分登入合作夥伴中心**Manager**角色。


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>合作夥伴中心帳戶與您組織的現有 Azure AD 租用戶建立關聯

如果您的組織已使用 Azure AD，請遵循下列步驟，將合作夥伴中心帳戶連結。

1.  從[合作夥伴中心](https://partner.microsoft.com/dashboard)，選取齒輪圖示 （附近的儀表板右上角），然後選取**開發人員設定**。 在 **設定**功能表上，選取**租用戶**。
2.  選取 **與您的合作夥伴中心帳戶的 Azure AD 相關聯**。
3.  輸入您想要建立關聯之租用戶的 Azure AD 認證。
4.  檢閱 Azure AD 租用戶的組織和網域名稱。 若要完成關聯，請選取 **\[確認\]**。
5.  如果關聯的不成功，您將則是可以新增和管理帳戶的使用者**使用者**在合作夥伴中心內的一節。

> [!IMPORTANT]
> 若要建立新使用者，或對 Azure AD 進行其他變更，您將需要使用擁有該租用戶之[全域系統管理員權限](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)的帳戶來登入該 Azure AD。 不過，您不需要全域管理員權限，才能將租用戶中，關聯，或該租用戶中的現有使用者加入您的合作夥伴中心帳戶中。


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>建立新的 Azure AD 與您的合作夥伴中心帳戶產生關聯

如果您需要設定新的 Azure AD 時要連結的合作夥伴中心帳戶，請遵循下列步驟。

1.  從[合作夥伴中心](https://partner.microsoft.com/dashboard)，選取齒輪圖示 （附近的儀表板右上角），然後選取**開發人員設定**。 在 **設定**功能表上，選取**租用戶**。
2.  選取 **\[建立新的 Azure AD\]**。
3.  輸入新 Azure AD 的目錄資訊︰
    - **網域名稱**:我們將使用您的 Azure AD 網域，連同的唯一名稱 」。 onmicrosoft.com"。 例如，如果您輸入 “example”，您的 Azure AD 網域就是 “example.onmicrosoft.com”。
    - **連絡人電子郵件**:電子郵件地址，其中我們可以連絡您關於您的帳戶如有必要。
    - **全域管理員使用者帳戶資訊**:第一個名稱，最後一個名稱、 使用者名稱和您想要使用新的全域系統管理員帳戶的密碼。
4.  按一下 [建立] 來確認新網域和帳戶資訊。
5.  使用您新的 Azure AD 全域管理員使用者名稱與密碼來登入，開始[新增和管理其他的帳戶使用者](add-users-groups-and-azure-ad-applications.md)。


## <a name="manage-azure-ad-tenant-associations"></a>管理 Azure AD 租用戶關聯

您有與您的合作夥伴中心帳戶相關聯的 Azure AD 租用戶之後，您可以新增新的租用戶，或移除現有的租用戶，從**租用戶**頁面。


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>將多個 Azure AD 租用戶新增至您的合作夥伴中心帳戶

任何使用者都有**Manager**合作夥伴中心帳戶的角色可以關聯至 Azure AD 租用戶帳戶。

若要關聯新的租用戶，請選取 **\[關聯另一個 Azure AD 租用戶\]**，然後依照上述步驟進行。 請注意，系統會提示您輸入您想要關聯之 Azure AD 租用戶的認證。


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>從合作夥伴中心帳戶中移除 Azure AD 租用戶

任何使用者都有**Manager**合作夥伴中心帳戶的角色可以移除該帳戶的 Azure AD 租用戶。

> [!IMPORTANT]
> 當您移除租用戶時，已新增到合作夥伴中心帳戶，從該租用戶的所有使用者將不再能夠登入帳戶。 

若要移除租用戶，其名稱的上找到**租用戶**網頁 (在**帳戶設定**)，然後選取**移除**。 系統會提示您確認您要移除租用戶。 一旦您這樣做時，該租用戶中的使用者無法登入合作夥伴中心帳戶，並將移除任何您已設定為這些使用者的權限。

> [!TIP]
> 如果您目前登入合作夥伴中心使用相同的租用戶中的帳戶，您無法移除租用戶。 若要移除租用戶，您必須登入合作夥伴中心**Manager**與帳戶相關聯的其他租用戶。 如果只有一個租用戶關聯至帳戶，則必須以開啟該帳戶的 Microsoft 帳戶登入後，才能移除該租用戶。


