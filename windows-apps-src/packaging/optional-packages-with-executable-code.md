---
author: laurenhughes
title: 具可執行程式碼的選用套件
description: 了解如何使用 Visual Studio 建立具可執行程式碼的選用套件。
ms.author: lahugh
ms.date: 9/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, app installer, AppInstaller, sideload, related set, optional packages, 應用程式安裝程式, 側載, 相關集合, 選用套件
ms.localizationpriority: medium
ms.openlocfilehash: f5660649b6f82135cdb45a8678a3f871a0f5e61d
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/08/2018
ms.locfileid: "4414883"
---
# <a name="optional-packages-with-executable-code"></a>具可執行程式碼的選用套件
 
具可執行程式碼的選用套件適用於分割較大型或複雜的 App，或新增到已發佈的 App。 使用 Visual Studio 2017 版本 15.7 和 .NET Native 2.1，您可以從 C++ 與 C# 選用套件載入可執行程式碼。

## <a name="prerequisites"></a>必要條件
- Visual Studio 2017，版本 15.7
- Windows 10，版本 1709
- Windows 10，版本 1709 SDK

若要取得最新的開發工具，請參閱[適用於 Windows 10 的下載項目與工具](https://developer.microsoft.com/windows/downloads)。 

> [!NOTE]
> 若要提交使用了選用套件和/或相關集合的 App 至 Microsoft Store，您將需要權限。 選用套件和相關集合可在沒有開發人員中心的許可下用於企業營運 (LOB) 或企業應用程式，只要沒有提交到 Microsoft Store 即可。 請參閱 [Windows 開發人員支援](https://developer.microsoft.com/windows/support)，以取得提交使用選用套件和相關集合應用程式的權限。

## <a name="c-optional-packages-with-executable-code"></a>具可執行程式碼的 C++ 選用套件

若要從 C++ 選用套件載入程式碼，請參閱 GitHub 上的 [OptionalPackageSample](https://github.com/AppInstaller/OptionalPackageSample) 存放庫。 [OptionalPackageDLL](https://github.com/AppInstaller/OptionalPackageSample/tree/master/OptionalPackageDLL) 示範如何建立專案且其具有可從主要套件執行的程式碼。 MyMainApp 專案示範如何從 OptionalPackageDLL.dll 檔案[載入程式碼](https://github.com/AppInstaller/OptionalPackageSample/blob/bf6b4915ff1f3b8abfdaacb1ad9e77184c49fe18/MyMainApp/MainPage.xaml.cpp#L182)。

## <a name="c-optional-packages-with-executable-code"></a>具可執行程式碼的 C# 選用套件

若要開始以 C# 建置選用的程式碼套件，請依照下列步驟來設定您的解決方案：

1. 建立新的 UWP 應用程式且最小版本設定為 Windows 10 Fall Creators Update SDK (組建 16299) 或更高。

2. 新增**選用的程式碼套件 (Windows 通用)** 專案到解決方案。 確保**最小版本**和**目標版本**符合您的主應用程式。

3. 如果您打算提交 App 到 Microsoft Store，以滑鼠右鍵按一下兩個專案並選取** Microsoft Store -> 關聯 App 與 Microsoft Store...**

4. 開啟主應用程式的 `Package.appxmanifest` 檔案並尋找 `Identity Name` 值。 為下一個步驟記下這個值。

5. 開啟選用應用程式套件的 `Package.appxmanifest` 檔案並尋找 `uap3:MainAppPackageDependency Name` 值。 更新 `uap3:MainAppPackageDependency Name` 值以符合上一個步驟中的主應用程式套件的 `Identity Name` 值。 

    以下是主應用程式的 `Package.appxmanifest` 的 `Identity` 範例。
    ```XML
    <Identity Name="12345.MainAppProject" Publisher="CN=PublisherName" Version="1.0.0.0" />
    ```

    選用應用程式套件的 `uap3:MainPackageDependency` 必須符合主應用程式 `Identity`。
    ```XML
    <uap3:MainPackageDependency Name="12345.MainAppProjectTest" />
    ```

6. 新增 `Bundle.mapping.txt` 檔案到主應用程式中。 請依照此[相關集合](https://docs.microsoft.com/windows/uwp/packaging/optional-packages#related-sets)一節中的步驟，建立包含這兩個 App 的相關的設定。 

7. 建置選用套件專案，然後從 `..\[PathToOptionalPackageProject]\bin\[architecture]\[configuration]\Reference` 找到的組建瀏覽到輸出中的套件參考資料夾。 請注意，您可以選擇參考資料夾的路徑中的任何架構，因為 `.winmd` 檔案 (步驟 8) 是架構獨立。

8. 新增主應用程式專案的參考到此資料夾中找到的 `.winmd` 檔案。 每次您在選用套件專案中變更 API 介面區域，此 `.winmd` 檔案**必須**更新。 此參考提供主應用程式專案編譯所需的資訊。

9. 在主應用程式專案中，瀏覽至專案組建內容，並選取 \[**利用 .NET 原生工具鏈進行編譯**\]。 目前，C# 的選用程式碼套件建立只支援在 .NET Native 中偵錯。 移至專案偵錯內容，並選取 \[**部署選用套件**\]。 這樣可確保每當您部署主應用程式專案時，兩個套件是同步的。

一旦您完成這些步驟，您可以新增程式碼到選用套件專案，如果其是受管理的 WinRT 元件專案。 若要存取主應用程式專案中的程式碼，請呼叫選用套件專案中公開的公用方法。