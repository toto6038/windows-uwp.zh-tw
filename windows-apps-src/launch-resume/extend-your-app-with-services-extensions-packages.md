---
author: TylerMSFT
title: 透過服務、擴充功能和套件延伸您的 App
description: 了解如何在更新通用 Windows 平台 (UWP) 市集應用程式時執行背景工作。
ms.author: twhitney
ms.date: 05/7/2018
ms.topic: article
keywords: windows 10, uwp, 延伸, 組件化, app 服務, 套件, 擴充功能
ms.localizationpriority: medium
ms.openlocfilehash: 624d52ff96fb2537afa3affb2d842aa29e1e6667
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "5749640"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>透過服務、擴充功能和套件延伸您的 App

Windows 10 中有各種協助您延伸和組件化 App 的不同技術。 這表格有助於判斷應該將哪一種技術使用於您的案例。 接著是案例與技術的簡短描述。

| 案例                           | 資源套件   | 資產套件      | 選用套件   | 一般套件組合        | 應用程式延伸模組      | 應用程式服務        | 串流安裝  |
|------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| 第三方程式碼外掛程式            |                    |                    |                    |                    | :heavy_check_mark: |                    |                    |
| 同處理序程式碼外掛程式              |                    |                    | :heavy_check_mark: |                    |                    |                    |                    |
| UX 資產 (字串/影像)         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| 隨選內容 <br/> (例如其他層級) |      |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| 個別授權和授權取得 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: |                    |
| 應用程式內取得                 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| 最佳化安裝時間              | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| 減少磁碟使用量              | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |                    |                    |
| 最佳化封裝                 |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |
| 減少發佈時間             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |

## <a name="scenario-descriptions-the-rows-in-the-table-above"></a>案例描述 (上表中各列)

**第三方外掛程式**  

可從市集下載且可從 App 執行的程式碼。 例如，Microsoft Edge 瀏覽器的擴充功能。

**同處理序程式碼外掛程式**  

與您的 App 同在一個處理序中執行的程式碼。 僅支援 C++。 可能還包括內容。 程式碼在同一個處理序中執行，因此假設有較高階的信任層級。 您可以選擇不向第三方開放這種擴充性。

**UX 資產 (字串/影像)**  

使用者介面資產，例如當地語系化字串、影像，以及您要根據地區設定或任何其他原因納入考量的任何其他 UI 內容。

**隨選內容**  

您要稍後再下載的內容。 例如，允許您下載新等級、面板或功能的 App 內購買。

**個別授權和授權取得**  

可以在不考慮 App 的情況下授權並取得內容。

**應用程式內取得**  

表示 App 是否在其程式設計上提供可從其中取得內容的支援。

**最佳化安裝時間**

提供縮短從市集取得 App 和開始執行所需時間的功能。

**減少的磁碟使用量**減少應用程式大小的部分僅止於所需的應用程式或資源。

**最佳化封裝**最佳化大規模或複雜應用程式的應用程式封裝程序。

**減少發佈時間**將在 Microsoft Store、本機共用或網頁伺服器中發佈應用程式所需的時間量降到最低。

## <a name="technology-descriptions-the-columns-in-the-table-above"></a>技術描述 (上表中各欄)

**資源套件**

資源套件是僅含資產的套件，可讓您的 App 配合多種顯示大小和系統語言進行調整。 資源套件針對使用者語言、系統縮放比例和 DirectX 功能，依據各種使用者案例量身打造 App。 雖然應用程式套件可能包含數個資源，但作業系統會根據使用者裝置，僅下載相關的資源，以節省頻寬和磁碟空間。

**資產套件**資產套件是常見的集中式可執行檔原始碼，或應用程式所使用的非可執行檔的檔案。 這些通常都是非處理器或語言特定的檔案。 例如，這可能包括一個資產套件中收藏的相片，和另一個資產套件中的影片，而這兩者全在應用程式中使用。 例如，這可能包括一個資產套件中收藏的相片，和另一個資產套件中的影片。 如果您的應用程式支援多架構和多種語言，這些資產可能會包含在架構套件或資源套件，但這也表示資產會跨不同的架構套件多次，重複佔用多次磁碟空間。 如果使用資產套件，這些只需要包含在整體應用程式套件中一次。 請參閱[資產套件簡介](../packaging/asset-packages.md)以深入了解。

