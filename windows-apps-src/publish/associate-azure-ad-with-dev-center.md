---
author: jnHs
Description: "若要新增和管理帳戶使用者，您必須先將開發人員中心帳戶與組織的 Azure Active Directory 產生關聯。"
title: "將 Azure Active Directory 與您的開發人員中心帳戶產生關聯"
ms.author: wdg-dev-content
ms.date: 07/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: ace9470cc707206461baa8c3dd72828ea68a8eb4
ms.sourcegitcommit: eaacc472317eef343b764d17e57ef24389dd1cc3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2017
---
# <a name="associate-azure-active-directory-with-your-dev-center-account"></a>將 Azure Active Directory 與您的開發人員中心帳戶產生關聯

若要[新增和管理帳戶使用者](add-users-groups-and-azure-ad-applications.md)，您必須先將開發人員中心帳戶與組織的 Azure Active Directory 產生關聯。 

Windows 開發人員中心會針對多使用者管理與角色指派來運用 Azure AD。 如果您的組織已經使用 Office 365 或其他 Microsoft 所提供的商務服務，您就已經具備 Azure AD。 否則，您可以從開發人員中心免費建立新的 Azure AD 租用戶。

> [!IMPORTANT]
> 若要將 Azure AD 關聯至您的開發人員中心帳戶，您將需要使用[全域管理員](http://go.microsoft.com/fwlink/?LinkId=746654)帳戶登入您的 Azure AD 租用戶。
> 
> 將您的開發人員中心帳戶關聯至 Azure AD 之後，一律需要使用 Azure AD 全域管理員帳戶 (非個人的 Microsoft 帳戶) 來登入開發人員中心，以便新增和管理帳戶使用者。

請注意，只有一個開發人員中心帳戶可以與一個 Azure AD 租用戶相關聯。 同樣地，只有一個 Azure AD 租用戶可以與一個開發人員中心帳戶相關聯。 一旦建立關聯之後，您就無法在不連絡支援的情況下移除它。


## <a name="associate-your-dev-center-account-with-your-organizations-existing-azure-ad-tenant"></a>將開發人員中心帳戶與組織現有的 Azure AD 租用戶產生關聯

如果您的組織已經使用 Azure AD，請依照下列步驟連結您的開發人員中心帳戶。

1.  移至您的 **\[帳戶設定\]**，然後按一下 **\[管理使用者\]**。
2.  按一下 **\[將 Azure AD 關聯至您的開發人員中心帳戶\]** 按鈕。
3.  登入您的 Azure AD 帳戶。 這個帳戶必須具備[全域管理員](http://go.microsoft.com/fwlink/?LinkId=746654)權限，才能設定關聯。
4.  檢閱 Azure AD 租用戶的組織和網域名稱。 若要完成關聯，請按一下 **\[確認\]**。
5.  如果關聯成功，則您已經準備好在開發人員中心的 **\[管理使用者\]** 區段上新增和管理帳戶使用者。


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account"></a>建立全新的 Azure AD 以便與您的開發人員中心帳戶產生關聯

如果您需要設定新的 Azure AD 以便與您的開發人員中心帳戶連結，請遵循下列步驟。

1.  移至您的 **\[帳戶設定\]**，然後按一下 **\[管理使用者\]**。
2.  按一下 **\[建立新的 Azure AD\]** 按鈕。
3.  輸入新 Azure AD 的目錄資訊︰
 - **網域名稱**︰我們會連同 “.onmicrosoft.com” 針對您的 Azure AD 網域使用的唯一名稱。 例如，如果您輸入 “example”，您的 Azure AD 網域就是 “example.onmicrosoft.com”。
 - **連絡人電子郵件**：我們可以在需要時針對帳戶相關資訊與您連絡的電子郵件地址。
 - **全域管理員使用者帳戶資訊**：您想要針對新的全域管理員帳戶使用的名字、姓氏、使用者名稱及密碼。
4.  按一下 **\[建立\]** 來確認新網域和帳戶資訊。
5.  使用您新的 Azure AD 全域管理員使用者名稱與密碼來登入，開始[新增和管理其他的帳戶使用者](add-users-groups-and-azure-ad-applications.md)。



