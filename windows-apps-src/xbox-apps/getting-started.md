---
title: 開始使用 Xbox One 上的 UWP 應用程式開發
description: 如何設定您的電腦和 Xbox One 以進行 UWP 開發。
ms.date: 10/12/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 19756730177485c6d16ad9a42ff1174eba8ca3b9
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2019
ms.locfileid: "67820313"
---
# <a name="getting-started-with-uwp-app-development-on-xbox-one"></a>開始使用 Xbox One 上的 UWP 應用程式開發

**請謹慎**依照這些步驟來順利設定您的電腦和 Xbox One 以進行通用 Windows 平台 (UWP) 開發。 在您完成設定之後，即可在 [Xbox One 上的 UWP](index.md) 頁面上深入了解 Xbox One 上的開發人員模式，以及建置 UWP App。 

## <a name="before-you-start"></a>開始之前

開始之前，您需要執行下列動作：
-   設定 Windows 10 最新版本的電腦。
<!-- -  Install Microsoft Visual Studio 2015 Update 3 or Microsoft Visual Studio 2019.

    > [!NOTE]
    > Visual Studio 2019 is required if you are using the Windows 10, build 15063 SDK. -->

- 在 Xbox One 主機上至少有 5 GB 的可用空間。

## <a name="setting-up-your-development-pc"></a>設定開發電腦

1.  安裝 Visual Studio 2015 Update 3，Visual Studio 2017 或 Visual Studio 2019。

    如果您要安裝 Visual Studio 2015 Update 3，請確定您選擇**自訂**安裝，並選取**通用 Windows App 開發工具**核取方塊 – 它不是預設的一部分安裝。 如果您是 C++ 開發人員，請務必選擇 [自訂安裝]  ，並選取 [C++]  。

    如果您要安裝 Visual Studio 2017 或 Visual Studio 2019，請確定您選擇**通用 Windows 平台開發**工作負載。 如果您是C++開發人員，請在**摘要**右邊的窗格底下**通用 Windows 平台開發**，請確定您選取**C++通用 Windows平台工具**核取方塊。 它不是預設安裝的一部分。

    如需詳細資訊，請參閱 <<c0> [ 設定您在 Xbox 開發環境上的 UWP](development-environment-setup.md)。

2.  安裝最新[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)。

3.  啟用開發人員模式的開發電腦 (**設定 / 更新與安全性 / 適用於開發人員使用開發人員功能 / 開發人員模式**)。

現在開發電腦已準備就緒，您可以觀看這段影片或繼續閱讀本文，了解如何設定您的 Xbox One 進行開發，以及建立和部署 UWP app。
</br>
</br>
<iframe src="https://channel9.msdn.com/Events/Xbox/App-Dev-on-Xbox/Get-started-with-App-Dev-on-Xbox/player#time=51s:paused" width="600" height="338"  allowFullScreen frameBorder="0"></iframe>

## <a name="setting-up-your-xbox-one-console"></a>設定 Xbox One 主機

1.  在 Xbox One上啟用開發人員模式。 下載應用程式，取得啟用程式碼，然後輸入到**管理 Xbox One 主控台**合作夥伴中心應用程式開發人員帳戶中的頁面。 如需詳細資訊，請參閱[啟用 Xbox One 開發人員模式](devkit-activation.md)頁面。 

2.  開啟**開發人員模式下啟動**應用程式並選取**參數，並重新啟動**。 恭喜，您現在已經有處於開發人員模式的 Xbox One 了！
  
  > [!NOTE]
  > 您的零售版遊戲和 App 將無法在開發人員模式中執行，但您所建立的 App 或遊戲則可以。 切換回零售模式，以執行您最愛的遊戲和 App。
    
  > [!NOTE]
  > 在您可以於開發人員模式中將 App 部署到 Xbox One 之前，主機上必須已有使用者登入。 您可以使用現有的 Xbox Live 帳戶，或在開發人員模式中為主機建立一個新帳戶。 

## <a name="creating-your-first-project-in-visual-studio"></a>在 Visual Studio 中建立第一個專案

如需詳細資訊，請參閱 <<c0> [ 設定您在 Xbox 開發環境上的 UWP](development-environment-setup.md)。

1.  **針對C#** :建立新的通用 Windows 專案，然後在**方案總管**，以滑鼠右鍵按一下專案，然後選取**屬性**。 選取**偵錯**索引標籤中變更**目標裝置**來**遠端機器**，輸入 IP 位址或主機名稱到您的 Xbox One 主控台**遠端電腦**欄位，然後選取**通用 （未加密的通訊協定）** 中**Mode**下拉式清單。   

    您可以透過啟動主機上的開發人員首頁 (位於首頁右側的大型磚)，並查看左上角來找到 Xbox One IP 位址。 如需開發人員首頁的詳細資訊，請參閱 [Xbox One 工具簡介](introduction-to-xbox-tools.md)。  

2.  **針對C++和 HTML/Javascript 專案**:請遵循類似的路徑C#專案，但在專案屬性中移至**偵錯**索引標籤上，選取**遠端機器**在偵錯工具若要開啟下拉式清單中，輸入 IP 位址或主機名稱到主控台**電腦名稱**欄位，然後選取**通用 （未加密的通訊協定）** 中**驗證類型**欄位。

3. 選取  **x64**從頂端功能表列中的綠色的播放按鈕的左邊的下拉式清單。
   
4.  按 F5 鍵時，您的 App 將會建置並開始部署到您的 Xbox One 上。
  
5.  第一次這樣做時，Visual Studio 會提示您輸入 Xbox One 的 PIN。 您可以取得 pin 碼以在您的 Xbox One 上開始開發人員的首頁，然後選取**顯示 Visual Studio pin**  按鈕。
  
6.  配對之後，您的 App 就會開始部署。 第一次這樣做時，處理速度可能會有點慢 (我們必須將所有工具都複製到您的 Xbox)，但如果花費的時間超過數分鐘，則可能有發生錯誤。 請確定您已依照上述所有步驟進行 (特別您是否有將 [驗證模式]  設為 [通用]  ？)，並且使用有線網路連線來連線到 Xbox One。  

7. 請放鬆一下。 享受您在主機上執行的第一個 app！  

## <a name="thats-it"></a>就這麼容易！

![Hello World](images/getting-started-hello-world.png)

## <a name="see-also"></a>另請參閱  
- [常見問題集](frequently-asked-questions.md)  
- [在 Xbox 開發人員計劃上的 UWP 的已知的問題](known-issues.md)
- [在 Xbox One UWP](index.md) 
