---
title: 在合作夥伴中心中設定單一登入
description: 描述如何設定單一登入合作夥伴中心，以允許使用者登入您的服務，使用 Xbox Live ID 的標題
ms.assetid: ''
ms.date: 02/21/2018
ms.topic: article
ms.localizationpriority: medium
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 udc、 通用的開發人員中心、 單一登入
ms.openlocfilehash: 32f06edd407d8c1fa74795d0a230c7d56ba8e838
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632873"
---
# <a name="configure-single-sign-on-in-partner-center"></a>在合作夥伴中心中設定單一登入

單一登入可讓播放程式來登入您的服務，使用 Xbox Live 登入使用您的標題。 這可以讓播放程式帶正負號到 Xbox Live 而不需要登入第二次使用不同的帳戶認證以服務的特定執行的應用程式或遊戲，為您的服務。

> [!NOTE]
> 本主題不適用於 Xbox Live 創作者計劃中的項目。

比方說，您的標題可能是應用程式，可讓您的服務，將內容串流 （視訊、 音樂等） 至其裝置，只要他們具備有效的帳戶，與您的服務。 如果使用者已登入他們的 Xbox Live 帳戶，他們應該可以將內容串流而不必每次登入您的服務即可。

此外，如果您的應用程式會將 Kinect 資料傳送至外部服務，您可以將其這裡。

如果您的服務需要使用者建立帳戶上分開 Xbox Live，則應該提供方法，以讓使用者以他們的帳戶，您的服務，一個為其 Xbox Live 帳戶連結至動作的時間。

當您設定單一登入時，您可以指定 Url 和其信賴憑證者的合作對象。 您的應用程式呼叫任何 Url 指定，任何時候 Xbox Live 會自動連結的 Xbox 安全權杖服務 (XSTS) 語彙基元。 接收金鑰的服務就所謂*信賴憑證者的合作對象*，而且您必須設定[信賴憑證者的合作對象](https://developer.microsoft.com/en-US/xboxconfig/relyingparties/index)才能設定單一登入。 每個信賴憑證者的合作對象組態指定何種資訊包含在 XSTS 語彙基元，以及唯一的加密金鑰的信賴憑證者的合作對象可以用來解碼 XSTS 語彙基元。

新增組態，執行下列動作：

1. 選取您的標題，在後[合作夥伴中心](https://partner.microsoft.com/dashboard)，瀏覽至**Services** > **Xbox Live**。

2. 按一下連結，即可**Xbox Live 單一登入**。

3. 按一下 **加入 URL**按鈕，以建立新單一登入的項目。 這會新增新的資料列的設定清單底部。

4. 在 [URL] 方塊中，輸入您的服務使用的完整的網域名稱的 URL。 您可能會使用萬用字元來取代的最低層級的子網域 ('\*')。 這會比對任何具有相同的上層網域的 URL。 例如，"*。 example.com&quot; "bar.example.com 」 或 「 foo.bar.example.com"會比對。

5. 在 信賴憑證者合作對象 方塊中，選取 信賴憑證者的合作對象組態，指定如何編碼 XSTS 語彙基元。

6. 按一下 **儲存**按鈕以儲存您的變更。

![[單一登入組態] 頁面的螢幕擷取畫面](../../images/dev-center/single-signon.png)
