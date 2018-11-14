---
author: jwmsft
ms.assetid: 9899F6A0-7EDD-4988-A76E-79D7C0C58126
title: 通用 Windows 平台元件和最佳化互通性
description: 建立使用 UWP 元件和原生與 Managed 類型之間的互通性，同時可避免互通性效能問題的通用 Windows 平台 (UWP) app。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 726dc4aaa34b9b68aa198e236abcef57b78b21f4
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6645891"
---
# <a name="uwp-components-and-optimizing-interop"></a>UWP 元件和最佳化 Interop


建立使用 UWP 元件和原生與 Managed 類型之間的互通性，同時可避免互通性效能問題的通用 Windows 平台 (UWP) app。

## <a name="best-practices-for-interoperability-with-uwp-components"></a>UWP 元件互通性的最佳做法

如果您不夠謹慎，使用 UWP 元件有可能對應用程式效能造成很大的影響。 本捷討論如何在應用程式使用 UWP 元件時獲得良好的效能。

### <a name="introduction"></a>簡介

互通性對於效能可能會有很大的影響，而且您可能正在使用它卻不自覺。 UWP 會為您處理許多的互通性問題，這樣您就可以提高生產力，並重複使用以其他語言撰寫的程式碼。 我們建議您善加利用 UWP 的功能，但是請了解它可能會影響效能。 本節討論您可以執行的操作，以減輕互通性對應用程式效能的影響。

UWP 具有一個類型庫，這個類型庫可從任何可撰寫 UWP 應用程式的語言存取。 您可以使用 C# 或 Microsoft Visual Basic 中的 UWP 類型，其使用方式與 .NET 物件的使用方式相同。 您不需要執行平台叫用方法呼叫，就可以存取 UWP 元件。 這使得撰寫應用程式比較不複雜，但是請務必了解會發生比預期更多的互動性。 如果 UWP 元件是以非 C# 或 Visual Basic 的語言來撰寫，當您使用該元件時，會跨越互通性界限。 跨越互通性界限可能會影響應用程式的效能。

當您在 C# 或 Visual Basic 中開發 UWP app 時，您使用的兩組最常見的 API 是 UWP API 與適用於 UWP app 的 .NET API。 一般來說，在 UWP 中定義的類型位於開頭為 "Windows." 的命名空間， 而 .NET 類型則位於開頭為 "System." 的命名空間， 但還是有些例外。 使用適用於 UWP app 的 .NET API 中所列的類型時，並不需要互通性。 如果您發現在使用 UWP 時效能不佳，可以改用適用於 UWP App 的 .NET，以獲得較佳的效能。

**注意：** 大部分與 windows 10 隨附的 UWP 元件實作在 c + + 中，因此當您從 C# 或 Visual Basic 使用時，會跨越互通性界限。 一如往常，請務必先評估您的應用程式以了解使用 UWP 元件是否會影響應用程式的效能，再花費時間和精力變更程式碼。

在這個主題中，當我們提到「UWP 元件」時，是指以 C# 或 Visual Basic 以外的語言所撰寫的元件。

 

每次您在 UWP 元件存取屬性或是呼叫方法時，都會產生互通性成本。 事實上，建立 UWP 元件比建立 .NET 物件的成本更高。 這個問題的原因是 UWP 所執行的程式碼，是從應用程式的語言轉換到元件的語言。 另外，如果您將資料傳遞給元件，在 Managed 和 Unmanaged 類型之間必須轉換資料。

### <a name="using-uwp-components-efficiently"></a>有效使用 UWP 元件

如果您發現需要獲得較佳的效能，可以確保程式碼盡可能有效使用 UWP 元件。 本節討論使用 UWP 元件時改善效能的一些秘訣。

必須在短期間內有大量的呼叫，才能使效能影響顯現出來。 將呼叫從商務邏輯與其他 Managed 程式碼封裝成 UWP 元件的設計良好的應用程式，應該不會產生龐大的互通性成本。 但是如果您的測試指出使用 UWP 元件會影響應用程式的效能，則在本節中討論的秘訣可協助您改善效能。

### <a name="consider-using-net-for-uwp-apps"></a>考慮使用適用於 UWP 應用程式的 .NET

