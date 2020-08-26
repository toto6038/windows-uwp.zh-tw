---
Description: 若要新增及管理帳戶使用者，您必須先將合作夥伴中心帳戶與組織的 Azure Active Directory 產生關聯。
title: 將 Azure Active Directory 與您的合作夥伴中心帳戶建立關聯
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad, azure 租用戶, 新增租用戶, azure ad 租用戶, 租用戶管理, 租用戶
ms.localizationpriority: medium
ms.openlocfilehash: fa26f4ea3fa91f43dea117ff5ffd2fec87514544
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846758"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>將 Azure Active Directory 與您的合作夥伴中心帳戶建立關聯

若要 [新增及管理帳戶使用者](add-users-groups-and-azure-ad-applications.md)，您必須先將合作夥伴中心帳戶與組織的 Azure Active Directory 產生關聯。 

[合作夥伴中心](https://partner.microsoft.com/dashboard) 利用 Azure AD 進行多使用者帳戶存取和管理。 如果您的組織已使用 Microsoft 365 或 Microsoft 的其他商務服務，您就已經有 Azure AD。 否則，您可以從合作夥伴中心內建立新的 Azure AD 租使用者，而不需額外付費。

> [!TIP]
> 本主題是[合作夥伴中心](https://partner.microsoft.com/dashboard)中的 Windows 應用程式開發人員計畫專屬的，但讓租使用者與管理使用者的運作方式類似于 Windows 傳統型應用程式計畫中的帳戶 (如需詳細資訊，請參閱 Windows 傳統型應用程式計畫。如需詳細資訊，請參閱[Windows Desktop Application Program](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users)) ，在 Windows 硬體開發人員計畫**中，管理員角色的**參考也適用于具有**系統管理員**角色的硬體帳戶;如需詳細資訊，請參閱[儀表板管理](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration)) 。

單一 Azure AD 租使用者可以與多個合作夥伴中心帳戶相關聯。 您只需要有一個 Azure AD 租使用者與您的合作夥伴中心帳戶建立關聯，才能新增多個帳戶使用者，但您也可以選擇將多個 Azure AD 租使用者新增至單一合作夥伴中心帳戶。 在合作夥伴中心帳戶內，所有具有**管理員**角色的使用者，都可以選擇從帳戶新增和移除 Azure AD 租用戶。

> [!IMPORTANT]
> 在您將合作夥伴中心帳戶與 Azure AD 租使用者建立關聯之後，若要新增及管理該租使用者中的帳戶使用者，您必須以擁有 **管理員** 角色的相同租使用者中的使用者身分登入合作夥伴中心。


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>將您的合作夥伴中心帳戶與組織的現有 Azure AD 租使用者建立關聯

如果您的組織已使用 Azure AD，請遵循下列步驟來連結您的合作夥伴中心帳戶。

1.  從 [合作夥伴中心](https://partner.microsoft.com/dashboard)中，選取 [儀表板] 右上角附近的齒輪圖示 () 然後選取 [ **開發人員設定**]。 在 [ **設定** ] 功能表中 **，選取 [** 租使用者]。
2.  選取 [ **將 Azure AD 與您的合作夥伴中心帳戶建立關聯**]。
3.  為您要建立關聯的租用戶輸入 Azure AD 認證。
4.  檢閱您 Azure AD 租用戶的組織和網域名稱。 若要完成關聯，請選取 [確認]。
5.  如果關聯成功，您可以接著在合作夥伴中心的 [使用者] 區段中，新增和管理帳戶使用者。

> [!IMPORTANT]
> 若要建立新使用者，或對 Azure AD 進行其他變更，您將需要使用擁有該租用戶之[全域系統管理員權限](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)的帳戶來登入該 Azure AD。 不過，您不需要全域管理員許可權，就能關聯租使用者，或將該租使用者中已存在的使用者新增至您的合作夥伴中心帳戶。


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>建立與您的合作夥伴中心帳戶相關聯的全新 Azure AD

如果您需要設定新的 Azure AD 來與您的合作夥伴中心帳戶連結，請遵循下列步驟。

1.  從 [合作夥伴中心](https://partner.microsoft.com/dashboard)中，選取 [儀表板] 右上角附近的齒輪圖示 () 然後選取 [ **開發人員設定**]。 在 [ **設定** ] 功能表中 **，選取 [** 租使用者]。
2.  選取 **\[建立新的 Azure AD\]**。
3.  輸入新 Azure AD 的目錄資訊：
    - **功能變數名稱**：我們將用於 Azure AD 網域的唯一名稱，以及 "onmicrosoft.com"。 例如，如果您輸入 “example”，您的 Azure AD 網域就是 “example.onmicrosoft.com”。
    - **連絡人電子郵件**：我們可以在必要時就帳戶相關問題連絡您的電子郵件地址。
    - **全域管理員使用者帳戶資訊**：您想要用於新全域管理員帳戶的名字、姓氏、使用者名稱和密碼。
4.  按一下 [ **建立** ] 以確認新的網域和帳戶資訊。
5.  使用您新的 Azure AD 全域管理員使用者名稱與密碼來登入，開始[新增和管理其他的帳戶使用者](add-users-groups-and-azure-ad-applications.md)。


## <a name="manage-azure-ad-tenant-associations"></a>管理 Azure AD 租用戶關聯

將 Azure AD 租使用者與您的合作夥伴中心帳戶建立關聯之後，您可以新增租 **使用者，或從 [租** 使用者] 頁面移除現有的租使用者。


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>將多個 Azure AD 租使用者新增至您的合作夥伴中心帳戶

任何有合作夥伴中心帳戶 **管理員** 角色的使用者，都可以將 Azure AD 租使用者與帳戶產生關聯。

若要關聯新的租用戶，請選取 **\[關聯另一個 Azure AD 租用戶\]**，然後依照上述步驟進行。 請注意，系統會提示您輸入您想要關聯之 Azure AD 租用戶的認證。


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>從您的合作夥伴中心帳戶移除 Azure AD 租使用者

任何有合作夥伴中心帳戶 **管理員** 角色的使用者，都可以從帳戶中移除 Azure AD 的租使用者。

> [!IMPORTANT]
> 當您移除租用戶時，從該租用戶新增至合作夥伴中心帳戶的所有使用者將無法再登入該帳戶。 

若要移除租使用者，請 **在 [租** 使用者] 頁面上尋找其名稱 (在 [ **帳戶設定** ]) 中，然後選取 [ **移除**]。 系統會提示您確認您要移除租用戶。 一旦您這麼做，該租用戶中的使用者將無法登入合作夥伴中心帳戶，且會移除您為這些使用者設定的任何權限。

> [!TIP]
> 如果您目前使用相同租使用者中的帳戶登入合作夥伴中心，就無法移除該租使用者。 若要移除租用戶，您必須以另一個租用戶 (與帳戶相關聯) 的**管理員**身分登入合作夥伴中心。 如果只有一個租用戶與此帳戶相關聯，則只有在用以開啟帳戶的 Microsoft 帳戶登入之後，才能移除該租用戶。


