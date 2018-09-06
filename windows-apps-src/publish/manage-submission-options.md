---
author: jnHs
Description: Manage submission options such as publishing hold options, notes for certification, and more.
title: 管理提交選項
ms.author: wdg-dev-content
ms.date: 05/09/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 發佈暫停, 發佈日期, 傳送提交以發佈, 受限制的功能核准
ms.localizationpriority: medium
ms.openlocfilehash: 147f34c40cc5d2b612dcdd92edc0c76340cf58f7
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2018
ms.locfileid: "3401850"
---
# <a name="manage-submission-options"></a>管理提交選項

應用程式提交程序的 **\[提交選項\]** 頁面可供您提供更多資訊，以協助我們正確測試您的產品。 這是選用步驟，但對許多提交都是建議項目。 如果您想要延遲發佈程序，也可以選擇設定發佈暫停選項。


## <a name="publishing-hold-options"></a>發佈暫停選項

根據預設，您的提交一通過認證我們就會立即發佈 (或根據您在 **\[定價和可用性\]** 頁面的 [\[排程\]](configure-precise-release-scheduling.md) 區段中所指定的日期)。 您也可以選擇暫停發佈您的提交直到特定日期，或直到您以手動方式指示發佈為止。 本區段中的選項說明如下。 


### <a name="publish-your-submission-as-soon-as-it-passes-certification-or-per-dates-you-specify"></a>通過認證之後就立即發佈此提交項目 (或根據您指定的日期)

**\[通過認證之後就立即發佈此提交項目 (或根據您在 \[排程\] 區段中選取的日期)\]** 是預設選項，表示一旦您的提交通過認證，就會開始進入發佈程序，除非您在 **\[定價和可用性\]** 頁面的 [\[排程\]](configure-precise-release-scheduling.md) 區段中有設定日期。   

對於大部分提交，我們建議您將 **\[發佈暫停選項\] ** 區段設定為這個選項。 如果您想為您的提交指定特定的發佈日期，請使用 **\[通過認證之後就立即發佈此提交項目 (或根據您在 \[排程\] 區段中選取的日期)\]**。 將此區段保留為預設選項，不會讓提交的發佈日期早於您在 **\[排程\]** 區段中設定的日期。 您在 [**排程**\] 區段中選取的日期將會用來判斷您的產品給客戶在市集中供使用時。


### <a name="publish-your-submission-manually"></a>以手動方式發佈您的提交

如果您還不想設定發行日期，並且希望提交項目維持未發佈狀態，直到您手動決定開始發佈程序，您可以選擇 **\[不要發佈此提交項目，直到我選取 [立即發佈]\]**。 選擇此選項表示您的提交項目不會發行，直到您指示其發行。 在您的提交通過認證之後, 您可以將它發行在認證狀態頁面上，選取 [**立即發佈**，或以相同的方式選取特定日期，如下所述。


### <a name="start-publishing-your-submission-on-a-certain-date"></a>在特定日期開始發佈您的提交項目

選擇 **\[在以下日期開始發佈此提交項目\]** 可確保提交項目直到某個日期才會發佈。 使用此選項，您的提交項目會在指定日期的當天或之後立即發行。 此日期至少必須是未來的 24 小時後。 除了日期，您還能指定應開始發佈提交項目的時間。 

您可以變更此發行日期，在提交您的產品之後，只要它還沒有進入發佈 \] 步驟即可。 
 
如上所述，如果您想為提交指定特定的發佈日期，請使用 **\[通過認證之後就立即發佈此提交項目 (或根據您在 \[排程\] 區段中選取的日期)\]**，並將 **\[發佈暫停選項\]** 設定保留為預設選項。 使用 **\[在以下日期開始發佈此提交項目\]** 選項表示您的提交在該日期之前都不會開始發佈程序，但延遲認證或發佈可能會造成實際發行日期晚於您選取的日期。 


## <a name="notes-for-certification"></a>認證注意事項

在您提交 app 時，可以選擇使用**\[認證注意事項\]** 區段，為認證測試人員提供額外的資訊。 這些資訊有助於確保正確測試您的應用程式。 

如需詳細資訊，請參閱[認證注意事項](notes-for-certification.md)。


## <a name="restricted-capabilities"></a>受限制的功能

如果我們偵測到您的套件宣告任何[受限制的功能](../packaging/app-capability-declarations.md#restricted-capabilities)，您必須提供本區段中的資訊以接收核准。 針對每個功能，請告訴我們為何您的應用程式需要宣告此功能，以及如何使用它。 請務必提供詳細資料以協助我們了解您的產品需要宣告功能的原因。 

認證過程中，我們的測試人員將會檢閱您所提供的資訊，以判斷您的提交是否已核准使用功能。 請注意，提交可能需要更多時間以完成認證程序。 如果我們核准您使用功能，您的應用程式將繼續進行認證程序的其餘部分。 當您提交應用程式更新，通常不會重複功能核准程序（除非您宣告其他功能）。 

如果我們不核准您使用功能，您的提交無法通過認證，而且我們將會在認證報告中提供意見反應。 然後，您可以選擇建立新的提交和上傳不宣告功能的套件，或 (如果適用) 解決有關您使用功能的任何問題，並在新提交中要求核准。

請注意，有一些受限制的功能很少獲得核准。 如需受限功能的詳細資訊，請參閱[應用程式功能宣告](../packaging/app-capability-declarations.md#restricted-capabilities)。

