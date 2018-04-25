---
author: jnHs
Description: Manage submission options such as publishing hold options, notes for certification, and more.
title: 管理提交選項
ms.author: wdg-dev-content
ms.date: 04/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 發佈暫停, 發佈日期, 傳送提交至發佈
ms.localizationpriority: high
ms.openlocfilehash: 4f18a4cae88f78b16ea43856cc6166c67a228c2c
ms.sourcegitcommit: 9f059b37e45099b4314c96a0b604449e966d3c3c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/04/2018
---
# <a name="manage-submission-options"></a>管理提交選項

應用程式提交程序的 **\[提交選項\]** 頁面可供您提供更多資訊，以協助我們正確測試您的產品。 這是選用步驟，但對許多提交都是建議項目。 如果您想要延遲發佈程序，也可以選擇設定發佈暫停選項。


## <a name="publishing-hold-options"></a>發佈暫停選項

根據預設，您的提交一通過認證我們就會立即發佈 (或根據您在 **\[定價和可用性\]** 頁面的 [\[排程\]](configure-precise-release-scheduling.md) 區段中所指定的日期)。 您也可以選擇暫停發佈您的提交直到特定日期，或直到您以手動方式指示發佈為止。 本區段中的選項說明如下。


### <a name="publish-your-submission-as-soon-as-it-passes-certification-or-per-dates-you-specify"></a>通過認證之後就立即發佈此提交項目 (或根據您指定的日期)

**\[通過認證之後就立即發佈此提交項目 (或根據您在 \[排程\] 區段中選取的日期)\]** 是預設選項，表示一旦您的提交通過認證，就會開始進入發佈程序，除非您在 **\[定價和可用性\]** 頁面的 [\[排程\]](configure-precise-release-scheduling.md) 區段中有設定日期。   

對於大部分提交，我們建議您將 **\[發佈暫停選項\] ** 區段設定為這個選項。 如果您想為您的提交指定特定的發佈日期，請使用 **\[通過認證之後就立即發佈此提交項目 (或根據您在 \[排程\] 區段中選取的日期)\]**。 將此區段保留為預設選項，不會讓提交的發佈日期早於您在 **\[排程\]** 區段中設定的日期。 您在 **\[排程\]** 區段中選取的日期，將用於決定您的應用程式在 Microsoft Store 中對客戶提供的時間。


### <a name="publish-your-submission-manually"></a>以手動方式發佈您的提交

如果您還不想設定發行日期，並且希望提交項目維持未發佈狀態，直到您手動決定開始發佈程序，您可以選擇 **\[不要發佈此提交項目，直到我選取 [立即發佈]\]**。 選擇此選項表示您的選取項目不會發行，直到您指示其發行。 應用程式通過認證後，您可以在認證狀態頁面上選取 **\[立即發佈\]** 將它發佈，或是以如下所述的相同方式選取特定日期。


### <a name="start-publishing-your-submission-on-a-certain-date"></a>在特定日期開始發佈您的提交項目

選擇 **\[在以下日期開始發佈此提交項目\]** 可確保提交項目直到某個日期才會發佈。 使用此選項，您的提交項目會在指定日期的當天或之後立即發行。 此日期至少必須是未來的 24 小時後。 除了日期，您還能指定應開始發佈提交項目的時間。 

您也可以在提交應用程式之後變更此發行日期，只要還沒有進入 \[發佈\] 步驟即可。 
 
如上所述，如果您想為提交指定特定的發佈日期，請使用 **\[通過認證之後就立即發佈此提交項目 (或根據您在 \[排程\] 區段中選取的日期)\]**，並將 **\[發佈暫停選項\]** 設定保留為預設選項。 使用 **\[在以下日期開始發佈此提交項目\]** 選項表示您的提交在該日期之前都不會開始發佈程序，但延遲認證或發佈可能會造成實際發行日期晚於您選取的日期。 


## <a name="notes-for-certification"></a>認證注意事項

在您提交 app 時，可以選擇使用**認證注意事項**頁面，為認證測試人員提供額外的資訊。 這些資訊有助於確保正確測試您的應用程式。 

如需詳細資訊，請參閱[認證注意事項](notes-for-certification.md)。

