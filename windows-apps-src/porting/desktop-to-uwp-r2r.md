---
Description: 本指南說明如何設定 Visual Studio 方案，以最佳化應用程式二進位檔與原生映像。
Search.Product: eADQiWindows 10XVcnh
title: 最佳化您的.NET 桌面應用程式，使用原生映像
ms.date: 06/11/2018
ms.topic: article
keywords: windows 10 中，編譯器的原生映像
ms.localizationpriority: medium
ms.openlocfilehash: 3071b843a1605d765ab5b087d5e1bfb96a220218
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642873"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>最佳化您的.NET 桌面應用程式，使用原生映像

> [!NOTE]
> 正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。

您可以預先編譯的二進位檔，以改善您的.NET Framework 應用程式的啟動時間。 您可以使用這項技術的封裝，並透過 Microsoft Store 散發的大型應用程式。 在某些情況下，我們觀察到 20%的效能改進。 您可以深入了解這種技術[技術概觀](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md)。

我們發行了預覽版本的原生映像編譯器做[NuGet 套件](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler)。 您可以將此套件套用至任何.NET Framework 4.6.2 版為目標的.NET Framework 應用程式或更新版本。 這個套件會將 post 的建置步驟，其中包含應用程式所使用的所有二進位檔的原生裝載。 當應用程式在.NET 4.7.2 和更新版本以前的版本仍然會載入的 MSIL 程式碼時，將會載入此最佳化的承載。

[.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/)納入[Windows 10 April 2018 update](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/)。 您也可以在電腦上執行 Windows 7 + 和 Windows Server 2008 R2 + 安裝這個版本的.NET Framework。

> [!IMPORTANT]
> 如果您想要產生原生映像封裝的 Windows 應用程式封裝專案的應用程式，請務必將專案的目標平台最低版本設定為 Windows 年度更新版。

## <a name="how-to-produce-native-images"></a>如何產生原生映像

請遵循下列指示來設定您的專案。

1. 設定目標架構，為 4.6.2 或更新版本

2. 將目標平台設定為 x86 或 x64 

3. 新增 NuGet 套件。

4. 建立發行組建。

## <a name="configure-the-target-framework-as-462-or-above"></a>設定目標架構，為 4.6.2 或更新版本

若要設定專案目標.NET Framework 4.6.2 中，您會需要的.NET Framework 4.6.2 開發工具或更新版本。 這些工具都是透過 Visual Studio 安裝程式為.NET 桌面開發工作負載的選用元件：

![安裝.NET 4.6.2 開發工具](images/desktop-to-uwp/install-4.6.2-devpack.png)

或者，您可以取得從.NET 開發人員套件： [https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>將目標平台設定為 x86 或 x64

原生映像的編譯器最佳化特定平台的程式碼。 若要使用它，您需要設定您的應用程式，例如 x86 或 x64 一個特定的平台為目標。

如果您有多個專案方案中時，只有項目點專案 （最有可能會產生可執行檔專案） 已編譯為 x86 或 x64。 從主專案中參考的其他二進位檔將會處理在主專案中，指定架構，即使它們會編譯為 AnyCPU。

若要設定您的專案：

1. 以滑鼠右鍵按一下您的方案，然後按**Configuration Manager**。

2. 選取 **< 新...>** 中**平台**專案，以產生可執行檔名稱旁邊的下拉式功能表。

3. 在 **新專案平台**對話方塊方塊中，請確定**複製設定值**下拉式清單設定為**Any CPU**。

![設定 x86](images/desktop-to-uwp/configure-x86.png)

重複這個步驟`Release/x64`如果您想要產生 x64 二進位檔。

>[!IMPORTANT]
> 原生映像編譯器不支援 AnyCPU 的組態。

## <a name="add-the-nuget-packages"></a>新增 NuGet 套件

原生映像編譯器可作為您要新增至會產生可執行檔的 Visual Studio 專案的 NuGet 套件。 這通常是您的 Windows Form 或 WPF 專案。 若要這樣做，使用此 PowerShell 命令。

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> 預覽套件會為未列出狀態發佈於 NuGet.org 中。 瀏覽 NuGet.org 或在 Visual Studio 中使用套件管理員 UI，您將不會找到它們。 不過，您可以從安裝套件管理員主控台中，以及當您從一部電腦還原。 當我們發行的第一個非預覽版本時，我們要讓封裝完全可供存取。

## <a name="create-a-release-build"></a>建立發行組建

NuGet 套件設定專案以執行發行組建的額外工具。 此工具會將相同的二進位檔中的原生程式碼。
若要確認此工具已經處理的二進位檔中，您可以檢閱組建輸出到請確定它包含此類訊息：

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>常見問題集

**問:。在沒有.NET Framework 4.7.2 機器上，新的二進位檔作業嗎？**

A. 當執行以.NET Framework 4.7.2 最佳化的二進位檔將會從改進中獲益。 執行舊版的.NET framework 用戶端會從二進位檔載入的非最佳化的 MSIL 程式碼。

**問:。如何提供意見反應或回報問題？**

A. 使用 Visual Studio 2017 中的意見反應工具回報的問題。 [更多資訊](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017)。

**問:。將原生映像新增至現有的二進位檔的影響為何？**

A. 最佳化的二進位檔包含 managed 和原生程式碼，讓更大，最終的檔案。

**問:。可以釋放使用這項技術的二進位檔？**

A. 此版本包含 Go Live 授權，您可以立即使用。
