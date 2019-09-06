---
title: Windows 執行階段類型的 .NET 對應
description: 下表列出 .NET 在通用 Windows 平臺（UWP）類型和 .NET 類型之間所做的對應。
ms.assetid: 5317D771-808D-4B97-8063-63492B23292F
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c035db58fc6aa484f9d47a9af61176a2b05d55ee
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393652"
---
# <a name="net-mappings-of-windows-runtime-types"></a>Windows 執行階段類型的 .NET 對應

下表列出 .NET 在通用 Windows 平臺（UWP）類型和 .NET 類型之間所做的對應。 在使用 managed 程式碼撰寫的通用 Windows 應用程式中，Visual Studio IntelliSense 會顯示 .NET 類型，而不是 UWP 類型。 例如，如果 Windows 執行階段方法接受&lt;IVector 字串&gt;類型的參數，則 IntelliSense 會顯示類型為 IList&lt;string&gt;的參數。 同樣地，在使用 managed 程式碼撰寫的 Windows 執行階段元件中，您會在成員簽章中使用 .NET 類型。 當[Windows 執行階段中繼資料匯出工具（Winmdexp）](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool)產生您的 Windows 執行階段元件時，.net 類型會轉譯成對應的 UWP 類型。

UWP 和 .NET 中具有相同命名空間名稱和類型名稱的大部分類型都是結構（或與結構相關聯的類型，例如列舉）。 在 UWP 中，結構沒有欄位以外的成員，而且需要協助程式類型（.NET 會隱藏）。 這些結構的 .NET 版本具有屬性和方法，可提供隱藏的 helper 類型的功能。

## <a name="uwp-types-that-map-to-net-types-with-the-same-name-and-namespace"></a>對應至具有相同名稱和命名空間之 .NET 類型的 UWP 類型

### <a name="in-net-assembly-systemobjectmodeldll"></a>在 .NET 元件系統中 System.collections.objectmodel.observablecollection .dll

| 命名空間 | Type |
|-|-|
| Windows.UI.Xaml.Input | ICommand |

### <a name="in-net-assembly-systemruntimewindowsruntimedll"></a>在 .NET assembly System.runtime.interopservices.windowsruntime.asyncinfo .dll 中

| 命名空間 | Type |
|-|-|
| Windows.Foundation | 點 |
| Windows.Foundation | Rect |
| Windows.Foundation | Size |
| Windows.UI | 色彩 |

### <a name="in-net-assembly-systemruntimewindowsruntimeuixamldll"></a>在 .NET 元件 System.web 中 System.runtime.interopservices.windowsruntime.asyncinfo。

| 命名空間 | Type |
|-|-|
| Windows.UI.Xaml | CornerRadius |
| Windows.UI.Xaml | Duration |
| Windows.UI.Xaml | DurationType |
| Windows.UI.Xaml | GridLength |
| Windows.UI.Xaml | GridUnitType |
| Windows.UI.Xaml | Thickness |
| Windows.UI.Xaml.Controls.Primitives | GeneratorPosition |
| Windows.UI.Xaml.Media | 矩陣 |
| Windows.UI.Xaml.Media.Animation | KeyTime |
| Windows.UI.Xaml.Media.Animation | RepeatBehavior |
| Windows.UI.Xaml.Media.Animation | RepeatBehaviorType |
| Windows.UI.Xaml.Media.Media3D | Matrix3D |

## <a name="uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace"></a>對應至具有不同名稱和/或命名空間之 .NET 類型的 UWP 類型

### <a name="in-net-assembly-systemobjectmodeldll"></a>在 .NET 元件系統中 System.collections.objectmodel.observablecollection .dll

| UWP 類型/命名空間 | .NET 類型/命名空間 |
|-|-|
| INotifyCollectionChanged (Windows.UI.Xaml.Interop) | INotifyCollectionChanged (System.Collections.Specialized) | 
| NotifyCollectionChangedEventHandler (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventHandler (System.Collections.Specialized) | 
| NotifyCollectionChangedEventArgs (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventArgs (System.Collections.Specialized) | 
| NotifyCollectionChangedAction (Windows.UI.Xaml.Interop) | NotifyCollectionChangedAction (System.Collections.Specialized) | 
| INotifyPropertyChanged (Windows.UI.Xaml.Data) | INotifyPropertyChanged (System.ComponentModel) | 
| PropertyChangedEventHandler (Windows.UI.Xaml.Data) | PropertyChangedEventHandler (System.ComponentModel) | 
| PropertyChangedEventArgs (Windows.UI.Xaml.Data) | PropertyChangedEventArgs (System.ComponentModel) | 

### <a name="in-net-assembly-systemruntimedll"></a>在 .NET 元件 System.web 中

| UWP 類型/命名空間 | .NET 類型/命名空間 |
|-|-|
| AttributeUsageAttribute (Windows.Foundation.Metadata) | AttributeUsageAttribute (System) |
| AttributeTargets (Windows.Foundation.Metadata) | AttributeTargets (System) |
| DateTime (Windows.Foundation) | DateTimeOffset (System) |
| EventHandler&lt;T&gt; (Windows.Foundation) | EventHandler&lt;T&gt; (System) |
| HResult (Windows.Foundation) | Exception (System) |
| IReference&lt;T&gt; (Windows.Foundation) | Nullable&lt;T&gt; (System) |
| TimeSpan (Windows.Foundation) | TimeSpan (System) |
| Uri (Windows.Foundation) | Uri (System) |
| IClosable (Windows.Foundation) | IDisposable (System) |
| IIterable&lt;T&gt; (Windows.Foundation.Collections) | IEnumerable&lt;T&gt; (System.Collections.Generic) |
| IVector&lt;T&gt; (Windows.Foundation.Collections) | IList&lt;T&gt; (System.Collections.Generic) |
| IVectorView&lt;T&gt; (Windows.Foundation.Collections) | IReadOnlyList&lt;T&gt; (System.Collections.Generic) |
| IMap&lt;K,V&gt; (Windows.Foundation.Collections) | IDictionary&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IMapView&lt;K,V&gt; (Windows.Foundation.Collections) | IReadOnlyDictionary&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IKeyValuePair&lt;K,V&gt; (Windows.Foundation.Collections) | KeyValuePair&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IBindableIterable (Windows.UI.Xaml.Interop) | IEnumerable (System.Collections) |
| IBindableVector (Windows.UI.Xaml.Interop) | IList (System.Collections) |
| TypeName (Windows.UI.Xaml.Interop) | Type (System) |

### <a name="in-net-assembly-systemruntimeinteropserviceswindowsruntimedll"></a>在 .NET assembly System.web 中。 System.runtime.interopservices.outattribute. System.runtime.interopservices.windowsruntime.asyncinfo .dll

| UWP 類型/命名空間 | .NET 類型/命名空間 |
|-|-|
| EventRegistrationToken (Windows.Foundation) | EventRegistrationToken (System.Runtime.InteropServices.WindowsRuntime) |

## <a name="related-topics"></a>相關主題

* [使用C#和 Visual Basic Windows 執行階段元件](creating-windows-runtime-components-in-csharp-and-visual-basic.md)