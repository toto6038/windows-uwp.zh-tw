---
title: EDS 授權
assetID: bd0bdc8e-084a-7140-98da-6d3721bda112
permalink: en-us/docs/xboxlive/rest/edsauthorization.html
description: " EDS 授權"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3e5c5ef3bf3c864215544391bc291a26f6c05d0f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607603"
---
# <a name="eds-authorization"></a>EDS 授權
 
  * [簡介](#ID4EN)
  * [授權程序](#ID4EFB)
  * [3.0 權杖：多使用者與單一使用者](#ID4EEC)
  * [EDS 是否支援多使用者？](#ID4EYC)
 
<a id="ID4EN"></a>

 
## <a name="introduction"></a>簡介
 
娛樂探索服務 (EDS) 3.1 不支援匿名流量。 需要 EDS 的所有要求驗證。 EDS 需要 XToken 從正確地驗證用戶端的呼叫端。 這些語彙基元 XSTS 所產生，而且可以取得各種 Xbox 驗證服務 (XAS)。 有不同的驗證服務的裝置、 使用者和項目會將所有定義的權杖的身分識別。
 
XSTS 是 Xbox LIVE 的閘道管理員。 它是 defense 來判斷使用者或裝置是否有權連線到任何 Xbox LIVE 服務的第一行。 驗證使用者之後, XSTS 會產生可用來安全地在服務上的任何元件辨識自己 XToken。 此 XToken 是您的 passport 存留。
 
人員和項目，想要使用我們的服務。 而我們希望大部分的這些項目和人員，也可以使用我們的服務。 但是，如何執行我們請確定項目不偽裝成人員，而且，人實際上他們所聲稱的人嗎？ 我們提供他們確認其身分識別給其他人可以使用語彙基元。
 
這些權杖是由產生 XSTS 和通常稱為 XTokens。 XToken 是一個廣義的詞彙是用來涵蓋語彙基元，其包含各種不同的項目，並且可以有許多不同的形式，但它們所有建立、 登入，並選擇性地加密 XSTS 伺服器。 就內部而言，XSTS 使用 MXAN 建立和格式化語彙基元。 MXAN 是唯一的元件，曾經從 XToken 中擷取資訊。 使用權杖的服務會將要求標頭傳遞至 MXAN 被破解。 處理和驗證權杖，以表示權杖的宣告會傳回至服務。 服務可接著使用這些宣告識別呼叫的使用者或裝置，並根據該資訊執行動作的值。
 
基本的身分識別權杖-使用者、 裝置，以及標題-提供 Xbox 驗證服務 (XAS)。 每個 XAS 負責產生指定值，他們會負責的各種宣告的身分識別權杖。
 
   * XASD (XAS 裝置): 建立可提供裝置身分識別 DToken
   * XASU (XAS 使用者): 建立 UToken 提供的使用者身分識別
   * XAST (XAS 項目): 建立 TToken 提供標題身分識別
   
<a id="ID4EFB"></a>

 
## <a name="authorization-process"></a>授權程序
 
   * 取得一或多個身分識別權杖。 您可以要求 XToken 與最多一個每個 D、 U 和 T 的語彙基元。 您必須提供至少其中 D 或 u。 
     * 要求從 XASD DToken，藉由提供裝置驗證詳細資料
     * 要求 UToken 從 XASU 使用某種形式的使用者驗證。 可能的 MSA (RPS) 權杖的形式提供使用者驗證。
     * 要求 XAST TToken。 可用的項目取決於目前正在執行，因此您必須提供要 XAST DToken 以及平台。
  
   * 建立 XSTS 要求。
 
     * 定義您所要求的權杖的信賴憑證者合作對象。
     * 填入 D、 U 和/或 T 權杖的要求屬性。
    * 執行 XSTS 要求並快取產生 XToken。 傳回的 XToken （最多） 包含的所有裝置、 使用者和標題身分識別資訊從身分識別權杖和任何額外的宣告 （目前的訂用帳戶狀態、 使用者群組）。
   
<a id="ID4EEC"></a>

 
## <a name="30-tokens-multiuser-vs-single-user"></a>3.0 權杖：多使用者與單一使用者
 
這是 3.0 版權杖的形式： `XBL3.0 x=&lt;hash>;&lt;token>`
 
取決於&lt;雜湊 >，權杖會以不同方式處理：
 
   * 如果&lt;雜湊 > 相當於 * （星號），則任何特定的使用者執行要求，並在權杖中的所有使用者都都出現在已還原序列化的主體。 這是的則為 true 的多使用者表單。
   * 如果&lt;雜湊 > 等於-(dash)，則任何使用者不執行要求。 移除已還原序列化的主體中的所有使用者。
   * 如果&lt;雜湊 > 不等於 * 或-則表示 在權杖中的使用者提出要求的識別碼。 只有指定的使用者才會出現在已還原序列化的主體。 移除所有其他使用者。這是單一使用者 3.0 語彙基元。
   
<a id="ID4EYC"></a>

 
## <a name="does-eds-support-multi-users"></a>EDS 是否支援多使用者？
 * 答案是沒有。 在案例中所述，在主控台仍會一律傳送單一的使用者語彙基元。 即使有多位使用者登入，必須是指定 「 呼叫者 」，會在卸除所有其他身分識別。
  
<a id="ID4E6C"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EBD"></a>

 
##### <a name="parent"></a>父系  

[其他參考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4END"></a>

 
##### <a name="further-information"></a>進一步的資訊 

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)

   