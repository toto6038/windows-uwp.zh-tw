---
author: Jwmsft
Description: "使用標籤向使用者表示應該在緊鄰的控制項中輸入的內容。 您也可以為一組相關的控制項加上標籤，或是在一組相關的控制項附近顯示說明文字。"
title: "標籤"
ms.assetid: CFACCCD4-749F-43FB-947E-2591AE673804
label: Labels
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 54610e29b0fcaa8b7e90cf00676098a2ea50b827
ms.lasthandoff: 02/07/2017

---
# <a name="labels"></a>標籤

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

標籤是控制項或一組相關控制項的名稱或標題。

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>Header 屬性</li>
<li>[**TextBlock 類別**](https://msdn.microsoft.com/library/windows/apps/br209652)</li>
</ul>
</div>


在 XAML 中，許多控制項都具備可用來顯示標籤的內建 Header 屬性。 對於沒有 Header 屬性的控制項，或是要對一組控制項加上標籤，則可改用 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)。


## <a name="example"></a>範例


![說明標準標籤控制項的螢幕擷取畫面](images/label-standard.png)

## <a name="recommendations"></a>建議事項


-   使用標籤向使用者表示應該在緊鄰的控制項中輸入的內容。 您也可以為一組相關的控制項加上標籤，或是在一組相關的控制項附近顯示說明文字。
-   為控制項加上標籤時，可填寫名詞或簡要的名詞片語，而非長句或說明文字。 請避免使用冒號或其他標點符號。
-   當您的標籤中有說明文字時，您對於文字字串的長度限制可能會更寬鬆，而且也會使用標點符號。


## <a name="get-the-sample-code"></a>取得範例程式碼
* [XAML UI 基本知識範例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>相關主題
* [文字控制項](text-controls.md)

**適用於開發人員**
* [**TextBox.Header 屬性**](https://msdn.microsoft.com/library/windows/apps/dn252861)
* [**PasswordBox.Header 屬性**](https://msdn.microsoft.com/library/windows/apps/dn299051)
* [**ToggleSwitch.Header 屬性**](https://msdn.microsoft.com/library/windows/apps/br209713)
* [**DatePicker.Header 屬性**](https://msdn.microsoft.com/library/windows/apps/dn279460)
* [**TimePicker.Header 屬性**](https://msdn.microsoft.com/library/windows/apps/dn299286)
* [**Slider.Header 屬性**](https://msdn.microsoft.com/library/windows/apps/dn252829)
* [**ComboBox.Header 屬性**](https://msdn.microsoft.com/library/windows/apps/dn279416)
* [**RichEditBox.Header 屬性**](https://msdn.microsoft.com/library/windows/apps/dn252726)
* [**TextBlock 類別**](https://msdn.microsoft.com/library/windows/apps/br209652)

 

 





