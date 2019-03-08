---
title: 疑難排解在 Windows 電腦上的 Xbox Live 設定
description: 了解如何疑難排解您的 Xbox Live 的開發環境的 Windows 電腦上。
ms.assetid: 9cfebdcd-0c1c-4fc2-9457-e7e434b6374c
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 一個 xbox 疑難排解
ms.localizationpriority: medium
ms.openlocfilehash: c1f055a49fe34be35335e50dc8b1efbfb7b9b922
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57647733"
---
# <a name="troubleshooting-xbox-live-setup-on-windows-pc"></a>疑難排解在 Windows 電腦上的 Xbox Live 設定

在 Windows 10 電腦，您可以確保您的電腦已正確地進行這些步驟的設定：

1. 變更您的電腦，以指向 XDKS.1 沙箱範例設計為執行。  執行此指令碼來執行這項操作：

        {*SDK source root*}\Tools\SwitchSandbox.cmd XDKS.1

1. 「 SourcesAndSamples.zip"找到在 SDK 內之 zip 檔案的內容解壓縮。
1. 開啟範例方案：
    1. 適用於 c + + API: {*SDK 來源根目錄*} \Samples\Social\UWP\Cpp\Social.Cpp.140.sln
    1. WinRT api，使用C#: {*SDK 來源根目錄*} \Samples\Social\UWP\CSharp\Social.CSharp.140.sln
    1. WinRT api，使用 C + + /CX: {*SDK 來源根目錄*} \Samples\TitleStorage\UWP\CppCx\TitleStorageUniversal.sln
1. 將建置的目標平台變更為"Win32"或"x64"。
1. 以滑鼠右鍵按一下方案，並重新建置所有項目。
1. 啟動偵錯工具中的應用程式。
1. 使用您在建立開發帳戶登入[Xbox 開發人員入口網站](https://xdp.xboxlive.com)，或使用授權在零售開發人員帳戶[合作夥伴中心](https://partner.microsoft.com/dashboard)。
1. 授與存取您的 Xbox Live 資訊的應用程式權限。
1. 請確認應用程式可以擷取您的資訊，而且您可以看到您的玩家代號。