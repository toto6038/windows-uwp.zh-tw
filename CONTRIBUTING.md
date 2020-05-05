---
ms.openlocfilehash: 5340c8375be9cf33a8f56fdba7bd36e9743767ab
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "80482314"
---
# <a name="contributing-to-uwp-conceptual-documentation"></a>參與 UWP 概念文件編輯

感謝您關注通用 Windows 平台 (UWP) 文件！ 我們非常感謝您對我們文件的意見反應、編輯和新增。

## <a name="writing-content"></a>撰寫內容

我們的文件是使用 Markdown 撰寫的，這是種輕量型的文字樣式語法。 如果您不熟悉 Markdown，可以[在 GitHub 上了解基本知識](https://guides.github.com/features/mastering-markdown/)。 如果有不確定的地方，可隨時複製其他文件頁面的樣式來調整格式。

## <a name="public-contributions"></a>公開投稿

如果您「不」  是 Microsoft 員工，可以透過[公用內容存放庫](https://github.com/MicrosoftDocs/windows-uwp)來投稿。 對於現有頁面的變更和闡明也屬於公開投稿。

### <a name="editing-a-file"></a>編輯檔案

如果目前已位在公用內容存放庫中，請先瀏覽至想要變更的檔案。 請在該處選取顯示內容上方的鉛筆圖示，即可開始編輯。

或者，如果您正在檢視 [docs.microsoft.com](https://docs.microsoft.com) 中的頁面，可以選取頁面右上方部分的 [編輯]  按鈕。 此操作會將您重新導向存放庫中的相關來源檔案。

當您開始編輯時，GitHub 會自動將官方存放庫分為個人 GitHub 帳戶，您可以在其中進行變更。 當您完成時，請將提取要求提交回 **docs** 分支。

### <a name="pull-requests"></a>提取要求

提交提取要求之後，會針對內容品質檢查清單進行評估，以確保其符合我們的基本標準。 如果通過，則會將其指派給 UWP 文件小組的成員，以進行進一步的審查。 如果失敗，會告知您需進行哪些變更。

指派的檢閱者可能會核准或拒絕提取要求，或與您合作以進一步變更。

## <a name="internal-contributions"></a>內部投稿

如果您是 Microsoft 員工，可以透過[私人內容存放庫](https://github.com/microsoftdocs/windows-uwp-pr)來貢獻。 您可以在 [Windows 撰寫指南](https://review.docs.microsoft.com/windows-authoring-guide/uwp/?branch=master)中，取得此存放庫的使用指導方針。 對於即將推出的新功能，其相關文件僅能透過私人存放庫投稿。

### <a name="editing-a-file"></a>編輯檔案

就像公用存放庫一樣，您可以在瀏覽器上對私人存放庫進行少量變更，而不需要建立本機複本。 請務必  確定是在正確的分支上編輯投稿。 如需更多建立個人分支的相關資訊，請參閱 [Windows 撰寫指南中的指示](https://review.docs.microsoft.com/windows-authoring-guide/uwp/conceptual/branches?branch=master)。

### <a name="making-substantial-changes"></a>大幅變更

若要對現有文章進行更多的變更、新增或變更影像，或提供新的文章，請建立私人內容存放庫的本機複本。 如需詳細資訊，請遵循 [Windows 撰寫指南中的指示](https://review.docs.microsoft.com/windows-authoring-guide/uwp/conceptual/)。

### <a name="pull-requests"></a>提取要求

在內部存放庫中建立提取要求時，請確定要將您的個人分支合併至其建立來源的分支。

提交提取要求之後，我們會使用 [PR 合併](https://review.docs.microsoft.com/help/contribute/prmerger-overview?branch=master)進行評估，以確保其符合我們的基本標準。 如果通過，您可以使用 `#sign-off` 註解將其傳送給 UWP 文件小組的成員，以進行進一步的審查。 如果失敗，我們會在您登出前，告知您需進行哪些變更。

指派的檢閱者可能會核准或拒絕提取要求，或與您合作以進一步變更。 在您核准之前，檢閱者不會合併提取要求。

## <a name="using-issues-to-provide-feedback-on-uwp-conceptual-documentation"></a>透過問題提供對 UWP 概念文件的意見反應

如果您想要針對文件提供意見反應，而並非想要自行編輯，請再[在公用存放庫中建立問題](https://github.com/MicrosoftDocs/windows-uwp/issues)。 選取 [問題]  索引標籤，並選取 [新增問題]  按鈕。 請務必附上主題標題和頁面 URL。 您的問題會指派給 UWP 文件小組的成員進行審核。

* 對於內部問題，請使用 [WDG 內容要求工具](http://sesuw2-iis02a/WSCPubRequest/WindowsContentRequestTool.aspx)。
