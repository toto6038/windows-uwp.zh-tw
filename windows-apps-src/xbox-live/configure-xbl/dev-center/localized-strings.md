---
title: 當地語系化字串
description: 了解如何在合作夥伴中心內的字串當地語系化
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 11/17/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live、 Xbox、 遊戲、 uwp、 windows 10、windows Xbox，當地語系化的字串，合作夥伴中心
ms.openlocfilehash: 127f566dc5ae57b920d396623b6a84ff5d5eed96
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656593"
---
# <a name="configuring-localized-strings-in-partner-center"></a>在合作夥伴中心內設定的當地語系化字串

若要將所有您 Xbox Live 設定您的遊戲所支援的所有語言的當地語系化，您可以使用此頁面。 所有您已建立在任何後續的 Xbox Live 頁面的服務組態會加入您可以下載的檔案。

您可以使用[合作夥伴中心](https://partner.microsoft.com/dashboard)設定您的遊戲與相關聯的所有語言的當地語系化的字串。 新增組態，執行下列動作：

1. 瀏覽至**當地語系化字串**一節以取得您的標題，位於**Services** > **Xbox Live** > **當地語系化字串**。
2. 按一下 **下載**下載 localization.xml 檔案在本機電腦上的按鈕。

![在合作夥伴中心內的當地語系化的字串組態 頁面的螢幕擷取畫面](../../images/dev-center/localized-strings/localized-strings-1.png)

3. 您可以藉由複製加入當地語系化的字串 <Value locale="en-US">Mazes 播放</Value> 標記和地區設定的值變更為您選擇的語言和當地語系化的字串值。 您必須至少一個值的標記內的開發人員顯示地區設定，以避免發生錯誤。

![編輯當地語系化的字串](../../images/dev-center/localized-strings/localized-strings.gif)

4. 一旦您加入的所有當地語系化的字串，您可以藉由拖曳，或瀏覽您的本機電腦上傳檔案。

![Localization.xml 檔案上傳按鈕的影像](../../images/dev-center/localized-strings/localized-strings-2.png)

請注意當您上傳 localization.xml 檔案時，可能會出現下列錯誤：

| 錯誤 | 原因 |
|---------------------------|-------------|
| 失敗的 XSD 驗證：命名空間中的項目 'LocalizedString' 'http://config.mgt.xboxlive.com/schema/localization/1' 不能包含文字。 可能需要的元素清單：命名空間中的 'value''http://config.mgt.xboxlive.com/schema/localization/1' | 格式不正確的 XML 文件時，發生這種的情況 |
| 遺漏的開發人員顯示地區設定的項目當地語系化字串。 | 這發生在遺漏其地區設定不符合開發人員顯示地區設定的項目 」 的當地語系化的字串。 |
| 失敗的 XSD 驗證：'地區設定' 屬性無效-值 ' '是根據其資料類型無效'http://config.mgt.xboxlive.com/schema/localization/1:NonEmptyString'-失敗條件約束的模式。 | 這發生在遺漏地區設定值，在當地語系化的字串。 <Value> tag|
