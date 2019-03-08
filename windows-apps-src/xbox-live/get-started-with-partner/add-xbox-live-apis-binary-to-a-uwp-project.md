---
title: 將二進位 Xbox Live Api 新增至 UWP 專案
description: 了解如何使用 NuGet 將 Xbox Live Api 的二進位封裝新增至您的 UWP 專案。
ms.assetid: 1e77ce9f-8a0e-402c-9f46-e37f9cda90ed
ms.date: 11/28/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 nuget
ms.localizationpriority: medium
ms.openlocfilehash: 20b4d4ae27282ccf71964d31da4d1f577c280de8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593943"
---
# <a name="add-xbox-live-apis-binary-package-to-your-uwp-project"></a>UWP 專案中加入 Xbox Live Api 的二進位封裝

## <a name="requirements"></a>需求

2. **[Windows 10](https://microsoft.com/windows)**。
3. **[Visual Studio](https://www.visualstudio.com/)**。 您可以建置 UWP 應用程式，Visual Studio 2015 Update 3 或更新版本。 我們建議您使用最新版的 Visual Studio 開發人員和安全性更新。
4. **[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) v10.0.10586.0**或更新版本。

## <a name="add-the-binary-package-via-nuget"></a>新增二進位透過 NuGet 套件

若要使用 Xbox Live API 從您的專案，您可以藉由使用 NuGet 套件，或加入 API 來源加入至二進位檔的參考。 新增 NuGet 套件可以加快編譯過程，而新增來源可以讓偵錯變容易。 本文將逐步引導您使用 NuGet 套件。 如果您想要使用來源，請參閱[編譯的 Xbox Live Api 來源專案中的 UWP](add-xbox-live-apis-source-to-a-uwp-project.md)。

Xbox 服務 API UWP 和 XDK，以及 c + + 和 WinRT 類別有和其命名空間建構為**Microsoft.Xbox.Live.SDK.*。UWP**和**Microsoft.Xbox.Live.SDK.*。XboxOneXDK**。

1. **UWP**適用於開發人員建置 UWP 遊戲時，它可以在 PC、 Xbox One 或 Windows Phone 上執行。
2. **XboxOneXDK**適合ID@Xbox和管理使用 Xbox One XDK 的開發人員。
3. C + + SDK 可用來針對 c + + 的遊戲引擎，其中當做 WinRT SDK 是使用 c + + 撰寫的遊戲引擎C#，或 JavaScript。
4. 使用 c + + 引擎 WinRT 時，您應該使用 C + + /CX 使用 hats (^)。 C + + 是建議的 API，可用於 c + + 的遊戲引擎。  

> [!TIP]
> 您可以深入了解 Xbox One 在上執行 UWP [Xbox One 上的 UWP 應用程式開發入門](https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started)。

您可以加入 Xbox Live SDK NuGet 封裝：

1. 在 Visual Studio 中，移至**工具** > **NuGet 套件管理員** > **管理方案的 NuGet 套件...**.
2. 在 [NuGet 套件管理員] 中，按一下**瀏覽**，然後輸入**Xbox.Live.SDK**在搜尋方塊中。
3. 選取的 Xbox Live SDK，您想要從左側清單中使用的版本。
3. 在視窗的右側，請檢查您的專案旁邊的方塊，然後按一下**安裝**。

> [!NOTE]
> Xbox Live 創作者計劃開發人員必須使用其中一個 Xbox Live SDK 的 UWP 版本，因為不支援 XDK。

![新增透過 NuGet XBL](../images/getting_started/vs-add-nuget-xbl.gif)

> [!IMPORTANT]
> 針對`Microsoft.Xbox.Live.SDK.Cpp.*`專案，請務必包含標頭`#include <xsapi\services.h>`中專案的來源。
