---
title: 使用 Xbox Live Api 內建的 XDK
description: 了解如何使用內建的 Xbox Live Api Xbox Developer Kit (XDK) 專案中。
ms.assetid: 539caca3-58bc-49d9-8432-ca8e57755be2
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c762dd8abbbc80948d232610e4123b6e4893936d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598403"
---
# <a name="using-xbox-live-apis-built-into-the-xdk"></a>使用 Xbox Live Api 內建的 XDK

1. 您在 Visual Studio 中的專案上按一下滑鼠右鍵，然後選擇 [參考]。
1. 選擇 [新增新參考]
1. 按一下 「 Durango。<build number>" 並在左側面板的 [擴充功能]
1. 在中間，選擇：
- 如果您想要使用 WinRT XSAPI API，請選擇 「 Xbox 服務 API 」
- 如果您想要使用 c + + XSAPI API，請選擇 「 Xbox 服務 API Cpp"
1. 按一下 [確定]

注意：如果您的建置系統不支援 props 檔案，您必須手動新增程式庫中所示的前置處理器定義 `%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox.Services.API.Cpp\8.0\DesignTime\CommonConfiguration\Neutral\Xbox.Services.API.Cpp.props`
