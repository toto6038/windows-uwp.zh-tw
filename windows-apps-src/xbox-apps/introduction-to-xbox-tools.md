---
title: Xbox One 工具簡介
description: 使用 Windows Device Portal 的 Xbox One 特有工具「開發人員首頁」。
ms.date: 10/04/2017
ms.topic: article
keywords: windows 10, uwp, xbox one, 工具
ms.assetid: 6eaf376f-0d7c-49de-ad78-38e689b43658
ms.localizationpriority: medium
ms.openlocfilehash: ed106095d83ed0c6e055d22a1a0cf229380cff71
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8883021"
---
# <a name="introduction-to-xbox-one-tools"></a>Xbox One 工具簡介

本節涵蓋如何透過開發人員首頁應用程式存取 Xbox 裝置入口網站。

## <a name="dev-home"></a>開發人員首頁

開發人員首頁是 Xbox One Development Kit 上的一種工具體驗，設計來協助開發人員提高生產力。 「開發人員首頁」提供功能來管理和設定您的開發人員套件。

開發人員首頁是當您的主機啟動為開發人員模式時開啟的預設應用程式。 也可以透過選取主畫面上的 **\[開發人員首頁\]** 磚，開啟開發人員首頁。 如果未出現任何磚，表示主機未處於開發人員模式。

如需開發人員首頁的詳細資訊，請參閱[主機上的開發人員首頁 (開發人員首頁)](dev-home.md)。

## <a name="xbox-device-portal"></a>Xbox 裝置入口網站
Xbox 裝置入口網站是瀏覽器為基礎的裝置管理工具，可讓您新增遊戲和應用程式、加入 Xbox Live 測試帳戶、變更沙箱，以及其他更多。

若要在 Xbox One 主機上啟用 Xbox 裝置入口網站：

1. 選取主畫面上的 **\[開發人員首頁\]** 磚。

  ![選取 [開發人員首頁] 磚](images/introduction-to-xbox-one-tools-1.png)

2. 在開發人員首頁中，瀏覽到 **\[首頁\]** 索引標籤，然後選取 **\[遠端存取\]** 區段中的 **\[遠端存取設定\]**。

  ![遠端管理工具](images/introduction-to-xbox-one-tools-2.png)

3. 選取 **\[啟用 Xbox 裝置入口網站\]** 核取方塊。

4. 在 **\[驗證\]** 下，選取 **\[需要驗證以從 Web 或電腦工具遠端存取此主機\]** 核取方塊。

5. 輸入 **\[使用者名稱\]** 和 __\[密碼\]__，並選取 **\[儲存\]**。 這些認證是用來從瀏覽器驗證對您開發人員套件的存取權。

6. 選取 **\[關閉\]**，並記下**\[首頁\]** 索引標籤上 **\[遠端存取\]** 底下所列的 URL。

7. 在您的瀏覽器中輸入 URL。 您將收到有關所提供憑證的警告 (與下列螢幕擷取畫面類似)，因為 Xbox One 主機所簽署的安全性憑證不是視為已知的受信任發行者。 在 Edge 中，按一下 **\[詳細資料\]**，然後 **\[移至網頁\]** 存取 Xbox 裝置入口網站。

    ![安全性憑證警告](images/introduction-to-xbox-one-tools-3.png)

8. 使用您設定的認證登入。

## <a name="xbox-dev-mode-companion"></a>Xbox 開發人員模式小幫手
Xbox 開發人員模式小幫手是可讓您在不需離開電腦的情況下，於主機上進行工作的工具。 該 App 允許您檢視主機畫面並向它傳送輸入。 如需詳細資訊，請參閱 [Xbox 開發人員模式小幫手](xbox-dev-mode-companion.md)。

## <a name="see-also"></a>另請參閱
- [如何在針對 UWP 進行開發時使用 Fiddler 搭配 Xbox One](uwp-fiddler.md)
- [Windows Device Portal 概觀](../debug-test-perf/device-portal.md)
- [Xbox One 上的 UWP](index.md)