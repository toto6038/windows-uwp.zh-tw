---
author: jnHs
Description: "您可以透過商務用 Windows 市集，將企業營運 (LOB) app 直接發佈到企業來進行大量取得，而不需使 app 可在市集中廣泛取得。"
title: "將 LOB 應用程式發佈到企業"
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: e29809f34facafc442b9b26580b91e17ed0a364e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="distribute-lob-apps-to-enterprises"></a>將 LOB 應用程式發佈到企業


您可以透過商務用 Windows 市集，將企業營運 (LOB) app 直接發佈到企業來進行大量取得，而不需使 app 可在市集中廣泛取得。

> **重要**  目前只有免費的應用程式可透過商務用 Windows 市集，單獨發佈到企業。 若您提交付費 app 做為 LOB，則目前無法將其提供給企業使用。 

## <a name="setting-up-the-enterprise-association"></a>設定企業關聯


單獨將 LOB app 發佈到企業的第一個步驟是在您的帳戶與企業的私人市集之間建立關聯。

> **重要**：此關聯程序必須由企業初始化，且必須傳送至您帳戶之 **\[連絡資訊\]** 中的電子郵件地址。 如需詳細資訊，請參閱[使用企業營運 app](http://go.microsoft.com/fwlink/p/?LinkId=698846)。

當企業選擇邀請您發佈 app 以供他們專用時，您將會收到一封電子郵件，其中包含確認關聯的連結。 您也可以移至 **\[帳戶設定\]** 的 **\[企業關聯\]** 區段來確認這些關聯。

若要確認關聯，請按一下 **\[接受\]**。 您的帳戶接著能夠發佈 app，以供該企業專用。

## <a name="submitting-an-lob-app"></a>提交 LOB App


一旦準備好發佈 app 以供某個企業專用之後，程序就會類似 app 提交程序。 App 會執行相同的認證程序，而且必須遵守所有 [Windows 市集原則](https://msdn.microsoft.com/library/windows/apps/dn764944)。 程序只有一些部分不同。

### <a name="distribution-and-visibility"></a>配送和可見性

在您設定企業關聯之後，每當您提交 app 時，就會在提交的 **\[定價和可用性\]** 頁面之 **\[配送和可見性\]** 區段中看見一個下拉式方塊。 根據預設，這會設定為 **\[零售經銷\]**。 若要使 app 供某家企業專用，您需要選擇 **\[企業營運 (LOB) 發佈\]**。

選取 **\[企業營運 (LOB) 發佈\]** 之後，一般的 **\[配送和可見性\]** 選項將會利用您可為其發佈專屬 app 的企業清單來取代。

選取應該能夠取得該 app 的企業。 沒有其他任何人能夠存取它。

> **注意**  至少必須選取一家企業，以便將應用程式當成企業營運應用程式來發佈。

### <a name="organizational-licensing"></a>組織授權

根據預設，在您提交 app 時，會勾選 **\[市集管理 (線上) 大量授權\]** 的方塊。 發佈 LOB app 時，必須勾選此方塊，讓企業能夠大量取得您的 app。 這樣一來，您於 **\[配送和可見性\]** 區段中選取之企業外部的任何使用者都將無法使用該 app。

如果您想要讓企業透過中斷連線 (離線) 授權來取得該 app，您也可以勾選 **\[中斷連線 (離線) 授權\]** 方塊。

如需詳細資訊，請參閱[組織授權選項](organizational-licensing.md)。

### <a name="age-ratings"></a>年齡分級
針對 LOB app，提交程序的[年齡分級](age-ratings.md)步驟的運作方式與零售版 App 相同，但您也有其他選擇，可讓您手動指出您 App 的市集年齡分級，而不是完成問卷或是匯入現有的 IARC 分級識別碼。 此手動分級僅能搭配 LOB 散佈使用，因此，如果您曾經將 App 的 **\[配送和可見性\]** 設定變更為 **\[零售經銷\]**，您將需要先接受年齡分級問卷調查，然後才可以發佈提交。

### <a name="enterprise-deployment-of-lob-apps"></a>LOB App 的企業部署

按一下 **\[提交到市集\]** 之後，app 將進入認證程序。 一旦準備好之後，企業的系統管理員就必須在商務用 Windows 市集入口網站中將它新增到私人市集。 企業接著可將該 app 部署到它的使用者。

> **注意** 若要取得您的 LOB app，組織必須位於[支援的市場](https://technet.microsoft.com/itpro/windows/whats-new/windows-store-for-business-overview#supported-markets)，且提交您的 app 時不能排除該市場。 

如需詳細資訊，請參閱[使用企業營運 App](http://go.microsoft.com/fwlink/p/?LinkId=698846) 和[使用您的私人市集發佈 App](http://go.microsoft.com/fwlink/p/?LinkId=698847)。

### <a name="updating-lob-apps"></a>更新 LOB App

若要將更新發佈到您已經準備好發佈為 LOB 的 app，只需建立新的提交。 您可以上傳新的套件或進行任何其他變更，然後按一下 **\[提交到市集\]**，讓更新的版本可供使用。 請確定會在 **\[配送和可見性\]** 中使企業選取項目保持不變 (除非您故意想要變更它們，例如，選取其他企業來取得該 app，或是移除您先前已發佈該 app 的其中一個企業)。

如果您想要停止提供您先前已發佈為企業營運 app 的 app 並防止任何新的取得，您需要建立新的提交。 首先，需要將 **\[配送和可見性\]** 選取項目從 **\[企業營運 (LOB) 發佈\]** 變更為 **\[零售經銷\]**。 接著，在 **\[配送和可見性\]** 選項中，選擇 **\[隱藏這個 app 並防止擷取\]**。 在提交進入認證程序之後，該 App 將不再供新的下載數使用 (儘管任何已經擁有它的人還是能夠繼續使用)。

> **注意**：將 App 變更為 **\[零售經銷\]** 時，如果您還未完成[年齡分級問卷](age-ratings.md)，則必須先完成該問卷 (即使 App 將不再供新的下載數使用)。

### <a name="distributing-lob-apps-through-sideloading"></a>透過側載發佈 LOB App

透過商務用市集使 app 可供使用，可確保該 app 已經過市集簽署，並且遵守標準的市集原則。

在某些情況下，公司基於各種不同原因，可能不想透過 Windows 開發人員中心來提交它們的 LOB app (例如，規範的原因或需要其他功能的 app)。 在此情況下，企業可以透過側載將 app 直接部署到電腦，而不需使用商務用 Windows 市集。

如需詳細資訊，請參閱[在 Windows 10 中側載 LOB App](http://go.microsoft.com/fwlink/p/?LinkId=623433)。

 

 




