---
author: laurenhughes
title: 使用資產套件與套件摺疊進行開發
description: 了解如何使用資產套件與套件摺疊有效地組織您的應用程式。
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
keywords: windows 10, 封裝, 套件配置, 資產套件
ms.localizationpriority: medium
ms.openlocfilehash: efdf560158e2b57ae9e05ecc31d49c7cf981d8c0
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6025550"
---
# <a name="developing-with-asset-packages-and-package-folding"></a>使用資產套件與套件摺疊進行開發 

> [!IMPORTANT]
> 如果您想要提交應用程式至 Microsoft Store，您需要連絡[Windows 開發人員支援](https://developer.microsoft.com/windows/support)，並取得核准使用資產套件和套件摺疊。

資產套件可以減少應用程式整體封裝大小和發佈至 Microsoft Store 的時間。 您可以在[資產套件簡介](asset-packages.md)深入了解資產套件，以及了解它如何加速您的開發反覆運算。

如果您正在考慮將資產套件用於應用程式或已經知道要使用它，您可能想知道資產套件如何變更開發程序。 簡言之，應用程式開發保持不變，這可能是因為資產套件的套件摺疊。

## <a name="file-access-after-splitting-your-app"></a>分割應用程式之後的檔案存取

若要了解套件摺疊如何不會影響您的開發程序，讓我們首先回來了解當您將應用程式分成多個套件（使用資產套件或資源套件）時的行為。 

從高階角度來看，分割部分應用程式檔案為其他套件（不是架構套件），您將無法從程式碼執行的相對位置存取直接這些檔案。 這是因為這些套件的安裝目錄不同於架構套件的安裝位置。 例如，如果您正在製作遊戲，您的遊戲翻譯為法文及德文，而且建置適用於 x86 與 x64 電腦，則您應該會有這些遊戲的應用程式套件組合中的應用程式套件檔案：

-   MyGame_1.0_x86.appx
-   MyGame_1.0_x64.appx
-   MyGame_1.0_language-fr.appx
-   MyGame_1.0_language-de.appx

當您的遊戲安裝到使用者的電腦時，每個應用程式套件檔案將會有它自己的**WindowsApps**目錄中的資料夾。 因此執行 64 位元 Windows 的法文使用者，您的遊戲看起來像這樣：

```example
C:\Program Files\WindowsApps\
|-- MyGame_1.0_x64
|   `-- …
|-- MyGame_1.0_language-fr
|   `-- …
`-- …(other apps)
```

請注意，不適用於使用者的檔案將不會在應用程式套件安裝 （x86 與德文套件）。 

針對這位使用者，遊戲主要可執行檔將會在**MyGame_1.0_x64**資料夾，並將會在該處執行，以及通常它只有此資料夾內檔案的存取權。 為了存取**MyGame_1.0_language-fr**資料夾中的檔案，您必須使用 MRT API 或 PackageManager API。 MRT Api 可以從已安裝的語言，自動選取最適合的檔案，您可以深入了解在[Windows.ApplicationModel.Resources.Core](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core)MRT Api。 或者，您可以使用[PackageManager 類別](https://docs.microsoft.com/uwp/api/Windows.Management.Deployment.PackageManager)找到法文語言套件的安裝位置。 您絕不應該假設應用程式套件的安裝位置，因為這會變更而且因使用者有所不同。 

## <a name="asset-package-folding"></a>資產套件摺疊

如何存取資產套件中的檔案？ 您可以繼續使用正在使用中的檔案存取 API 來存取架構套件中的任何其他檔案。 這是因為透過套件摺疊處理程序安裝時，資產套件檔案會摺疊到架構套件中。 此外，由於資產套件檔案應原本是架構套件中的檔案，這表示在開發程序中，從鬆散檔案部署移至封裝部署，就不需要變更 API 使用。 

若要深入了解有關套件摺疊的運作方式，讓我們開始範例。 如果您有使用下列檔案結構的遊戲專案：

```example
MyGame
|-- Audios
|   |-- Level1
|   |   `-- ...
|   `-- Level2
|       `-- ...
|-- Videos
|   |-- Level1
|   |   `-- ...
|   `-- Level2
|       `-- ...
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
```

如果您想要將遊戲分為 3 套件：x64 架構套件，音訊資產套件和視訊資產套件，遊戲可分成這些套件：

```example
MyGame_1.0_x64.appx
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
MyGame_1.0_Audios.appx
`-- Audios
    |-- Level1
    |   `-- ...
    `-- Level2
        `-- ...
MyGame_1.0_Videos.appx
`-- Videos
    |-- Level1
    |   `-- ...
    `-- Level2
        `-- ...
```

當您安裝遊戲，會先部署 x64 套件。 然後兩個資產套件仍然會部署至自己的資料夾，就像前一個範例中的**MyGame_1.0_language-fr**。 不過，因為套件摺疊，資產套件檔案也會永久連結出現在**MyGame_1.0_x64**資料夾（即使檔案出現在兩個位置，它們不會佔用磁碟空間兩次）。 資產套件檔案將會出現的位置，正是相對於套件根目錄的位置。 部署遊戲的最後配置看起來如下：

```example 
C:\Program Files\WindowsApps\
|-- MyGame_1.0_x64
|   |-- Audios
|   |   |-- Level1
|   |   |   `-- ...
|   |   `-- Level2
|   |       `-- ...
|   |-- Videos
|   |   |-- Level1
|   |   |   `-- ...
|   |   `-- Level2
|   |       `-- ...
|   |-- Engine
|   |   `-- ...
|   |-- XboxLive
|   |   `-- ...
|   `-- Game.exe
|-- MyGame_1.0_Audios
|   `-- Audios
|       |-- Level1
|       |   `-- ...
|       `-- Level2
|           `-- ...
|-- MyGame_1.0_Videos
|   `-- Videos
|       |-- Level1
|       |   `-- ...
|       `-- Level2
|           `-- ...
`-- …(other apps)
```

將套件摺疊用於資產套件時，您仍然可以相同方式存取已分成資產套件的檔案（請注意架構資料夾和原始專案資料夾具有相同的結構），而且您可以新增資產套件或在資產套件之間移動檔案，而不會影響程式碼。 

現在示範更複雜的套件摺疊範例。 假設您想要根據層級分割檔案，而且如果您想要保留原始專案資料夾的相同結構，您的套件應該看起來像這樣：

```example
MyGame_1.0_x64.appx
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
MyGame_Level1.appx
|-- Audios
|   `-- Level1
|       `-- ...
`-- Videos
    `-- Level1
        `-- ...

MyGame_Level2.appx
|-- Audios
|   `-- Level2
|       `-- ...
`-- Videos
    `-- Level2
        `-- ...
```
這可允許**MyGame_Level1**套件中的**Level1**資料夾和檔案以及**MyGame_Level2**套件中的**Level2**資料夾和檔案於套件摺疊期間合併至**Audios**和**Videos**資料夾。 一般規則是，為對應檔案中的封裝檔案或 MakeAppx.exe 的[封裝配置](packaging-layout.md)指定的相對路徑，是套件摺疊之後應該用來存取它們的路徑。 

最後，如果不同資產套件中的兩個檔案有相同相對路徑，這會造成套件摺疊期間的衝突。 發生衝突時，應用程式部署會導致錯誤及失敗。 此外，因為套件摺疊利用永久連結，如果您使用資產套件，應用程式將無法部署至非 NTFS 磁碟機。 如果您知道使用者可能會將您的應用程式移到抽取式磁碟機，您不應該使用資產套件。 


