---
title: 建立並測試新的建立者標題
description: 了解如何建立新的 Xbox Live 創作者計劃標題並發行至測試環境。
ms.assetid: ced4d708-e8c0-4b69-aad0-e953bfdacbbf
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 建立者、 測試
ms.localizationpriority: medium
ms.openlocfilehash: 5b39a2ed67d2fe77fa8904408b81a329125d73c7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594593"
---
# <a name="create-a-new-xbox-live-creators-program-title-and-publish-to-the-test-environment"></a>建立新的 Xbox Live 創作者計劃標題並發行至測試環境

## <a name="introduction"></a>簡介

撰寫任何 Xbox Live 程式碼之前，您必須在您的服務設定入口網站上設定新的標題。  您可以深入了解中的服務組態[Xbox Live 服務組態](../xbox-live-service-configuration.md)。

這篇文章會逐步解說中設定的標題所需的所有項目[合作夥伴中心](https://partner.microsoft.com/dashboard)、 新的專案建立，並準備進行測試的 Xbox Live。 這篇文章的假設如下：

1. 您使用 Xbox Live 創作者計劃。
2. 您正在開發通用 Windows 平台 (UWP) 的標題。  Xbox One、 Windows 10 桌上型電腦和行動裝置上可以執行的 UWP 標題
3. 您要設定您的標題中[合作夥伴中心](https://partner.microsoft.com/dashboard)。
4. 您的開發電腦正在執行 Windows 10。

> [!NOTE]
> 如果您的 Xbox Live 創作者計劃的一部分，上述的假設適用於您和您應該依照這篇文章

## <a name="partner-center-setup"></a>合作夥伴中心安裝程式

您必須在中建立的 Xbox Live 啟用標題[合作夥伴中心](https://partner.microsoft.com/dashboard)的必要條件是使用任何 Xbox Live 的功能。

### <a name="create-a-microsoft-account"></a>建立 Microsoft 帳戶
如果您沒有 Microsoft 帳戶 (也稱為 MSA)，您必須先建立一個[登入的 Microsoft 帳戶-](https://go.microsoft.com/fwlink/p/?LinkID=254486)。 如果您有 Office 365 帳戶，使用 Outlook.com 或 Xbox Live 帳戶-您可能已經有 MSA。

### <a name="register-as-an-app-developer"></a>註冊為應用程式開發人員
您必須註冊為應用程式開發人員，才可以在合作夥伴中心建立新的標題。

若要註冊，請前往[註冊為應用程式開發人員](https://developer.microsoft.com/store/register)並遵照註冊程序。

### <a name="create-a-new-uwp-title"></a>建立新的 UWP 標題
您必須定義在合作夥伴中心 UWP 標題。 您的做法是第一個要[合作夥伴中心](https://partner.microsoft.com/dashboard)。

接下來，建立新的標題。 您必須保留名稱。

![](../images/getting_started/first_xbltitle_newapp.png)

螢幕擷取畫面顯示主要頁面，您將設定 Xbox Live，位於**Services** > **Xbox Live**功能表。

![](../images/creators_udc/creators_udc_xboxlive_page.png)

## <a name="enable-xbox-live-services"></a>啟用 Xbox Live 服務
當您按一下  **Xbox Live**連結底下**服務**產品的第一次，您將會進入 啟用 Xbox Live 創作者計劃 頁面。  接下來，按一下**啟用**按鈕，即可開啟 Xbox Live 的 設定 對話方塊。

![](../images/creators_udc/creators_udc_xboxlive_enable.png)

在 [設定] 對話方塊中，選取您想要啟用的 Xbox Live 服務的平台 （Xbox One 及 Windows 10 電腦預設會選取）。  按一下 **確認**按鈕來啟用您的遊戲的 Xbox Live 創作者計劃。

> [!IMPORTANT]
> 遊戲只支援 Xbox Live。 若要成功發行您的 Xbox Live 的遊戲，您必須設定您的產品類型以"Game"的屬性區段中提交。

![](../images/creators_udc/creators_udc_xboxlive_enable_dialog.png)

## <a name="test-xbox-live-service-configuration-in-your-game"></a>測試您的遊戲中的 Xbox Live 服務組態
當您變更到 Xbox Live 設定您的遊戲，您需要在 Xbox Live 的其餘部分所見，並可以看到您的遊戲之前，將變更發行到特定的環境。

當您仍然使用您的遊戲時，您會發佈到您自己開發的沙箱。  開發沙箱可讓您能夠對您的遊戲在隔離的環境中的變更。

在公開發行您的遊戲時，Xbox Live 的組態將會自動發佈至零售沙箱。

根據預設，一個 Xbox 和 Windows 10 電腦是零售沙箱中。

### <a name="publish-xbox-live-configuration-to-the-test-environment"></a>將 Xbox Live 設定發行至測試環境

當您啟用 Xbox Live 服務，並對 Xbox Live 服務組態進行變更時，若要讓變更生效，您需要將這些變更發佈至您開發的沙箱。

在 Xbox Live 設定 頁面上，按一下 **測試** 按鈕，將目前的 Xbox Live 設定發佈到您開發的沙箱。

![](../images/creators_udc/creators_udc_xboxlive_config_test.png)

### <a name="authorize-devices-and-users-for-the-development-sandbox"></a>授權裝置和開發沙箱的使用者

只有授權之的裝置與使用者可以存取您的開發沙箱中的遊戲的 Xbox Live 設定。

根據預設，您加入合作夥伴中心帳戶所有 Xbox One 的開發主控台都可以存取您的開發沙箱。  若要新增的 Xbox One 的主控台，請前往[管理 Xbox One 主控台](https://partner.microsoft.com/xboxconfig/devices)。 如果您已經在合作夥伴中心帳戶中，您可以前往**帳戶設定** > **帳戶設定** > **開發裝置** >  **Xbox One 開發主控台**。

您也可以授權存取您的開發沙箱的一般 Xbox Live 帳戶。  若要在 Xbox Live 帳戶存取權授權給您的開發沙箱，請前往[Manage Accounts](https://developer.microsoft.com/xboxtestaccounts/configurecreators)。

## <a name="next-steps"></a>後續步驟
既然您已建立新的標題，您現在可以設定您的遊戲引擎、 Visual Studio 或選擇的建置環境中的 Xbox Live 啟用標題。

請參閱[逐步指南將 Xbox Live](creators-step-by-step-guide.md)
