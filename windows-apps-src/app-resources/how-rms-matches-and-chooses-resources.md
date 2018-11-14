---
author: stevewhims
Description: When a resource is requested, there may be several candidates that match the current resource context to some degree. The Resource Management System will analyze all of the candidates and determine the best candidate to return. This topic describes that process in detail and gives examples.
title: 資源管理系統如何比對和選擇資源
template: detail.hbs
ms.author: stwhi
ms.date: 10/23/2017
ms.topic: article
keywords: Windows 10, uwp, 資源, 影像, 資產, MRT, 限定詞
ms.localizationpriority: medium
ms.openlocfilehash: c7576f98045bce3bcfcee093aa8d61059354d45a
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "6201904"
---
# <a name="how-the-resource-management-system-matches-and-chooses-resources"></a>資源管理系統如何比對和選擇資源
當要求資源時，也許有幾個某程度符合目前資源內容的候選項目。 資源管理系統將會分析所有的候選項目，並判斷要傳回的最佳候選項目。 做法是為所有的候選項目排名時將所有限定詞列入考慮。

在這個排名程序中，會提供不同的優先順序給不同的限定詞：語言對於整體排名上有最大的影響，接著是對比，然後是縮放等。 對於每個限定詞，候選限定詞會與內容限定詞值做比較，以判定相符項的品質。 使用的比較方式取決於限定詞。

如需有關如何執行語言標記比對的詳細資訊，請參閱[資源管理系統如何比對語言標記](how-rms-matches-lang-tags.md)。

對於某些限定詞，例如縮放和對比，一定有些最小程度的相符。 例如，符合 「 縮放 100 」 的內容相符 「 縮放 400 」 某些小程度，雖然但卻不以及符合的候選項目 「 縮放比例-200 」 或 （完全相符的） 「 縮放 400 」 的候選項目。

不過，對於其他限定詞，諸如語言或家鄉地區可能會有非相符項的比較 (以及相符的程度)。 例如，符合語言為「en-US」的候選項目與「EN-GB」的內容部分相符，但是符合「fr」的候選項目則完全不相符。 同樣地，符合家鄉地區為「155」(西歐) 的候選項目多少與家鄉地區設定為「FR」的使用者內容相符，但是符合「US」的候選項目則完全不相符。

評估候選項目時，如果任何限定詞有非相符項目比較，則該候選項目將會取得整體非相符項的排名，並不會被選取。 如此一來，較高優先順序的限定詞在選取最佳相符者時可以有最大的權數，但即使較低優先順序的限定詞也可以因為非相符項排除候選項目。

如果針對某限定詞候選項目完全沒有標示，則候選項目與該限定詞的關係為中立。 對於任何限定詞，中立的候選項目始終是內容限定詞值的相符項，但只是相符項的品質比任何為該限定詞標示的候選項目來得低，並且某程度上 (完全或部分) 相符。 例如，如果我們有符合「en-US」、「en」、「fr」的候選項目，同時也是非語言相關的候選項目，則對於語言限定詞值為「en-GB」的內容，候選項目將會以下列順序排名：「en」、「en-US」、中立和「fr」。 若是如此，「fr」一點都不符合，同時其他候選項目某程度符合。

整體排名程序是從評估與最高優先順序的限定詞相關的候選項目開始，也就是語言。 非相符項目會排除。 剩餘的候選項目會依照其與語言相符的品質來排名。 如果有任何關係，則考慮下一個最高優先順序的限定詞、對比，使用對比的相符品質目來區分相關的候選項目。 在對比之後，使用縮放限定詞來區分剩餘的關係，然後以此類推視需要盡可能使用許多限定詞直到井然有序地完成排名。

如果因為限定詞與內容不相符而不考慮所有的候選項目，資源載入器會進行第二輪尋找要顯示的預設候選項目。 預設候選項目的判定是在建立 PRI 檔案時，並且需要確保任何執行階段內容都有一些候選項目可供選取。 如果候選項目有任何不相符的限定詞，並且不是預設值，則永久不予考慮該資源候選項目。

對於仍在考慮中的所有資源候選項目，資源載入器會查看最高優先順序的內容限定詞值，並選擇一個最相符者或最佳預設分數。 任何實際相符項目會被視為比預設分數來得好。

如果有關係，則會檢查下一個最高優先順序的內容限定詞值並持續進行此程序，直到找到最佳相符者。

## <a name="example-of-choosing-a-resource-candidate"></a>選擇資源候選項目的範例
請考慮這些檔案。

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
fr/images/contrast-high/logo.scale-400.jpg
fr/images/contrast-high/logo.scale-100.jpg
de/images/logo.jpg
```

並假設這些都是目前內容中的設定。

```console
Application language: en-US; fr-FR;
Scale: 400
Contrast: Standard
```

資源管理系統會排除三個檔案，因為高對比以及德文語言與設定所定義的內容不相符。 剩下這些候選項目。

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

對於剩餘的候選項目，資源管理系統會使用的最高優先順序內容限定詞，也就是語言。 英文資源是比法文資源更接近，因為在設定中英文列在法文之前。

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
```

接下來，資源管理系統使用下一個最高優先順序內容限定詞，也就是縮放。 因此，這是傳回的資源。

```console
en/images/logo.scale-400.jpg
```

您可以使用進階的 [**NamedResource.ResolveAll**](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live) 方法，以符合內容設定的順序來擷取所有候選項目。 對於我們剛剛瀏覽過的範例，**ResolveAll** 是以此順序傳回候選項目。

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

## <a name="example-of-producing-a-fallback-choice"></a>遞補選擇的範例
請考慮這些檔案。

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

並假設這些都是目前內容中的設定。

```console
User language: de-DE;
Scale: 400
Contrast: High
```

所有檔案因為與內容不相符而都被排除。 所以我們輸入預設輪次，其中在建立 PRI 檔案期間的預設值 (請參閱[手動以 MakePri.exe 編譯資源](compile-resources-manually-with-makepri.md)) 是這個。

```console
Language: fr-FR;
Scale: 400
Contrast: Standard
```

這會留下與目前的使用者或預設值相符的所有資源。

```console
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

資源管理系統會使用最高優先順序內容限定詞，也就是語言來傳回分數最高的具名資源。

```console
de/images/contrast-standard/logo.jpg
```

## <a name="important-apis"></a>重要 API
* [NamedResource.ResolveAll](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live)

## <a name="related-topics"></a>相關主題
* [使用 MakePri.exe 來手動編譯資源](compile-resources-manually-with-makepri.md)