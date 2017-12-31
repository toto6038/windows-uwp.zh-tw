---
author: jnHs
Description: "選取應用程式的基本價格並排程價格變更。 您也可以針對特定市場自訂這些選項。"
title: "設定與排程應用程價格"
ms.author: wdg-dev-content
ms.date: 08/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 65d2fa64e4d442da42b8c546693c61eda817aa18
ms.sourcegitcommit: 968187e803a866b60cda0528718a3d31f07dc54c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2017
---
# <a name="set-and-schedule-app-pricing"></a>設定與排程應用程價格

[價格和可用性](set-app-pricing-and-availability.md)頁面的 **\[價格\]** 區段可讓您選取應用程式的基本價格。 您也可以[排程價格變更](#schedule-price-changes)，表示應用程式的價格應變更的日期和時間。 此外，您可以選擇[針對特定市場自訂這些變更](#customize-pricing-for-specific-markets)。 

> [!NOTE]
> 雖然本主題是關於應用程式，但是附加元件提交的價格選擇使用相同的程序。

## <a name="base-price"></a>基本價格

當您選取您應用程式的 **\[基本價格\]** 時，會在每個販售您應用程式的市場中使用該價格，除非您為特定市場指定自訂價格。

您可以將 [**基本價格**] 設定為 [**免費**]，或者您可以選擇可用的價格區間，藉此設定您選擇散發應用程式的所有國家/地區的銷售價格。 價格區間從 $0.99 美元開始，其餘區間依次遞增提供 ($1.09 美元、$1.19 美元，以此類推)。 遞增值一般會隨著價格變高而增加。 

> [!NOTE]
> 這些價格區間也適用於附加元件。 

每個價格區間對於市集所提供 60 種以上的貨幣都有對應值。 我們使用這些值協助您在全球以可資比較的價格帶來銷售應用程式。 您可以選擇任何貨幣的基本價格，我們會針對不同市場自動使用不同的對應值。

在 [**價格**] 區段中，按一下 [**檢視表格**] 查看所有貨幣的相對價格。 這也會顯示與每個價區間相關聯的識別碼，如果您使用 [Windows 市集提交 API](../monetize/manage-app-submissions.md#price-tiers) 輸入價格就會需要此識別碼。 您可以按一下 [**下載**]，下載 .csv 檔案格式的價格區間表複本。

請記住，您選取的價格區間可以包含客戶必須支付的銷售或增值稅。 若要深入了解您的應用程式在選取市場中的稅務關連相關資訊，請參閱[付費應用程式的稅務詳細資料](tax-details-for-paid-apps.md)。 另外建議您檢閱[特定市場的定價考量](define-pricing-and-market-selection.md#price-considerations-for-specific-markets)。

## <a name="schedule-price-changes"></a>排程價格變更

如果您想要在特定日期和時間變更應用程式的基本價格，可以選擇性地排程一次或多次價格變更。 

> [!IMPORTANT]
> 價格變更只會對使用 Windows 10 的客戶顯示。 如果您的應用程式支援舊版作業系統，則不適用價格變更。 
>
> - 對於使用 Windows 8 的客戶，應用程式一律以 [**基本價格**] 提供 (而非任何市場特定價格)，即使您排程額外的價格變更。 
> - 對於使用 Windows 8.1 及 Windows Phone 8.1 和較舊版本的客戶，應用程式將一律以客戶市場的初始價格提供，即使您在該市場中排程額外的價格變更。
> 
> 排程價格變更時務必記得這點。 例如，如果一開始以較低價格發行應用程式，然後排程在某一天調高價格，使用舊版作業系統且購買應用程式的客戶會支付較低 (原始) 價格。

按一下 [**排程價格變更**] 查看價格變更選項。 選擇您要使用的價格區間，然後選取日期、時間和時區。

您可以按一下 [**再次排程價格變更**]，依您想要的次數排程後續變更。

> [!NOTE]
> 排程的價格變更與[銷售定價](put-apps-and-add-ons-on-sale.md)運作方式不同。 當您降價促銷應用程式時，檢視您市集清單的客戶會看到價格降低，且他們可以在您選取的期限內以較低的價格購買。 促銷期間結束後，就會恢復基本價格。
>
> 有了排程的價格變更，您就可以調高或調低價格。 變更將在您指定的日期生效，但是不會在市集中顯示為降價促銷；應用程式只會有新的基本價格。 

## <a name="customize-pricing-for-specific-markets"></a>針對特定市場自訂價格

根據預設，您在上方選取的選項將適用於提供應用程式所在的所有市場。 若要自訂特定市場的價格，請選取 [**針對特定市場自訂**]。 [**選擇市場**] 快顯視窗將會出現，列出您選擇提供應用程式的所有市場。 如果您在 [市場] 區段中排除任何市場，這些市場就不會在此處顯示。 

> [!IMPORTANT]
> 使用 Windows 8 的客戶一律會看見應用程式的 [**基本價格**]，即使您針對其市場選取不同價格。

若要針對某一市場新增自訂價格，請選取該市場並按一下 [**儲存**]。 您將會看見相同的 [**基本價格**] 和 [**排程價格變更**] 選項，如上所述，但您所做的選擇會是針對該特定市場。

若要針對多個市場新增自訂價格，您將會建立 [*市場群組*]。 若要這樣做，請選取您想要包括的市場，然後輸入群組的名稱。 (此名稱僅供參考，不會對客戶顯示)。當您完成時，按一下 [**儲存**]。 您將會看見相同的 **\[基本價格\]** 和 **\[排程價格變更\]** 選項，如上所述，但您所做的選擇會是針對該特定市場群組。

> [!NOTE]
> 一個市場不可屬於您在 [價格] 區段中使用的多個市場群組。

若要為其他市場或為其他市場群組新增自訂價格，請再次選取 **\[針對特定市場自訂\]** 並重複這些步驟。 若要變更市場群組中包含的市場，請選取其名稱。 若要移除市場群組 (或個別市場) 的價格，請按一下 **\[移除\]**。


