---
author: Mtoepke
title: 啟用 Xbox One 開發人員模式
description: 如何啟用開發人員模式，讓您能在零售模式和開發人員模式之間切換。
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: ade80769-17ae-46e9-9c2f-bf08ae5a51ee
ms.localizationpriority: medium
ms.openlocfilehash: 7e497eb404129a7d8cc8ce9784c2e28a9c9456fc
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6052203"
---
# <a name="xbox-one-developer-mode-activation"></a>啟用 Xbox One 開發人員模式

## <a name="how-developer-mode-works"></a>開發人員模式的運作方式
Xbox One 有兩種模式，*零售*模式 (**1**) 和*開發人員*模式 (**2**)。 在零售模式中，主機將處於任何 Xbox One 主機的客戶或使用者都會使用的狀態：您能以使用者的身分進行遊戲和執行 App。 在開發人員模式中，您可以為主機開發軟體，但您並無法進行零售遊戲或是執行零售 App。

開發人員模式可以在任何零售版的 Xbox One 主機上啟用。 啟用開發人員模式之後，您可以在零售 (**2a**) 和開發人員模式 (**2b**) 之間來回切換。

![Xbox One 模式](images/dev-mode-flow.png)

## <a name="activate-developer-mode-on-your-retail-xbox-one-console"></a>在零售版 Xbox One 主機上啟用開發人員模式

1.  啟動您的 Xbox One 主機。

2.  在 Xbox One 上的 Microsoft Store 中搜尋並安裝「**啟用開發人員模式**」app。

    ![安裝「啟用開發人員模式」App](images/devkit-activation-1.png)

3.  從 Microsoft Store 頁面啟動 app。

    ![啟用開發人員模式 app](images/devkit-activation-2.png)

4.  請注意到在「啟用開發人員模式」App 中顯示的代碼。

    ![啟用步驟 5](images/activation-step-5.png)  
    
5.  移至[partner.microsoft.com/xboxactivate](https://partner.microsoft.com/xboxactivate)。

6.  以您的開發人員中心帳戶登入開發人員中心。

7.  輸入在「啟用開發人員模式」App 中顯示的啟用代碼。 您的帳戶具備有限的啟用次數。 啟用開發人員模式之後，開發人員中心將會指出與您帳戶相關連的啟用次數已使用一次。

    ![啟用步驟 8](images/activation-step-8-rs2.png)    
    
8.  按一下 **\[同意並啟用\]**。 這會導致頁面重新整理，您將會看見您的裝置填入表格當中。 您可在[Xbox One 開發人員模式啟用計畫](http://go.microsoft.com/fwlink/p/?LinkId=760399)中找到 Xbox One 開發人員模式啟用計畫合約。

9.  在您輸入啟用代碼之後，您的主機將會顯示啟用程序的進度畫面。  
    
10. 啟用完成之後，請開啟「啟用開發人員模式」App 並按一下 **\[切換並重新啟動\]** 以移至開發人員模式。 請注意，這可能會花費比平常還要久的時間。

    ![啟用步驟 12](images/activation-step-12.png)   

## <a name="switch-between-retail-and-developer-mode"></a>在零售和開發人員模式之間切換
當開發人員模式已在您的主機上啟用之後，請使用 **\[開發人員首頁\]** 來在零售模式和開發人員模式之間切換。 如需深入了解啟動並使用 [開發人員首頁] 的方式，請參閱 [Xbox One 工具簡介](introduction-to-xbox-tools.md)。

* 若要切換至零售模式，請開啟**開發人員首頁**。 在 **/[快速控制項目/]** 下，選取 **/[離開開發人員模式/]**。 這會將您的主機重新啟動為零售模式。    

  ![啟用步驟 13](images/activation-step-13-rs4.png)  
  
* 如需切換至開發人員模式，請使用「啟用開發人員模式」App。 開啟 app 並選取 **\[切換並重新啟動\]**。 這會將您的主機重新啟動為開發人員模式。  

  ![啟用步驟 14](images/activation-step-12.png)  

## <a name="see-also"></a>另請參閱
- [停用 Xbox One 開發人員模式](devkit-deactivation.md)
- [Xbox One 上的 UWP](index.md)
