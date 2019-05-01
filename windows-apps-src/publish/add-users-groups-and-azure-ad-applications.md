---
Description: 您可以新增使用者、 群組和您的合作夥伴中心帳戶的 Azure AD 應用程式。
title: 新增使用者、 群組和您的合作夥伴中心帳戶的 Azure AD 應用程式
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10、 uwp、 azure ad 應用程式、 aad、 使用者、 群組、 多個使用者、 多使用者
ms.localizationpriority: medium
ms.openlocfilehash: ddbe47d94e17db0d272aedcff56df95fccf3434d
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2019
ms.locfileid: "63787282"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>新增使用者、 群組和您的合作夥伴中心帳戶的 Azure AD 應用程式

**使用者**一節[合作夥伴中心](https://partner.microsoft.com/dashboard)(在**帳戶設定**) 可讓您使用 Azure Active Directory 將使用者新增至您的合作夥伴中心帳戶。 為每位使用者指派角色（或一組自訂權限），定義帳戶的存取權。 您也可以加入[使用者群組](#groups)並[Azure AD 應用程式](#azure-ad-applications)授與他們存取您的合作夥伴中心帳戶。

新增使用者到帳戶之後，您可以[編輯帳戶詳細資料](#edit)、變更[角色與權限](set-custom-permissions-for-account-users.md)，或[移除使用者](#remove)。

> [!IMPORTANT]
> 若要將使用者新增至您的帳戶，您必須先[合作夥伴中心帳戶與您組織的 Azure Active Directory 租用戶建立關聯](associate-azure-ad-with-partner-center.md)。 

當新增使用者，您必須指定您的合作夥伴中心帳戶其存取權指派[角色或自訂的權限集](set-custom-permissions-for-account-users.md)。 

請記住，所有的合作夥伴中心使用者 （包括群組和 Azure AD 應用程式） 必須具備有效的帳戶在[與合作夥伴中心帳戶相關聯的 Azure AD 租用戶](associate-azure-ad-with-partner-center.md)。 使用者管理是一次在一個租用戶上進行；您必須使用要在其中新增或編輯使用者的租用戶的管理員帳戶登入。 在合作夥伴中心建立新的使用者也會建立該使用者的帳戶在 Azure AD 租用戶的登入，並對使用者的名稱，在合作夥伴中心內的變更會在您組織的 Azure AD 租用戶中進行相同的變更。

> [!NOTE]
> 如果您的組織使用[目錄整合](https://go.microsoft.com/fwlink/p/?LinkID=724033)同步內部部署目錄服務與 Azure AD，您將無法在合作夥伴中心建立新的使用者、 群組或 Azure AD 應用程式。 您 （或您的內部部署目錄中的另一個系統管理員），必須建立它們直接在內部部署目錄中，您將能夠看到，並且將它們加入合作夥伴中心之前。


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>將使用者新增至您的合作夥伴中心帳戶

若要將使用者新增至您的合作夥伴中心帳戶，請前往**使用者**頁面**帳戶設定**，然後選取**新增使用者。** 您必須使用要在其中工作的 Azure AD 租用戶的管理員帳來登入。 

### <a name="add-existing-users"></a>新增現有使用者 

您可以選取已經存在於您組織的租用戶，並讓他們存取您的合作夥伴中心帳戶的使用者。 

<span id="from-directory" />

1.  選取齒輪圖示 （附近的合作夥伴中心右上角），然後選取**開發人員設定**。 在 **設定**功能表上，選取**使用者**。
2.  從 **\[使用者\]** 頁面選取 **\[新增使用者\]**。 
3.  從出現的清單中選取一或多位使用者。 您可以使用搜尋方塊來搜尋特定的使用者。
    > [!TIP]
    > 如果您選取多個使用者新增到您的合作夥伴中心帳戶，您必須指派它們，相同的角色或自訂的權限集。 若要新增多個具有不同角色/權限的使用者，請針對每個角色或一組自訂權限重複下列步驟。
4.  當您完成選取使用者時，請按一下 **\[新增選取的項目\]**。
5.  在 **\[角色\]** 區段中，指定所選使用者的[角色或自訂權限](set-custom-permissions-for-account-users.md)。
6.  按一下 [儲存] 。

### <a name="additional-methods-for-adding-users"></a>新增使用者的其他方法

如果您已登入管理員帳戶也有[全域管理員](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)您使用 Azure AD 租用戶的權限，您必須將使用者新增至您的合作夥伴中心帳戶的其他選項。 您必須選取下列其中一項：

-   **將現有的使用者新增**:選擇使用者已存在於您組織的目錄，並讓他們存取您的合作夥伴中心帳戶，使用上述的方法。
-   **建立新的使用者**:建立全新的使用者帳戶，將同時貴組織的目錄和您的合作夥伴中心帳戶
-   **邀請外部使用者**:傳送電子郵件邀請給目前不在您的組織目錄的使用者。 可存取您的合作夥伴中心帳戶和新獲邀[來賓使用者](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)為他們在 Azure AD 租用戶中建立帳戶。

<span id="new-user" />

### <a name="create-new-users"></a>建立新的使用者

> [!IMPORTANT]
> 您必須使用您的 Azure AD 租用戶中的全域管理員帳戶登入，才能建立新的使用者。

1.  從**使用者**網頁 (底下**帳戶設定**)，選取**將使用者新增**，然後選擇**建立新的使用者**。
2.  輸入新使用者的名字、姓氏及使用者名稱。
3.  如果您希望新的使用者在您的組織目錄中具有[全域管理員帳戶](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)，請核取標示為 **\[將這個使用者設為 Azure AD 中的全域管理員，可完整控制所有目錄資源\]** 方塊。 這將可讓使用者完整存取您公司的 Azure AD 中的所有系統管理功能。 他們將能夠新增及管理您的組織目錄中的使用者 (但不能在合作夥伴中心，除非您授與帳戶的適當[角色/權限](set-custom-permissions-for-account-users.md))。 如果您核取此方塊，必須為使用者提供**密碼復原電子郵件**。
4.  如果您選取 **\[將這個使用者設為 Azure AD 中的全域管理員\]** 方塊，請輸入使用者需要復原其密碼時可以使用的電子郵件。
5.  在 **\[群組成員資格\]** 區段中，選取任何您要讓新使用者隸屬的群組。
6.  在 **\[角色\]** 區段中，指定使用者的[角色或自訂權限](set-custom-permissions-for-account-users.md)。
7.  按一下 [儲存] 。
8.  在確認頁面上，您將會看見新使用者的登入資訊 (包括暫時密碼)。 請確定會記下此資訊，並將它提供給新的使用者，因為您在離開此頁面之後將無法存取該暫時密碼。


<span id="email" />

### <a name="invite-outside-users"></a>邀請外部使用者

> [!IMPORTANT]
> 您必須使用您的 Azure AD 租用戶中的全域管理員帳戶登入，才能邀請外部使用者。

1.  從**使用者**網頁 (底下**帳戶設定**)，選取**將使用者新增**，然後選擇**邀請使用者透過電子郵件**。
1.  輸入一或多個電子郵件地址 (最多十個)，以逗號或分號分隔。
2.  在 **\[角色\]** 區段中，指定使用者的[角色或自訂權限](set-custom-permissions-for-account-users.md)。
3.  按一下 [儲存] 。

您邀請的使用者會取得電子郵件邀請來加入您的帳戶，而在您的 Azure AD 租用戶中將為其建立新的[來賓使用者](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)帳戶。 使用者必須接受邀請才能存取您的帳戶。

如果您需要重新傳送邀請，請在 **\[使用者\]** 頁面上尋找使用者，然後選取其電子郵件地址 (或選取表示**邀請擱置中**的文字)。 然後，按一下頁面底部的 **\[重新傳送邀請\]**。

> [!IMPORTANT]
> 外部使用者邀請加入，您的合作夥伴中心帳戶可以指派相同的角色和權限以其他使用者的身分。 不過，外部使用者無法在 Visual Studio 中執行某些工作，例如將關聯應用程式至 Microsoft Store，或是建立套件以上傳至 Microsoft Store。 如果使用者需要執行這些工作，請選擇 **\[建立新的使用者\]** 而不是 **\[邀請外部使用者\]**。 (如果您不想將這些使用者新增至您現有的 Azure AD 租用戶，您可以[建立新的租用戶](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account)，然後在該租用戶中為他們建立新的使用者帳戶)。 


### <a name="changing-a-users-directory-password"></a>變更使用者的目錄密碼

如果您的其中一個使用者需要變更其密碼，只要您已在建立使用者帳戶時提供**密碼復原電子郵件**，他們就可以自行進行變更。 您也可以依照下列步驟更新使用者的密碼 (如果您使用 Azure AD 租用戶的全域管理員帳戶登入，以變更使用者的密碼)。 請注意，這會變更您的 Azure AD 租用戶中使用者的密碼，他們用來存取合作夥伴中心及密碼。 

1.  從**使用者**網頁 (底下**帳戶設定**)，選取您想要編輯的使用者帳戶的名稱。
2.  選取 **重設密碼**在頁面底部的按鈕。
3.  隨即會出現確認頁面，顯示該使用者的登入資訊 (包括暫時密碼)。

    > [!IMPORTANT]
    >  請務必先列印或複製此資訊並提供給使用者，當您將無法存取的暫時密碼之後您離開此頁面。

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>將群組新增至您的合作夥伴中心帳戶

您可以將群組從貴組織的目錄加入合作夥伴中心帳戶。 當您這樣做時，隸屬於該群組成員的每位使用者都能使用與群組指派角色相關聯的權限來存取它。

### <a name="add-groups-from-your-organizations-directory"></a>從組織的目錄新增群組

1.  選取齒輪圖示 （附近的合作夥伴中心右上角），然後選取**開發人員設定**。 在 **設定**功能表上，選取**使用者**。
2. 從**使用者**頁面上，選取**新增群組**。
2.  從出現的清單中選取一或多個群組。 您可以使用搜尋方塊來搜尋特定的群組。
    > [!TIP]
    > 如果您選取多個群組新增到您的合作夥伴中心帳戶，您必須指派它們，相同的角色或自訂的權限集。 若要新增多個具有不同角色/權限的群組，請針對每個角色或一組自訂權限重複下列步驟。

3.  當您完成選取群組時，請按一下 **\[新增選取的項目\]**。
4.  在 **\[角色\]** 區段中，指定所選群組的[角色或自訂權限](set-custom-permissions-for-account-users.md)。 所有群組的成員都可以存取合作夥伴中心帳戶的權限您套用至群組，不論其個別的帳戶相關聯的角色/權限。
5.  按一下 [儲存] 。


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>在您的組織目錄中建立新的群組帳戶，並將它新增至您的合作夥伴中心帳戶

如果您想要授與合作夥伴中心存取權的全新的群組，您可以建立新的群組中**使用者**一節。 請注意，將會在您的組織目錄中，不只是在您的合作夥伴中心帳戶中建立新的群組。

1.  從**使用者**網頁 (底下**開發人員設定**)，按一下 **新增群組**。
2.  在下一步 頁面上，選取**新的群組**。
3.  輸入新群組的顯示名稱。
4.  指定群組的[角色或自訂權限](set-custom-permissions-for-account-users.md)。 所有群組的成員都可以存取合作夥伴中心帳戶的權限您套用至群組，不論其個別的帳戶相關聯的角色/權限。
5.  從出現的清單中選取要指派至新群組的使用者。 您可以使用搜尋方塊來搜尋特定的使用者。
6.  完成選取使用者時，按一下 **\[新增選取的項目\]**，將其新增到新群組。
7.  按一下 [儲存] 。


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>新增至您的合作夥伴中心帳戶的 Azure AD 應用程式

您可以讓應用程式或服務屬於貴組織的 Azure AD 以存取您的合作夥伴中心帳戶。 這些 Azure AD 應用程式使用者帳戶可以用來呼叫 [Microsoft Store 服務](../monetize/using-windows-store-services.md)提供的 REST API。


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>從組織的目錄新增 Azure AD 應用程式

1.  1.  選取齒輪圖示 （附近的合作夥伴中心右上角），然後選取**開發人員設定**。 在 **設定**功能表上，選取**使用者**。
2. 從 **\[使用者\]** 頁面，選取 **\[新增 Azure AD 應用程式\]**。
3.  從出現的清單中選取一或多個 Azure AD 應用程式。 您可以使用搜尋方塊來搜尋特定的 Azure AD 應用程式。
    > [!TIP]
    > 如果您選取一個以上的 Azure AD 應用程式，將新增至您的合作夥伴中心帳戶，您必須指派它們，相同的角色或自訂的權限集。 若要新增多個具有不同角色/權限的 Azure AD 應用程式，請針對每個角色或一組自訂權限重複下列步驟。

4.  當您完成選取 Azure AD 應用程式時，請按一下 **\[新增選取的項目\]**。
5.  在 **\[角色\]** 區段中，指定所選 Azure AD 應用程式的[角色或自訂權限](set-custom-permissions-for-account-users.md)。
6.  按一下 [儲存] 。


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>建立新的 Azure AD 應用程式帳戶在您的組織目錄中，並將它新增至您的合作夥伴中心帳戶

如果您想要授與合作夥伴中心帳戶的存取權的品牌新的 Azure AD 應用程式，您可以建立一種**使用者**一節。 請注意，這就會建立新的帳戶，您的組織目錄中，不只是在您的合作夥伴中心帳戶。

> [!TIP]
> 如果您主要用於此 Azure AD 應用程式合作夥伴中心驗證，並不需要直接存取的使用者，您可以輸入任何有效的位址，如**回覆 URL**並**應用程式識別碼 URI**，只要做為這些值不會使用您的目錄中任何其他 Azure AD 應用程式。

1.  從**使用者**網頁 (底下**帳戶設定**)，選取**加入 Azure AD 應用程式**。
2.  在下一步 頁面上，選取**新增 Azure AD 應用程式**。
3.  輸入新的 Azure AD 應用程式的 **\[回覆 URL\]**。 使用者可以從此 URL 登入並使用您的 Azure AD 應用程式 (也稱為 App URL 或登入 URL)。 **回覆 URL** 長度不可超過 256 個字元，而且在您的目錄中必須是唯一的。
4.  輸入新 Azure AD 應用程式的 [**應用程式識別碼 URI**]。 這是 Azure AD 應用程式的邏輯識別碼，是在傳送單一登入要求至 Azure AD 時提供。 請注意，在目錄中的每個 Azure AD 應用程式的 **\[應用程式識別碼 URI\]** 必須是唯一的，且長度不能超過 256 個字元。 如需**應用程式識別碼 URI** 的詳細資訊，請參閱[整合應用程式與 Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant)。
5.  在 **\[角色\]** 區段中，指定 Azure AD 應用程式的[角色或自訂權限](set-custom-permissions-for-account-users.md)。
6.  按一下 [儲存] 。

新增或建立 Azure AD 應用程式之後，您可以返回 **\[使用者\]** 區段，然後選取應用程式名稱以檢閱應用程式的設定，包括租用戶識別碼、用戶端識別碼、回覆 URL，以及 App 識別碼 URI。

> [!NOTE]
> 如果您想要使用 [Microsoft Store 服務](../monetize/using-windows-store-services.md)所提供的 REST API，您將需要此頁面上所顯示的 [租用戶識別碼] 和 [用戶端識別碼] 的值，以取得 Azure AD 存取權杖，您可以使用此存取權杖向服務驗證呼叫。   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>管理 Azure AD 應用程式的金鑰

如果您的 Azure AD 應用程式會在 Microsoft Azure AD 讀取或寫入資料，則它需要金鑰。 您可以藉由編輯其資訊在合作夥伴中心建立 Azure AD 應用程式的金鑰。 您也可以移除不再需要的金鑰。

1.  從**使用者**網頁 (底下**帳戶設定**)，選取 Azure AD 應用程式的名稱。
    > [!TIP]
    > 當您按一下 Azure AD 應用程式的名稱時，您會看到所有作用中的索引鍵，Azure AD 應用程式，包括在建立金鑰和到期的日期。 若要移除不再需要的金鑰，請按一下 **[移除]**。

2.  若要加入新的金鑰，請選取**新增新的金鑰**。
3.  您將會看到顯示 **\[用戶端識別碼\]** 和 **\[金鑰\]** 值的畫面。
    > [!IMPORTANT]
    > 請務必先列印或複製此資訊，因為您將無法再存取它之後您離開此頁面。

4.  如果您想要建立多個索引鍵，選取**新增另一個索引鍵**。

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>編輯使用者、群組或 Azure AD 應用程式

新增使用者、 群組和/或您的合作夥伴中心帳戶的 Azure AD 應用程式之後，您可以變更其帳戶資訊。 

> [!IMPORTANT]
> 所做的變更[角色或權限](set-custom-permissions-for-account-users.md)只會影響到合作夥伴中心存取。 （例如 Azure AD 應用程式變更使用者的名稱或群組成員資格，或回覆 URL 和應用程式識別碼 URI） 的所有其他變更將反映在貴組織的 Azure AD 租用戶也與您的合作夥伴中心帳戶中。 

1.  從**使用者**網頁 (底下**帳戶設定**)，選取使用者、 群組或您想要編輯的 Azure AD 應用程式帳戶的名稱。
2.  進行所需的變更。 可以編輯的項目如下：
    -   對於 **\[使用者\]**，可以編輯使用者的名字、姓氏或使用者名稱。 您也可以選取或取消選取 **\[群組成員資格\]** 區段中的群組，以更新其群組成員資格。
    -   對於 **\[群組\]**，您可以編輯群組的名稱  (若要更新群組成員資格，請編輯要在群組中新增或移除的使用者，並在 **\[群組成員資格\]** 區段中進行變更)。
    -   對於 **\[Azure AD 應用程式\]**，您可以為 **\[回覆 URL\]** 或 **\[應用程式識別碼 URI\]** 輸入新值。
    請記住這些變更都會在您的組織目錄以及您的合作夥伴中心帳戶。
3.  如需與合作夥伴中心存取相關的變更，選取或取消選取您想要套用，或選取的角色**自訂權限**並進行所需的變更。 這些變更只會影響合作夥伴中心存取並不會變更您的組織 Azure AD 租用戶內的任何權限。
3.  按一下 [儲存] 。


## <a name="view-history-for-account-users"></a>檢視帳戶使用者歷程記錄

身為帳戶擁有者，您可以檢視已加入至帳戶中的任何其他使用者的詳細瀏覽記錄。

上**使用者**網頁 (底下**帳戶設定**)，選取顯示在下面的連結**上次活動**使用者想要檢閱其瀏覽歷程記錄。 您將可以檢視使用者在最近 30 天中瀏覽之所有頁面的 URL。

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>移除使用者、群組及 Azure AD 應用程式

若要從您的合作夥伴中心帳戶中移除使用者、 群組或 Azure AD 應用程式，請選取**移除**連結，會依其名稱出現在**使用者**頁面。 確認您想要將它移除之後, 該使用者、 群組或 Azure AD 應用程式將不再能夠存取您的合作夥伴中心帳戶 （除非您稍後再加入它）。

> [!IMPORTANT]
> 移除使用者、 群組或 Azure AD 應用程式，以表示它將不再有合作夥伴中心帳戶的存取權。 它**不**會從組織的目錄中刪除使用者、群組或 Azure AD 應用程式。

 

