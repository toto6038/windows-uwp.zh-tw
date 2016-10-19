---
author: seksenov
title: "託管的 Web 應用程式 - 使用 Mac 將您的 Web 應用程式轉換為 Windows 應用程式"
description: "使用 Mac 將您的網站搖身一變，成為適用於 Windows 10 的通用 Windows 平台 (UWP) App。"
kw: Hosted Web Apps with a Mac, Porting to Windows 10 with a Mac, Convert website to Windows with Mac, Packaging web application with ManfoldJS for Windows Store, Add website to Windows Store with App Studio
translationtype: Human Translation
ms.sourcegitcommit: 0458dcd2aab862ccdecf1ebbc51e883405a929a6
ms.openlocfilehash: 3ba820e2ec8a3556874c0c7c7e328831bab783ca

---

# 使用 Mac 建立託管的 Web 應用程式

只要透過網站 URL，即可快速建立適用於 Windows 10 的通用 Windows 平台 App。 

> [!NOTE]
> 下列指示適用於搭配 Mac 開發平台使用。 Windows 使用者請參閱[關於使用 Windows 開發平台的指示](/hwa-create-windows.md)。

## 在 Mac 上開發所需的項目

- 網頁瀏覽器。
- 命令提示字元。

## 選項 1：ManifoldJS

[ManifoldJS](http://manifoldjs.com/) 是一種可自 NPM 輕鬆安裝的 Node.js 應用程式。 其會取得關於您網站的中繼資料，並產生跨 Android、iOS 和 Windows 平台的原生託管應用程式。 若您的網站沒有 [Web 應用程式資訊清單](https://www.w3.org/TR/appmanifest/)，則系統會自動為您產生該清單。

1. 安裝 [NodeJS](https://nodejs.org/) 其中包含 NPM (Node Package Manager)。 <br>

2. 開啟命令提示字元並以 NPM 安裝 ManifoldJS:
```
npm install -g manifoldjs
```

3. 在您的網站 URL 上執行 `manifoldjs` 命令：
```
manifoldjs http://codepen.io/seksenov/pen/wBbVyb/?editors=101
```

4. 遵循以下影片中的步驟，完成託管 Web 應用程式的封裝並發佈至 Windows 市集。

[![使用 ManifoldJS 在 Mac 上發佈 UWP Web App](images/hwa-to-uwp/mac_manifoldjs_video.png)](https://sec.ch9.ms/ch9/0a67/9b06e5c7-d7aa-478d-b30d-f99e145a0a67/ManifoldJS_high.mp4 "使用 ManifoldJS 在 Mac 上發佈 UWP Web App")

## 選項 2︰App Studio

[App Studio](http://appstudio.windows.com/) 是可讓您快速建置 Windows 10 App 的免費線上 App 建立工具。

1. 在您的網頁瀏覽器中，開啟 [App Studio](http://appstudio.windows.com/)。

2. 按一下 \[立即開始!\]****。

3. 在 \[Web app templates\] \(Web App 範本\)**** 下方，按一下 \[Hosted Web App\] \(託管的 Web App\)****。

4. 依照畫面上的指示產生隨時可發佈至 Windows 市集的套件。

## 相關主題

- [存取通用 Windows 平台 (UWP) 功能以增強您的 Web 應用程式](/hwa-access-features.md)
- [通用 Windows 平台 (UWP) App 指南](http://go.microsoft.com/fwlink/p/?LinkID=397871)
- [下載 Windows 市集應用程式的設計資產](https://msdn.microsoft.com/library/windows/apps/xaml/bg125377.aspx)



<!--HONumber=Aug16_HO3-->


