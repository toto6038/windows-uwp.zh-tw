---
description: 了解如何將您的應用程式更新成使用最新支援的 Microsoft Advertising 程式庫，並確定您的應用程式會繼續接收橫幅廣告。
title: 將您的應用程式更新到橫幅廣告的最新 Advertising 程式庫
ms.date: 08/23/2017
ms.topic: article
keywords: Windows 10, UWP, 廣告, AdControl, AdMediatorControl, 移轉
ms.assetid: f8d5b2ad-fcdb-4891-bd68-39eeabdf799c
ms.localizationpriority: medium
ms.openlocfilehash: adac5cfdb1b4a10674fb7173e5b84a86b509f130
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8475277"
---
# <a name="update-your-app-to-the-latest-advertising-libraries-for-banner-ads"></a>將您的應用程式更新到橫幅廣告的最新 Advertising 程式庫

從 2017 年 4 月 1 日，我們停止提供橫幅廣告至使用未受支援廣告 SDK 版本的 app。 如果您使用 **AdControl** 在您的通用 Windows 平台 (UWP) app 中顯示橫幅廣告，請使用本文中的資訊來判斷您是否使用尚未受到支援的廣告 SDK，並將您的應用程式移轉到支援的 SDK。

## <a name="overview"></a>概觀

顯示橫幅廣告的 UWP app 必須使用在 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp) 中散佈，來自 Advertising 程式庫的 **AdControl**。 此 SDK 支援一組最基本的廣告功能，包括透過來自美國互動廣告局 (Interactive Advertising Bureau, IAB) 的[行動多媒體廣告介面定義 (Mobile Rich-media Ad Interface Definitions, MRAID) 1.0 規格](http://www.iab.com/wp-content/uploads/2015/08/IAB_MRAID_VersionOne.pdf)提供 HTML5 多媒體的功能。 許多我們的廣告商都尋求這些功能，而且我們要求 App 開發人員使用這些 SDK 版本，來協助讓我們的應用程式生態體系對廣告商更具吸引力，並最終為您賺取更多收入。

在此 SDK 發行前，我們在數個舊版廣告 SDK 中提供 **AdControl** 類別。 因為不支援上述最基本的廣告功能，已不再支援這些舊版的廣告 SDK。 從 2017 年 4 月 1 日，我們停止提供橫幅廣告至使用未受支援廣告 SDK 版本的 app。 如果您有仍然使用未受支援廣告 SDK 版本的 app，您將會看到以下行為：

* 將不會再為您應用程式中的任何 **AdControl** 提供橫幅廣告，您將無法再從這些控制項賺取廣告收入。

* 當您應用程式中的 **AdControl** 要求新的廣告時，將會引發控制項的 **ErrorOccurred** 事件，而事件引數的 **ErrorCode** 屬性值將會是 **NoAdAvailable**。

* 與您應用程式相關的任何廣告單元將會被停用。 您無法從您 DePartnerv 中心帳戶移除這些停用的廣告單元。 如果您更新應用程式為使用 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)，請忽略這些廣告單元，並建立新的廣告單元。

* 也不再提供橫幅廣告給多個 app 中使用的任何廣告單元。 請確定您的每個廣告單元只在單一 app 中使用。

如果您的現有應用程式 (已經在 Microsoft Store 中，或仍在開發) 顯示橫幅廣告使用 **AdControl**，但您不確定應用程式使用哪個廣告 SDK，請依照本文中的指示，判斷您是否需要更新應用程式為使用支援的 SDK。 如果您遭遇任何問題或需要協助，請[連絡客戶支援](http://go.microsoft.com/fwlink/?LinkId=393643)。

> [!NOTE]
> 如果您的應用程式已使用 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp) (適用於 UWP app)，則不需要進一步變更您的應用程式。

## <a name="prerequisites"></a>必要條件

* 適用於使用 **AdControl** 之應用程式的完整原始程式碼和 Visual Studio 專案檔。
* 應用程式的 .appx 套件。

> [!NOTE]
> 若您不再擁有應用程式的 .appx 套件，但開發電腦仍具備用來建置應用程式的 Visual Studio 版本及廣告 SDK，則您可在 Visual Studio 中重新產生 .appx 套件。

<span id="part-1" />

## <a name="part-1-determine-whether-you-need-to-update-your-uwp-app"></a>第 1 部分︰判斷是否需要更新您的 UWP app

請依照下列各節中的指示來判斷是否需要更新您的應用程式。

1. 建立您應用程式的 .appx 套件複本以免影響到正本，將複本重新命名，使其副檔名為 .zip，然後將該檔案的內容解壓縮。

2. 檢查您應用程式套件的解壓縮內容：
  * 如果您看到 Microsoft.Advertising.dll 檔案，即表示您的應用程式使用舊 SDK，您必須依照下列各節中的指示來更新您的專案。 繼續進行[第 2 部分](update-your-app-to-the-latest-advertising-libraries.md#part-2)。
  * 如果您沒看到 Microsoft.Advertising.dll 檔案，即表示您的 UWP 應用程式已經使用最新的可用廣告 SDK，您不需要對您的專案進行任何變更。


<span id="part-2" />

## <a name="part-2-install-the-latest-sdk"></a>第 2 部分：安裝最新的 SDK

如果您的應用程式使用舊的 SDK 版本，請依照下列指示來確定您開發電腦上的 SDK 是最新的。

1. 請確定您的開發電腦已安裝 Visual Studio 2015 或更新版本。
    > [!NOTE]
    > 若 Visual Studio 在您的開發電腦中為開啟狀態，請先將其關閉再執行下列步驟。

1.  將您開發電腦上的所有舊版 Microsoft Advertising SDK 和 Ad Mediator SDK 解除安裝。

2.  開啟 **\[命令提示字元\]** 視窗，然後執行下列命令，以清除可能已與 Visual Studio 一起安裝但未出現在電腦上已安裝程式清單中的任何 SDK 版本：
    ```syntax
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  安裝 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)。

## <a name="part-3-update-your-project"></a>第 3 部分：更新您的專案

從專案中移除所有現有的 Microsoft Advertising 程式庫參考，然後依照[這些指示](install-the-microsoft-advertising-libraries.md#reference)來新增必要的參考。 這將確保您的專案使用正確的程式庫。 您可以保留現有的標記和程式碼。

## <a name="part-4-test-and-republish-your-app"></a>第 4 部分：測試並重新發佈您的應用程式

請測試您的應用程式以確定它如預期般顯示橫幅廣告。

如果[建立新的提交](../publish/app-submissions.md)在合作夥伴中心來重新發佈您的應用程式的應用程式已更新的 「 市集 」 中已經有您的應用程式的先前版本。
