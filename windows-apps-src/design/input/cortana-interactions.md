---
description: 使用在 Windows 應用程式中啟動和執行單一動作的語音命令，擴充 **Cortana** 的基本功能。
title: Windows 應用程式中的 Cortana 互動
ms.assetid: 4C11A7CF-DA26-4CA1-A9B9-FE52670101F5
label: Cortana
template: detail.hbs
keywords: Cortana, Cortana 畫布, Cortana 設計, 使用者介面, 語音命令, VCD
ms.date: 01/27/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6187022f4fcce254d142c61f1c2fe222c11bd8a5
ms.sourcegitcommit: 8fe992f3a6d8f7975af4911ad88e855bee50083e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/05/2021
ms.locfileid: "99606043"
---
# <a name="cortana-interactions-in-windows-apps"></a>Windows 應用程式中的 Cortana 互動

>[!WARNING]
> 這項功能已不再支援，因為 Windows 10 2020 版更新 (2004 版（codename "20H1" ) ）。
>
> 請參閱 [Microsoft 365 中](/microsoft-365/admin/misc/cortana-integration) cortana 如何改造新式生產力體驗的 cortana。

使用在 Windows 應用程式中啟動和執行單一動作的語音命令，擴充 **Cortana** 的基本功能。

您可以在前景中啟動目標應用程式 (應用程式取得焦點，並在背景中關閉 **cortana**) 或在背景中啟動 (**cortana** 會保留焦點，但會根據互動的複雜度，從應用程式) 提供結果。 一般而言，需要額外內容或使用者輸入的語音命令最好在前景應用程式中處理，而基本命令可透過背景應用程式在 **Cortana** 中處理。

藉由整合應用程式的基本功能，並為使用者提供中央進入點來完成大部分的工作，而不直接開啟您的應用程式， **Cortana** 就成為您的應用程式與使用者之間的聯絡。 為應用程式功能提供此快捷方式，並減少切換應用程式的需求，可以節省使用者大量時間和精力。

> [!NOTE]
> 語音命令是具有特定意圖的單一語句，定義于語音命令定義 (VCD) 檔，透過 **Cortana** 導向已安裝的應用程式。
>
> VCD 檔案會定義一或多個語音命令，每個命令都有獨特的意圖。
>
> 語音命令定義可能會有不同的複雜度。 它們可以支援從單一、受限的語句到更具彈性的自然語言語句集合的任何作業，而且全都代表相同的意圖。

## <a name="other-speech-and-conversation-components"></a>其他語音和交談元件

### <a name="speech-voice-and-conversation-in-windows-10"></a>Windows 10 中的語音、語音和對話

如需各種 Windows 開發架構如何針對建立 Windows 應用程式的開發人員提供語音辨識、語音合成和對話支援的相關資訊，請參閱 [Windows 10 中的語音、語音和交談](/windows/apps/speech) 。

### <a name="cortana-skills-kit"></a>Cortana 技能套件

如果您想要藉由新增自己的技能，讓使用者透過 Cortana 與您的 **服務** 互動，請參閱 [cortana 技能套件](/cortana/skills/)。 [淘汰 **通知：** 在我們的目標中，藉由將 cortana 內嵌到 Microsoft 365 來轉換新式產能體驗的一部分，我們即將淘汰適用于消費者的 cortana 技能套件 (開發人員平臺) 以及所有在此平臺上建立的技能。]

## <a name="related-articles"></a>相關文章

- [VCD 元素和屬性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 設計指導方針](cortana-design-guidelines.md)
- [Cortana 語音命令範例](https://go.microsoft.com/fwlink/p/?LinkID=619899)
