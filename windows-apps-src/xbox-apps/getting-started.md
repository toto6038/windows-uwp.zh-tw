---
title: 開始使用 Xbox One 上的 UWP 應用程式開發
description: 瞭解如何設定您的開發電腦和 Xbox One 主控台，以在 Xbox One 上開始使用通用 Windows 平臺 (UWP) 應用程式開發。
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 243f8972f1ea5879ee77f036d97c05e29d446b12
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304700"
---
# <a name="getting-started-with-uwp-app-development-on-xbox-one"></a>開始使用 Xbox One 上的 UWP 應用程式開發

**請謹慎**依照這些步驟來順利設定您的電腦和 Xbox One 以進行通用 Windows 平台 (UWP) 開發。 在您完成設定之後，即可在 [Xbox One 上的 UWP](index.md) 頁面上深入了解 Xbox One 上的開發人員模式，以及建置 UWP App。 

## <a name="before-you-start"></a>在您開始使用 Intune 之前

開始之前，您需要執行下列動作：
-   使用最新版本的 Windows 10 來設定電腦。
<!-- -  Install Microsoft Visual Studio 2015 Update 3 or Microsoft Visual Studio 2019.

    > [!NOTE]
    > Visual Studio 2019 is required if you are using the Windows 10, build 15063 SDK. -->

- 在 Xbox One 主機上至少有 5 GB 的可用空間。

## <a name="setting-up-your-development-pc"></a>設定開發電腦

1.  安裝 Visual Studio 2015 Update 3、Visual Studio 2017 或 Visual Studio 2019。

    如果您要安裝 Visual Studio 2015 Update 3，請務必選擇 [ **自訂** 安裝]，然後選取 [ **通用 Windows 應用程式開發工具** ] 核取方塊-它不是預設安裝的一部分。 如果您是 C++ 開發人員，請務必選擇 **\[自訂安裝\]**，並選取 **\[C++\]**。

    如果您要安裝 Visual Studio 2017 或 Visual Studio 2019，請務必選擇 **通用 Windows 平臺開發** 工作負載。 如果您是 c + + 開發人員，請在右側的 [ **摘要** ] 窗格中，于 [ **通用 Windows 平臺開發**] 底下，確定您選取 [ **c + + 通用 Windows 平臺工具** ] 核取方塊。 它不是預設安裝的一部分。

    如需詳細資訊，請參閱 [在 Xbox 開發環境上設定 UWP](development-environment-setup.md)。

2.  安裝最新的 [WINDOWS 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)。

3.  為開發電腦啟用開發人員模式 (**設定/更新 & 安全性/適用于開發人員/使用開發人員功能/開發人員模式**) 。

現在開發電腦已準備就緒，您可以觀看這段影片或繼續閱讀本文，了解如何設定您的 Xbox One 進行開發，以及建立和部署 UWP app。
</br>
</br>
<iframe src="https://channel9.msdn.com/Events/Xbox/App-Dev-on-Xbox/Get-started-with-App-Dev-on-Xbox/player#time=51s:paused" width="600" height="338"  allowFullScreen frameBorder="0"></iframe>

## <a name="setting-up-your-xbox-one-console"></a>設定 Xbox One 主機

1.  在 Xbox One上啟用開發人員模式。 下載應用程式、取得啟用代碼，然後將它輸入合作夥伴中心應用程式開發人員帳戶中的 [ **管理 Xbox One 主控台** ] 頁面。 如需詳細資訊，請參閱[啟用 Xbox One 開發人員模式](devkit-activation.md)頁面。 

2.  開啟 **開發模式啟用** 應用程式，然後選取 [ **切換並重新啟動**]。 恭喜，您現在已經有處於開發人員模式的 Xbox One 了！
  
  > [!NOTE]
  > 您的零售版遊戲和 App 將無法在開發人員模式中執行，但您所建立的 App 或遊戲則可以。 切換回零售模式，以執行您最愛的遊戲和 App。
    
  > [!NOTE]
  > 在您可以於開發人員模式中將 App 部署到 Xbox One 之前，主機上必須已有使用者登入。 您可以使用現有的 Xbox Live 帳戶，或在開發人員模式中為主機建立一個新帳戶。 

## <a name="creating-your-first-project-in-visual-studio"></a>在 Visual Studio 中建立您的第一個專案

如需詳細資訊，請參閱 [在 Xbox 開發環境上設定 UWP](development-environment-setup.md)。

1.  若**為 c #**：建立新的通用 Windows 專案，然後在**方案總管**中，以滑鼠右鍵按一下專案，然後選取 [**屬性**]。 選取 [**調試**程式] 索引標籤，將**目標裝置**變更為**遠端電腦**，在 [**遠端電腦**] 欄位中輸入 Xbox One 主控台的 IP 位址或主機名稱，然後選取 [**驗證模式]** 下拉式清單中的 [**通用 (未加密的通訊協定]) ** 。   

    您可以透過啟動主機上的開發人員首頁 (位於首頁右側的大型磚)，並查看左上角來找到 Xbox One IP 位址。 如需開發人員首頁的詳細資訊，請參閱 [Xbox One 工具簡介](introduction-to-xbox-tools.md)。  

2.  **針對 c + + 和 HTML/JAVAscript 專案**：您可以遵循類似 c # 專案的路徑，但在專案屬性中，請移至 [**調試**程式] 索引標籤，選取 [偵錯工具] 中的 [**遠端電腦**] 以開啟下拉式清單，在 [**電腦名稱稱**] 欄位中輸入主控台的 IP 位址或主機名稱，然後選取 [**驗證類型**] 欄位中的 [ **)  (通用**

3. 在頂端功能表列中，從 [綠色播放] 按鈕左邊的下拉式清單中選取 [ **x64** ]。
   
4.  按 F5 鍵時，您的 App 將會建置並開始部署到您的 Xbox One 上。
  
5.  第一次這樣做時，Visual Studio 會提示您輸入 Xbox One 的 PIN。 您可以在 Xbox One 上啟動開發人員首頁，然後選取 [ **顯示 Visual Studio 釘** 選] 按鈕，來取得 pin。
  
6.  配對之後，您的 App 就會開始部署。 第一次這樣做時，處理速度可能會有點慢 (我們必須將所有工具都複製到您的 Xbox)，但如果花費的時間超過數分鐘，則可能有發生錯誤。 請確定您已依照上述所有步驟進行 (特別您是否有將 **\[驗證模式\]** 設為 **\[通用\]**？)，並且使用有線網路連線來連線到 Xbox One。  

7. 請放鬆一下。 享受您在主機上執行的第一個 app！  

## <a name="thats-it"></a>這樣就完成了！

![Hello World](images/getting-started-hello-world.png)

## <a name="see-also"></a>另請參閱  
- [常見問題集](frequently-asked-questions.md)  
- [Xbox 開發人員計畫上的 UWP 已知問題](known-issues.md)
- [Xbox One 上的 UWP](index.md) 
