---
title: 版本調適型應用程式
description: 了解如何利用新的 API 並維持與先前版本的相容性
ms.date: 09/17/2018
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 435bbdbfaaf1bec90fa1ee2d598b4a3fe78d3789
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8487180"
---
# <a name="version-adaptive-apps-use-new-apis-while-maintaining-compatibility-with-previous-versions"></a>版本調適型應用程式：使用新的 API 並同時維持與先前版本的相容性

每個版本的 Windows 10 SDK 都新增了令人興奮、讓您想要使用的新功能。 不過，並非您所有的客戶都會同時將他們的裝置更新至最新版 Windows 10，且您會想盡可能讓您的 App 可以適用廣泛的裝置類型。 在這裡，我們會說明如何設計您的 App，讓它可以在較早版本的 Windows 10 上執行，同時讓 App 在安裝最新更新的裝置上執行時也會利用新功能。

若要確保您的 App 能支援廣泛的 Windows 10 裝置，需進行 3 個步驟。

- 首先，請將您的 Visual Studio 專案設定為以最新的 API 為目標。 這會影響當您編譯 App 時的情況。
- 其次，進行執行階段檢查，以確定您只有呼叫存在於 App 執行所在之裝置上的 API。
- 其三，在 Windows 10 的最小版本和目標版本上測試應用程式。

## <a name="configure-your-visual-studio-project"></a>設定您的 Visual Studio 專案

支援多個 Windows 10 版本的第一個步驟，是在您的 Visual Studio 專案中指定「目標」** 與「最低」** 支援的 OS/SDK 版本。

- 目標**：Visual Studio 編譯 App 程式碼並執行所有工具的目標 SDK 版本。 於編譯期間，此 SDK 版本中所有的 API 與資源，都會在您的 App 程式碼中提供使用。
- 最低**：支援可執行您 App (且會由市集部署至) 之最早 OS 版本的 SDK 版本，以及 Visual Studio 編譯您 App 標記程式碼的目標版本。 

在執行階段期間，您的 App 將針對它所部署至的 OS 版本執行，因此如果您使用或呼叫該版本中沒有提供使用的資源或 API，您的 App 便會擲回例外狀況。 我們稍後於本文章中會示範如何使用執行階段檢查，以呼叫正確的 API。

[目標] 和 [最低] 設定指定了 OS/SDK 版本範圍的兩端。 不過，如果您在最低版本上測試您的 App，便可以確定它將可以在「最低」和「目標」之間的任何版本上執行。

> [!TIP]
> Visual Studio 不會針對 API 的相容性向您提出警告。 進行測試是您的責任，以確保您的 App 可如預期地在「最低」和「目標」的 OS 版本，以及它們之間的所有 OS 版本上執行。

當您在 Visual Studio 2015 (Update 2 或更新版本) 中建立新的專案，系統會提示您設定 App 支援的「目標」和「最低」版本。 根據預設，「目標」版本是已安裝的最高 SDK 版本，而「最低」版本是已安裝的最低 SDK 版本。 您只能從已安裝在您電腦上的 SDK 版本中挑選「目標」和「最低」版本。 

![在 Visual Studio 中設定目標 SDK](images/vs-target-sdk-1.png)

我們通常會建議您保留預設值。 不過，如果您已安裝 SDK 的預覽版本，且您正在撰寫實際執行程式碼，您應該將 Preview SDK 的「目標」版本變更為最新的正式 SDK 版本。 

若要變更 Visual Studio 中已經建立專案的「最低」和「目標」版本，請移至 [專案] -&gt; [屬性] -&gt; [應用程式] 索引標籤 -&gt; [目標預測]。

![在 Visual Studio 中變更目標 SDK](images/vs-target-sdk-2.png)

下表顯示各個 SDK 的組建編號，供您參考。

| 易記名稱 | 版本 | OS/SDK 組建 |
| ---- | ---- | ---- |
| RTM | 1507 | 10240 |
| 11 月份更新 | 1511 | 10586 |
| 年度更新版 | 1607 | 14393 |
| Creators Update | 1703 | 15063 |
| Fall Creators Update | 1709 | 16299 |
| 2018 年 4 月更新 | 1803 | 17134 |
| 2018 年 10 月更新 | 1809 | 17763 |

