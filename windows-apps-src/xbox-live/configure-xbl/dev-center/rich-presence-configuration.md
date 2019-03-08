---
title: 在合作夥伴中心內豐富的目前狀態設定
description: 了解如何在合作夥伴中心設定豐富的組態選項的字串
ms.date: 02/27/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live、 Xbox、 遊戲、 uwp、 windows 10，其中一個 Xbox、 豐富的組態選項的字串，合作夥伴中心
ms.openlocfilehash: 5c2add99c6fc6ad1c7f085eb35dc4fdba9e0688d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624123"
---
# <a name="configure-rich-presence-in-partner-center"></a>在合作夥伴中心設定豐富的組態選項

顯示使用者的遊戲內活動的豐富的組態選項的字串。 它們會顯示在播放程式中的玩家代號**朋友 & 梅花**也如同其 Xbox Live 使用者設定檔的清單。 設定的豐富的組態選項字串會附加至遊戲的名稱正在播放。 如果您建立稱為 BubblePop 遊戲，並設定豐富的組態選項字串"Popping 泡泡與朋友 」，您已設定的字串會產生 「 BubblePop-Popping 泡泡，與朋友 」 與狀態。 以下您可以看到豐富的組態選項的字串出現在內容的方式。

在下列螢幕擷取畫面 Xbox Live 使用者**咆哮最後一個可對**並**Lucha Uno**玩遊戲使用豐富的組態選項的字串。

![朋友清單範例](../../images/rich_presence/RichPresence_FriendsList_Screen.jpg)

您可以在下列螢幕擷取畫面中看到**Lucha Uno**完整他的設定檔中的豐富的組態選項字串。

![設定檔範例](../../images/rich_presence/RichPresence_Config_ProfileScreen.jpg)

> [!IMPORTANT]
> 不適用於 Xbox Live 創作者計劃標題的豐富的組態選項的字串，並因此無法進行任何設定這些項目。 這篇文章中的內容為ID@Xbox和管理的夥伴標題。

## <a name="requirements"></a>需求

設定豐富的組態選項的字串之前您和您的標題必須符合下列準則：

- 您必須具有 Windows 開發用的帳戶。
- 您開發的帳戶必須註冊在ID@Xbox程式或受管理的協力廠商開發人員帳戶。
- 您的標題必須註冊在合作夥伴中心，而且已啟用的 Xbox Live。

您可以使用豐富的組態選項的字串之前，您必須在合作夥伴中心內設定。

## <a name="rich-presence-configuration-page"></a>豐富的目前狀態設定 頁面

豐富的組態選項的字串會設定為您的標題，在 Xbox Live 服務的一部分[合作夥伴中心](https://partner.microsoft.com/dashboard)。

瀏覽至 [豐富的組態選項設定] 頁面，使用下列指示：

1. 移至[合作夥伴中心](https://partner.microsoft.com/dashboard)developer.microsoft.com 上。
2. 如果要求登入，請登入您已註冊的 Windows 開發人員帳戶。
3. 選擇您的 Xbox Live 啟用標題或從應用程式**概觀**頁面。 請勿選取創作者計劃標題，因為它不會啟用豐富的組態選項字串組態。
4. 按一下  **Services**下拉式清單，然後選取 Xbox Live。
5. 向下捲動至**豐富存在**連結，然後按一下它。

豐富的組態選項 頁面會顯示服務時，按鈕，以建立新的豐富的組態選項字串，並可搜尋的一份您先前設定的字串的簡短描述。 您可以從這個頁面設定新的字串，以及編輯和檢閱您已設定的字串。

![豐富的組態選項字串設定 頁面的範例](../../images/rich_presence/RichPresence_ConfigPage_New.JPG)

> [!NOTE]
> 字串，例如 「 遊戲 Net 執行器-Moon 10 人使用的基底上的多人遊戲生死。 」 將無法使用資料平台 2017年更新的開發人員。 資料平台 2013年*變數*資料平台 2017年中無法使用。 變數會在此情況下是數目會終止"10"。 對等的字串後的資料平台 2017年更新 「 遊戲 Net 執行器-Moon 基底上的多人遊戲生死。 」 「 播放 Net 執行器-功能表中的 」 仍是有效的豐富的組態選項字串。

## <a name="create-a-new-rich-presence-string"></a>建立新的豐富的組態選項字串

若要建立豐富的組態選項的字串，請按一下  按鈕**新的豐富是否存在字串**。 您會看到 UI 來填寫**組態選項的詳細資料**包括**豐富的目前狀態的唯一識別碼**，以及**顯示字串**為新的豐富的組態選項字串。

![新的豐富的組態選項字串 UI](../../images/rich_presence/RichPresence_Config_NewString.JPG)

**豐富的目前狀態的唯一識別碼**-唯一的豐富的目前狀態識別碼是用來識別您豐富的組態選項的字串的字串。 此字串會用來設定您的遊戲玩家的狀態，並與您想要顯示的特定字串相關聯。 您的識別碼最多可有 50 個字元。

**顯示字串**-顯示的字串是您會想要顯示附加到的一些玩家玩遊戲狀態的字串。 這是您將在其中填入豐富是否存在字串中您想顯示產生您的遊戲的興趣。 您的顯示最多可有 100 個字元，但會有執行個體將會顯示前 40 個字元。

若要建立新的豐富的組態選項字串，填入欄位，然後按下**儲存** 按鈕。
一旦您按一下 [的儲存您會前往上一步，您會看到新的豐富的組態選項字串新增至設定的字串清單的豐富存在組態] 頁面。

## <a name="review-edit-and-delete-strings"></a>檢閱、 編輯和刪除字串

這裡您會看到幾個設定的字串與豐富的組態選項組態頁面。
![豐富的目前狀態的頁面設定範例](../../images/rich_presence/RichPresence_ConfigPage_Configured.JPG)

若要檢閱先前建立的字串，只需瀏覽豐富的組態選項的 [設定] 頁面上的清單。 那里您所見的唯一識別碼豐富，」 和 「 顯示字串在一起。 當您需要在項目的程式碼中使用唯一的豐富的目前狀態識別碼，來指定豐富的組態選項的字串時，這會很有用。

若要編輯豐富的組態選項，只要按一下字串**豐富的目前狀態的唯一識別碼**您想要編輯字串 連結。 您將會進入相同的 UI，用來建立新的豐富的組態選項字串以目前的字串設定為編輯填入。 按一下 [編輯後**儲存**] 按鈕，以您的變更更新設定的字串。

若要刪除設定的豐富的組態選項字串按一下**刪除**相同的資料列做為您想要刪除的豐富是否存在字串中的豐富存在組態 頁面上的連結。 系統會要求您確認刪除。

## <a name="further-reading"></a>進階閱讀

若要取得更多關於豐富的組態選項字串特徵，以及如何實作它們的深入概念性資訊，請閱讀[豐富的組態選項的文件](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/social-platform/rich-presence-strings/rich-presence-strings-overview)。