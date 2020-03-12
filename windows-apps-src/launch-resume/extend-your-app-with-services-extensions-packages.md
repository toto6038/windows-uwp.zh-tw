---
title: 透過服務、延伸模組和套件擴充您的應用程式
description: 描述如何建立當您的通用 Windows 平臺（UWP）存放區應用程式更新時執行的背景工作。
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, 延伸, 組件化, app 服務, 套件, 擴充功能
ms.localizationpriority: medium
ms.openlocfilehash: a2d12fbf1c45c05c2e9917c77c0d15f15bccaf72
ms.sourcegitcommit: 756217c559155e172087dee4d762d328c6529db6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/09/2020
ms.locfileid: "78935339"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>透過服務、延伸模組和套件擴充您的應用程式

Windows 10 中有許多技術可擴充及搭配您的應用程式。 此表格應協助您根據需求來判斷應該使用哪一種技術。 接著是案例與技術的簡短描述。

| 案例                           | 資源套件   | 資產套件      | 選用套件   | 一般套件組合        | 應用程式延伸模組      | 應用程式服務        | 串流安裝  |
|------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| 協力廠商程式碼外掛程式            |                    |                    |                    |                    | :heavy_check_mark: |                    |                    |
| 同處理序程式碼外掛程式              |                    |                    | :heavy_check_mark: |                    |                    |                    |                    |
| UX 資產 (字串/影像)         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| 隨選內容 <br/> （例如，其他層級） |      |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| 個別授權和授權取得 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: |                    |
| 應用程式內取得                 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| 最佳化安裝時間              | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| 減少磁碟使用量              | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |                    |                    |
| 最佳化封裝                 |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |
| 減少發佈時間             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |

## <a name="scenario-descriptions-the-rows-in-the-table-above"></a>案例描述 (上表中各列)

**協力廠商外掛程式**  

可從市集下載且可從 App 執行的程式碼。 例如，Microsoft Edge 瀏覽器的擴充功能。

**進程內程式碼外掛程式**  

與您的 App 同在一個處理序中執行的程式碼。 可能還包括內容。 程式碼在同一個處理序中執行，因此假設有較高階的信任層級。 您可以選擇不要向協力廠商公開這種擴充性。

**UX 資產（字串/影像）**  

使用者介面資產，例如當地語系化字串、影像，以及您要根據地區設定或任何其他原因納入考量的任何其他 UI 內容。

**點播內容**  

您要稍後再下載的內容。 例如，允許您下載新等級、面板或功能的 App 內購買。

**個別的授權和收購**  

可以在不考慮 App 的情況下授權並取得內容。

**應用程式內取得**  

表示 App 是否在其程式設計上提供可從其中取得內容的支援。

**優化安裝時間**

提供縮短從市集取得 App 和開始執行所需時間的功能。

**減少的磁碟使用量**減少應用程式大小的部分僅止於所需的應用程式或資源。

**優化封裝**針對大規模或複雜的應用程式優化應用程式封裝進程。

**減少發佈時間**將在 Microsoft Store、本機共用或網頁伺服器中發佈應用程式所需的時間量降到最低。

## <a name="technology-descriptions-the-columns-in-the-table-above"></a>技術描述 (上表中各欄)

**資源套件**

資源套件是僅含資產的套件，可讓您的 App 配合多種顯示大小和系統語言進行調整。 資源套件針對使用者語言、系統縮放比例和 DirectX 功能，依據各種使用者案例量身打造 App。 雖然應用程式套件可能包含數個資源，但作業系統會根據使用者裝置，僅下載相關的資源，以節省頻寬和磁碟空間。

**資產套件**資產套件是可執行檔的通用、集中式來源，或是應用程式使用的無法執行檔案。 這些通常是非處理器或語言特定的檔案。 例如，這可能包括一個資產套件中收藏的相片，和另一個資產套件中的影片，而這兩者全在應用程式中使用。 如果您的應用程式支援多個架構和多種語言，這些資產可能會包含在架構套件或資源套件中，但這也表示資產會在各種架構套件中重複多次，並採用磁碟空間。 如果使用資產套件，這些只需要包含在整體應用程式套件中一次。 請參閱[資產套件簡介](/windows/msix/package/asset-packages)以深入了解。

