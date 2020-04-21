---
Description: 您可以透過商務用 Microsoft 網上商店和教育用 Microsoft 網上商店，在應用程式提交的 [組織授權] 區段中，指出您的應用程式是否可以大量購買及其方式。
title: 組織授權選項
ms.assetid: 1EB139B0-67E7-4F66-AAEF-491B1E52E96F
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 商務用 store, 教育用 store, 組織, 大量授權, 企業版, 教育 store, 商務 store, 大量購買, 大量
localizationpriority: high
ms.openlocfilehash: 880e472b9a6ed19bae85c00b4014b431e3185f61
ms.sourcegitcommit: a7effa01ca1c810e792b60f89ba38ce3bf0b310e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2020
ms.locfileid: "81545018"
---
# <a name="organizational-licensing-options"></a>組織授權選項


您可以透過商務用 Microsoft 網上商店和教育用 Microsoft 網上商店，在應用程式提交之[定價和可用性](set-app-pricing-and-availability.md#organizational-licensing)頁面的 [組織授權]  區段中，指出您的應用程式是否可以大量購買及其方式。

透過這些設定，您可以選擇允許您的 app 讓組織 (企業和教育) 使用，這些組織為其使用者取得和部署的多重授權，讓您有機會提升跨 Windows 10 裝置類型 (包括電腦、平板電腦及電話) 之組織的普及。

您也需要針對您直接發佈給企業的任何[企業營運 (LOB) 應用程式](distribute-lob-apps-to-enterprises.md)允許組織授權。

> [!NOTE]
> 每個應用程式選項的設定各不相同。 您可以建立新的提交以隨時變更 app 的喜好設定，您的變更會在提交完成[認證程序](the-app-certification-process.md)之後生效。

> [!IMPORTANT]
> 使用 [Microsoft Store 提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) 進行的提交，無法在商務用 Microsoft Store 及教育用 Microsoft Store 供應。 若要讓您的應用程式可供組織大量購買，您必須在「合作夥伴中心」中建立並提交您的項目。


## <a name="allowing-your-app-to-be-offered-to-organizations"></a>允許將您的 App 提供給組織

根據預設，已核取標示 [利用市集管理 (線上) 授權和散佈提供組織使用我的 App]  的方塊。 這表示您希望您的 App 能夠加入我們可以讓組織大量取得的 App 型錄中，具有透過市集的線上授權系統管理的 App 授權。

> [!NOTE]
> 這並不保證您的應用程式可以讓所有組織使用。

如果您不想讓我們將您的 app 提供給組織進行大量取得，請取消核取此方塊。 請注意，這項變更只有在 app 完成認證程序之後才會發生。 如果任何組織先前曾取得您的 app 的授權，這些授權仍然有效，且已具有 app 的人員可以繼續使用它。

> [!TIP]
> 提示：若要發佈企業營運 (LOB) 應用程式以供特定組織專用，您可以設定企業關聯，並允許組織將 app 直接新增到它們的私人市集。 如需詳細資訊，請參閱[發佈 LOB app 到企業](distribute-lob-apps-to-enterprises.md)。


## <a name="allowing-disconnected-offline-licensing"></a>允許中斷連線 (離線) 授權

許多組織需要 app 啟用離線授權。 例如，某些組織需要將 App 部署到很少或永遠不會連線到網際網路的裝置。 如果您想要讓您的 App 可供這些客戶使用，請核取標示為 [允許組織管理 (離線) 授權並為組織散佈]  的方塊。

請注意，根據預設，這個方塊為 [未核取]  狀態。 您必須核取此方塊，讓您的應用程式可供已驗證的組織使用，這些組織會使用組織管理 (離線) 授權進行安裝。 組織必須通過其他驗證，才能以這種方式向使用者安裝付費 App。

離線授權可讓組織大量取得您的 app，然後不需要每個裝置都連絡市集的授權系統，即可安裝 app。 組織可以下載您的 app 套件以及授權，當使用特定授權時，讓他們將它安裝到裝置 (透過他們自己的管理工具或預先載入作業系統映像上的 app) 而不通知市集。 啟用這個案例會大幅增加部署彈性，它可能會大幅增加 app 對這些客戶的吸引力。

> [!IMPORTANT]
> .xap 套件不支援離線授權。

 
## <a name="paid-app-support"></a>付費應用程式支援

目前，位於特定市場中的開發人員帳戶都能透過商務用 Microsoft 網上商店提供大量取得付費應用程式。 

> [!NOTE]
> 在某些市場中，針對相同的價格區間，商務用 Microsoft Store 或教育用 Microsoft Store 中顯示的應用程式價格可能會不同於 Microsoft Store 中顯示零售客戶價格。 組織購買的收益支付與消費者購買您 app 的運作方式相同。 如需詳細資訊，請參閱[獲得報酬](getting-paid-apps.md)和[應用程式開發人員合約](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)。 如需可提供商務用 Microsoft 網上商店和教育用 Microsoft 網上商店的市場清單，請參閱[商務用 Microsoft 網上商店和教育用 Microsoft 網上商店概觀](https://docs.microsoft.com/windows/manage/windows-store-for-business-overview#supported-markets) \(部分機器翻譯\)。

如果以下未列出您的國家或地區，您的付費應用程式目前不會在商務用 Microsoft 網上商店和教育用 Microsoft 網上商店中提供使用。 如果是這種情況，您針對您的付費應用程式所做的組織授權選項可能會稍後套用，因為我們未來可能會新增從其他開發人員帳戶市場支援付費應用程式提交。

目前，位於下列國家和地區的開發人員可以透過商務用 Microsoft Store 和教育用 Microsoft Store，將付費應用程式發佈給組織客戶：

- 奧地利
- 比利時
- 保加利亞
- 加拿大
- 克羅埃西亞
- 賽普勒斯
- 捷克
- 丹麥
- 愛沙尼亞
- 芬蘭
- 法國
- 德國
- 希臘
- 匈牙利
- 愛爾蘭
- 曼城島
- 義大利
- 拉脫維亞
- 列支敦斯登
- 立陶宛
- 盧森堡
- 馬爾他
- 摩納哥
- 荷蘭
- 挪威
- 波蘭
- 葡萄牙
- 羅馬尼亞
- 斯洛伐克
- 斯洛維尼亞
- 西班牙
- 瑞典
- 瑞士
- 英國
- 美國
