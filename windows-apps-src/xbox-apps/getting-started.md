---
author: Mtoepke
title: "開始使用 Xbox One 上的 UWP 應用程式開發"
description: "如何設定您的電腦和 Xbox One 以進行 UWP 開發。"
area: Xbox
ms.sourcegitcommit: bdf7a32d2f0673ab6c176a775b805eff2b7cf437
ms.openlocfilehash: b070b6bec350c3c893f934e212b1de743b349861

---

#開始使用 Xbox One 上的 UWP 應用程式開發

**請謹慎**依照這些步驟順利設定您的電腦和 Xbox One 以進行 UWP 開發。 在您完成設定之後，即可深入了解 Xbox One 上的開發人員模式，以及在 \[[UWP for Xbox One](index.md)\] 頁面上建置 UWP 應用程式。 

## 開始之前
開始之前，您需要執行下列動作：
-   建立 [Windows 開發人員中心](https://dev.windows.com)帳戶。
-   加入 [Windows 測試人員計畫](https://insider.windows.com/)。 您需要有此項目才能取得 Preview Windows SDK。
-   設定 Windows 10 電腦 (任何版本都會包括最新 Windows 10 測試人員正式發行前小眾測試版)；針對此 Preview，我們的開發工具需要您執行 Windows 10。 
-   將您的 Xbox One 主機連線到網路。 為獲得最佳效能，請使用有線連線。
- Xbox One 主機上至少有 5 GB 的可用空間。

## 設定開發電腦
1.  安裝 Visual Studio 2015 Update 2。 確定您選擇 \[**自訂**\] 安裝，然後選取 \[**通用 Windows 應用程式開發工具**\] 核取方塊 (它不是預設安裝的一部分)。 如需詳細資訊，請參閱[開發環境設定](development-environment-setup.md) (而且，如果您是 C++ 開發人員，請確定選擇 \[自訂\] 安裝，同時選取 C++)。

2.  安裝最新的 Windows 10 SDK 預覽版。 您可以從 [Windows 測試人員計畫](http://go.microsoft.com/fwlink/p/?LinkId=780552)取得此項目。
  
  > **重要** &nbsp;&nbsp;在電腦上安裝此 Preview SDK，會讓您無法將應用程式提交到此電腦上所建置的存放區，因此請不要在生產開發電腦上執行這項作業。 

## 設定 Xbox One 主機
1.  在 Xbox One上啟用開發人員模式 下載應用程式，並取得啟用碼，然後將它輸入您開發人員中心帳戶中的 xboxactivate 頁面。 如需詳細資訊，請參閱[在 Xbox One 上啟用開發人員模式](devkit-activation.md)。 

2.  等到 Xbox One 取得系統更新，以執行開發人員預覽。 這最多需要 4 小時。 如果您不想等待，請按住電源按鈕 10 秒，然後重新開機，來強制重新啟動主機。 這會觸發更新。  

3.  前往「啟用開發人員模式」應用程式，然後選取 \[**切換並重新啟動**\]。 恭喜，您現在已經有處於開發人員模式的 Xbox One 了！
  
  > **注意** &nbsp;&nbsp;您的零售版遊戲和應用程式無法在開發人員模式中執行，但您建立的應用程式或遊戲可以。 切換回零售模式，以執行您最愛的遊戲和應用程式。
  
  > **注意** &nbsp;&nbsp;在您於開發人員模式中將 app 部署到 Xbox One 之前，主機上必須有已有使用者登入。 您可以使用現有的 Xbox Live 帳戶，或在開發人員模式中為主機建立一個新帳戶。 

## 在 Visual Studio 2015 中建立第一個專案

如需詳細資訊，請參閱[開發環境設定](development-environment-setup.md)。

1.  針對 C#：建立新的通用 Windows 專案、前往專案屬性，並選取 \[**偵錯**\] 索引標籤、將 \[**目標裝置**\] 變更為 \[**遠端電腦**\]、將 Xbox One 的 IP 位址或主機名稱輸入 \[**遠端電腦**\] 欄位，並在 \[**驗證模式**\] 下拉式清單中選取 \[**通用 (未加密的通訊協定)**\]。   

    啟動主機上的 開發首頁 \(首頁 右側的大型磚\)，並查看左上角，即可找到 Xbox One IP 位址。 如需 開發首頁 的詳細資訊，請參閱 [Xbox One 工具簡介](introduction-to-xbox-tools.md)。  

2.  針對 C++ 和 HTML/Javascript 專案：您是依照類似的路徑進行，但在專案屬性中前往 \[**偵錯**\] 索引標籤、在偵錯工具中選取 \[**遠端電腦**\] 來開啟下拉式清單、將主機的 IP 位址或主機名稱輸入 \[**電腦名稱**\] 欄位，並在 \[**驗證類型**\] 欄位中選取 \[**通用 (未加密的通訊協定)**\]。
   
3.  按 F5 鍵時，您的應用程式將會建置並開始部署在您的 Xbox One 上。
  
4.  第一次這樣做時，Visual Studio 會提示您輸入 Xbox One 的 PIN。 您可以在 Xbox One 上啟動 開發首頁，並按 \[**與 Visual Studio 配對**\] 按鈕，以取得 PIN。
  
5.  配對之後，就會開始部署您的應用程式。 第一次這樣做時，可能會有點慢 (我們需要將所有工具都複製到您的 Xbox)，但是，如果超過數分鐘，則可能發生錯誤。 請確定您已依照上述所有步驟進行 (特別是，是否將 \[**驗證模式**\] 設為 \[**通用**\]？)，並且使用有線網路連線來連線到 Xbox One。  

6. 放鬆一下。 享受您在主機上執行的第一個應用程式！  
   ![Hello World](images/getting-started-hello-world.png)
   

## 另請參閱  
- [常見問題集](frequently-asked-questions.md)  
- [已知問題](known-issues.md)
- [Xbox One 上的 UWP](index.md)



<!--HONumber=Jun16_HO5-->


