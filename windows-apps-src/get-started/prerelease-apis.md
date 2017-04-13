---
author: drewbat
ms.assetid: 
title: "使用發行前版本 API 開發 UWP app"
description: "了解使用 UWP SDK 預覽版中所包含發行前版本 API 的優點和注意事項。"
ms.openlocfilehash: ede7e8d5e9cce39850edbdb70a715d76c78f0c01
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="developing-uwp-apps-with-pre-release-apis"></a>使用發行前版本 API 開發 UWP app

Microsoft 發行通用 Windows 平台 (UWP) SDK 預覽版本，讓開發人員可以在這些版本完成之前銜接新的平台功能。 這樣就可以搶先將功能納入您的應用程式，協助您在 SDK 官方 RTM 版本推出之後盡快發行 App。 使用發行前版本 API 還能讓您提供意見反應給 Microsoft，協助決定日後發行平台的方向。

## <a name="important-limitations-on-the-use-of-pre-release-apis"></a>發行前版本 API 在使用上的重要限制
在您的 App 中使用發行前版本 API 之前，請注意這樣做有下列重要含意︰ 
* 在以 RTM 版本正式發行 API 之前，您無法將使用發行前版本 API 的 App 提交至 Windows 市集。 強烈建議您在目前已發行 App 的程式碼之外，另行保留發行前版本的開發程式碼。 
* 您無法將使用預覽版 SDK 所開發的 App 提交到市集，即使沒有使用任何發行前版本 API 也是如此。 您應該在電腦或虛擬機器 (不同於您用來進行主要開發的生產環境電腦) 上安裝預覽工具。 
* 發行前版本 API 可能會在 RTM 之前變更。 當 API 包含在預覽版 SDK 時，這些 API 支援的功能或案例可能也會包含在最終的 SDK。 但是特定 API 的名稱、簽章及行為可能會在最終發行前變更，而且 API 還可能整個遭到移除。. 

## <a name="how-to-identify-a-prerelease-api"></a>如何識別發行前版本 API 
在 UWP 的 API 參考文件中，做為發行前版本的 API 是以下列文字來標示︰ 

本文討論可能在正式發行之前大幅修改的發行前版本 API 或功能。 現在就可以使用這項功能進行開發和測試，但除非是在正式發行功能之後，否則您無法將使用該功能的 App 提交到 Windows 市集。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。 如需有關使用發行前版本 API 進行開發的詳細資訊，請參閱「使用發行前版本 API 開發 UWP app」。 

## <a name="get-the-latest-sdk-insider-preview"></a>取得最新 SDK Insider Preview 
1. [註冊 Windows 測試人員計畫](https://insider.windows.com/)以存取 SDK 預覽版。 
3. 安裝發人員預覽工具前，請先檢閱[目前的 SDK 與行動裝置版模擬器版本資訊](http://go.microsoft.com/fwlink/?LinkId=829180)。
4. 安裝 [SDK Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK)。
5. 查看 [Windows Insider Preview 社群論壇。](http://go.microsoft.com/fwlink/p/?LinkId=507620)。