您可以從 [Windows SDK 與模擬器封存](https://developer.microsoft.com/downloads/sdk-archive)下載任何已發行的 SDK 版本。 您可以從 [Windows 測試人員](https://insider.windows.com/Home/BuildWithWindows)網站的開發人員區段下載最新的 Windows Insider Preview SDK。

 如需 Windows 10 更新的詳細資訊，請參閱[Windows 10 版本資訊](https://technet.microsoft.com/windows/release-info)。 如需 Windows 10 的重要資訊會支援週期，請參閱[Windows 生命週期事實表](https://support.microsoft.com/help/13853/windows-lifecycle-fact-sheet)。

## <a name="perform-api-checks"></a>執行 API 檢查

版本調適型應用程式的關鍵是 API 協定與 [ApiInformation](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation) 類別的結合。 這個類別可讓您偵測指定的 API 協定、類型或成員是否存在，讓您可以放心地跨各種裝置和 OS 版本進行 API 呼叫。

### <a name="api-contracts"></a>API 協定

裝置系列內的一組 API 會細分成小部分，稱為 API 協定。 您可以使用 **ApiInformation.IsApiContractPresent** 方法來測試 API 協定是否存在。 如果您要測試相同版本的 API 協定中是否有許多 API 存在，這是相當有用的方法。

```csharp
    bool isScannerDeviceContract_1_Present =
        Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent
            ("Windows.Devices.Scanners.ScannerDeviceContract", 1);
```

什麼是 API 協定？ API 協定基本上代表一個功能：一組聯合提供某個特定功能的相關 API。 假設的 API 協定可能會代表包含兩個類別、五個介面、一個結構、兩個列舉等等的一組 API。

邏輯相關的類型會群組成一個 API 協定，並且從 Windows 10 開始，每個 Windows 執行階段 API 都是某個 API 協定的成員。 有了 API 協定，您在檢查的會是裝置上特定功能或 API 的可用性，其實也就是檢查裝置的功能，而不是檢查是否有特定裝置或作業系統。 實作 API 協定中任何 API 的平台也必須該 API 協定中的每個 API。 這表示您可以測試執行中 OS 是否支援特定 API 協定；如果支援，則呼叫該 API 協定中的任何 API，而不需個別檢查每一個 API。

最大且最常使用的 API 協定是 **Windows.Foundation.UniversalApiContract**。 其中包含通用 Windows 平台中的大多數 API。 [裝置系列擴充功能 SDK 及 API 協定](https://docs.microsoft.com/uwp/extension-sdks/)文件說明各種可用的 API 協定。 您將了解它們大部分代表一組在功能上相關的 API。

> [!NOTE]
> 如果您已安裝的預覽版 Windows 軟體開發套件 (SDK) 尚未記載於文件，還是可以在位於 SDK 安裝資料夾 (\(Program Files (x86))\Windows Kits\10\Platforms\<platform>\<SDK version>\Platform.xml) 的「Platform.xml」檔案中找到 API 協定的相關資訊。

### <a name="version-adaptive-code-and-conditional-xaml"></a>版本調適型程式碼和條件式 XAML

在 Windows 10 的所有版本中，都可以使用程式碼條件中的 ApiInformation 類別，測試您要呼叫的 API 是否存在。 您可以在調適型程式碼中使用該類別的各種方法 (例如 IsTypePresent、IsEventPresent、IsMethodPresent 和 IsPropertyPresent)，以您所需的細微性來測試 API 是否存在。

如需詳細資訊和範例，請參閱**[版本調適型程式碼](version-adaptive-code.md)**。

如果您的應用程式最小版本是組建 15063 (Creators Update) 或更新版本，您可以在標記中使用*條件式 XAML*來設定屬性和具現化物件，而不需使用程式碼後置。 條件式 XAML 提供一種在標記中使用 ApiInformation.IsApiContractPresent 方法的方式。

如需詳細資訊和範例，請參閱**[條件式 XAML](conditional-xaml.md)**。

## <a name="test-your-version-adaptive-app"></a>測試版本調適型應用程式

當您使用版本調適型程式碼或條件式 XAML 來撰寫版本調適型應用程式時，您必須在執行 Windows 10 最小版本的裝置及執行目標版本的裝置上測試該應用程式。

您不可只在單一裝置上測試所有的條件程式碼路徑。 為了確保測試過所有的程式碼路徑，您必須在執行支援的最小 OS 版本的遠端裝置 (虛擬機器) 上部署和測試應用程式。
如需遠端偵錯的詳細資訊，請參閱[部署和偵錯 UWP app](deploying-and-debugging-uwp-apps.md)。

## <a name="related-articles"></a>相關文章

- [什麼是 UWP app](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [利用 API 協定動態偵測功能](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
- [API 協定](https://channel9.msdn.com/Events/Build/2015/3-733) (組建 2015 影片)
