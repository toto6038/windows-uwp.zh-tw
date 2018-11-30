---
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: 在主控台 （開發人員首頁） 上的開發人員首頁
description: 提供 Xbox one 開發人員首頁應用程式的相關資訊。
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, UWP
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 4113df37446d93883cf395e7c1e86b1de6c1b328
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8323352"
---
# <a name="developer-home-on-the-console-dev-home"></a>在主控台 （開發人員首頁） 上的開發人員首頁
   
  
開發人員首頁是設計來協助開發人員提高生產力的 Xbox One development kit 上一種工具體驗。 開發人員首頁 」 提供功能來管理和設定您的開發套件、 管理使用者、 啟動已安裝的標題和執行會擷取和追蹤。 在未來版本我們會繼續進行擴充，需要啟用其他功能，根據您的意見反應，也可以啟用擴充性和您自己的工具，新增的功能。   
   
  
我們有非常興趣您的意見反應上開發人員首頁，以及您感興趣最看到它支援的案例。 請透過應用程式的主功能表**傳送意見反應**底下所述的方法，或透過您開發人員帳戶管理員 (DAM)，提供您的註解。   
   
  
若要啟動開發人員首頁年 11 月 2015 或更新版本的修復：  
 
   1. 藉由移動首頁上向左或 double 按一下 [Nexus] 按鈕開啟本指南  
   1. 向下移動，以**設定**（齒輪圖示）   
   1. 選取 [**所有設定**  
   1. 從預設的**開發人員**頁面上，選取 [**開發人員首頁**（家用圖示）   

 ![](images/dev_home_icons.png)   
  
較舊版本的復原**內容精選應用程式**中選取主畫面右側的 [開發人員首頁] 磚或檢視應用程式清單在 Xbox One 管理員和啟動**開發人員首頁**。   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>使用者介面  
   
  
開發人員首頁 」 使用者介面的標頭包含下列重要 「 簡介 」 的開發主機的相關資訊：   
 
   *  **主機 IP:** 目前的 IP 位址的主控台。   
   *  **主控台名稱：** 在主控台目前主機名稱。  
   *  **沙箱：** 主機將處於沙箱的名稱。  
   *  **OS 版本：** 目前復原版本主機上執行。
   *  目前的系統時間。   

   
  
開發人員首頁 UI 的其餘部分會分成下列頁面。 如需這些頁面工具的相關詳細資訊，請參閱其個別的主題。   
 
   *  [家庭](devhome-home.md)  
   *  [Xbox Live](devhome-live.md)  
   *  [設定](devhome-settings.md)  
   *  [媒體擷取](devhome-capture.md)  
   *  [網路](devhome-networking.md)  
   *  [效能](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>主功能表  
   
  
藉由控制器上，按下 [**功能表]** 按鈕，您可以存取主功能表，以允許設定的應用程式工作區，能夠管理認證存取網路位置和相關意見反應提供應用程式的詳細資訊。   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>貼齊模式 UX  
   
  
開發人員首頁，例如網路功能和多人遊戲，在數個現有和未來工具被專供時貼齊側邊您正在執行您的標題，這樣在測試時，可以讓您輕鬆存取工具。   
   
  
若要存取貼齊模式，反白顯示標題的適當的工具、 按下您控制器上的 [**檢視**] 按鈕，並從操作功能表中選取 [**貼齊**：  
 ![](images/dev_home_4.png)   
  
開發人員首頁將貼齊右側。 如常點選兩次 \[Nexus\] 按鈕，即可切換內容。  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>自訂開發人員首頁  
   
  
開發人員首頁設計成可進行自訂與個人化。 您可以設定來符合您的工作流程應用程式，然後再儲存，做為工作區。 此工作區可以匯出和匯入，以便您可以將配置複製到其他主機作為所需。 在主功能表底下**\] 工作區**中可以找到這些選項。 匯出的檔案將會位於系統可用磁碟機中`Dev Home\Workspaces`目錄。   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>重新調整大小並重新排序工具  
   
  
若要變更的大小或位置的工具，請使用操作功能表按鈕 （控制器上的 [檢視] 按鈕） 時標題具有焦點。 操作功能表中選取 [**移動**或**調整大小**。   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>變更佈景主題色彩和背景影像  
   
  
您可以從主功能表，選取**\] 工作區**，然後**變更佈景主題色彩**。 選取新的色彩，並選取 [**儲存**] 更新用於焦點醒目提示的佈景主題色彩。   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>設定預設的應用程式套件  
   
  
如果套件包含多個應用程式，開發人員首頁可讓您設定預設的應用程式啟動。 反白顯示的啟動程式中的套件，然後按下**A**按鈕來開啟可用的應用程式清單。 反白顯示的一個您想要設定為預設值並按下 [**檢視**] 按鈕，然後從操作功能表選擇 [**設為預設值**。   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>使用開發人員首頁註冊並啟動從網路共用的標題  
   
  
從已安裝應用程式和遊戲清單的底部的啟動器，您可以選取**註冊從網路共用的遊戲**的選項，從遠端執行標題的鬆散檔案版本。   
 ![](images/dev_home_8.png)   
  
您接著可以輸入網路路徑 appxmanifest.xml 檔案，以供您想要註冊的標題。 開發人員首頁將會嘗試註冊任何現有的認證使用適用於該共用，標題，而且需要將會提示您輸入新網路認證。 如果您需要存取其他的共用資料夾 （例如，象徵連結的存取個別的伺服器上的資源） 將需要新增這些透過下列選項。   
   
  
您可以管理這些儲存的認證 （並加入額外的） 透過主功能表**管理網路認證**選項主機上。   
 ![](images/dev_home_9.png)   
  
您可以檢視主機上目前的認證、 選取認證的路徑，並按一下 [ **A**按鈕編輯認證和移除認證選取移除連結，然後按一下 [ **A**按鈕。   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>本節內容  
  
[首頁 （開發人員首頁）](devhome-home.md)  


&nbsp;&nbsp;提供快速存取開發主機會定期執行的工作。 
  
  
[Xbox Live 頁面 （開發人員首頁）](devhome-live.md)  


&nbsp;&nbsp;擷取多人遊戲的資訊，並顯示的 Xbox Live 服務的目前狀態。 
  
  
[設定 \] 頁面 （開發人員首頁）](devhome-settings.md)  


&nbsp;&nbsp;提供存取權的開發主機的各種設定。 
  
  
[媒體擷取頁面 （開發人員首頁）](devhome-capture.md)  


&nbsp;&nbsp;開發人員首頁的**媒體擷取**頁面會擷取視訊的主機目前正在執行的標題。 
  
  
[網路功能頁面 （開發人員首頁）](devhome-networking.md)  


&nbsp;&nbsp;會模擬各種網路功能的條件來進行疑難排解。 
  
  
[效能頁面 （開發人員首頁）](devhome-performance.md)  


&nbsp;&nbsp;會模擬各種磁碟活動和 CPU 使用量條件來進行疑難排解。 
 