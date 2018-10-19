---
author: PatrickFarley
title: 開發教育用 App。
description: 本節說明可供您撰寫 Windows10 平台教育用 App 的通用 Windows App 資源。
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，教育版
ms.assetid: 2431f253-efe3-4895-b131-34653b61f13c
ms.localizationpriority: medium
ms.openlocfilehash: da03a3c478ca45cc2d2b518988738e510a6c5ea9
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/19/2018
ms.locfileid: "4961401"
---
# <a name="develop-universal-windows-apps-for-education"></a>開發教育用通用 Windows App
![「 進行測驗應用程式螢幕擷取畫面](images/take-a-test-screen-small.png)

下列資源會協助您撰寫教育用通用 Windows App。

### <a name="accessibility"></a>協助工具
教育用 App 必須為無障礙 App。 請參閱[開發無障礙 App](https://developer.microsoft.com/windows/accessible-apps) 瞭解詳細資訊。


### <a name="secure-assessments"></a>安全的評量
評量/測驗 App 通常需要產生*鎖定*環境，以避免學生在測驗期間使用其他電腦或網際網路資源。 這項功能僅能透過[ API](take-a-test-api.md) 使用。 請參閱 Windows IT 中心中的[進行測驗](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10) Web App，來取得針對高度利害攸關測驗鎖定線上存取的測驗環境範例。

### <a name="user-input"></a>使用者輸入
使用者輸入是教育用 App 的重要部分；UI 控制項必須具備回應性且直覺化，才不會干擾使用者的注意力。 如需通用 Windows App 中可用輸入選項的一般概觀，請參閱＜設計與 UI＞一節中 [輸入基本資訊](https://docs.microsoft.com/windows/uwp/design/input/input-primer) 與其底下的主題。 此外，以下範例 App 也示範了通用 Windows 平台中基本 UI 的處理方式。
- [基本輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput) (英文) 示範如何在通用 Windows App 中處理輸入。
- [使用者互動模式範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode) (英文) 示範如何偵測及回應使用者互動模式。
- [焦點視覺效果範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) (英文) 示範如何利用新的系統繪製焦點視覺效果，若系統繪製不符合您的需求，也可以建立您自己自訂的焦點視覺效果。

Windows Ink 平台可以融入學生熟悉的輸入模式來讓教育用 App 更出色。 請參閱[手寫筆互動與 Windows Ink](https://docs.microsoft.com/windows/uwp/design/input/pen-and-stylus-interactions) 和其底下的主題，以瞭解在您的 App 中實作 Windows Ink 的完整指南。 以下範例 App 提供此 API 的使用範例。
- [手寫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Ink) (英文) 示範如何使用 JavaScript 在通用 Windows App 中使用手寫功能 (例如，擷取、操縱和解譯筆墨筆劃)。
- [簡易手寫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk) (英文) 示範如何使用 C# 在通用 Windows App 中使用手寫功能 (例如，從使用者輸入擷取筆跡，以及對筆墨筆劃執行手寫辨識)。
- [複雜手寫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk) (英文) 示範如何使用進階的 InkPresenter 功能，讓筆跡與其他物件交錯、選取筆跡、複製/貼上，及處理事件。 範例是在通用 Windows 平台上以 C++ 建立，並且可在電腦版和行動裝置版 Windows10 SKU 上執行。


### <a name="microsoft-store"></a>Microsoft Store
教育用 App 通常是在特殊情況下向特定組織發行。 請參閱[向企業散發企業營運應用程式](https://msdn.microsoft.com/windows/uwp/publish/distribute-lob-apps-to-enterprises)以取得相關詳細資訊。

## <a name="related-topics"></a>相關主題
- Windows IT 中心中的[教育用 Windows10](https://technet.microsoft.com/edu/windows/index)
