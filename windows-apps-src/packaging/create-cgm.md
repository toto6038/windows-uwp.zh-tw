---
author: laurenhughes
ms.assetid: ff2523cb-8109-42be-9dfc-cb5d09002574
title: 建立和轉換來源內容群組對應
description: 若要讓您的通用 Windows 平台 (UWP) 應用程式準備就緒以使用 UWP App 串流安裝，您需要建立內容群組對應。 此文章將協助您了解建立和轉換內容群組對應時所需要的詳細資訊，以及提供一些過程中的秘訣與技巧。
ms.author: lahugh
ms.date: 9/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 內容群組對應, 串流安裝, uwp app 串流安裝, 來源內容群組對應
ms.localizationpriority: medium
ms.openlocfilehash: 4ce32958d5a99dc9f3f772d6272450a4f2b0f81b
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2018
ms.locfileid: "5406249"
---
# <a name="create-and-convert-a-source-content-group-map"></a>建立和轉換來源內容群組對應

若要讓您的通用 Windows 平台 (UWP) 應用程式準備就緒以使用 UWP App 串流安裝，您需要建立內容群組對應。 此文章將協助您了解建立和轉換內容群組對應時所需要的詳細資訊，以及提供一些過程中的秘訣與技巧。

## <a name="creating-the-source-content-group-map"></a>建立來源內容群組對應

您將需要建立一個 `SourceAppxContentGroupMap.xml` 檔案，並使用 Visual Studio 或 **MakeAppx.exe** 工具將此檔案轉換為最終版本︰`AppxContentGroupMap.xml`。 雖然您可以藉由直接建立 `AppxContentGroupMap.xml` 來省略一些步驟，但我們仍建議 (通常也會比較容易) 您先建立 `SourceAppxContentGroupMap.xml` 並進行轉換，因為在 `AppxContentGroupMap.xml` 中並不允許使用萬用字元 (但萬用字元能帶來極大的幫助)。 

讓我們帶領您探索一個簡單的案例：在此案例中，UWP App 串流安裝將會非常有幫助。 

假設您已建立了一個 UWP 遊戲，但您應用程式的最終大小超過 100 GB。 接著就需要較長的時間下載從 Microsoft Store，可以是相當程度的不便。 若您選擇使用 UWP App 串流安裝，就可以指定您應用程式中檔案下載的順序。 藉由告訴市集先下載必要檔案，使用者就可以提早進入您的應用程式，並在背景繼續下載其他非必要的檔案。

> [!NOTE]
> UWP App 串流安裝會受到您應用程式檔案組織的極大影響。 我們建議您盡早針對 UWP App 串流安裝考慮您應用程式內容的配置，以簡化切割您應用程式檔案的工作，

首先，我們會建立一個 `SourceAppxContentGroupMap.xml` 檔案。

在我們進一步討論細節之前，以下是一個已經完成的簡易 `SourceAppxContentGroupMap.xml` 檔案︰

```xml
<?xml version="1.0" encoding="utf-8"?>  
<ContentGroupMap xmlns="http://schemas.microsoft.com/appx/2016/sourcecontentgroupmap" 
                 xmlns:s="http://schemas.microsoft.com/appx/2016/sourcecontentgroupmap"> 
    <Required>
        <ContentGroup Name="Required">
            <File Name="StreamingTestApp.exe"/>
        </ContentGroup>
    </Required>
    <Automatic>
        <ContentGroup Name="Level2">
            <File Name="Assets\Level2\*"/>
        </ContentGroup>
        <ContentGroup Name="Level3">
            <File Name="Assets\Level3\*"/>
        </ContentGroup>
    </Automatic>
</ContentGroupMap>
```

內容群組對應中有兩個主要的元件︰**必要**區塊 (其中包含了必要的內容群組)，以及**自動**區塊 (其中包含了多個自動內容群組)。

### <a name="required-content-group"></a>必要內容群組

