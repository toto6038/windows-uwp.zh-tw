---
author: rido-min
Description: This guide explains how to configure your Visual Studio Solution to optimize the application binaries with native images.
Search.Product: eADQiWindows 10XVcnh
title: 最佳化您的.NET 桌面應用程式與原生映像
ms.author: normesta
ms.date: 06/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 原生映像編譯器
ms.localizationpriority: medium
ms.openlocfilehash: d98b576fb51a8f9507802796ab359d0d00d21998
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "2788270"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>最佳化您的.NET 桌面應用程式與原生映像

> [!NOTE]
> 正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。

您可以改善.NET Framework 應用程式的啟動時間預先編譯程式二進位檔案。 您可以使用這項技術在大型的封裝與散佈到 Windows 市集應用程式。 在某些情況下，我們已觀察到 20%的效能改進。 您可以深入了解 ＜[技術概觀 （英文)](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md)此技術。

我們已發行原生映像編譯器預覽的版本為[NuGet 套件](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler)。 您可以套用此套件是針對.NET Framework 版本 4.6.2 任何.NET Framework 應用程式或更新版本。 此套件新增包含原生承載至您的應用程式所使用的所有二進位檔案張貼建置步驟。 當此應用程式執行的.NET 4.7.2 和上方時舊版仍會載入 MSIL 程式碼會載入此最佳化的承載。

[.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/)隨附於[Windows 10 年 4 月 2018年更新](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/)。 您也可以安裝此版本的.NET Framework 電腦上的執行 Windows 7 + 與 Windows Server 2008 R2 +。

> [!IMPORTANT]
> 如果您想要產生的 Windows 應用程式封裝專案封裝應用程式的原生映像，請確定 Windows 紀念日 update 設定專案的目標平台最小版本。

## <a name="how-to-produce-native-images"></a>如何產生原生映像

請遵循這些指示來設定您的專案。

1. 設定的目標架構 4.6.2 或以上

2. 設定目標平台為 x86 或 x64 

3. 新增 NuGet 套件。

4. 建立版本的組建。

## <a name="configure-the-target-framework-as-462-or-above"></a>設定的目標架構 4.6.2 或以上

若要設定您要設為目標.NET Framework 4.6.2 的專案需要.NET Framework 4.6.2 開發工具或更新版本。 這些工具是可透過 Visual Studio 安裝程式以.NET 桌面開發工作負載下的選用元件：

![安裝.NET 4.6.2 開發工具](images/desktop-to-uwp/install-4.6.2-devpack.png)

或者，您可以取得從.NET 開發人員套件：[https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>設定目標平台為 x86 或 x64

原生映像編譯器最佳化給定的平台的程式碼。 若要使用它，您需要設定如 x86 或 x64 一個特定的平台為目標應用程式。

如果您有多個專案的解決方案，只有項目點專案 （很有可能會產生可執行檔專案） 已編譯為 x86 或 x64。 即使他們已編譯為 AnyCPU，將在主專案中，指定的架構與處理從主專案所參考的其他二進位檔案。

若要設定您的專案：

1. 以滑鼠右鍵按一下您的解決方案，然後選取 [ **Configuration Manager**。

2. 選取 [ **< 新.>** 會產生您可執行檔的專案名稱旁的**平台**的下拉式清單功能表中。

3. 在 [**新增專案平台**] 對話方塊中，請確定**複本設定從**下拉式清單會設為**任何 CPU**。

![設定 x86](images/desktop-to-uwp/configure-x86.png)

重複此步驟`Release/x64`如果您想要產生 x64 二進位檔案。

>[!IMPORTANT]
> 原生映像編譯器不支援 AnyCPU 組態。

## <a name="add-the-nuget-packages"></a>新增 NuGet 套件

原生映像編譯器會提供為您要新增至 Visual Studio 專案產生可執行檔 NuGet 套件。 這通常是 Windows Form 或 WPF 專案。 使用下列 PowerShell 命令來執行。

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> 預覽套件會在 NuGet.org 中發佈為未列出。 您將不會尋找其瀏覽 NuGet.org 或使用 Visual Studio 中的封裝管理員 UI。 不過，您可以從封裝管理員主控台中，安裝它們與當您還原從不同的電腦。 我們要封裝完全可以存取我們將發佈的第一個非預覽版本時。

## <a name="create-a-release-build"></a>建立版本的組建

NuGet 封裝設定要執行的版本的組建其他工具的專案。 此工具會將原生的程式碼新增至相同的二進位檔案。
若要確認此工具已處理的二進位檔案可以檢閱以確定其包含的訊息等這一個輸出組建：

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>常見問題

**Q。 新的二進位檔案工作不含.NET Framework 4.7.2 機器吗？**

A. 最佳化的二進位檔案有益改良功能以.NET Framework 4.7.2 執行時。 執行先前的.NET framework 版本的用戶端將會從二進位載入的非最佳化 MSIL 程式碼。

**Q。 如何提供意見反應或報告問題？**

A. 使用 Visual Studio 2017 中的意見反應工具報告發生問題。 [更多的資訊](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017)。

**Q。 新增至現有的二進位檔案的原生映像的影響為何？**

A. 最佳化的二進位檔案包含受管理與原生程式碼，讓更大的最後一個檔案。

**Q。 可以釋出使用此技術的二進位檔案吗？**

A. 此版包含您可以利用移 Live 授權。
