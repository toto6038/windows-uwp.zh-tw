---
title: 在您的測試環境授權 Xbox Live 帳戶
description: 了解如何授權公用 Xbox Live 帳戶，用於在您的開發環境中測試。
ms.assetid: 9772b8f1-81db-4c57-a54d-4e9ca9142111
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 測試帳戶的帳戶
ms.localizationpriority: medium
ms.openlocfilehash: 662f85f985baf58eef050060f8132f5a4b92444d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663853"
---
# <a name="authorize-xbox-live-accounts-for-testing-in-your-environment"></a>Xbox Live 帳戶授權您的環境中測試

本主題會瀏覽您的發行者測試環境的 Xbox Live 帳戶所設定的程序

## <a name="prerequisites"></a>必要條件

您需要下列授權 Xbox Live 的測試帳戶：

* [Xbox Live 帳戶](https://support.xbox.com/browse/my-account/manage-account/Create%20account)

## <a name="navigate-to-the-xbox-test-account-page"></a>瀏覽至 Xbox 測試帳戶頁面

這位於合作夥伴中心的 [帳戶設定] 區段

您可以找到此節中有兩種

1. 在 [合作夥伴中心](https://partner.microsoft.com/dashboard/windows/overview)，按一下設定齒輪 ⚙️，在右上方的儀表板中，選取**開發人員設定**從下拉式清單。 這會開啟的畫面，您將在其中選取 Xbox Live 的下拉式清單中的左側導覽功能表，然後**Xbox 測試帳戶**連結。
2. 從您的 Xbox Live 創作者組態頁面找出 [測試] 區段並按一下 [標題] 連結**授權 Xbox Live 帳戶針對您的測試環境**

## <a name="authorize-an-xbox-live-account-for-your-test-environment"></a>授權為您的測試環境的 Xbox Live 帳戶

* 一次在 Xbox 測試帳戶頁面中，您應該看到一份所有目前的獲授權的帳戶。 若要授權的新帳戶，請按一下 [新增帳戶] 按鈕

![新增的 Xbox Live 帳戶](../images/creators_udc/add_test_account.png)

* 含有一個文字方塊，您可以在其中輸入所需的帳戶電子郵件地址的畫面應該顯示強制回應

![將 Xbox Live 帳戶強制回應](../images/creators_udc/add_test_account_modal.png)

* 按一下 驗證電子郵件地址是否存在，而且有相關聯的 Xbox Live 帳戶的 新增 按鈕。 如果通過檢查強制回應便會消失，，您會看到它，表示資料表中新的帳戶現在已成功授權用於測試環境

![加入 Xbox Live 帳戶成功](../images/creators_udc/add_test_account_success.png)

## <a name="troubleshooting"></a>疑難排解

在強制回應中輸入的電子郵件是執行一些檢查，包括確保 Xbox Live 帳戶關聯與查閱。 如果任何這些檢查失敗，帳戶不是加入資料表中，因此未獲授權，您可能會收到 「 抱歉，加入您的電子郵件地址時發生問題 」 的錯誤。

如果您有問題的良好檢查是嘗試使用的帳戶登入[Xbox.com](https://www.xbox.com/live/)。 如果您無法登入然後帳戶不是 Xbox Live 帳戶。
