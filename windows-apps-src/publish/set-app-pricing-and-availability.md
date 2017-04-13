---
author: jnHs
Description: "您可以從應用程式提交程序的 [價格與可用性] 頁面決定您的應用程式價格、您是否提供免費試用，以及客戶如何、何時、何處可取得您的應用程式。"
title: "設定應用程式價格與可用性"
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: cf781345d0fe089f779db9b42fcf9eb56f229028
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="set-app-pricing-and-availability"></a>設定應用程式價格與可用性


您可以從 [app 提交程序](app-submissions.md)的 **\[價格與可用性\]** 頁面決定您的 app 價格、您是否提供免費試用，以及客戶如何、何時、何處可取得您的 app。 我們將在這裡逐步解說此頁面上的選項，以及輸入這項資訊時所應考慮的事項。

## <a name="base-price"></a>基本價格


此頁面上的第一個項目可讓您選取 app 的基本價格。 您可以選擇免費提供，或者選取其中一個適用的價格區間。 您必須指定基本價格，才能提交 app。

如需詳細資訊，請參閱[制定價格和選擇市場](define-pricing-and-market-selection.md)。

## <a name="free-trial"></a>免費試用


許多開發人員選擇使用市集提供的試用功能，讓客戶免費試用 app。 根據預設，app 無法免費試用，但如果您願意提供，請從 **\[免費試用\]** 下拉式清單選取值。

選擇 [**試用版永久有效**]，讓客戶無限期免費存取您的 app。 您會想要鼓勵他們購買完整版本，所以請務必新增程式碼以[排除或限制試用版中的功能](../monetize/in-app-purchases-and-trials.md)。

您也可以選取 [**1 天**]、[**7 天**]、[**15 天**] 或 [**30 天**] 的時間限定試用版的選項。 您在試用期間仍然可以限制功能，或者可以讓客戶在該時間期間存取完整功能。

> **注意**  Windows Phone 8.1 和較舊版本不會對客戶顯示限時試用。

## <a name="markets-and-custom-prices"></a>市場和自訂價格


根據預設，您的 app 會以基本價格列在所有可能的市場中。 您可以在 **\[市場和自訂價格\]** 區段中變更這些設定以便納入或排除特定市場，以及變更任何販售市場中的 app 價格。 如需詳細資訊，請參閱[制定價格和選擇市場](define-pricing-and-market-selection.md)。

## <a name="sale-pricing"></a>銷售定價


如果您想要以降低的價格提供 App 一段有限的時間，您可以建立及排程銷售。 如需詳細資訊，請參閱[降價銷售應用程式與附加內容](put-apps-and-add-ons-on-sale.md)。

## <a name="distribution-and-visibility"></a>配送和可見性


**\[配送和可見性\]** 區段可讓您設定探索及取得您 app 之方式的限制。

預設設定為 **\[在市集推出此 app\]**。 這表示您的 app 將列於市集中，讓客戶能夠透過 app 的直接連結和/或其他方法來尋找，包括搜尋、瀏覽和包含於經過挑選的清單中。

如果您想要在市集中隱藏您的 app，但仍要讓特定人員能夠使用它，請選取下列其中一個選項來限制 app 的可用性。 請注意，如果您選擇這其中一個選項，Windows 8 和 Windows 8.1 的客戶將無法取得該 app。

-   **隱藏這個 app 並避免購買。具備促銷碼的客戶仍然能夠在 Windows 10 裝置上下載該 app**：沒有客戶可以在市集中透過搜尋或瀏覽找到您的 app，但是您可以[產生促銷碼](generate-promotional-codes.md)以配送給 Windows 10 上的特定人員。 他們可以使用連結和代碼以免費取得您的 app，即使您並未將該 app 提供給其他任何客戶也一樣。
-   **在市集中隱藏這個 app。具有 app 清單直接連結的客戶仍能下載該 app，但在 Windows 8 與 Windows 8.1 上除外**：沒有客戶可以在市集中透過搜尋或瀏覽找到您的 app，但是具有 app 清單直接連結的任何客戶可以在執行 Windows 10 或 Windows Phone 8.1 和較舊版本的裝置上下載您的 app。
-   **隱藏這個 app，而且只有您在下方指定的人員可以使用它，這類人員可以在 Windows Phone 8.x 裝置上下載這個 app。促銷碼可用來在 Windows 10 裝置上下載這個 app**：沒有客戶可以在市集中透過搜尋或瀏覽找到您的 app，只有您在方塊中 (使用分號分隔) 輸入其電子郵件地址 (與其 Microsoft 帳戶相關聯) 的 Windows Phone 8.x 客戶可以使用直接連至其清單的連結來下載您的 app。 您也可以[產生促銷碼](generate-promotional-codes.md)以配送給 Windows 10 上的特定人員。 這個選項通常用於 Windows Phone 8.1 和更舊版本的[搶鮮版 (Beta) 測試](beta-testing-and-targeted-distribution.md)。 請注意，只有當您先前發行 app 時，未將 **\[配送和可見性\]** 選項設定為 **\[任何人都可以在市集中找到您的 app\]**，才能夠選取此選項。

> **注意**  若要完全停止為新客戶提供某個 app，請按一下 \[App 概觀\] 頁面的 **\[停止提供 app\]**。 在確認您想要停止提供該 app 之後，在數小時內，將無法在市集中看見該 app，而所有的新客戶都將無法透過任何方法來取得它。 這個動作將覆寫您在此處選擇的所有選項：新客戶將完全無法取得它。 若要再次為新客戶提供該 app，可隨時按一下 [App 概觀] 頁面的 **\[提供 app\]**。 如需詳細資訊，請參閱[從市集移除 app](guidance-for-app-package-management.md#removing-an-app-from-the-store)。

## <a name="windows-10-device-families"></a>Windows 10 裝置系列

裝置系列可用性現在已在您提交的 **\[套件\]** 頁面上管理。 如需詳細資訊，請參閱[裝置系列可用性](upload-app-packages.md#device-family-availability)。

## <a name="organizational-licensing"></a>組織授權


根據預設，您的 app 可能會提供給組織大量購買。 您可以指出是否在此區段提供您的 app 以及其方式。

如需詳細資訊，請參閱[組織授權選項](organizational-licensing.md)。

## <a name="publish-date"></a>發佈日期


您可以在 **\[發佈日期\]** 區段中選擇選項，指出何時要發佈您的 app (或更新)。

-   選擇 **\[通過認證之後就立即發佈此提交項目\]** 可讓此提交項目在市集中以最快方式取得。
-   如果您希望提交項目要到您明確指定時才發佈，請選擇 **\[手動發佈此提交項目\]**。 您可以從認證狀態頁面按一下 **\[立即發佈\]** 或依照下面描述選取特定日期進行手動發佈。
-   選擇 [**不早於 \[日期\]**] 可確保提交項目要到某個日期之後才會發佈。 使用此選項，您的提交項目會在指定日期的當天或之後立即發行。 此日期至少必須是未來的 24 小時後。 除了日期，您還能指定應開始發佈提交項目的時間。

   > **注意**  認證或發行期間的延遲會造成實際的發行日期晚於您要求的日期。 Windows 市集無法保證將會在特定日期推出您的 app (或更新)。

您也可以在提交 app 之後變更發行日期，只要還沒有進入 **\[發佈\]** 步驟即可。
 

 