在某些情況下，您可以使用 UWP 或適用於 UWP 應用程式的 .NET 來完成工作。 建議您試著不要混合使用 .NET 類型與 UWP 類型。 試著只使用其中一種或其他種類型。 例如，您可以使用 [**Windows.Data.Xml.Dom.XmlDocument**](https://msdn.microsoft.com/library/windows/apps/BR206173) 類型 (UWP 類型) 或是 [**System.Xml.XmlReader**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.xmlreader.aspx) 類型 (.NET 類型)，來分析 XML 的資料流。 使用來自與資料流相同技術的 API。 例如，如果您從 [**MemoryStream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.memorystream.aspx) 讀取 XML，請使用 **System.Xml.XmlReader** 類型，因為兩個類型都是 .NET 類型。 如果您從檔案讀取，請使用 **Windows.Data.Xml.Dom.XmlDocument** 類型，因為檔案 API 與 **XmlDocument** 是 UWP 元件。

### <a name="copy-window-runtime-objects-to-net-types"></a>將 Window 執行階段物件複製到 .NET 類型

UWP 元件傳回 UWP 物件時，將傳回的物件複製到 .NET 物件，可能會非常實用。 有兩個地方特別重要，那就是使用集合與資料流時。

如果您呼叫會傳回集合的 UWP API，然後儲存和存取該集合許多次，則將該集合複製到 .NET 集合，並從之後都使用 .NET 版本，可能會非常實用。

### <a name="cache-the-results-of-calls-to-uwp-components-for-later-use"></a>快取呼叫 UWP 元件的結果以供後續使用

您可以將值儲存到區域變數，而不是多次存取 UWP 類型，以獲得較佳的效能。 如果您使用迴圈中的值，將特別實用。 請評估您的應用程式，以了解使用區域變數是否可改善應用程式的效能。 使用快取的值可以提升應用程式的速度，因為它在互通性上花費較少的時間。

### <a name="combine-calls-to-uwp-components"></a>結合對 UWP 元件的呼叫

試著盡量以最少數目的 UWP 物件呼叫來完成工作。 例如，通常最好是一次從資料流中讀取大量的資料，而不是讀取少量的資料。

透過將工作包含在盡可能愈少呼叫中的方式來使用 API，而不是透過做比較少的工作但需要更多呼叫的方式來使用 API。 例如，最好透過呼叫會初始化多個屬性的建構函式來建立物件，而不是呼叫預設的建構函式並一次指派一個屬性。

### <a name="building-a-uwp-components"></a>建置 UWP 元件

如果您撰寫的 UWP 元件可由 C++ 或 JavaScript 撰寫的應用程式使用，請確定您所設計的元件具有良好的效能。 所有針對如何使應用程式獲得良好效能的建議，也適用於如何使元件獲得良好效能。 請評估您的元件以了解哪些 API 具有高流量模式，而且對於那些部分，請考慮提供可讓使用者以很少的呼叫執行工作的 API。

## <a name="keep-your-app-fast-when-you-use-interop-in-managed-code"></a>在 Managed 程式碼中使用互通性時保持應用程式的快速效能

UWP 可讓原生與 Managed 程式碼之間的互通更為容易，但只要一不注意就很容易產生效能成本。 我們將說明在您的 Managed UWP 應用程式中使用互通性時如何獲得最佳效能。

UWP 可讓開發人員以選擇的語言編寫使用 XAML 的應用程式，這都要歸功於可在每個語言中使用的 UWP API 設計。 以 C# 或 Visual Basic 撰寫應用程式時，互通性雖然方便卻有其代價，因為 UWP API 通常會在機器碼中實作，而任何來自 C# 或 Visual Basic 的 UWP 叫用會要求 CLR 從 Managed 轉換成原生堆疊框架，然後將函式參數封送處理為機器碼可以存取的表示法。 大部分應用程式會忽略這個額外的成本。 但是當您在某個應用程式的關鍵路徑中多次呼叫 (數十萬到數百萬個呼叫) UWP API 時，這個成本便非常可觀。 一般而言，您會希望語言之間轉換所花的時間比執行程式碼其他部分所花的時間相對較少。 如下圖所示。

![互通性轉換不應占去大量程式執行時間。](images/interop-transitions.png)

[**.NET for Windows apps**](https://msdn.microsoft.com/library/windows/apps/xaml/br230232.aspx) 中列出的類型從 C# 或 Visual Basic 中使用時不會產生這個互通性成本。 做為經驗法則，您可以假設命名空間中以 “Windows.” 為開頭的類型 屬於 UWP，而命名空間中以 “System.” 為開頭的類型 則是 .NET 類型。 請記住，就算是簡單的 UWP 類型使用 (例如配置或屬性存取) 也會產生互通性成本。

您應該測量應用程式並判斷互通性是否佔用了應用程式大部分執行時間，然後再最佳化互通性成本。 使用 Visual Studio 分析 app 效能前，可以使用 [**功能**] 檢視並查看呼叫至 UWP 中的方法所耗費的時間，輕易取得互通性成本上限。

如果您的 app 因互通性負荷而變慢，您可以在最忙碌的程式碼路徑中減少對 UWP API 的呼叫，以便提高 app 的效能。 例如，若要在不斷查詢 [**UIElements**](https://msdn.microsoft.com/library/windows/apps/BR208911) 位置和維度以執行大量物理計算的遊戲引擎中節省執行時間，您可以將來自 **UIElements** 的必要資訊儲存到區域變數，並在這些快取的值上進行計算，然後在計算完成後將最終結果指派回 **UIElements**。 另一個例子：如果 C# 或 Visual Basic 程式碼大量存取某個集合，則使用 [**System.Collections**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.aspx) 命名空間中的集合比使用 [**Windows.Foundation.Collections**](https://msdn.microsoft.com/library/windows/apps/BR206657) 命名空間中的集合更有效率。 您也可以考慮將 UWP 元件的呼叫結合起來；可以這樣做的其中一個範例就是使用 [**Windows.Storage.BulkAccess**](https://msdn.microsoft.com/library/windows/apps/BR207676) API。

### <a name="building-a-uwp-component"></a>建置 UWP 元件

如果您撰寫供 C++ 或 JavaScript 撰寫的應用程式使用的 UWP 元件，請確定您所設計的元件具有良好的效能。 您的 API 表面會定義互通性界限並定義使用者必須思考本主題所述之指導方針的程度。 如果您要將元件發佈給其他人，那麼這就變得異常重要了。

所有針對如何使應用程式獲得良好效能的建議，也適用於如何使元件獲得良好效能。 請評估您的元件以了解哪些 API 具有高流量模式，而且對於那些部分，請考慮提供可讓使用者以很少的呼叫執行工作的 API。 為了讓應用程式可以使用 UWP 而不需經常跨越互通性界限，在 UWP 的設計上花費了許多精力。

 

