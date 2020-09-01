---
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: 主機上的開發人員首頁 (開發人員首頁)
description: 瞭解 Xbox One 主控台開發工具組的 Dev Home 工具體驗如何協助開發人員生產力。
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 40100adb1bd9337d933b8ebd155847bde71e341a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172812"
---
# <a name="developer-home-on-the-console-dev-home"></a>主機上的開發人員首頁 (開發人員首頁)
   
  
開發人員首頁是 Xbox One 開發套件上的一種工具體驗，設計來協助開發人員提高生產力。 「開發人員首頁」提供功能來管理和設定您的開發人員套件、管理使用者、啟動已安裝的標題，以及執行擷取和追蹤。 在未來版本，我們將會繼續擴充功能，根據您的意見反應啟用額外功能，以及讓您擴充及加入自己的工具。   
   
  
我們對於您在開發人員首頁的意見反應以及您最希望新增支援的案例非常有興趣。 請透過應用程式主功能表的 **\[傳送意見反應\]** 中所述的方法，或透過您的開發人員中心帳戶管理員 (DAM) 提供您的意見。   
   
  
若要在 2015 年 11 月或更新版本復原工具上啟動開發人員首頁：  
 
   1. 在 \[首頁\] 向左移動或按兩下 \[Nexus\] 按鈕來開啟互動中心  
   1. 向下移至 **\[設定\]**（齒輪圖示）   
   1. 選取 **\[所有設定\]**  
   1. 從預設 **\[開發人員\]** 頁面上，選取 **\[開發人員首頁\]**（首頁圖示）   

 ![](images/dev_home_icons.png)   
  
在較早的復原工具上，選取 **\[精選內容\]** 中主螢幕右側的開發人員首頁磚，或檢視 Xbox One 管理員中的應用程式清單並啟動 **\[開發人員首頁\]**。   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>使用者介面  
   
  
開發人員首頁使用者介面的標頭包含下列有關開發主機的重要「簡介」資訊：   
 
   *  **主機 IP；** 主機目前的 IP 位址。   
   *  **主機名稱：** 主機目前的主機名稱。  
   *  **沙箱：** 主機所在的沙箱名稱。  
   *  **作業系統版本：** 在主機上執行的目前復原版本。
   *  目前的系統時間。   

   
  
其餘的開發人員首頁 UI 分為下列頁面。 如需這些頁面上工具的相關資訊，請參閱個別的主題。   
 
   *  [首頁](devhome-home.md)  
   *  [Xbox Live](devhome-live.md)  
   *  [設定](devhome-settings.md)  
   *  [媒體抓取](devhome-capture.md)  
   *  [網路功能](devhome-networking.md)  
   *  [效能](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>主功能表  
   
  
按下控制器的 **\[選項\]** 按鈕，您可以存取主功能表以設定 app 工作區，管理存取網路位置的認證，以及 app 的意見反應資訊。   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>貼齊模式 UX  
   
  
開發人員首頁中的數個現有和即將推出的工具，例如網路功能與多人遊戲，可在您執行標題時貼齊側邊，讓您測試時可以輕鬆存取工具。   
   
  
若要存取貼齊模式，請反白顯示適當工具的標題、按控制器上的 **\[檢視\]** 按鈕，以及選取操作功能表上的 **\[貼齊\]**：  
 ![](images/dev_home_4.png)   
  
開發人員首頁將貼齊右側。 如常點選兩次 \[Nexus\] 按鈕，即可切換內容。  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>自訂開發人員首頁  
   
  
開發人員首頁設計成可進行自訂與個人化。 您可以設定 app 以符合您的工作流程，並儲存為工作區。 此工作區可以匯出與匯入，讓您視需要將配置複製到其他主機。 在主要功能表的**工作區**中找到這些選項。 匯出的檔案將會位於系統 scratch 磁碟機的 `Dev Home\Workspaces` 目錄中。   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>調整工具大小並重新排序工具  
   
  
若要變更工具的大小或位置，請在標題具有焦點時使用操作功能表按鈕 (控制器上的檢視按鈕) 在操作功能表上，選取 **\[移動\]** 或 **\[調整大小\]**。   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>變更佈景主題色彩和背景影像  
   
  
從主功能表，您可以選取 **\[工作區\]**，然後選取 **\[變更佈景主題色彩\]**。 選取新的色彩，然後選取 **\[儲存\]** 更新用於焦點醒目提示的佈景主題色彩。   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>設定套件的預設應用程式  
   
  
如果套件包含多個應用程式，開發人員首頁可讓您設定要啟動的預設應用程式。 在啟動程式中反白顯示套件，然後按 **A** 按鍵以開啟可使用的應用程式清單。 反白顯示您想要設為預設的應用程式，並按下 **\[檢視\]** 按鈕，然後從操作功能表選擇 **\[設為預設值\]**。   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>使用開發人員首頁從網路共用註冊及啟動標題  
   
  
從啟動程式、已安裝的 app 和遊戲清單底部，您可以選取選項 **\[Register a game from a network share\]** (從網路共用註冊遊戲)，遠端執行標題的鬆散檔案版本。   
 ![](images/dev_home_8.png)   
  
您可以針對要註冊之標題輸入 appxmanifest.xml 檔案的網路路徑。 開發人員首頁會嘗試使用該網路共用的任何現有認證來註冊標題，需要時將會提示您輸入新的網路認證。 如果您需要存取其他網路共用（例如，存取不同伺服器上的符號連結資源），則需要透過下列選項新增那些。   
   
  
您可以透過主功能表的**\[Manage network credentials\]** (管理網路認證) 選項，在主機上管理這些儲存的認證（以及新增其他認證）。   
 ![](images/dev_home_9.png)   
  
您可以檢視主機上目前的認證，選取認證的路徑並按一下 **A** 按鍵編輯認證，以及選取移除連結並按一下**A** 按鍵移除認證。   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>本節內容  
  
[首頁 (開發人員首頁)](devhome-home.md)  


&nbsp;&nbsp;可讓您快速存取在開發主機上例行性執行的工作。 
  
  
[Xbox Live 頁面 (開發人員首頁)](devhome-live.md)  


&nbsp;&nbsp;擷取多人遊戲資訊，並顯示 Xbox Live 服務的目前狀態。 
  
  
[設定頁面 (開發人員首頁)](devhome-settings.md)  


&nbsp;&nbsp;為開發主機提供各種不同設定的存取。 
  
  
[媒體擷取頁面 (開發人員首頁)](devhome-capture.md)  


&nbsp;&nbsp;Dev Home 的 [ **Media capture** ] 頁面會捕獲目前在主控台上執行之標題的影片。 
  
  
[網路功能頁面 (開發人員首頁)](devhome-networking.md)  


&nbsp;&nbsp;模擬各種不同的網路條件以進行疑難排解。 
  
  
[效能頁面 (開發人員首頁)](devhome-performance.md)  


&nbsp;&nbsp;模擬各種磁碟活動和 CPU 使用條件以進行疑難排解。 
 