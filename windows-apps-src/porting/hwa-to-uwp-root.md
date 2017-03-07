---
author: seksenov
title: "託管 Web 應用程式 - 將 Web 應用程式轉換為通用 Windows 平台 (UWP) 應用程式，及存取原生 Windows 10 功能"
description: "尋找資源，將您的 Web 應用程式轉換成適用 Windows 市集的通用 Windows 平台 (UWP) 應用程式。"
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "託管的 Web 應用程式, 將網站轉換成 Windows 應用程式, Windows 市集中的 Web 應用程式, 適用於 Windows 的 Chrome 應用程式"
translationtype: Human Translation
ms.sourcegitcommit: 2e230e95be01f0b14fa6346be9fa836c66a812cf
ms.openlocfilehash: c9239f3a3c14bf9da99e60cfef8154eefb4305b4
ms.lasthandoff: 01/20/2017

---

# <a name="hosted-web-apps---access-windows-10-features-from-your-web-app"></a>託管的 Web 應用程式 - 從您的 Web 應用程式存取 Windows 10 功能

您的 Web 應用程式可具備對通用 Windows 平台 (UWP) 的完整存取權，包括直接從伺服器上託管的指令碼呼叫 Windows 執行階段 API、利用 Cortana 整合，以及使用線上驗證提供者。 此外亦支援混合式應用程式，因為您可以將要從託管之指令碼呼叫的本機程式碼包含在內，以及管理遠端和本機頁面之間的應用程式導覽。

## <a name="get-started"></a>入門

無論您是使用 Mac 或 PC，都可以在數分鐘內建立託管的 Web 應用程式。 最佳的入門使用方式，即是使用免費全功能的 [Visual Studio Community 2015](https://www.visualstudio.com/vs/community/)，尤其適合使用 Windows 裝置的情況。 若您無法使用 Visual Studio，則可從中選擇幾個選項。 若您已熟悉命令列介面 (CLI) 公用程式，請試試看 [ManifoldJS](http://manifoldjs.com/)。 您亦可使用免費、無須編碼的 [App Studio](http://appstudio.windows.com/) 線上建立工具，快速建置 Windows 10 應用程式。

- [關於使用 Windows 將您 Web 應用程式轉換為通用 Windows 平台 (UWP) 應用程式的逐步指示](hwa-create-windows.md)

- [關於使用 Mac 將您 Web 應用程式轉換為通用 Windows 平台 (UWP) 應用程式的逐步指示](hwa-create-mac.md)

## <a name="enhance-your-app"></a>增強您的應用程式

- 透過[原生 Windows 執行階段功能](hwa-access-features.md)讓您的應用程式更顯眼，可以直接從 JavaScript 中呼叫。

- 透過內容安全性原則 (CSP) 模型設定應用程式內容 URI 規則 (ACUR)，以維護應用程式安全性。

- 整合 Cortana 命令，運用強大的語音功能執行應用程式。

- 藉由宣告應用程式功能，針對使用者資源與裝置功能授與程式設計方式存取權。

- 使用 OpenID 與 OAuth 進行身分識別驗證，以針對使用者建立簡易順暢的登入流程。

- 無須判斷使用封裝或託管的 Web 應用程式。 只要建立混合式應用程式即可兩者兼得。

## <a name="convert-your-existing-chrome-app"></a>轉換您現有的 Chrome 應用程式

您可輕易地[將現有的 Chrome 託管應用程式轉換為](hwa-chrome-conversion.md) Windows 託管 Web 應用程式。 [ManifoldJS](http://manifoldjs.com/) 現已支援使用 Chrome 資訊清單做為輸入形式。 此外亦可運用我們開發的 [CLI 工具](https://github.com/MicrosoftEdge/hwa-cli)，自您的現有 `.zip` 或 `.crx` 檔案產生 `.appx` 套件。

## <a name="demos"></a>示範

- [Contoso Travel App](http://contosotravel.azurewebsites.net/)


