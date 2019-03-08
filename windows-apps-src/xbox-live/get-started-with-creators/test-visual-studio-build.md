---
title: 在 Visual Studio 中測試的 Unity 遊戲
description: 在 Visual Studio 中，建置為 Unity 的使測試成功的檢查清單。
ms.date: 03/19/2018
ms.topic: get-started-article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 unity
ms.openlocfilehash: 4d5a1a5596beba2839e01ca5be6e6d2dbff0c148
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589873"
---
# <a name="test-your-unity-game-build-in-visual-studio"></a>在 Visual Studio 中測試您的 Unity 遊戲組建

若要測試您的 Unity 遊戲以真實資料的 Xbox Live 功能，您必須建置遊戲中所述**建置和測試專案**一節[Unity 文件中設定 Xbox Live](configure-xbox-live-in-unity.md)。 下列文章會提供您的項目，以協助確保成功的測試，您的 Unity 遊戲，Visual Studio 中的檢查清單。

1. **再次確認您有與您的 Unity 遊戲相關聯的正確設定的標題。**
    如果您啟用 Xbox Live 透過 Xbox Live 關聯精靈，您會想要先熟悉[合作夥伴中心](https://partner.microsoft.com/dashboard)。 合作夥伴中心可讓您設定您的標題的 Xbox Live 設定，而且必須安裝在您與 Xbox Live 通訊的標題的訂單中正常運作。 發行項[建立新的 Xbox Live 創作者計劃標題並發行至測試環境](create-and-test-a-new-creators-title.md)會引導您完成在合作夥伴中心安裝程序。 如果您已經設定您的遊戲，透過**Xbox 組態精靈**在 Unity 中，您可以跳到 [] 區段[測試 Xbox Live 服務組態，在您的遊戲](create-and-test-a-new-creators-title.md#test-xbox-live-service-configuration-in-your-game)。 在合作夥伴中心，請務必檢查看看 Xbox Live 設定為 Unity 遊戲中的資訊符合您的遊戲的合作夥伴中心組態。

2. **請確定您的標題具有授權的 Microsoft Account(with gamertag) 可以登入您的標題。**
    沒有授權的帳戶，您將無法完成登入中測試您的標題，時，也將您能夠使用其他的 Xbox Live 功能。 若要確定您已授權的 Microsoft 帳戶和玩家代號閱讀[授權 Xbox Live 帳戶針對您的環境中測試](authorize-xbox-live-accounts.md)。

3. **請確定您的標題，已發行的測試。**
    當您變更您的標題這些變更必須發行至您的沙箱中才能使用。 請確定您推送**測試**按鈕，以將變更發佈至您的設定。

    ![發佈以供測試映像](../images/creators_udc/creators_udc_xboxlive_config_test.png)

    **測試** 按鈕位於[合作夥伴中心](https://partner.microsoft.com/dashboard)下標題的 Xbox Live 服務設定。 如果您進行任何變更到您的設定，例如加入新的授權的測試帳戶，您會想要推送**測試**按鈕以發行到 Xbox Live 服務的新的組態設定。

4. **請檢查並確定您的電腦是在開發人員模式，且您使用適當的 Xbox Live 沙箱。**
    當發行您的標題以供測試會發佈到特定的環境，稱為沙箱。 如果您開發電腦未設定為使用該沙箱，您不能測試 Xbox Live 的功能。 了解如何檢查及變更與您的 PC 沙箱[Xbox Live 沙箱簡介](xbox-live-sandboxes-creators.md)。

5. **請確定您建置為 x64 專案建置目標來建置您的電腦上本機電腦**

    ![組建設定](../images/unity/get-started-with-creators/vsBuildSettings.JPG)