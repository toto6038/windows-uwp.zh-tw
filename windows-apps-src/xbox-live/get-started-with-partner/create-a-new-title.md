---
title: 建立新的標題
description: 了解如何建立新的標題使用合作夥伴中心 Xbox Live。
ms.assetid: b8bd69e6-887a-4b1f-a42d-8affdbec0234
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1aa2447a2044bec9b2013b30c05e45342b763fc3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656563"
---
# <a name="create-a-new-title-for-xbox-live"></a>建立新的標題 Xbox Live

## <a name="introduction"></a>簡介

再撰寫任何程式碼，您必須在您的服務設定入口網站上設定新的標題。  您可以深入了解中的服務組態[Xbox Live 服務組態](../xbox-live-service-configuration.md)

本文將逐步引導您做出以下假設此程序

1. 您正在開發通用 Windows 平台 (UWP) 的標題。  Xbox One、 Windows 10 桌上型電腦和行動裝置上執行的 UWP 標題
2. 您要設定您的標題中[合作夥伴中心](https://partner.microsoft.com/dashboard)。
3. 您會使用自訂的遊戲引擎或 Unity 中使用 Visual Studio。
4. 您的開發電腦正在執行 Windows 10。

提供上述條件，則為 true，這篇文章的其餘部分將逐步引導您取得在合作夥伴中心，建立新的專案和 Xbox Live 登入程式碼撰寫和測試中設定的標題所需的所有項目。

> [!NOTE]
> 如果您的 Xbox Live 創作者計劃的一部分，上述的假設適用於您，您應該依照這篇文章。

## <a name="partner-center-setup"></a>合作夥伴中心安裝程式

您必須在中建立的 Xbox Live 啟用標題[合作夥伴中心](https://partner.microsoft.com/dashboard)的必要條件是成任何 Xbox Live 功能運作。

### <a name="create-a-microsoft-account"></a>建立 Microsoft 帳戶
如果您沒有 Microsoft 帳戶 (也稱為 MSA)，您必須先建立一個[ https://go.microsoft.com/fwlink/p/?LinkID=254486 ](https://go.microsoft.com/fwlink/p/?LinkID=254486)。  如果您有 Office 365 帳戶，使用 Outlook.com 或 Xbox Live 帳戶-您可能已經有 MSA。

### <a name="register-as-an-app-developer"></a>註冊為應用程式開發人員
您必須先登錄身為應用程式開發人員，才能允許您在合作夥伴中心建立新的標題。

若要註冊，請前往要 https://developer.microsoft.com/en-us/store/register並遵照註冊程序。

### <a name="create-a-new-uwp-title"></a>建立新的 UWP 標題
接下來，您必須定義在合作夥伴中心 UWP 標題。  這麼做，第一次移至儀表板

![](../images/getting_started/first_xbltitle_dashboard.png)

<p>
</p>
<br>
<p>
</p>

接著，建立新的標題。  您必須保留名稱。

![](../images/getting_started/first_xbltitle_newapp.png)

您將會被重新導向*應用程式概觀*您應用程式頁面。  主要頁面，您將設定 Xbox Live 正在服務]-> [Xbox Live 的功能表，如下所示。

![](../images/getting_started/first_xbltitle_leftnav.png)

<div id="createxblaccount"></div>

## <a name="create-an-xbox-live-account"></a>建立 Xbox Live 帳戶
您必須登入 Xbox Live 的 Xbox Live 帳戶。  如果您已經有在您的 Xbox One 主控台中，或在 Windows 10 上的 Xbox 應用程式中，您用來登入，您可以使用一個。

否則您應該開啟您的電腦和登入 Xbox 應用程式與您 Microsoft 帳戶。  然後將用於 Xbox Live 啟用。

您可以尋找 Xbox 應用程式進入 *[開始] 功能表*並輸入"Xbox"，如下所示。

![](../images/getting_started/first_xbltitle_xboxapp.png)

## <a name="next-steps"></a>後續步驟
既然您已建立新的標題，您現在可以設定您的遊戲引擎、 Visual Studio 或選擇的建置環境中的 Xbox Live 啟用標題。

請參閱[逐步指南將 Xbox Live](partners-step-by-step-guide.md)
