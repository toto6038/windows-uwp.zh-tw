---
author: Mtoepke
title: 在 Xbox 開發環境上設定 UWP
description: 在 Xbox 開發環境上設定並測試 UWP 的步驟。
area: Xbox
---

# 在 Xbox 開發環境上設定 UWP

Xbox 開發環境上的通用 Windows 平台 (UWP) 包含透過區域網路連線到 Xbox One 主機的開發電腦。
開發主機需具備 Windows 10、Visual Studio 2015 Update 2、Windows 10 SDK Preview 組建 14295，以及一系列支援工具。


本文涵蓋設定並測試開發環境的步驟。

## Visual Studio 安裝

1. 安裝 Visual Studio 2015 Update 2 或更新版本。 如需詳細資訊及如何安裝，請參閱[適用於 Windows 10 的下載項目與工具](https://dev.windows.com/downloads)。

1. 安裝 Visual Studio 2015 Update 2 時，請確保您已選取 [通用 Windows 應用程式開發工具]**** 核取方塊。

  ![安裝 Visual Studio 2015 Update 2](images/vs_install_tools.png)

## Windows 10 SDK 安裝

安裝 Windows 10 SDK Preview 組建 14295。 如需安裝資訊，請參閱[下載開發人員適用的 Insider Preview 更新](http://go.microsoft.com/fwlink/p/?LinkId=780552)。

  > **重要** &nbsp;&nbsp;您必須安裝最新的 SDK，但您並_不_需要安裝作業系統最新的 Windows Insider Preview 版本。

## 建立您的第一個應用程式

1. 請確保您的開發電腦位於和目標 Xbox One 主機相同的區域網路上。 這通常代表它們應該要使用相同的路由器，並位於相同的子網路上。 建議使用有線的網路連線。

1. 請確保您的 Xbox One 主機處於開發人員模式。  如需詳細資訊，請參閱[在 Xbox One 上啟用開發人員模式](devkit-activation.md)。

1. 決定您的 UWP App 要使用的程式語言。

1. 在您的開發電腦上，選取 [新增專案]****，然後選取 [Windows / 通用 / 空白應用程式]****。

### 開始 C# 專案

  ![[新增專案] 對話方塊](images/vs_universal_blank.jpg)

1. 選取 [新增通用 Windows 專案]**** 對話方塊中的預設選項。 如果出現 [開發人員模式]**** 對話方塊，按一下 [確定]****。 這將會建立新的空白應用程式。

1. 針對遠端偵錯設定開發環境：

  1. 以滑鼠右鍵按一下專案，然後選取 [內容]****。
  1. 在 [偵錯]**** 索引標籤上，將 [目標裝置]**** 變更為 [遠端電腦]****。
  1. 在 [遠端電腦]**** 中，輸入 Xbox One 主機的系統 IP 位址或主機名稱。 如需取得 IP 位址或主機名稱的資訊，請參閱 [Xbox One 工具簡介](introduction-to-xbox-tools.md)。
  1. 在 [驗證模式]**** 下拉式清單中，選取 [通用 (未加密通訊協定)]****。

    ![C++ BlankApp 屬性頁面](images/vs_remote.jpg)

### 開始 C++ 專案

  ![C++ 專案](images/vs_universal_cpp_blank.jpg)

1. 選取 [新增通用 Windows 專案]**** 對話方塊中的預設選項。 如果出現 [開發人員模式]**** 對話方塊，按一下 [確定]****。 這將會建立新的空白應用程式。

1. 針對遠端偵錯設定開發環境：

   1. 以滑鼠右鍵按一下專案，然後選取 [內容]****。
   1. 在 [偵錯]**** 索引標籤上，將 [要啟動的偵錯工具]**** 變更為 [遠端電腦]****。
   1. 在 [電腦名稱]**** 中，輸入 Xbox One 主機的系統 IP 位址或主機名稱。 如需取得 IP 位址或主機名稱的資訊，請參閱 [Xbox One 工具簡介](introduction-to-xbox-tools.md)。
   1. 在 [驗證類型]**** 下拉式清單中，選取 [通用 (未加密通訊協定)]****。

    ![C++ BlankApp 屬性頁面](images/vs_remote_cpp.jpg)

### 與 Visual Studio 以 PIN 配對您的裝置

1. 儲存您的設定，並確保您的 Xbox One 處於開發人員模式。

1. 按下 F5。

1. 如果這是您第一次部署，您將會收到來自 Visual Studio 的對話方塊，要求以 PIN 配對您的裝置。

  1. 若要取得 PIN，請從 Xbox One 主機上的主畫面開啟 [開發人員首頁]****。
  1. 選取 [與 Visual Studio 配對]****。

    ![[與 Visual Studio 配對] 對話方塊](images/devhome_visualstudio.png)

  1. 將您的 PIN 輸入 [與 Visual Studio 配對]**** 對話方塊。 下列的 PIN 只是個範例，您的 PIN 將會是不一樣的。

    ![[與 Visual Studio PIN 配對] 對話方塊](images/devhome_pin.png)

  1. 如果有部署錯誤，錯誤將會出現在 [輸出]**** 視窗中。

恭喜，您已成功在 Xbox 上建立並部署您的第一個 UWP App！



## 另請參閱
- [在 Xbox One 上啟用開發人員模式](devkit-activation.md)  
- [適用於 Windows 10 的下載項目與工具](https://dev.windows.com/downloads)  
- [下載開發人員適用的 Insider Preview 更新](http://go.microsoft.com/fwlink/?LinkId=780552)  
- [Xbox One 工具簡介](introduction-to-xbox-tools.md) 
- [Xbox One 上的 UWP](index.md)

----


<!--HONumber=May16_HO2-->


