---
author: seksenov
title: "託管的 Web 應用程式 - 使用 Visual Studio 將您的 Web 應用程式轉換為 Windows 應用程式"
description: "使用 Visual Studio 將您的網站搖身一變，成為適用於 Windows 10 的通用 Windows 平台 (UWP) 應用程式。"
kw: Hosted Web Apps tutorial, Porting to Windows 10 with Visual Studio, How to convert website to Windows, How to add website to Windows Store, Packaging web application for Microsoft Store, Test Windows 10 native features and runtime APIs with CodePen, How to use Windows Cortana Live Tiles Built-in Camera on my Website with remote JavaScript
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "託管的 Web 應用程式, 將 Web 應用程式移植到 Windows 10, 將網站轉換成 Windows, 封裝適用 Windows 市集的 Web 應用程式"
ms.assetid: a58d2c67-77f8-4d01-bea3-a6ebce2d73b9
ms.openlocfilehash: e135b63e2ba5f455dbbf6524e5519a33acd822b0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="convert-your-web-application-to-a-universal-windows-platform-uwp-app"></a>將您的 Web 應用程式轉換為通用 Windows 平台 (UWP) 應用程式

了解如何只要透過網站 URL，即可快速建立適用於 Windows 10 的通用 Windows 平台 App。 

> [!NOTE]
> 下列指示適用於搭配使用 Windows 開發平台。 Mac 使用者請參閱[關於使用 Mac 開發平台的指示](./hwa-create-mac.md)。

## <a name="what-you-need-to-develop-on-windows"></a>在 Windows 上開發所需的項目

- [Visual Studio 2015。](https://www.visualstudio.com/) 免費且功能完整的 Visual Studio Community 2015 包含 Windows 10 開發人員工具、通用應用程式範本、程式碼編輯器、強大的偵錯工具、Windows Mobile 模擬器以及豐富的語言支援等等，所有項目皆可立即使用於生產環境中。
- (選擇性) [適用於 Windows 10 的 Windows 獨立 SDK](https://dev.windows.com/downloads/windows-10-sdk)。 若您使用非 Visual Studio 2015 的其他開發環境，則可下載適用於 Windows 10 安裝程式的獨立 Windows SDK。 請注意，若您使用的是 Visual Studio 2015 則無須安裝此 SDK – 其已內含。

## <a name="step-1-pick-a-website-url"></a>步驟 1︰挑選網站 URL
選擇可順暢運作為單一頁面應用程式的現有網站。 我們強烈建議您必須為網站的擁有者或開發人員，方可執行所有的必要變更。 若您沒有目標 URL，請嘗試使用此 [Codepen 範例](http://codepen.io/seksenov/pen/wBbVyb/?editors=101)做為網站。 複製您的 URL 或 Codepen URL，以供整個教學課程使用。 

![步驟 #1：挑選網站 URL](images/hwa-to-uwp/windows_step1.png)

## <a name="step-2-create-a-blank-javascript-app"></a>步驟 2：建立空白的 JavaScript App

啟動 Visual Studio。
1. 按一下 **\[檔案\]**。
2. 按一下 **\[新專案\]**。
3. 在 **\[JavaScript\]**、**\[Windows 通用\]** 的下方，按一下 **\[空白應用程式 \(通用 Windows\)\]**。

![步驟 #2：建立空白的 JavaScript App](images/hwa-to-uwp/windows_step2.png)

## <a name="step-3-delete-any-packaged-code"></a>步驟 3︰刪除所有封裝程式碼

由於此為託管的 Web 應用程式，其內容是由遠端伺服器提供，因此您不需要使用 JavaScript 範本預設隨附的大部分本機應用程式檔案。 刪除所有本機 HTML、JavaScript 或 CSS 資源。 您僅應保留 `package.appxmanifest` 檔案，以用來設定 App 和影像資源。

![步驟 #3︰刪除所有封裝程式碼](images/hwa-to-uwp/windows_step3.png)

## <a name="step-4-set-the-start-page-url"></a>步驟 4︰設定開始頁面 URL

1. 開啟 `package.appxmanifest` 檔案。
2. 在 **\[應用程式\]** 索引標籤下方，尋找 **\[開始頁面\]** 文字欄位。
3. 將 `default.html` 取代為您的網站 URL。

![步驟 #4︰設定開始頁面 URL](images/hwa-to-uwp/windows_step4.png)

## <a name="step-5-define-the-boundaries-of-your-web-app"></a>步驟 5︰定義您的 Web App 界限

應用程式內容 URI 規則 (ACUR) 會指定允許存取您 App 與通用 Windows API 的遠端 URL。 您至少必須新增適用於開始頁面的 ACUR，以及該頁面使用的所有網路資源。 如需關於 ACUR 的詳細資訊，請[按一下這裡](./hwa-access-features.md)。
1. 開啟 `package.appxmanifest` 檔案。
2. 按一下 **\[內容 URI\]** 索引標籤。
3. 針對開始頁面新增所有必要的 URI。

例如：
```
1. http://codepen.io/seksenov/pen/wBbVyb/?editors=101
2. http://*.codepen.io/
```
4. 針對每個新增的 URI，將 **\[WinRT 存取\]** 設為 **\[全部\]**。

![步驟 #5︰定義您的 Web 應用程式界限](images/hwa-to-uwp/windows_step5.png)

## <a name="step-6-run-your-app"></a>步驟 6：執行 App

現在您已擁有可存取通用 Windows API 的全功能 Windows 10 App！

若您是依照我們的 Codepen 範例操作，請按一下 **\[快顯通知\]** 按鈕以從託管指令碼呼叫 Windows API。

![步驟 #6：執行 App](images/hwa-to-uwp/windows_step6.png)

## <a name="bonus-add-camera-capture"></a>獎勵︰新增相機擷取

複製並貼上下方的 JavaScript 程式碼，以啟用相機擷取。 若您是依自有網站操作，請建立按鈕叫用 `cameraCapture()` 方法。 若您是依照我們的 Codepen 範例操作，則在 HTML 中已顯示按鈕。 按一下按鈕並拍照。

```JavaScript
function cameraCapture() {
  if(typeof Windows != 'undefined') {
   var captureUI = new Windows.Media.Capture.CameraCaptureUI();
   //Set the format of the picture that's going to be captured (.png, .jpg, ...)
   captureUI.photoSettings.format = Windows.Media.Capture.CameraCaptureUIPhotoFormat.png;
   //Pop up the camera UI to take a picture
   captureUI.captureFileAsync(Windows.Media.Capture.CameraCaptureUIMode.photo).then(function (capturedItem) {
      // Do something with the picture
   });
  }
}
```

## <a name="related-topics"></a>相關主題

- [存取通用 Windows 平台 (UWP) 功能以增強您的 Web 應用程式](hwa-access-features.md)
- [通用 Windows 平台 (UWP) App 指南](http://go.microsoft.com/fwlink/p/?LinkID=397871)
- [下載 Windows 市集應用程式的設計資產](https://msdn.microsoft.com/library/windows/apps/xaml/bg125377.aspx)
