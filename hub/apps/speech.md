---
title: Windows 10 中的語音、語音和對話
description: 本頁面提供的資訊可讓您開始開發啟用語音的 Windows 應用程式。
ms.topic: article
ms.date: 09/12/2019
keywords: 語音（Windows 10、語音、語音、對話、win32 語音應用程式、UWP 語音應用程式、WPF 語音應用程式、WinForms 語音應用程式）
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: d810f08a2db60309e4528167bcb4bddc95d850c6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174152"
---
# <a name="speech-voice-and-conversation-in-windows-10"></a>Windows 10 中的語音、語音和對話

![語音主圖影像](images/hero-speech-composite-small.png)

語音可以是一種有效、自然且好用的方式，可讓使用者與您的 Windows 應用程式互動、補充，甚至是根據滑鼠、鍵盤、觸控、控制器或手勢取代傳統的互動體驗。

語音辨識、聽寫、語音合成 (（例如語音辨識、聽寫、語音合成也稱為文字轉換語音或 TTS) ，以及交談語音助理 (例如 Cortana 或 Alexa) ，可提供可存取和內含的使用者體驗，讓使用者在其他輸入裝置可能無法使用您的應用程式時，可以使用您的應用程式。

本頁面提供各種 Windows 開發架構如何針對建立 Windows 應用程式的開發人員提供語音辨識、語音合成和對話支援的相關資訊。

## <a name="platform-specific-documentation"></a>特定平台的文件

:::row:::
   :::column:::
      ![通用 Windows 平台 (UWP)](images/platform-uwp.png)

      **通用 Windows 平台 (UWP)**

      在新式平臺上建立支援語音的應用程式，以在任何 Windows (裝置上 Windows 10 應用程式和遊戲，包括電腦、手機、Xbox One、HoloLens 及更多) ，並將其發佈至 Microsoft Store。

      [語音互動](/windows/uwp/design/input/speech-interactions)

      [語音辨識](/windows/uwp/design/input/speech-recognition)

      [連續聽寫](/windows/uwp/design/input/enable-continuous-dictation)

      [語音合成](/uwp/api/windows.media.speechsynthesis)

      [對話代理程式](/uwp/api/windows.applicationmodel.conversationalagent)

      [Cortana voice 命令](/cortana/voice-commands/vcd)
   :::column-end:::
   :::column:::
      ![Win32 平台應用程式](images/platform-win32.png)

      **Win32 平台**

      使用此處提供的工具、資訊和範例引擎和應用程式，為 Windows 桌上型電腦和 Windows Server 開發啟用語音的應用程式。

      [Microsoft 語音平台 - 軟體開發套件 (SDK) (版本 11)](https://www.microsoft.com/download/details.aspx?id=27226)
      
      [Microsoft 語音 SDK，版本 5.1](https://www.microsoft.com/download/details.aspx?id=10121)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![.NET](images/platform-dotnet.png)

      **.NET Framework**

      在針對受管理 Windows 應用程式建立的平台上，利用 XAML UI 模型和 .NET Framework 開發無障礙的應用程式和工具。

      [適用於 .NET Framework 的 System.Speech 程式設計指南](/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))
   :::column-end:::
   :::column:::
      ![Azure 語音服務](images/platform-azure-speech.png)

      **Azure 語音服務**

      使用 Azure 語音服務來設計、建立及測試可存取的網站。

      [語音轉換文字](https://azure.microsoft.com/services/cognitive-services/speech-to-text/)

      [文字轉換語音](https://azure.microsoft.com/services/cognitive-services/text-to-speech/)
      
      [語音翻譯](https://azure.microsoft.com/services/cognitive-services/speech-translation/)

      [語音優先虛擬助理](/azure/cognitive-services/speech-service/voice-first-virtual-assistants)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **舊版功能**

      舊版、已淘汰及/或不支援的 Microsoft 語音技術版本。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Microsoft 代理程式](/windows/win32/lwef/microsoft-agent)

      [Microsoft 語音應用程式軟體發展工具組 (SASDK) 1.0 版](https://www.microsoft.com/download/details.aspx?id=2200)
   :::column-end:::
   :::column:::
      [Microsoft Speech API (SAPI) 5。3](/previous-versions/windows/desktop/ms723627(v=vs.85))

      [Microsoft Speech API (SAPI) 5。4](/previous-versions/windows/desktop/ee125663(v=vs.85))

      [Bing 語音辨識控制項](/previous-versions/bing/speech/dn434583(v=msdn.10))
   :::column-end:::
:::row-end:::

## <a name="samples"></a>範例

下載並執行示範各種協助工具特性和功能的完整 Windows 範例。

:::row:::
   :::column:::
      [程式碼範例瀏覽器](/samples/browse/?term=speech)

      新的範例瀏覽器 (取代了 MSDN 程式碼庫) 。
   :::column-end:::
   :::column:::
      [GitHub 上的 Windows 傳統範例](https://github.com/microsoft/Windows-classic-samples/search?q=speech&unscoped_q=speech)

      這些範例示範 Windows 和 Windows Server 的功能和程式設計模型。 
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [GitHub 上的通用 Windows 平台 (UWP) 範例](https://github.com/microsoft/Windows-universal-samples/search?q=speech&unscoped_q=speech)

      這些範例示範適用於 Windows 10 的 Windows 軟體開發套件 (SDK) 中通用 Windows 平台 (UWP) 的 API 使用模式。
   :::column-end:::
   :::column:::
      [XAML 控制項庫](https://github.com/microsoft/Xaml-Controls-Gallery)

      此應用程式示範 Fluent Design 系統中支援的各種 Xaml 控制項。
   :::column-end:::
:::row-end:::

## <a name="videos"></a>視訊

涵蓋如何建立併入語音互動之 Windows 應用程式的各種影片。

:::row:::
   :::column:::
      **深入瞭解 Cortana 和語音平臺**
   :::column-end:::
   :::column:::
      **通用 Windows 應用程式中的 Cortana 擴充性**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2015/3-716/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2015/2-691/player]
   :::column-end:::
:::row-end:::

## <a name="other-resources"></a>其他資源

:::row:::
   :::column:::
      **部落格和新聞**

      Microsoft 語音世界的最新消息。
   :::column-end:::
   :::column:::
      **社群與支援**

      Windows 開發人員和使用者都能以何種方式與您一起學習。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [在新聞中](https://news.microsoft.com/?s=speech)

      [語音 blog](https://blogs.windows.com/windowsdeveloper/?s=speech)
   :::column-end:::
   :::column:::
      [Windows 社區-語音](https://community.windows.com/search?q=speech)

      [Windows 語音開發人員論壇](https://social.msdn.microsoft.com/Forums/home?filter=alltypes&sort=firstpostdesc&searchTerm=speech)
   :::column-end:::
:::row-end:::