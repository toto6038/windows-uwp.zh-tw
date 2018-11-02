---
author: v-angraf
title: Xbox One 上的 UWP 新功能
description: 針對 Xbox One 上的 UWP App 新功能進行重點摘要。
ms.author: v-angraf
ms.date: 03/29/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: cc2168014e714de0b43b6ffffe84126764f0a4a3
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5937775"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Xbox One 上的 UWP 最新更新中適用於開發人員的新功能

最新的更新的通用 Windows 平台 (UWP) 在 Xbox One 上包含下列的新功能，更新現有的功能，以及錯誤修正。

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>x86 應用程式和遊戲已不再支援在 Xbox 上  
Xbox 不再支援 x86 應用程式開發或 x86 應用程式提交至Microsoft Store。

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>應用程式現在支援瀏覽回到先前的應用程式 
Xbox One 的應用程式上的 UWP 現在可支援瀏覽回到先前的應用程式。 若要這樣做，訂閱[**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893595)事件並將**Handled**屬性設**為 false**事件處理常式中。

> [!NOTE]
> 基於相容性，這項功能是僅適用於 Xbox One 上 UWP 的最新發行版本內建 app。 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>開發人員首頁現在是開發主機上的預設首頁體驗
開發主機現在啟動開發人員首頁，做為預設首頁體驗。 這可讓您取得正確運作而不需要透過按一下在零售主畫面上。 開發人員首頁現在包含啟動零售主畫面上快速控制項目。 此外，新的設定可讓您進行零售 Home 預設體驗。 

## <a name="new-dev-home-user-interface"></a>新的開發人員首頁使用者介面
開發人員首頁 」 使用者介面現在包含下列的生產力增強功能：
 - 重要的資料 （例如 IP 位址 （英文） 和復原版本現在顯示在畫面的可見性頂端。 
 - 開發人員首頁現在具有索引標籤式的 UI 成邏輯集合，群組工具允許，快速瀏覽。
 - 開發人員首頁的第一個索引標籤上的快速動作按鈕可讓快速存取最常使用的動作。 

## <a name="wdp-for-xbox-enhancements"></a>WDP 適用於 Xbox 的增強功能
Windows Device Portal (WDP) 現在包含其他支援主控台設定。 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>您現在可以切換您的 UWP 標題 「 應用程式 」 和 「 遊戲 」 之間的類型
切換您的 UWP 標題 「 應用程式 」 和 「 遊戲 」 之間的類型，可讓您測試遊戲的案例，而發佈到市集。 在開發人員首頁應用程式的窗格中選取**的遊戲與應用程式**、 按控制器上的 [檢視] 按鈕、 選取**應用程式詳細資料**，然後的類型變更為 「 應用程式 」 或 「 遊戲 」。

## <a name="see-also"></a>請參閱
- [已知問題](known-issues.md)
- [Xbox One 上的 UWP](index.md)
 - 您現在可以擷取主機的螢幕擷取畫面。 如需製作螢幕擷取畫面的詳細資訊，請參閱 [/ext/screenshot](wdp-media-capture-api.md) 參考主題。
 - 此工具可以部署 App 的鬆散檔案組建。 如需鬆散檔案組建的詳細資訊，請參閱 [/api/app/packagemanager/register](wdp-loose-folder-register-api.md) 參考主題。
 - 可以從開發電腦的 [檔案總管] 存取主機上的開發人員檔案。 如需透過 [檔案總管] 存取檔案的詳細資訊，請參閱 [/ext/smb/developerfolder](wdp-smb-api.md) 參考主題。

## <a name="see-also"></a>另請參閱
- [已知問題](known-issues.md)
- [Xbox One 上的 UWP](index.md)