**選擇性套件**

選用套件可用來補充或延伸應用程式套件的原始功能。 您可以發行 App 後再接著發行選用套件，也可以同時發行 App 和選用套件。 透過選用套件延伸 App 有其優點，您可以將內容當做個別應用程式套件來發佈和創造收益。 選用套件會使用主 App 的身分識別來執行 (這點與應用程式擴充功能不同)，因此通常是由原本的 App 開發人員所開發。 視定義選用套件的方式而定，可以將程式碼、資產或程式碼與資產兩者從選用套件載入至您的主 App。 如果您需要以可分別販賣、授權及散佈的內容來增強應用程式，則選用套件可能是您的正確選擇。 如需實作詳細資料，請參閱[選用套件及相關集合的製作](/windows/msix/package/optional-packages)。

**一般套件組合**
[一般套件組合應用程式套件](/windows/msix/package/flat-bundles)類似一般的應用程式套件組合，不同點在於不是將所有應用程式套件包括在資料夾中，一般套件組合只會在應用程式套件中包含*參考*。 因包含的是應用程式套件的參考，而不是檔案本身，一般套件組合將會減少封裝和下載應用程式所需的時間量。

**應用程式延伸模組**

[應用程式延伸模組](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)可讓您的 UWP 應用程式裝載由其他 UWP 應用程式所提供的內容。 探索、列舉並存取來自那些 App 的唯讀內容。

如果 App 支援擴充功能，任何開發人員都可以提交適用於該 App 的擴充功能。 因此，主控 App 在其載入未經預先測試的擴充功能時必須強固十足。 擴充功能應該看作是未受信任。

應用程式無法從擴充功能載入程式碼。 如果您需要程式碼執行，請考慮使用「應用程式服務」。

**App Service**

Windows 應用程式服務可讓您的 UWP 應用程式將服務提供給另一個通用 Windows 應用程式，藉此啟用應用程式對應用程式的通訊。 應用程式服務可讓您建立 App 可於其所在裝置呼叫的無 UI 服務，而從 Windows 10 版本 1607 開始，則可在遠端裝置上呼叫這些服務。 如需詳細資訊，請參閱[建立和使用應用程式服務](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)。

應用程式服務是為其他 UWP app 提供服務的 UWP app。 它們類似于裝置上的 web 服務。 應用程式服務在主控 App 中以背景工作方式執行，並且可以將其服務提供給其他 App。 例如，應用程式服務可能會提供其他 App 可以使用的條碼掃描器服務。 或者，App 的企業套件也許會有可供套件中其他 App 使用的一般拼字檢查應用程式服務。

**UWP 應用程式串流安裝**

串流安裝是一種盡可能使您的 App 有效地傳遞給使用者的方式。 只要所需的部分下載完成，使用者即可投入與 App 的互動，而不是等待下載了整個 App 才來開始使用。 身為開發人員，您可以決定將 App 分割為用於基本啟用與啟動的必要區段和用於 App 其餘部分的其他內容。 如需詳細資訊和實作詳細資料，請參閱 [UWP app 串流安裝](/windows/msix/package/streaming-install)。

## <a name="see-also"></a>另請參閱

[建立和使用應用程式服務](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)  
[資產封裝簡介](/windows/msix/package/asset-packages)  
[使用封裝版面配置建立套件](/windows/msix/package/packaging-layout)  
[選用套件及相關集合的製作](/windows/msix/package/optional-packages)  
[使用資產套件和封裝折迭進行開發](/windows/msix/package/package-folding)  
[UWP 應用程式串流安裝](/windows/msix/package/streaming-install)  
[一般配套應用程式套件](/windows/msix/package/flat-bundles)  
[ApplicationModel. AppService 命名空間](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService)  
[ApplicationModel Extensions 命名空間](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)  
