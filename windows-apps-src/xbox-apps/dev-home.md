---
author: v-angraf
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: 在主控台 （開發人員首頁） 上的開發人員首頁
description: 提供一個 Xbox 開發首頁應用程式的相關的資訊。
ms.author: v-angraf@microsoft.com
ms.date: 08/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 3b802b9b53811e03e11ee3afd78f69db4bfd9986
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "1015357"
---
# <a name="developer-home-on-the-console-dev-home"></a>在主控台 （開發人員首頁） 上的開發人員首頁
   
  
開發人員常用功能工具在設計用來幫助開發人員的產能的 Xbox 一個 development kit。 開發人員首頁提供下列功能來管理和設定您開發套件 （英文）、 管理使用者、 啟動已安裝的標題及執行擷取與追蹤。 在未來的版本我們會繼續依序展開 [啟用其他功能根據您的意見反應及啟用擴充性和自己工具加入的功能。   
   
  
我們非常感興趣意見反應開發首頁與您最感興趣的看到其支援的案例。 請透過 [**傳送意見反應**主功能表的應用程式所述的方法或透過您開發人員帳戶管理員 (DAM)，提供您的註解。   
   
  
若要啟動年 11 月 2015年上開發首頁或更新版本的復原：  
 
   1. 靠攏首頁，在左邊緣或雙按一下連結關係] 按鈕開啟快速入門  
   1. 向下移動**設定**（齒輪圖示）   
   1. 選取**所有設定**  
   1. 從 [預設的**開發人員**] 頁面上，選取 [**開發人員首頁**（首頁圖示）   

 ![](images/dev_home_icons.png)   
  
在舊版的復原選取右側的 [首頁] 畫面上的 [開發人員首頁] 磚中**精選內容**或 Xbox 一管理員中檢視應用程式] 清單並啟動**開發首頁**。   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>使用者介面  
   
  
開發人員首頁使用者介面的標頭包含下列重要"at a glance"開發主控台的資訊：   
 
   *  **主控台 IP:** 在主控台目前 IP 位址。   
   *  **主控台名稱：** 在主控台目前主機名稱。  
   *  **沙箱：** 在主控台是在沙箱名稱。  
   *  **OS 版本：** 在主控台中執行目前復原版本。
   *  目前的系統時間。   

   
  
開發人員首頁 UI 的其餘部分會分成下列頁面。 這些頁面之工具的相關資訊，請參閱其個別的主題。   
 
   *  [家庭](devhome-home.md)  
   *  [Xbox Live](devhome-live.md)  
   *  [設定](devhome-settings.md)  
   *  [媒體擷取](devhome-capture.md)  
   *  [網路](devhome-networking.md)  
   *  [效能](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>主功能表  
   
  
在您控制站上按 [**功能表]** 按鈕，即可存取允許應用程式工作區，讓您管理存取網路位置和在應用程式提供意見反應資訊的認證設定主功能表。   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>貼齊模式 UX  
   
  
在開發人員首頁，例如網路與多人、 數個現有和未來工具設計被用來使用貼齊端執行時您的標題，以便您可以輕鬆存取工具時您要測試。   
   
  
若要存取嵌入式管理單元模式，反白顯示標題的適當的工具、 您控制站上按 [**檢視**] 按鈕及從快顯功能表中選取 [**貼齊**：  
 ![](images/dev_home_4.png)   
  
開發人員首頁將貼齊右側。 如常點選兩次 \[Nexus\] 按鈕，即可切換內容。  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>自訂開發人員首頁  
   
  
開發人員首頁設計成可進行自訂與個人化。 您可以設定以符合您的工作流程應用程式並儲存的為工作區。 此工作區可以匯出及匯入、 允許您將版面配置複製到其他同時為所需。 在**工作區**] 下的主功能表中找到這些選項。 匯出的檔案將會位於系統 scratch 磁碟機中`Dev Home\Workspaces`目錄。   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>調整大小和重新排序工具  
   
  
若要變更大小或位置的工具、 標題時使用快顯功能表] 按鈕 （在您控制站上的 [檢視] 按鈕） 具有焦點。 從快顯功能表選取 [**移動**或**調整大小**。   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>變更佈景主題色彩和背景影像  
   
  
在 [主要] 功能表中，您可以選取**工作區**，然後**變更佈景主題色彩**。 選取新的色彩並選取 [**儲存**] 以更新焦點醒目提示所使用的佈景主題色彩。   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>設定預設的應用程式套件  
   
  
如果套件包含多個應用程式、 開發人員首頁會允許您設定要啟動的預設應用程式。 反白顯示啟動器的封裝然後按下**的**按鈕開啟可用的應用程式清單。 反白顯示的一個您想要設為預設值與按下 [**檢視**] 按鈕，然後選擇 [從快顯功能表的 [**設成預設值**。   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>使用登錄及啟動標題從網路共用的開發人員首頁  
   
  
從啟動器底部的已安裝的應用程式與遊樂場] 清單中，您可以選取**註冊從網路共用遊戲**選項，以從遠端執行標題寬鬆的檔案版本。   
 ![](images/dev_home_8.png)   
  
您可以再輸入網路路徑 appxmanifest.xml 檔案中的 [您想要登錄的標題。 開發人員首頁會嘗試將註冊使用該共用位置，任何現有的認證的標題以及是否需要將新的網路認證提示。 如果您需要存取其他共用 （例如象徵連結存取資源一部伺服器） 會需要新增那些透過下列選項。   
   
  
您可以管理這些儲存的認證 （並新增其他 api） 在主控台透過主功能表的 [**管理網路認證**] 選項。   
 ![](images/dev_home_9.png)   
  
您可以在主控台目前檢視認證、 編輯選取認證的路徑並按一下 [ **A** ] 按鈕的認證和移除認證選取移除連結並按一下 [ **A** ] 按鈕。   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>本節內容  
  
[首頁 （開發人員首頁）](devhome-home.md)  


&nbsp;&nbsp;可快速存取開發主控台可針對定期執行的工作。 
  
  
[Xbox Live 頁面 （開發人員首頁）](devhome-live.md)  


&nbsp;&nbsp;擷取多人資訊並顯示 Xbox Live 服務的目前狀態。 
  
  
[設定] 頁面 （開發人員首頁）](devhome-settings.md)  


&nbsp;&nbsp;提供存取開發主控台的各種設定。 
  
  
[媒體擷取頁面 （開發人員首頁）](devhome-capture.md)  


&nbsp;&nbsp;開發人員常用的**媒體擷取**頁面擷取目前執行主控台上的標題的影片。 
  
  
[網路] 頁面 （開發人員首頁）](devhome-networking.md)  


&nbsp;&nbsp;會模擬為了疑難排解而各種網路條件。 
  
  
[效能頁面 （開發人員首頁）](devhome-performance.md)  


&nbsp;&nbsp;會模擬各種磁碟活動和為了疑難排解而 CPU 使用量條件。 
 