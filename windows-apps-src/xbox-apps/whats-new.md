---
title: Xbox One 上的 UWP 新功能
description: 針對 Xbox One 上的 UWP App 新功能進行重點摘要。
ms.date: 03/29/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: 27810fb850a54b70e620f06ea033b7c362792bfc
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372965"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Xbox One 上的 UWP 最新更新中適用於開發人員的新功能

Xbox One 上的通用 Windows 平台 (UWP) 最新更新包含以下新功能、現有功能的更新，以及錯誤修正。

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>Xbox 不再支援 x86 app 和遊戲  
Xbox 不再支援 x86 應用程式開發或 x86 應用程式提交至Microsoft Store。

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>應用程式現在支援瀏覽回到先前的 app 
Xbox One 上的 UWP 應用程式現在支援瀏覽回到先前的 app。 若要這樣做，請訂閱 [**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager) 事件，並將事件處理常式中的 **Handled** 屬性設為 **false**。

> [!NOTE]
> 基於相容性理由，使用 Xbox One 上的 UWP 最新版本所建置的 app，才能使用此功能。 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>開發人員首頁現在是開發主機上的預設首頁體驗
開發主機現在會啟動開發人員首頁，做為預設首頁體驗。 這可讓您直接工作，而不需從零售主螢幕不斷點按。 開發人員首頁現在包含快速控制項目，可啟動零售主螢幕。 此外，新的設定可讓零售主螢幕成為預設體驗。 

## <a name="new-dev-home-user-interface"></a>新的開發人員首頁使用者介面
開發人員首頁使用者介面現在包含下列產品增強功能：
 - 重要的資料，如 IP 位址和復原版本，現在會顯示螢幕頂端以提供可見度。 
 - 開發人員首頁現在有索引標籤式 UI，將工具群組成邏輯集合，讓您快速的瀏覽。
 - 開發人員首頁的第一個索引標籤上的快速控制項目按鍵可讓您快速存取最常使用的動作。 

## <a name="wdp-for-xbox-enhancements"></a>適用於 Xbox 的 WDP 增強功能
Windows Device Portal (WDP) 現在包含主機設定的其他支援。 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>您現在可以在「應用程式」與「遊戲」之間切換 UWP 標題的類型
在「應用程式」與「遊戲」之間切換 UWP 標題的類型，可讓您測試遊戲案例，而不發行至市集。 在開發人員首頁，選取 **[遊戲與應用程式]** 窗格中的應用程式，按控制器上的 \[檢視\] 按鈕，選取 **[App details]** (App 詳細資料)，然後變更至「應用程式」或「遊戲」的類型。

## <a name="see-also"></a>另請參閱
- [已知問題](known-issues.md)
- [在 Xbox One UWP](index.md)
 - 您現在可以擷取主機的螢幕擷取畫面。 如需製作螢幕擷取畫面的詳細資訊，請參閱 [/ext/screenshot](wdp-media-capture-api.md) 參考主題。
 - 此工具可以部署 App 的鬆散檔案組建。 如需鬆散檔案組建的詳細資訊，請參閱 [/api/app/packagemanager/register](wdp-loose-folder-register-api.md) 參考主題。
 - 可以從開發電腦的 [檔案總管] 存取主機上的開發人員檔案。 如需透過 [檔案總管] 存取檔案的詳細資訊，請參閱 [/ext/smb/developerfolder](wdp-smb-api.md) 參考主題。

## <a name="see-also"></a>另請參閱
- [已知問題](known-issues.md)
- [在 Xbox One UWP](index.md)
