---
Description: Describes the requirements for declaring your Universal Windows Platform (UWP) app as accessible in the Microsoft Store.
ms.assetid: 59FA3B87-75A6-4B30-BA7C-A0E769D68050
title: Store 中的協助工具
label: Accessibility in the Store
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6a9991cd4a0a3fce630b1c7be64650c79daf74e6
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7975719"
---
# <a name="accessibility-in-the-store"></a>Store 中的協助工具  



描述在 Microsoft Store 中，宣告您的通用 Windows 平台 (UWP) app 可提供無障礙功能的要求條件。

將您的 App 送出到 Microsoft Store 進行認證時，您可以宣告 App 可提供無障礙功能。 將您的應用程式宣告為無障礙應用程式，可以讓需要使用無障礙應用程式的使用者 (例如視覺障礙者) 比較容易找到這些程式。 使用者搜尋 Microsoft Store 時，可以使用 **\[無障礙\]** 篩選器尋找無障礙 app。 宣告您的 App 可提供無障礙功能時，也會在您的 App 介紹加上 **\[無障礙\]** 標記。

將 app 宣告為無障礙 app，說明該 app 具有符合使用者主要案例所需的[基本協助工具資訊](basic-accessibility-information.md)，可使用下列一或多個項目：

* 鍵盤。
* 高對比佈景主題。
* 可變的每英吋點數 (DPI) 設定。
* 常見的輔助技術，例如 Windows 協助工具功能，包含朗讀程式、放大鏡及螢幕小鍵盤。

如果您的應用程式建置時已考量無障礙性並已加以測試，則應該將它宣告為無障礙應用程式。 這表示您已執行下列動作：

* 針對 UI 元素 (包含名稱、角色、值等) 設定所有相關的協助工具資訊。
* 實作完整的鍵盤協助工具，可讓使用者：
    * 只使用鍵盤就能完成主要的應用程式案例。
    * 使用 Tab 鍵，以邏輯順序在 UI 元素之間輪換。
    * 使用方向鍵，在控制項內的 UI 元素之間瀏覽。
    * 使用鍵盤快速鍵，存取主要的應用程式功能。
    * 在未配置鍵盤的裝置，針對等同於 Tab 鍵和方向鍵的動作使用朗讀程式觸控手勢。
* 確保您的應用程式 UI 不會造成視覺上的障礙；文字對比率至少要 4.5:1，且不會單獨依賴色彩來傳達資訊等。
* 使用協助工具測試工具 (例如 [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) 和 [**UIAVerify**](https://msdn.microsoft.com/library/windows/desktop/Hh920986)) 驗證您實作的無障礙功能，並解決此類工具報告的所有第 1 優先順序錯誤。
* 使用朗讀程式、放大鏡、螢幕小鍵盤、高對比佈景主題，以及調整過的 DPI 設定，全盤檢驗 App 的各個主要功能。

請參閱[協助工具檢查清單](accessibility-checklist.md)，了解可協助您完成工作的相關程序和資源連結。

<span id="related_topics"/>

## <a name="related-topics"></a>相關主題    
* [協助工具](accessibility.md) 
