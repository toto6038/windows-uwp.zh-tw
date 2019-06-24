---
title: 在 Xbox 開發環境上設定 UWP
description: 在 Xbox 開發環境上設定並測試 UWP 的步驟。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 8801c0d9-94a5-41a2-bec3-14f523d230df
ms.localizationpriority: medium
ms.openlocfilehash: 02c33e0dbe1209f3c31937df800ceecb354475f5
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322127"
---
# <a name="set-up-your-uwp-on-xbox-development-environment"></a>在 Xbox 開發環境上設定 UWP

Xbox 開發環境上的通用 Windows 平台 (UWP) 包含透過區域網路連線到 Xbox One 主機的開發電腦。
開發電腦需具備 Windows 10、Visual Studio 2017 或 Visual Studio 2015 Update 3、Windows 10 SDK 組建 14393 或更新版本，以及一系列支援工具。


本文涵蓋設定並測試開發環境的步驟。

## <a name="visual-studio-setup"></a>Visual Studio 安裝

1. 安裝 Visual Studio 2017，Visual Studio 2015 Update 3 或 Visual Studio 的最新版本。 如需詳細資訊及如何安裝，請參閱[適用於 Windows 10 的下載項目與工具](https://developer.microsoft.com/windows/downloads)。 我們建議您，讓您可以為開發人員和安全性接收最新的更新時，才使用最新版本的 Visual Studio。

2. 如果要安裝 Visual Studio 2017，請確定您選擇 **\[通用 Windows 平台開發\]** 工作負載。 如果您是 C++ 開發人員，請務必也選取右側 **[摘要]** 中的 **[C++ 通用 Windows 平台工具]** 核取方塊 (在 **[通用 Windows 平台開發]** 底下)。 這不是預設安裝的一部分。

    ![安裝 Visual Studio 2017](images/development-environment-setup-1.png)

    若要安裝 Visual Studio 2015 Update 3，請確保您已選取 **\[通用 Windows 應用程式開發工具\]** 核取方塊。

    ![安裝 Visual Studio 2015 Update 2](images/vs_install_tools.png)

## <a name="windows-10-sdk-setup"></a>Windows 10 SDK 安裝

安裝最新版本的 Windows 10 SDK。 這是隨附於您的 Visual Studio 安裝，但如果您想要個別下載，請查看[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)。


## <a name="enabling-developer-mode"></a>啟用開發人員模式

從開發電腦部署應用程式之前，您必須啟用開發人員模式。 在 **\[設定\]** app 中，瀏覽至 **\[更新與安全性\]**  /  **\[適用於開發人員\]** ，然後在 **\[使用開發人員功能\]** ，選取 **\[開發人員模式\]** 。

## <a name="setting-up-your-xbox-one"></a>設定 Xbox One

在您可以將 App 部署到 Xbox One 之前，主機上必須已有使用者登入。 您可以使用現有的 Xbox Live 帳戶，或在開發人員模式中為主機建立一個新帳戶。 

## <a name="create-your-first-app"></a>建立您的第一個 App

1. 請確保您的開發電腦位於和目標 Xbox One 主機相同的區域網路上。 這通常代表它們應該要使用相同的路由器，並位於相同的子網路上。 建議使用有線的網路連線。

2. 請確保您的 Xbox One 主機處於開發人員模式。  如需詳細資訊，請參閱[啟用 Xbox One 開發人員模式](devkit-activation.md)頁面。

3. 決定您的 UWP App 要使用的程式語言。

4. 在開發電腦上，選取 Visual studio 中的 **\[新增 / 專案\]** 。

5. 在 **\[新專案\]** 視窗中，選取 **\[Windows 通用 / 空白應用程式 (通用 Windows)\]** 。

### <a name="starting-a-c-project"></a>開始 C# 專案

  ![[新增專案] 對話](images/development-environment-setup-2.png)

1. 在 **\[新增通用 Windows 專案\]** 對話方塊中，選取 **\[最小版本\]** 下拉式清單中的組建 14393 或更新版本。 在 **\[目標版本\]** 下拉式清單選取最新的 SDK。 如果出現 [開發人員模式]  對話方塊，按一下 [確定]  。 這將會建立新的空白應用程式。

2. 針對遠端偵錯設定開發環境：

    a. 在 **\[方案總管\]** ，在專案上按一下滑鼠右鍵，然後選取 **\[內容\]** 。

    b. 在 **\[偵錯\]** 索引標籤上，將 **\[平台\]** 變更為 **\[x64\]** 。 (x86 已不再是 Xbox 上支援的平台。)

    c. 在 **\[起始選項\]** 下，將 **\[目標裝置\]** 變更為 **\[遠端電腦\]** 。

    d. 在 [遠端電腦]  中，輸入 Xbox One 主機的系統 IP 位址或主機名稱。 如需取得 IP 位址或主機名稱的資訊，請參閱 [Xbox One 工具簡介](introduction-to-xbox-tools.md)。

    e. 在 [驗證模式]  下拉式清單中，選取 [通用 (未加密通訊協定)]  。

    ![C++ BlankApp 屬性頁面](images/vs_remote.jpg)

### <a name="starting-a-c-project"></a>開始 C++ 專案

  ![C++ 專案](images/development-environment-setup-3.png)

1. 在 **\[新增通用 Windows 專案\]** 對話方塊中，選取 **\[最小版本\]** 下拉式清單中的組建 14393 或更新版本。 在 **\[目標版本\]** 下拉式清單選取最新的 SDK。 如果出現 [開發人員模式]  對話方塊，按一下 [確定]  。 這將會建立新的空白應用程式。

2. 針對遠端偵錯設定開發環境：

   a. 在 **\[方案總管\]** ，在專案上按一下滑鼠右鍵，然後選取 **\[內容\]** 。

   b. 在 [偵錯]  索引標籤上，將 [要啟動的偵錯工具]  變更為 [遠端電腦]  。

   c. 在 [電腦名稱]  中，輸入 Xbox One 主機的系統 IP 位址或主機名稱。 如需取得 IP 位址或主機名稱的資訊，請參閱 [Xbox One 工具簡介](introduction-to-xbox-tools.md)。

   d. 在 [驗證類型]  下拉式清單中，選取 [通用 (未加密通訊協定)]  。

   e. 在 **\[平台\]** 下拉式清單中選取 **\[x64\]** 。

    ![C++ BlankApp 屬性頁面](images/development-environment-setup-4.png)

### <a name="pin-pair-your-device-with-visual-studio"></a>與 Visual Studio 以 PIN 配對您的裝置

1. 儲存您的設定，並確保您的 Xbox One 處於開發人員模式。

2. 在 Visual Studio 中開啟專案，然後按下 F5。

3. 如果這是您第一次部署，您將會收到來自 Visual Studio 的對話方塊，要求以 PIN 配對您的裝置。

    a. 若要取得 PIN，請從 Xbox One 主機上的主畫面開啟 [開發人員首頁]  。

    b. 在 **\[首頁\]** 索引標籤，在 **\[快速控制項目\]** 下選取 **\[顯示 Visual Studio Pin\]** 。
  
    ![[與 Visual Studio 配對] 對話方塊](images/development-environment-setup-5.png)

    c. 將您的 PIN 輸入 [與 Visual Studio 配對]  對話方塊。 下列的 PIN 只是個範例，您的 PIN 將會是不一樣的。

    ![[與 Visual Studio PIN 配對] 對話方塊](images/devhome_pin.png)

    d. 如果有部署錯誤，錯誤將會出現在 [輸出]  視窗中。

恭喜，您已成功在 Xbox 上建立並部署您的第一個 UWP App！

## <a name="see-also"></a>另請參閱
- [啟用 Xbox One 開發人員模式](devkit-activation.md)  
- [下載與工具，適用於 Windows 10](https://developer.microsoft.com/windows/downloads)  
- [Windows 測試人員計畫](https://go.microsoft.com/fwlink/?LinkId=780552)  
- [Xbox One 工具簡介](introduction-to-xbox-tools.md) 
- [在 Xbox One UWP](index.md)