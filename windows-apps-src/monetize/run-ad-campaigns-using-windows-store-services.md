---
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: 使用 Microsoft Store 提升 API 以程式設計方式管理您自己或您組織的合作夥伴中心帳戶已註冊的應用程式的促銷廣告活動。
title: 使用Microsoft Store 服務執行廣告行銷活動
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 促銷 API, 廣告行銷活動
ms.localizationpriority: medium
ms.openlocfilehash: 58325074a2f59dcd146a9534054b302b3ce9956b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619143"
---
# <a name="run-ad-campaigns-using-store-services"></a>使用Microsoft Store 服務執行廣告行銷活動

使用*Microsoft Store 提升 API*以程式設計方式管理您自己或您組織的合作夥伴中心帳戶已註冊的應用程式的促銷廣告活動。 這個 API 可讓您用來建立、更新及監視您的行銷活動，以及其他相關資產，例如目標和廣告素材。 此 API 是誰建立大量的活動，並想要這樣做而不使用合作夥伴中心開發人員而言特別有用。 這個 API 使用 Azure Active Directory (Azure AD) 來驗證您應用程式或服務的呼叫。

下列步驟說明端對端的程序：

1.  請確定您已經完成所有[先決條件](#prerequisites)。
2.  在呼叫 Microsoft Store 促銷 API 中的方法之前，請先[取得 Azure AD 存取權杖](#obtain-an-azure-ad-access-token)。 取得權杖之後，在權杖到期之前，您有 60 分鐘的時間可以使用這個權杖呼叫 Microsoft Store 促銷 API。 權杖到期之後，您可以產生新的權杖。
3.  [呼叫 Microsoft Store 促銷 API](#call-the-windows-store-promotions-api)。

或者，您可以建立並管理使用合作夥伴中心的廣告活動和您透過 Microsoft Store 促銷活動，也可以在合作夥伴中心存取 API 以程式設計方式建立任何廣告活動。 如需管理在合作夥伴中心內的廣告活動的詳細資訊，請參閱[建立您的應用程式廣告活動](../publish/create-an-ad-campaign-for-your-app.md)。

> [!NOTE]
> 合作夥伴中心帳戶使用任何開發人員可以使用 Microsoft Store 提升 API 來管理他們的應用程式的廣告活動。 媒體代理商也可要求存取這個 API，代表廣告客戶執行廣告行銷活動。 如果您是想要深入了解或要求存取這個 API 的媒體代理商，請將您的要求傳送至 storepromotionsapi@microsoft.com。

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-promotions-api"></a>步驟 1：完成使用 Microsoft Store 提升 API 的必要條件

開始撰寫程式碼以呼叫 Microsoft Store 促銷 API 之前，請先確定您已完成下列先決條件。

* 您可以成功地建立並啟動廣告活動使用此 API 之前，您必須先[建立另一種付費的廣告活動使用**廣告活動**在合作夥伴中心內的頁面](../publish/create-an-ad-campaign-for-your-app.md)，而且您必須加入至少一個付款在此頁面上的檢測。 執行此動作之後，您就可以使用此 API 成功地建立廣告行銷活動的可計費廣告播送行。 您建立使用此 API 的廣告活動的傳遞行會自動向上所選擇的預設付款方式**廣告活動**在合作夥伴中心內的頁面。

* 您 (或您的組織) 必須擁有 Azure AD 目錄，而且您必須具備目錄的[全域系統管理員](https://go.microsoft.com/fwlink/?LinkId=746654)權限。 如果您已經使用 Office 365 或其他 Microsoft 所提供的商務服務，您就已經擁有 Azure AD 目錄。 否則，您可以[在合作夥伴中心建立新的 Azure AD](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account)免費。

* 您必須與您的合作夥伴中心帳戶相關聯的 Azure AD 應用程式、 擷取租用戶識別碼和應用程式的用戶端識別碼，並產生金鑰。 Azure AD 應用程式代表您要呼叫 Microsoft Store 促銷 API 的 App 或服務。 您需要租用戶識別碼、用戶端識別碼和金鑰，才能取得傳遞給 API 的 Azure AD 存取權杖。
    > [!NOTE]
    > 您只需要執行此工作一次。 有了租用戶識別碼、用戶端識別碼和金鑰，每當您必須建立新的 Azure AD 存取權杖時，就可以重複使用它們。

若要與您的合作夥伴中心帳戶相關聯的 Azure AD 應用程式，並擷取所需的值：

1.  在合作夥伴中心[貴組織的合作夥伴中心帳戶和組織的 Azure AD 目錄產生關聯](../publish/associate-azure-ad-with-partner-center.md)。

2.  接下來，從**使用者**頁面**帳戶設定**一節的合作夥伴中心[加入 Azure AD 應用程式](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account)表示的應用程式或服務，您將使用管理您的合作夥伴中心帳戶的促銷活動。 請確定您指派此應用程式 **[管理員]** 角色。 如果應用程式不存在，但在您的 Azure AD 目錄，您可以[建立新的 Azure AD 應用程式在合作夥伴中心](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account)。 

3.  返回 **\[使用者\]** 頁面，按一下您 Azure AD 應用程式的名稱來移至應用程式設定，然後複製 **\[租用戶識別碼\]** 和 **\[用戶端識別碼\]** 的值。

4. 按一下 \[加入新的金鑰\]。 在下列畫面中，複製 \[金鑰\] 的值。 您離開這個頁面之後就無法再存取此資訊。 如需詳細資訊，請參閱[管理 Azure AD 應用程式的金鑰](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys)。

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>步驟 2：取得 Azure AD 存取權杖

在 Microsoft Store 促銷 API 中呼叫任何方法之前，您必須先取得傳遞至 API 中每個方法的 **Authorization** 標頭的 Azure AD 存取權杖。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以重新整理權杖以便在後續呼叫 API 時繼續使用。

若要取得存取權杖，請按照[使用用戶端認證的服務對服務呼叫](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/)中的指示，將 HTTP POST 傳送至 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 端點。 以下是範例要求。

```syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

針對*租用戶\_識別碼*POST URI 中的值和*用戶端\_識別碼*並*用戶端\_祕密*參數，指定的租用戶識別碼、 用戶端識別碼和您從上一節中的合作夥伴中心擷取您應用程式的金鑰。 對於 *resource* 參數，您必須指定 ```https://manage.devcenter.microsoft.com```。

存取權杖到期之後，您可以按照[這裡](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)的指示，重新整理權杖。

<span id="call-the-windows-store-promotions-api" />

## <a name="step-3-call-the-microsoft-store-promotions-api"></a>步驟 3：呼叫 Microsoft Store 提升 API

有了 Azure AD 存取權杖之後，就可以呼叫 Microsoft Store 促銷 API。 您必須將存取權杖傳送給每個方法的 **Authorization** 標頭。

在 Microsoft Store 中促銷 API 的內容中，廣告行銷活動包含 *「行銷活動」* 物件 (其中包含行銷活動的高階資訊)，以及其他物件 (代表廣告行銷活動的 *「廣告播送行」*、*「目標設定檔」* 及 *「廣告素材」*)。 API 包括不同組的方法，依物件類型分組。 若要建立行銷活動，您通常針對每個物件呼叫不同 POST 方法。 API 也提供 GET 方法，可用來擷取任何物件，以及 PUT 方法，可以使用它來編輯行銷活動、廣告播送行及目標設定檔物件。

如需有關這些物件與其相關方法的詳細資訊，請參閱下表。


| 物件       | 描述   |
|---------------|-----------------|
| 行銷活動 |  這個物件表示廣告行銷活動，它位於廣告行銷活動的物件模型階層頂端。 這個物件辨識您正在執行之行銷活動的類型 (付費、內部或社群)、行銷活動目標、行銷活動的廣告播送行，以及其他詳細資料。 每個行銷活動只能與一個 app 相關聯。<br/><br/>如需有關這個物件的相關方法的詳細資訊，請參閱[管理廣告行銷活動](manage-ad-campaigns.md)。<br/><br/>**注意**&nbsp;&nbsp;建立廣告行銷活動之後，您可以使用 [Microsoft Store 分析 API](access-analytics-data-using-windows-store-services.md) 中的[取得廣告行銷活動績效資料](get-ad-campaign-performance-data.md)方法，擷取行銷活動的績效資料。  |
| 廣告播送行 | 每個行銷活動有一或多個廣告播送行用於購買庫存廣告，並播送您的廣告。 對於每個廣告播送行，您可以設為目標，設定買方出價，以及設定預算和要使用之廣告素材的連結，決定要花多少費用。<br/><br/>如需有關這個物件的相關方法的詳細資訊，請參閱[管理廣告行銷活動的廣告播送行](manage-delivery-lines-for-ad-campaigns.md)。 |
| 鎖定設定檔 | 每個廣告播送行有一個目標設定檔，指定您想要鎖定的使用者、地區和庫存廣告類型。 目標設定檔可以建立做為範本，並跨廣告播送行共用。<br/><br/>如需有關這個物件的相關方法的詳細資訊，請參閱[管理廣告行銷活動的目標設定檔](manage-targeting-profiles-for-ad-campaigns.md)。 |
| 廣告素材 | 每個廣告播送行有一或多個廣告素材，代表行銷活動中對客戶顯示的廣告。 廣告素材可能會與一個或多個廣告播送行相關聯，甚至跨廣告行銷活動，但前提是它永遠代表相同 app。<br/><br/>如需有關這個物件的相關方法的詳細資訊，請參閱[管理廣告行銷活動的廣告素材](manage-creatives-for-ad-campaigns.md)。 |


下圖顯示行銷活動、廣告播送行、目標設定檔及廣告素材之間的關係。

![廣告行銷活動階層](images/ad-campaign-hierarchy.png)

## <a name="code-example"></a>程式碼範例

下列程式碼範例示範如何取得 Azure AD 存取權杖，並從 C# 主控台應用程式呼叫 Microsoft Store 促銷 API。 若要使用此程式碼範例，請針對您的案例將 *tenantId*、*clientId*、*clientSecret* 和 *appID* 變數指定為適當的值。 此範例需要 Newtonsoft 的 [Json.NET 套件](https://www.newtonsoft.com/json)，以還原序列化 Microsoft Store 促銷 API 傳回的 JSON 資料。

[!code-cs[PromotionsApi](./code/StoreServicesExamples_Promotions/cs/Program.cs#PromotionsApiExample)]

## <a name="related-topics"></a>相關主題

* [管理 ad 行銷活動](manage-ad-campaigns.md)
* [管理 ad 行銷活動傳遞明細行](manage-delivery-lines-for-ad-campaigns.md)
* [管理 ad 行銷活動的目標設定檔](manage-targeting-profiles-for-ad-campaigns.md)
* [管理 ad 行銷活動的素材](manage-creatives-for-ad-campaigns.md)
* [取得 ad 行銷活動效能資料](get-ad-campaign-performance-data.md)


 
