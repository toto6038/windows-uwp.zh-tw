---
author: Mtoepke
title: "Xbox One 工具簡介"
description: "使用 Windows Device Portal 的 Xbox One 特有工具「開發人員首頁」。"
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 4414677e942818506020888fa15e7e16ecaf4733

---

# Xbox One 工具簡介

本節涵蓋使用 Windows Device Portal 的 Xbox One 特有工具_開發人員首頁_。

## 開發人員首頁

_開發人員首頁_是 Xbox One Development Kit 上的一種工具體驗，設計來協助開發人員提高生產力。 「開發人員首頁」提供功能來管理和設定您的開發人員套件。

若要開啟開發人員首頁，請選取主畫面上的 [開發人員首頁]**** 磚。 如果未出現任何磚，表示主機未處於開發人員模式。

  ![Windows Device Portal](images/windowsdeviceportal_1.png)

### 使用者介面
「開發人員首頁」使用者介面被分成於下列各節所述的區域。 請注意，主機 IP 位址和易記名稱皆在此顯示。

  ![DevHome UI](images/devhome_ui.png)

#### 標頭
標頭包含開發人員套件的重要「簡介」資訊。 這包括主機名稱、其 IP 位址、所在的 Xbox Live 沙箱，以及執行的作業系統版本。 在標頭右側，會基於方便起見而顯示目前系統時間和日期。

#### 工具視窗
標頭下方是應用程式的主要區域，內含一組可設定的工具視窗。 這些是要讓開發人員自訂應用程式，以提供各種工具和資訊集的存取權。 如需工具的詳細資料，請參閱下列個別工具描述。 如需如何設定工具視窗的版面配置和外觀的相關資訊，請參閱本頁面後面的[自訂開發人員首頁](#customizing-dev-home)小節。

##### 主功能表
按下您控制器上的 [功能表]**** 按鈕，或瀏覽到畫面左上角的功能表 (“Hamburger”) 按鈕，可以存取主功能表，以讓您設定應用程式工作區的佈景主題色彩和背景影像，並提供應用程式的意見反應。

  ![主功能表](images/devhome_mainmenu.png)

#### 貼齊模式
「開發人員首頁」中的工具可在您執行標題時貼齊側邊，讓您可以在測試時輕鬆地存取工具。


若要存取 [貼齊]**** 模式，請選取適當工具的標題、按控制器上的 [檢視]**** 按鈕，以及選取操作功能表上的 [貼齊]****。

  ![貼齊模式](images/devhome_snapmode.png)

開發人員首頁將貼齊右側。 如常點選兩次 [Nexus]**** 按鈕，即可切換內容。

  ![Nexus](images/devhome_nexus.png)

##### 工具描述
| 工具  | 功能 |
|-------|--------------|
| 遊戲與應用程式  | 列出已在開發人員套件上安裝的標題和應用程式，以及快速開啟它們的功能。 您也可以檢視遊戲和應用程式的處理程序生命週期管理 (PLM) 狀態，並從操作功能表變更 PLM 狀態。 |
| 使用者 | 列出目前在主機上註冊的使用者。 啟用按一下使用者登出/登出、新增使用者和來賓，以及檢視使用者和來賓的詳細資料。 |
| [主機設定](#console-settings) | 提供「簡介」檢視以及主機設定和資訊的的編輯選項。 |
| Visual Studio | 可讓您將主機與允許部署的 Visual Studio 執行個體配對。 必要時，請清除所有現有配對的 VS 執行個體，以防止 UWP 應用程式部署到套件。 |
| [Windows Device Portal](#windows-device-portal) | 在套件上啟用 WDP (瀏覽器型裝置管理工具)。 |
| Xbox Live 狀態 | 提供 Xbox Live 服務的目前狀態。 |

### 管理開發人員儲存區配置的大小

若要增加或減少用於開發人員儲存區的磁碟空間大小，請選取主功能表中的 [管理開發人員儲存區]****。 變更 [開發人員儲存區]**** 軸的值，然後選取 [儲存並重新啟動]**** 來將主機重新啟動。
  ![管理開發人員儲存區配置](images/devhome_storage.png)

### 自訂開發人員首頁

開發人員首頁設計成可進行自訂與個人化。 您可以選擇背景影像和佈景主題色彩，來個人化您的開發人員首頁體驗。 在主功能表上可以找到這些選項。

#### 調整工具大小並重新排序工具
若要變更工具的大小或位置，請在標題具有焦點時使用操作功能表按鈕 (控制器上的**檢視** 按鈕) 在操作功能表上，選取 [移動]**** 或 [調整大小]****。

  ![移動或調整大小](images/devhome_move.png)

#### 變更佈景主題色彩和背景影像
在主功能表上，您可以選取 [變更佈景主題色彩]****。 若要更新用於焦點醒目提示的佈景主題色彩，請選取新的色彩，然後按一下 [儲存]****。

  ![變更佈景主題色彩](images/devhome_colors.png)

### 提供意見反應
若要提供對開發人員首頁或任何工具處理程序的意見反應，請選取主功能表上的 [提供意見反應]**** 選項。

  ![提供意見反應](images/devhome_feedback.png)

## 主機設定
主機設定工具可快速存取開發人員套件的設定。

### 設定主機的主機名稱
從您的開發電腦與主機進行通訊時，可以設定易記名稱 (稱為_主機名稱_)，讓 Xbox One 開發人員套件當成主機 IP 位址的替代項目來使用。 您的開發電腦和開發人員套件必須位在相同的子網路上，主機名稱連線才能運作。  

若要定義開發人員套件的主機名稱，請移至主機設定工具，然後在 [主機名稱]____ 方塊中輸入主機名稱。  

  > **注意**&nbsp;&nbsp;建立主機名稱時，不會強制執行名稱唯一性。 請注意避免名稱重複。 其中一種做法是從開發電腦名稱衍生主機名稱，這在組織內通常是唯一的。

## Windows Device Portal
Windows Device Portal (WDP) 是一種 OneCore 裝置管理工具，允許瀏覽器型裝置管理體驗。

在 Xbox One 主機上啟用 WDP：

1. 選取主畫面上的 [開發人員首頁] 磚。

  ![選取 [開發人員首頁] 磚](images/windowsdeviceportal_1.png)

2. 在開發人員首頁內，瀏覽到 [遠端管理]**** 工具。

  ![遠端管理工具](images/windowsdeviceportal_2.png)

3. 選取 [管理 Windows Device Portal]____ 然後按下 __A__。
4. 選取 [啟用 Windows Device Portal]____ 核取方塊。
5. 輸入__使用者名稱__和__密碼__，並儲存它們。 這些是用來從瀏覽器驗證對您開發人員套件的存取權。
6. 關閉 [設定]____ 頁面，然後記下列在 [遠端管理]__ 工具上用來連線的 URL。
7. 將該 URL 輸入您的瀏覽器，然後以您設定的認證登入。
8. 您將收到有關所提供憑證的警告 (與下列螢幕擷取畫面類似)，因為 Xbox One 主機所簽署的安全性憑證不是視為已知的受信任發行者。 按一下 [繼續瀏覽此網站]**** 來存取 Windows Device Portal。

  ![安全性憑證警告](images/security_cert_warning.jpg)

## 另請參閱
- [開發 UWP 時如何使用 Fiddler 搭配 Xbox One](uwp-fiddler.md)
- [Microsoft Developer 技術︰Windows Device Portal](https://msdn.microsoft.com/windows/uwp/debug-test-perf/device-portal-xbox)
- [Xbox One 上的 UWP](index.md)



----



<!--HONumber=Jul16_HO2-->