必要內容群組是位於 `SourceAppxContentGroupMap.xml` 中 `<Required>` 元素下的單一內容群組。 必要內容群組應包含所有啟動應用程式所必要的檔案，並提供最低限度的使用者體驗。 由於受 .NET Native 編譯的限制，所有程式碼 (應用程式可執行檔) 都必須是必要群組的一部分，並將資產和其他檔案留在自動群組之中。

例如：若您的應用程式是一款遊戲，則必要群組中就必須包括主功能表或遊戲主畫面所需要的所有檔案。

以下是我們原始 `SourceAppxContentGroupMap.xml` 範例檔案的程式碼片段︰ 
```xml
<Required>
    <ContentGroup Name="Required">
        <File Name="StreamingTestApp.exe"/>
    </ContentGroup>
</Required>
```

在這裡有幾件重要的事情值得注意︰

- `<Required>` 元素中的 `<ContentGroup>` **必須**命名為 "Required"。 這個名稱必須只保留給必要內容群組使用，並且在最終內容群組對應中不得由其他 `<ContentGroup>` 使用。
- 只有一個 `<ContentGroup>`。 這是刻意的，因為檔案中應該只能有一個必要檔案群組。
- 在這個範例中的檔案是一個 `.exe` 檔案。 必要內容群組中不一定只能有一個檔案。您可以在群組中添加數個檔案。 

要開始撰寫此檔案的簡便方法，就是在您最愛的文字編輯器中開啟一個新的頁面，快速執行 \[另存新檔\] 並將檔案儲存至您應用程式的專案資料夾中，然後將您剛建立的檔案命名為︰`SourceAppxContentGroupMap.xml`。

> [!IMPORTANT]
> 若您正在開發的是 C++ UWP app，您將需要調整您 `SourceAppxContentGroupMap.xml` 的檔案屬性。 將 `Content` 屬性設定為 **True**，並將 `File Type` 屬性設定為 **XML 檔案**。 