**選用套件**

選用套件可用來補充或延伸應用程式套件的原始功能。 您可以發行 App 後再接著發行選用套件，也可以同時發行 App 和選用套件。 透過選用套件延伸 App 有其優點，您可以將內容當做個別應用程式套件來發佈和創造收益。 選用套件會使用主 App 的身分識別來執行 (這點與應用程式擴充功能不同)，因此通常是由原本的 App 開發人員所開發。 視定義選用套件的方式而定，可以將程式碼、資產或程式碼與資產兩者從選用套件載入至您的主 App。 如果您想要使用可另行營利、授權和發佈的內容來增強 App，那麼選用套件可能就是您的最佳選擇。 如需實作詳細資料，請參閱[選用套件及相關集合的製作](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)。

**一般套件組合**
[一般套件組合應用程式套件](../packaging/flat-bundles.md)類似一般的應用程式套件組合，不同點在於不是將所有應用程式套件包括在資料夾中，一般套件組合只會在應用程式套件中包含*參考*。 因包含的是應用程式套件的參考，而不是檔案本身，一般套件組合將會減少封裝和下載應用程式所需的時間量。

**應用程式延伸模組**

[應用程式延伸模組](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)可讓您的 UWP 應用程式裝載由其他 UWP 應用程式所提供的內容。 探索、列舉並存取來自那些應用程式的唯讀內容。

如果 App 支援擴充功能，任何開發人員都可以提交適用於該 App 的擴充功能。 因此，主控 App 在其載入未經預先測試的擴充功能時必須強固十足。 擴充功能應該看作是未受信任。

應用程式無法從擴充功能載入程式碼。 如果您需要程式碼執行，請考慮使用「應用程式服務」。

**應用程式服務**

Windows 應用程式服務藉由允許 UWP app 提供服務給其他通用 Windows app 來啟用 App 間通訊。 應用程式服務可讓您建立 App 可於其所在裝置呼叫的無 UI 服務，而從 Windows 10 版本 1607 開始，則可在遠端裝置上呼叫這些服務。 如需詳細資訊，請參閱[建立和使用應用程式服務](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)。

應用程式服務是為其他 UWP app 提供服務的 UWP app。 這些服務可比擬為裝置上的網頁服務。 應用程式服務在主控 App 中以背景工作方式執行，並且可以將其服務提供給其他 App。 例如，應用程式服務可能會提供其他 App 可以使用的條碼掃描器服務。 或者，App 的企業套件也許會有可供套件中其他 App 使用的一般拼字檢查應用程式服務。

**UWP app 串流安裝**

串流安裝是一種盡可能使您的 App 有效地傳遞給使用者的方式。 只要所需的部分下載完成，使用者即可投入與 App 的互動，而不是等待下載了整個 App 才來開始使用。 身為開發人員，您可以決定將 App 分割為用於基本啟用與啟動的必要區段和用於 App 其餘部分的其他內容。 如需詳細資訊和實作詳細資料，請參閱 [UWP app 串流安裝](https://docs.microsoft.com/windows/uwp/packaging/streaming-install)。

## <a name="see-also"></a>請參閱

[建立和取用 App 服務](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)  
[資產套件簡介](../packaging/asset-packages.md)  
[使用封裝配置的套件建立](../packaging/packaging-layout.md)  
[選用套件及相關集合的製作](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)  
[使用資產套件與套件摺疊進行開發](../packaging/package-folding.md)  
[UWP app 串流安裝](https://docs.microsoft.com/windows/uwp/packaging/streaming-install)  
[一般套件組合應用程式套件](../packaging/flat-bundles.md)  
[Windows.ApplicationModel.AppService 命名空間](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService)  
[Windows.ApplicationModel.Extensions 命名空間](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)  