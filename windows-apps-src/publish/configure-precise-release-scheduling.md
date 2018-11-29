---
Description: You can set the precise date and time that your app should become available in the Store, giving you greater flexibility and the ability to customize dates for different markets.
title: 設定精確發行時間表
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 排程, 發行日期, 日期, 推出
ms.localizationpriority: medium
ms.openlocfilehash: a1477a426a9cdf240e694efb19bd7521fcd734cb
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "7992967"
---
# <a name="configure-precise-release-scheduling"></a>設定精確發行時間表

[定價和可用性](set-app-pricing-and-availability.md)頁面上的 **\[排程\]** 區段可讓您設定應用程式應在Microsoft Store中推出的精確日期和時間，讓您擁有更大的彈性和能力針對不同市場自訂日期。

> [!NOTE]
> 雖然本主題是關於應用程式，但是附加元件提交的發行時間表使用相同的程序。

此外，您可以選擇設定產品不再於Microsoft Store中提供的日期。 請注意，這表示不再能透過搜尋或瀏覽的方式在Microsoft Store中找到產品，但擁有直接連結的客戶仍看得見產品的Microsoft Store清單。 如果他們已擁有產品，或是有[促銷碼](generate-promotional-codes.md)且使用 Windows 10 裝置，才能進行下載。

根據預設，(除非您已選取[可見度](choose-visibility-options.md#discoverability)區段的其中一個 [**在Microsoft Store推出此應用程式，但不供搜尋**] 選項)，您的應用程式將在通過認證並完成發行程序後，立即對客戶提供。 若要選擇其他日期，請選取 [**顯示選項**] 展開這個區段。

請注意，您無法在 [**排程**] 區段中設定日期，如果您已選取[可見度](choose-visibility-options.md#discoverability)區段的其中一個 [**在Microsoft Store推出此應用程式，但不供搜尋**] 選項，因為您的應用程式不會對客戶發行，因此沒有發行日期可設定。

> [!IMPORTANT]
> 您在 [排程] 區段中指定的日期只適用於使用 Windows 10 的客戶。
>
>如果您先前發佈的應用程式支援舊版作業系統，您選取任何**停止取得**日期將不會套用到這些客戶;他們仍然能夠取得應用程式 （除非您提交新的選取範圍中 [[可見度](choose-visibility-options.md#discoverability)] 區段中，更新，或從**應用程式概觀**頁面選取 [**讓應用程式無法使用**）。


## <a name="base-schedule"></a>基本時間表

您針對 [基本時間表] 選取的項目將套用到提供應用程式的所有市場，除非您之後選取[針對特定市場自訂](#customize-the-schedule-for-specific-markets)，針對特定市場 (或市場群組) 新增日期。

您將會看到這兩個選項︰[**發行**] 和 [**停止取得**]。 

## <a name="release"></a>發行

在 [**發行**] 下拉式清單中，您可以設定要在Microsoft Store中提供應用程式的時間。 這表示，可在Microsoft Store中透過搜尋或瀏覽的方式搜尋應用程式，且客戶可以檢視其Microsoft Store清單並取得應用程式。

>[!NOTE]
> 您的應用程式已發行並且在Microsoft Store中提供之後，您就無法再選取 [**發行**] 日期 (因為應用程式已發行)。

以下是您可針對產品的 [**發行**] 排程設定的選項：
- **儘快**：產品將在通過認證及發佈後儘快發行。 此為預設選項。
- **於**：產品將在您選取的日期和時間發行。 您還有另外兩個選項：
   - **UTC**︰您選取的時間會是全球定位時間 (UTC)，如此應用程式在哪裡都會於相同時間發行。
   - **當地**︰您選取的時間將在與市場相關的每一個時區中使用。 (請注意，對於包含多個時區的市場，只會使用該市場中的一個時區。 若是美國，則會使用東岸時區)。
- **未排程**：應用程式將不會在Microsoft Store中提供。 如果您選擇此選項，稍後可以透過建立新的提交項目並選擇其中一個其他選項在Microsoft Store中提供應用程式。


## <a name="stop-acquisition"></a>停止取得

在 [**停止取得**] 下拉式清單中，您可以設定要停止新客戶從Microsoft Store取得應用程式或探索其清單的日期和時間。 如果您想要精確控制不再對新客戶提供應用程式的時間，例如您正在協調多個應用程式的可用性時，這會很實用。

根據預設，[**停止取得**] 設定為永不。 若要變更此項，請在下拉式清單中選取 [**於**]，然後指定日期和時間，如上所述。 在您選取的日期和時間，客戶將不再能取得應用程式。

請務必了解此選項會有相同的影響，和[可見度](choose-visibility-options.md#discoverability)\] 區段中選取 [**可搜尋的但無法使用推出此應用程式**，並選擇**停止取得： 擁有直接連結的任何客戶可以看到產品的市集清單，但它們只可以下載它已擁有該產品，或具備促銷碼而且使用 Windows 10 裝置。** 若要完全停止為新客戶提供某個應用程式，請按一下 [應用程式概觀] 頁面的 [**停止提供應用程式**]。 如需詳細資訊，請參閱[從Microsoft Store移除應用程式](guidance-for-app-package-management.md#removing-an-app-from-the-store)。

> [!TIP]
> 如果您選取 [**停止取得**] 的日期，且之後決定要再次提供應用程式，您可以建立新的提交項目，並將 [**停止取得**] 變更回 [**永不**]。 在您將更新的提交項目發行後，應用程式將再次提供。

## <a name="customize-the-schedule-for-specific-markets"></a>自訂特定市場的排程 

根據預設，您在上方選取的選項將適用於提供應用程式所在的所有市場。 若要自訂特定市場的價格，請按一下 [**針對特定市場自訂**]。 **\[選擇市場\]** 快顯視窗將會出現，列出您選擇提供應用程式的所有市場。 如果您在[市場](define-pricing-and-market-selection.md)區段中排除任何市場，這些市場就不會顯示。 

若要新增某一個市場的排程，請選取該市場並按一下 [**儲存**]。 然後您會看到與上述相同的 [**發行**] 和 [**停止取得**] 選項，但您所做的選擇只會套用至該市場。

若要新增套用至多個市場的排程，您會建立 [*市場群組*]。 若要這樣做，請選取您想要包括的市場，然後輸入群組的名稱。 (此名稱僅供參考，不會對客戶顯示)。例如，如果您想要建立北美地區的市場群組，可以選取 [**加拿大**]、[**墨西哥**] 和 [**美國**]，並將它命名為 [**北美**] 或您選擇的其他名稱。 當您完成建立市場群組時，按一下 [**儲存**]。 然後您會看到與上述相同的 [**發行**] 和 [**停止取得**] 選項，但您所做的選擇只會套用至該市場群組。

若要為其他市場或為其他市場群組新增自訂排程，只要再次按一下 [**針對特定市場自訂**] 並重複這些步驟即可。 若要變更市場群組中包含的市場，請選取其名稱。 若要移除市場群組 (或個別市場) 的自訂排程，請按一下 **\[移除\]**。

> [!NOTE]
> 一個市場不可屬於您在 **\[排程\]** 區段中使用的多個市場群組。 










