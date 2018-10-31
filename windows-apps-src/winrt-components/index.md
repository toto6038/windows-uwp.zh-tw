---
author: msatranjr
title: Windows 執行階段元件
description: Windows 執行階段元件是可讓您具現化並從任何語言 (包括 C#、Visual Basic、JavaScript 和 C++) 使用的獨立物件。
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b93b3028c7968c417d4476a6f183805cdec797b0
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5821984"
---
# <a name="windows-runtime-components"></a>Windows 執行階段元件
Windows 執行階段元件是可讓您具現化並從任何語言 (包括 C#、Visual Basic、JavaScript 和 C++) 使用的獨立物件。

您可以使用 Visual Studio 和 C#、Visual Basic 或 C++，來建立可在通用 Windows 平台 (UWP) 應用程式中使用的 Windows 執行階段元件。

| 主題 | 描述 |
|-------|-------------|
| [在 C++ 中建立 Windows 執行階段元件](creating-windows-runtime-components-in-cpp.md) | 本主題示範如何使用 C + + /CX 來建立 Windows 執行階段元件，可從使用 C#、 Visual Basic、 c + + 或 Javascript 建置的通用 Windows 應用程式呼叫的元件。 |
| [逐步解說：在 C++ 中建立基本 Windows 執行階段元件，然後從 JavaScript 或 C# 呼叫該元件](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | 本逐步解說示範如何建立可從 JavaScript、C# 或 Visual Basic 呼叫的基本 Windows 執行階段元件 DLL。 開始本逐步解說之前，請確定您了解一些概念，例如：抽象二進位介面 (ABI)、ref 類別，以及讓 ref 類別更容易使用的 Visual C++ 元件擴充功能。 如需詳細資訊，請參閱[在 C++ 中建立 Windows 執行階段元件](creating-windows-runtime-components-in-cpp.md)和 [Visual C++ 語言參考 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx)。 |
| [在 C# 和 Visual Basic 中建立 Windows 執行階段元件](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | 從 .NET Framework 4.5 開始，您可以使用 Managed 程式碼自行建立封裝在 Windows 執行階段元件中的 Windows 執行階段類型。 您可以在通用 Windows 平台 (UWP) app 中，將元件與 C++、JavaScript、Visual Basic 或 C# 搭配使用。 本主題概述建立元件，規則，並討論.NET Framework 支援的 Windows 執行階段中的某些層面。 一般而言，該支援依設計應可讓 .NET Framework 程式設計人員清楚理解。 但是，當您建立要與 JavaScript 或 C++ 搭配使用的元件時，必須了解這些語言對於 Windows 執行階段的支援方式有何差異。 |
| [逐步解說：建立簡單的 Windows 執行階段元件，並從 JavaScript 呼叫該元件](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | 本逐步解說示範如何使用 .NET Framework 搭配 Visual Basic 或 C#、建立您自己的 Windows 執行階段類型 (封裝於 Windows 執行階段元件中)，以及如何從使用 JavaScript 為 Windows 建置的通用 Windows 應用程式呼叫此元件。 |
| [在 Windows 執行階段元件中引發事件](raising-events-in-windows-runtime-components.md) | 如果您的 Windows 執行階段元件會在背景執行緒 (背景工作執行緒) 上引發使用者定義之委派類型的事件，而您希望 JavaScript 可以接收該事件，則可透過下列其中一種方式來實作和 (或) 引發此事件： | 
| [側載 UWP app 的代理 Windows 執行階段元件](brokered-windows-runtime-components-for-side-loaded-windows-store-apps.md) | 本主題討論 Windows 10 更新和更新版本所支援的企業導向功能，該功能允許方便觸控的 .NET 應用程式可使用負責重要業務關鍵作業的現有程式碼。 |