當您在建立 `SourceAppxContentGroupMap.xml` 的時候，在檔案名稱中使用萬用字元將會很有幫助。如需詳細資訊，請參閱[使用萬用字元的秘訣與技巧](#wildcards)區段。

若您使用 Visual Studio 開發您的應用程式，我們建議您在必要內容群組中增加下列內容︰

```xml
<File Name="*"/>
<File Name="WinMetadata\*"/>
<File Name="Properties\*"/>
<File Name="Assets\*Logo*"/>
<File Name="Assets\*SplashScreen*"/>
```

新增檔案名稱僅為一個萬用字元的檔案，會將 Visual Studio 新增至您專案資料夾中的所有檔案包括在內，例如：應用程式可執行檔，或 DLL 檔案。 WinMetadata 和 Properties 資料夾則是為了將其他 Visual Studio 產生的資料夾也包含在內。 Assets 萬用字元則是為了選取能讓應用程式順利安裝的必要 Logo 和 SplashScreen 影像。

請注意：您無法在檔案結構的根目錄中使用雙萬用字元「**」包含專案中的所有檔案，因為當您嘗試將 `SourceAppxContentGroupMap.xml` 轉換為最終的 `AppxContentGroupMap.xml` 時將會失敗。

請務必記得足跡檔案 (AppxManifest.xml、AppxSignature.p7x、resources.pri 等) 也不應該包含在內容群組對應之中。 若足跡檔案包含在您所指定的萬用字元檔案名稱之中，則系統會忽略他們。

### <a name="automatic-content-groups"></a>自動內容群組

自動內容群組是在使用者與已經下載完成的內容群組互動時，於背景繼續下載的資產。 其中包含了啟動應用程式所不需要的任何額外檔案。 例如：您可以將自動內容群組分為幾個不同層級，將每個層級定義為不同的內容群組。 如必要內容群組區段中所述︰由於受 .NET Native 編譯的限制，所有程式碼 (應用程式可執行檔) 都必須是必要群組的一部分，並將資產和其他檔案留在自動群組之中。

讓我們更仔細的觀察我們 `SourceAppxContentGroupMap.xml` 範例中的自動內容群組︰
```xml
<Automatic>
    <ContentGroup Name="Level2">
        <File Name="Assets\Level2\*"/>
    </ContentGroup>
    <ContentGroup Name="Level3">
        <File Name="Assets\Level3\*"/>
    </ContentGroup>
</Automatic>
```

自動群組的配置與必要群組的配置非常類似，除了有幾個例外︰

- 有多個內容群組。
- 除了保留給必要內容群組使用的「Required」之外，每個自動內容群組都可以有唯一的名稱。
- 自動內容群組不得包括**任何**必要內容群組中的檔案。 
- 自動內容群組可以包含同時位於其他自動內容群組中的檔案。 每個檔案都只會下載一次，並且會在第一個包含該檔案的自動內容群組中進行下載。

#### 使用萬用字元的秘訣與技巧<a name="wildcards"></a>

內容群組對應中的檔案配置總是會對應到您專案的根資料夾。

在我們的範例中，兩個 `<ContentGroup>` 元素中都使用了萬用字元，以取得「Assets\Level2」或「Assets\Level3」中一個檔案階層中的所有檔案。 若您使用了更深的資料夾結構，您可以使用雙萬用字元︰

```xml
<ContentGroup Name="Level2">
    <File Name="Assets\Level2\**"/>
</ContentGroup>
```

您也可以在檔案名稱中使用萬用字元。 例如：若您想要包含您「Assets」資料夾下所有檔案名稱包含「Level2」的檔案，您可以透過這種方式達成：

```xml
<ContentGroup Name="Level2">
    <File Name="Assets\*Level2*"/>
</ContentGroup>
```

## <a name="convert-sourceappxcontentgroupmapxml-to-appxcontentgroupmapxml"></a>將 SourceAppxContentGroupMap.xml 轉換為 AppxContentGroupMap.xml

若要將 `SourceAppxContentGroupMap.xml` 轉換為最終版本的 `AppxContentGroupMap.xml`，您可以使用 Visual Studio 2017 或 **MakeAppx.exe** 命令列工具。

若要使用 Visual Studio 轉換您的內容群組對應：
1. 新增 `SourceAppxContentGroupMap.xml` 至您的專案資料夾
2. 在 \[內容\] 視窗中變更 `SourceAppxContentGroupMap.xml` 的組建動作為「AppxSourceContentGroupMap」
2. 在方案總管中的專案上按一下滑鼠右鍵
3. 瀏覽至 \[市集\] -> \[轉換內容群組對應檔案\]

若您並未在 Visual Studio 中開發您的應用程式，或您比較喜歡使用命令列，您可以使用 **MakeAppx.exe** 工具轉換您的 `SourceAppxContentGroupMap.xml`。 

一道簡單的 **MakeAppx.exe** 命令看起來會像這樣︰
```syntax
MakeAppx convertCGM /s MyApp\SourceAppxContentGroupMap.xml /f MyApp\AppxContentGroupMap.xml /d MyApp\
```

/s 選項指定 `SourceAppxContentGroupMap.xml` 的路徑，/f 則指定了 `AppxContentGroupMap.xml` 的路徑。 最後的選項 /d 則是指定了展開檔案名稱萬用字元的基礎資料夾，而在這個範例中我們使用的即是應用程式專案資料夾。

如需有關 **MakeAppx.exe** 中可用選項的詳細資訊，請開啟命令提示字元，巡覽至 **MakeAppx.exe**，然後輸入︰

```syntax
MakeAppx convertCGM /?
```

這些就是取得您最終 `AppxContentGroupMap.xml` 檔案並為您的應用程式準備就緒所需要的所有步驟！ 還有更多執行您應用程式才會完全準備好在 Microsoft Store。 如需有關將 UAP App 串流安裝加入您應用程式之完整程序的詳細資訊，請參閱[這篇部落格文章](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/)。
