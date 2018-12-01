---
description: 設定 XAML 編譯，以在標記與程式碼後置之間加入部分類別。 程式碼部分類別定義在獨立的程式碼檔案中，標記部分類別則是在 XAML 編譯期間透過程式碼產生所建立的。
title: xClass 屬性
ms.assetid: 40A7C036-133A-44DF-9D11-0D39232C948F
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8eb1238499355cf37b3f5113dbb10c456de55961
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8338770"
---
# <a name="xclass-attribute"></a>x:Class 屬性


設定 XAML 編譯，以在標記與程式碼後置之間加入部分類別。 程式碼部分類別定義在獨立的程式碼檔案中，標記部分類別則是在 XAML 編譯期間透過程式碼產生所建立的。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法


``` syntax
<object x:Class="namespace.classname"...>
  ...
</object>
```

## <a name="xaml-values"></a>XAML 值

| 詞彙 | 說明 |
|------|-------------|
| 命名空間 | 選用。 指定包含 _classname_ 識別的部分類別的命名空間。 如果指定 _namespace_，則會使用點 (.) 分隔 _namespace_ 與 _classname_。 如果省略 _namespace_，會假設 _classname_ 沒有命名空間。 |
| classname | 必要。 指定連接載入的 XAML 與該 XAML 的程式碼後置的部分類別的名稱。 | 

## <a name="remarks"></a>備註

**x:Class** 可以宣告為建置動作正在編譯的 XAML 檔案/物件樹根目錄的任一元素的屬性，或宣告為已編譯應用程式的應用程式定義中的 [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 根目錄的屬性。 在任何情況下，為未使用 **\[頁面\]** 建置動作編譯的 XAML 檔案在根節點外的任何元素中宣告 **x:Class**，會導致編譯時期錯誤。

做為 **x:Class** 的類別不可以是巢狀類別。

**x:Class** 屬性的值，必須是指定類別完整名稱的字串。 只要命名空間資訊是關於程式碼後置如何建構 (而且您的類別定義在該類別層級定義開始)，您就可以省略該資訊。 頁面或應用程式定義的程式碼後置檔案，必須在專案所包含的程式碼檔案中。 程式碼後置類別必須是公開類別。 程式碼後置類別必須是部分類別。

## <a name="clr-language-rules"></a>CLR 語言規則

雖然您的程式碼後置檔案可能是 C++ 檔案，但某些慣例仍舊依循 CLR 語言格式，因此，在 XAML 語法上並無差異。 特別的是，任何 **x:Class** 值的命名空間與類別名稱元件之間的分隔符號永遠都是點 (".")，雖然在與 XAML 關聯的 C++ 程式碼檔案中，命名空間與類別名稱之間的分隔符號是 "::"。 如果您使用 C++ 宣告巢狀命名空間，則當您指定 **x:Class** 值的 *namespace* 部分時，後續巢狀命名空間字串之間的分隔符號也應該是 "." 而不是 "::"。

