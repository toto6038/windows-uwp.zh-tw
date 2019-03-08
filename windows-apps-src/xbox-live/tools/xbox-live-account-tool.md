---
title: Xbox Live 帳戶工具
description: 了解如何使用 Xbox Live 帳戶工具來快速建立測試帳戶，來測試您的 Xbox Live 啟用標題。
ms.assetid: ec5959f9-1c60-4aa4-94a6-5d8bdcf77a96
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，測試，測試帳戶
ms.localizationpriority: medium
ms.openlocfilehash: dead4e62e41b7b597ba9a578ee8f174386529937
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632823"
---
# <a name="xbox-live-account-tool"></a>Xbox Live 帳戶工具

## <a name="what-is-xbox-live-account-tool"></a>什麼是 Xbox Live 帳戶工具？
Xbox Live 帳戶工具是專門設計來協助設定現有的開發人員帳戶，以測試遊戲案例的標題開發人員工具。 例如，您可以使用 Xbox Live 帳戶工具來變更為開發人員帳戶的玩家代號、 或快速將 1000年粉絲新增至帳戶的好友名單。

## <a name="what-can-i-do-with-xbox-live-account-tool"></a>我可以使用 Xbox Live 帳戶工具來做什麼？
您可以：
  1. 檢視使用者的設定檔設定、 XUID 和使用中的權限
  2. 加入使用者的社交圖形，從文字檔或 Xbox 開發人員平台 csv 的粉絲清單
  3. 管理使用者的好友名單: 我的最愛，移除最愛，區塊中，並解除封鎖的使用者您遵循，並查看 是否它們遵循您上一步
  4. 變更您的開發人員使用者評價，（並立即查看未經處理的信譽 stat 值）
  5. 變更使用者的玩家代號

## <a name="where-can-i-find-xbox-live-account-tool"></a>哪裡可以找到 Xbox Live 帳戶工具？
Xbox Live 帳戶工具可從 Xbox Live 的工具套件的一部分[ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools)。

## <a name="how-do-i-log-in"></a>如何登入？
您將需要您想要管理，並指定正確的沙箱的使用者認證。 請確定您的開發人員帳戶具有存取權的沙箱，否則可能會失敗的登入。 記住使用沙箱的開發人員帳戶的設計工具。

## <a name="can-i-use-a-retail-account-or-does-it-have-to-be-a-sandboxed-account"></a>我可以使用零售帳戶，或沒有為沙箱化的帳戶嗎？
您當然可以使用 Xbox Live 帳戶工具管理零售帳戶，但並非所有的功能支援。 例如，您無法變更零售使用者的信譽。

## <a name="how-do-i-change-a-dev-users-gamertag"></a>如何變更開發使用者的玩家代號？
瀏覽至 「 玩家 」 索引標籤，然後輸入的玩家代號。 玩家必須只能包含數字、 字母和空格，而且可以是只有 15 個字元。 開發人員帳戶玩家的開頭必須為 2。 目前支援只有一個變更。

## <a name="how-do-i-see-my-block-list"></a>如何查看我的區塊清單？
瀏覽至 「 人 」 索引標籤，然後選取 「 封鎖 」 資料行標頭來排序目前已封鎖的使用者。

## <a name="how-do-i-follow-a-large-group-of-users"></a>我要如何追蹤一大群使用者？
如果您有一份您想要遵循的 XUIDs，請將其複製到文字檔案。 瀏覽至 [跟隨] 索引標籤，選取 「 匯入清單 」，然後選擇您的檔案。 XUIDs 應填入清單檢視中。 按一下 「 認可的變更 」 跟隨著使用者。

## <a name="how-do-i-block-someone"></a>如何封鎖其他人？
瀏覽至 「 人 」 索引標籤，然後選取您想要封鎖的使用者。 按下 「 區塊 」 按鈕，並將批次遭到封鎖。 如果您發現錯誤時，只是稍後再重試。

## <a name="how-do-i-change-my-dev-accounts-repuation"></a>如何變更我的開發人員帳戶 repuation？
瀏覽至 [「 信譽"] 索引標籤。選取您，並按下 「 認可的變更 」 按鈕來提交要求的預設值。 您會看到的值變更，如果要求成功的評價狀態。

## <a name="what-do-the-values-in-the-reputation-tab-mean"></a>評價 索引標籤中的值的意義為何？
整體評價從三個子評價計算：Fairplay （多人連線的管理辦法）、 使用者產生的內容 （視訊剪輯等等） 和 （訊息及語音） 的通訊。 每個類別的原始值範圍可以從 0 為 75，較高表示使用者的信譽是較佳的位置。 OverallStatIsBad 會告訴您的使用者是否具有 「 避免我 」 的評價。

## <a name="whats-the-black-area-at-the-bottom"></a>什麼是底部的黑色區域？
為了讓您持續掌握資訊，我們認為很有用是否偵錯輸出列印在 UI 中。 會讓您知道工具，最多區域，並列印它會遇到任何錯誤。

## <a name="wheres-my-gamerpic"></a>其中是我 gamerpic？
我們知道有些開發人員帳戶不會收到 gamerpic，在帳戶建立時自動產生的錯誤。

## <a name="why-are-things-happening-so-slowly"></a>為什麼會發生件事情慢半拍？
批次作業 （例如新增粉絲），此工具會自動執行以防止大量的要求大小的批次。

## <a name="how-do-i-run-xbox-live-account-tool"></a>如何執行 Xbox Live 帳戶工具呢？
將 Xbox Live SDK 解壓縮至資料夾，然後按兩下 Tools\XboxLiveAccountTool.exe 檔案來執行它。

## <a name="i-have-a-feature-request-how-do-i-get-my-feature-incorporated"></a>我有功能要求 ！ 如何取得我納入的功能？
向您的 Microsoft 代表的任何功能要求，我們會看到我們可以做什麼。
