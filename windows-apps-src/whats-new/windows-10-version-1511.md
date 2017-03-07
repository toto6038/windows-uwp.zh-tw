---
author: QuinnRadich
title: "Windows 10 (版本 1511) 中的新功能"
description: "Windows 10 版本 1511 與新的開發人員工具提供由新的通用 Windows 平台所提供的工具、功能及體驗。"
keywords: "新功能, 新功能, 更新, 多項更新, 功能, 新, Windows 10, 1511, 11 月"
ms.author: quradic
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.assetid: 0d6c65c5-2ad5-46c7-964e-a3a9833c94ce
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: d1acbde95d3d36cade7242a9f89a2bc01aaf8914
ms.lasthandoff: 02/08/2017

---

# <a name="whats-new-in-windows-version-1511"></a>Windows 版本 1511 中的新功能

Windows 10 版本 1511 與針對 Windows 開發人員工具的更新持續提供由通用 Windows 平台所提供的工具、功能及體驗。 在 Windows 10 上[安裝工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows app](https://msdn.microsoft.com/library/windows/apps/bg124288)，或是探索[如何在 Windows 上使用現有的 App 程式碼](https://msdn.microsoft.com/library/windows/apps/mt238321)。

## <a name="windows-10-version-1511---november-2015"></a>Windows 10 version 1511 - November 2015

功能 | 說明
 :---- | :----
 使用者體驗 | 新的 [Windows.UI.StartScreen.JumpList](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.aspx) 和 [Windows.UI.StartScreen.JumpListItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.aspx) 類別為 App 提供以程式設計方式選取它們欲使用之受系統管理的捷徑清單、新增指向其捷徑清單的自訂工作項目，以及新增自訂群組至其捷徑清單的功能。
 輸入 | [鍵盤傳遞攔截器](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.keyboarddeliveryinterceptor.aspx)。 讓 app 能夠覆寫由原始鍵盤輸入的系統處理，包括快速鍵、便捷鍵 (或熱鍵)、加速鍵、及應用程式鍵，但不包含 Secure Attention Sequence (SAS) 按鍵組合。 Secure Attention Sequence (SAS) 按鍵組合 (包括 Ctrl-Alt-Del 和 Windows-L) 將繼續由系統處理。 <br /><br />[UWP app](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.corewindow.aspx) 和[傳統型 Windows 應用程式](https://msdn.microsoft.com/library/windows/desktop/hh454903(v=vs.85).aspx)之指標輸入的交叉處理鏈結。 支援輸入之交叉處理鏈結的新指標事件。 <br /><br />[適用於傳統型應用程式的筆跡呈現器](https://msdn.microsoft.com/library/windows/desktop/mt622165(v=vs.85).aspx)。 筆跡呈現器 API 可以讓 Microsoft Win32 應用程式透過插入 App 之 [DirectComposition](https://msdn.microsoft.com/library/windows/desktop/hh437371(v=vs.85).aspx) 視覺化樹狀結構的 [InkPresenter](https://msdn.microsoft.com/library/windows/desktop/windows.ui.input.inking.inkpresenter.aspx) 物件，來管理筆跡輸入 (標準和已修改) 的輸入、處理及轉換。
網路功能 | Websocket 使用者︰[MessageWebSocket.OutputStream.FlushAsync](https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx) 和 [StreamWebSocket.OutputStream.FlushAsync](https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx) 已完整實作，並等候先前發出 WriteAsync 呼叫完成。 請注意，當您呼叫 [FlushAsync](https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx) 時，如果 WebSocket 處於無效的狀態，這可能會導致現有程式碼擲回例外狀況。 <br /><br />已新增新的特性 [CookieUsageBehavior](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx) 至現有的 [Windows.Web.Http.Filters.HttpBaseProtocolFilter class](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx) 類別。 這可讓開發人員控制系統處理 Cookie 的方式。
ORTC | Microsoft Edge 現在實作了 [ORTC (物件即時通訊)](https://msdn.microsoft.com/library/mt433097(v=vs.85).aspx)，可以直接在網頁瀏覽器、行動裝置和伺服器之間，透過原生 Javascript API 即時處理音訊和/或視訊通話。 透過對群組視訊通話、同時聯播、可調式視訊編碼 (SVC) 等功能的支援，開發人員現在可以使用 ORTC API，在 Microsoft Edge 瀏覽器之上建置進階的即時音訊/視訊通訊應用程式。 如需在 Microsoft Edge 瀏覽器之間透過 ORTC API 進行 1:1 音訊/視訊通話的示範，請造訪[適用產品網站和示範](https://developer.microsoft.com/microsoft-edge/testdrive/demos/ortcdemo/)。
Microsoft Edge F12 開發人員工具 | Microsoft Edge 在 F12 開發人員工具中引入強大的新改良功能，包括一些 [UserVoice](https://wpdev.uservoice.com/forums/257854-microsoft-edge-developer) 中最多人要求的功能。 探索 DOM 總管、主控台、偵錯工具、網路、效能、記憶體、模擬中的新功能，以及新的實驗工具，您可以在這些強大的新功能完成之前加以試用。 新工具內建於 TypeScript 中，並且會持續執行，因此不需要重新載入。 此外，F12 開發人員工具文件現已為 [Microsoft Edge 開發人員網站](https://developer.microsoft.com/microsoft-edge/)的一部分，並在 [GitHub](https://github.com/MicrosoftEdge/MicrosoftEdge-Documentation) 上完整提供。 從現在起，這些文件不僅需要您的寶貴意見，我們也邀請您對文件的修訂提供貢獻與協助。 如需 F12 開發人員工具的簡短影片介紹，請造訪 [Channel9 的開發人員短片](https://channel9.msdn.com/Blogs/One-Dev-Minute/Microsoft-Edge-F12-tools)。
Windows Hello | Windows Hello 可讓您的應用程式啟用臉部或指紋辨識登入 Windows 系統或裝置的功能。 提供者 API 可讓 IHV 和 OEM 對 UWP 公開電腦視覺的深度、紅外線和色彩相機 (和相關的中繼資料)，並將相機指定為參與 Windows Hello 臉部驗證的相機。 [Windows.Devices.Perception](https://msdn.microsoft.com/library/windows/apps/windows.devices.perception.aspx) 命名空間包含用戶端 API，可允許 UWP 應用程式存取電腦視覺攝影機的色彩、深度或紅外線資料。
新的遊戲 API | 使用新的 Windows.Gaming.UI.GameBar 類別，可在顯示或關閉遊戲列時收到通知。
藍牙 API | 現已新增數個 API 並進行更新，以擴充對藍牙 LE、裝置列舉及藍牙中其他功能的支援。 請參閱 [Windows.Devices.Bluetooth](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.aspx) 命名空間。
智慧卡 API | 已新增幾個 SmartCardCryptogram API 至 [Windows.Devices.SmartCards](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.aspx) 命名空間，以支援保護密碼付款通訊協定。 使用主機卡模擬支援輕觸付款的付款應用程式，可以利用這些 API 獲得更高的安全性和效能。 應用程式可以使用 TPM 建立金鑰，並保護使用受限的交易金鑰。 應用程式也可以利用 NGC (下一代認證) 架構，透過使用者的 PIN 來保護金鑰。 這些 API 會將產生密碼的工作委派給系統，以提高效能。 這也可以避免其他 app 存取金鑰與密碼。
已更新的儲存 API | 在 [Windows.Storage.DownloadsFolder](https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.aspx) 類別中，您的 App 現在可以為特定[使用者](https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx)在 \[Downloads\] 資料夾內部[建立檔案](https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.createfileforuserasync.aspx)或[建立資料夾](https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.createfolderforuserasync.aspx)。 在 [Windows.Storage.StorageLibrary](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagelibrary.aspx) 類別中，您的 App 現在可以為特定[使用者](https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx)[取得指定的程式庫](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagelibrary.getlibraryforuserasync.aspx)。
Windows 應用程式認證套件 | Windows 應用程式認證套件已經由更嚴格的測試進行更新。 如需更新項目的完整清單，請造訪 [Windows 應用程式認證套件](https://developer.microsoft.com/windows/develop/app-certification-kit)頁面。
設計下載 | 看看我們針對 Adobe Photoshop 新推出的 UWP 應用程式設計範本。 我們也會更新我們的 Microsoft PowerPoint 和 Adobe Illustrator 範本，並提供我們 PDF 版的指導方針。 [造訪設計下載項目頁面](https://developer.microsoft.com/windows/design/assets)。

