---
author: v-angraf
title: Xbox One 上的 UWP 新功能
description: 針對 Xbox One 上的 UWP App 新功能進行重點摘要。
ms.author: v-angraf
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: cbabe9d31b5b9762320df8e4a92d19ae4e33497d
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "301454"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Xbox One 上的 UWP 最新更新中適用於開發人員的新功能

最新的更新的通用 Windows 平台 (UWP) 上 Xbox 一個包含下列新功能至現有的功能和修正錯誤的更新。

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>x86 Xbox 不再支援的應用程式和遊樂場  
Xbox 不再支援 x86 應用程式開發或 x86 應用程式提交至Microsoft Store。

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>應用程式現在可支援瀏覽回至舊的應用程式 
在 Xbox 一部應用程式的 UWP 現在可支援瀏覽回至舊的應用程式。 若要這樣做，訂閱[**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893595)事件並**Handled**屬性設定為**false**事件處理常式中。

> [!NOTE]
> 基於相容性考量此功能僅適用於具有最新版本的上一個 Xbox UWP 建置的應用程式。 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>開發人員首頁現在是開發主控台的預設首頁經驗
開發同時立即啟動開發首頁與預設首頁體驗。 這可讓您做好從右至搭配使用而不需要透過按一下 [從零售主畫面。 開發人員首頁現在包含快速啟動零售版的 [首頁] 畫面的動作。 此外，指定新的設定可讓您進行零售首頁預設經驗。 

## <a name="new-dev-home-user-interface"></a>新的開發人員首頁使用者介面
開發人員首頁使用者介面現在包含下列的產能增強功能：
 - 重要的資料，像是 IP 位址和復原版本現在會顯示在 [可見度] 畫面的頂端。 
 - 開發人員首頁現在有的索引標籤式的 UI 群組工具至邏輯組允許的快速導覽。
 - 開發人員首頁的第一個索引標籤上的 [快速動作] 按鈕允許快地存取最常用的動作。 

## <a name="wdp-for-xbox-enhancements"></a>WDP 的 Xbox 增強功能
Windows 裝置入口網站 (WDP) 現在會包含主控台設定的其他支援。 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>您現在可以在切換"App"和"遊戲"之間您 UWP 標題的類型
切換之間"App"及"遊戲"您 UWP 標題的類型可讓您測試遊戲案例不發佈至存放區。 開發人員首頁的 [**遊樂場與應用程式**] 窗格中選取的應用程式、 控制站上按 [檢視] 按鈕、 選取**應用程式詳細資料**，然後類型變更為"App"或"遊戲"。

## <a name="see-also"></a>也請參閱
- [已知問題](known-issues.md)
- [Xbox One 上的 UWP](index.md)
 - 您現在可以擷取主機的螢幕擷取畫面。 如需製作螢幕擷取畫面的詳細資訊，請參閱 [/ext/screenshot](wdp-media-capture-api.md) 參考主題。
 - 此工具可以部署 App 的鬆散檔案組建。 如需鬆散檔案組建的詳細資訊，請參閱 [/api/app/packagemanager/register](wdp-loose-folder-register-api.md) 參考主題。
 - 可以從開發電腦的 [檔案總管] 存取主機上的開發人員檔案。 如需透過 [檔案總管] 存取檔案的詳細資訊，請參閱 [/ext/smb/developerfolder](wdp-smb-api.md) 參考主題。

## <a name="see-also"></a>另請參閱
- [已知問題](known-issues.md)
- [Xbox One 上的 UWP](index.md)
