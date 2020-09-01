---
ms.assetid: ba2ac5f5-1e0d-4f1d-a6f8-6a65b4cff501
description: 本節說明如何將您現有的應用程式移植到通用 Windows 平台 (UWP)，以便您在其中建立單一 Windows 10 應用程式套件，讓客戶安裝於各種不同裝置上。 您的 App 將可受益於令人興奮的新硬體、絕佳的賺錢機會、現代化 API 集、彈性 UI 控制項，以及各種輸入形式 (包括滑鼠/鍵盤、觸控及語音)。
title: 將應用程式移植到 Windows 10
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5774dd1fe37298277307fbd59b5c54f7b0481297
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162282"
---
# <a name="porting-apps-to-windows10"></a>將應用程式移植到 Windows 10


本節說明如何將您現有的應用程式移植到通用 Windows 平台 (UWP)，以便您在其中建立單一 Windows 10 應用程式套件，讓客戶安裝於各種不同裝置上。 您的 App 將可受益於令人興奮的新硬體、絕佳的賺錢機會、現代化 API 集、彈性 UI 控制項，以及各種輸入形式 (包括滑鼠/鍵盤、觸控及語音)。

Windows 執行階段 (WinRT) 是可讓您建置通用 Windows 平台 (UWP) app 的一種技術。 您可以參考[何謂通用 Windows 平台 (UWP) app？](../get-started/universal-application-platform-guide.md)，了解 WinRT 和 UWP app 的詳細背景。

本移植指南說明您目前 app 的技術與通用 Windows 平台 (UWP) 之間的差異。 一旦了解技術之間的途徑之後，您將能夠鑽研「開發人員中心」的其餘部分，這是一套適用於開發 UWP app 的完整資源。 當您準備好時，從[如何開發Microsoft Store app](/previous-versions/windows/apps/dn726537(v=win.10)) 開始會是個不錯的方式。

| 主題 | 描述 |
|-------|-------------|
| [從傳統型移動到 UWP](desktop-to-uwp-migrate.md) | 選擇數個選項中的其中一個，將 UWP 體驗帶入您的 Win32 和 .NET 傳統型應用程式。 |
| [從 Windows Runtime 8.x 移至 UWP](w8x-to-uwp-root.md) | 如果您有通用 8.1 app (無論它是針對 Windows 8.1、Windows Phone 8.1 或這兩者設計)，則會發現您的原始程式碼和技能將可順暢地移植到 Windows 10。 您可以使用 Windows 10 來建立 UWP app，這是可供客戶安裝至各種類型裝置的單一 app 套件。 |
| [適用於 Android 與 iOS 開發人員的 Windows 應用程式概念對應](android-ios-uwp-map.md) | 如果您是具備 Android 或 iOS 技巧或程式碼的開發人員，而且您想要移到 Windows 10 和通用 Windows 平台，則此資源擁有您在三個平台之間對應平台功能 (和您的知識) 所需的資訊。 |
| [從 iOS 移到 UWP](ios-to-uwp-root.md) | 您是 iOS 開發人員，想知道如何移至 Windows 10 與 UWP？ 過程並不如您所想的那麼恐怖。 我們已經有您需要的工具、技術以及資訊，可以讓 App 在 Windows 上運作得和在 iOS 裝置上一樣出色，或甚至更好！ |
| [從 Windows Phone Silverlight 移到 UWP](wpsl-to-uwp-root.md) | 如果您是 Windows Phone Silverlight app 的開發人員，便可以在移到 Windows 10 時，充分利用您的技能組合與原始程式碼。 您可以使用 Windows 10 來建立 UWP app，這是可供客戶安裝至各種類型裝置的單一 app 套件。 |
| [將您的 Web 應用程式轉換成 PWA](/microsoft-edge/progressive-web-apps) | 您現在可以將 Web 應用程式轉換成漸進式 Web 應用程式 (PWA) 並在任何平台上執行，包括 UWP！ [PWA Builder 工具](https://www.pwabuilder.com)會為您產生所需的資訊清單。 這會取代託管的 Web 應用程式 (HWA) 橋接器。 |

## <a name="related-topics"></a>相關主題

* [從 WPF 和 Silverlight 移至 WinRT](/previous-versions/windows/apps/dn263237(v=win.10))
* [從 Android 移至 WinRT](/previous-versions/windows/apps/jj945421(v=win.10))
* [從 Web 移至 WinRT](/previous-versions/windows/apps/hh465151(v=win.10))