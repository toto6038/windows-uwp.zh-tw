---
author: JordanEllis6809
title: 將 Unity 遊戲移到 Xbox 上的 UWP
description: 在 Xbox 上進行 Unity UWP 開發。
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: fca3267a-0c0f-4872-8017-90384fb34215
ms.localizationpriority: medium
ms.openlocfilehash: f9851b76b218ed4241ccb617979c127d03f57ee7
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2018
ms.locfileid: "6271630"
---
# <a name="bringing-unity-games-to-uwp-on-xbox"></a>將 Unity 遊戲移到 Xbox 上的 UWP


在這個逐步教學課程中，我們假設您已經有 Unity 的遊戲，並已準備好建置和部署。

另請參閱[本教學課程的影片版本](https://www.youtube.com/watch?v=f0Ptvw7k-CE)。

想要管理您 Unity UWP 專案的版本嗎？ 請參閱[針對您的 UWP 專案進行版本控制](development-lanes-unity-versioning.md)。

## <a name="step-0-ensure-unity-is-installed-correctly"></a>步驟 0：確定 Unity 已正確安裝

安裝 Unity 時，必須選取以下元件：

![Unity 安裝元件](images/unity-install-components.png)

## <a name="step-1-building-the-uwp-solution"></a>步驟 1︰建置 UWP 方案

在您的 Unity 遊戲專案中，開啟位於 **\[檔案\]** -&gt; **\[建置設定\]** 的 [建置設定] 視窗，然後移至 Microsoft Store 選項功能表。

![建置設定視窗](images/build-settings.png)

請確定 **\[SDK\]** 設定已設定為 **\[Universal 10\]**，然後選取 **\[建置\]** 按鈕，這將會啟動檔案總管視窗並詢問目的地資料夾。 在專案的 **\[資產\]** 目錄旁邊建立名為 **UWP** 的資料夾，然後選擇此資料夾做為建置的目的地資料夾。

![建置目的地資料夾](images/build-destination.png)

Unity 現在已經建立了將用來部署 UWP 遊戲的新 Visual Studio 解決方案。

![UWP VS 解決方案](images/uwp-vs-solution.png)

## <a name="step-2-deploying-your-game"></a>步驟 2︰部署您的遊戲

開啟在 **UWP** 資料夾中新產生的解決方案，然後將目標平台變更為 **\[x64\]**。

![x64 建置平台](images/x64-build-platform.png)

現在您已經有遊戲的 UWP Visual Studio 解決方案，[遵循下列步驟](getting-started.md)將可讓您順利將遊戲部署到零售版 Xbox One！

## <a name="step-3-modify-and-rebuild"></a>步驟 3︰修改並重新建置

如果您對指令碼以外的項目做出變更，若要將這些變更套用到您遊戲的 UWP 建置中，專案必須在編輯器中重新建置 (以__步驟 1__ 中所述的方式)。

## <a name="versioning-your-uwp-project"></a>管理 UWP 專案的版本

在一些常見的情況下，將新產生之 UWP 目錄的一部份加入版本控制，變成一項必要的動作。 例如，假如您正在新增新的相依性到 UWP 專案 (例如 Xbox Live SDK)。  我們會在[針對您的 UWP 專案進行版本控制](development-lanes-unity-versioning.md)中詳細討論此範例。

## <a name="see-also"></a>另請參閱
- [將現有的遊戲移到 Xbox](development-lanes-landing.md)
- [Xbox One 上的 UWP](index.md)
