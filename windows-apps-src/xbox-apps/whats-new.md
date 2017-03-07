---
author: v-angraf
title: "Xbox One 上的 UWP 新功能"
description: "針對 Xbox One 上的 UWP App 新功能進行重點摘要。"
ms.author: v-angraf
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 96f9c9ef355382c72423187a7f81635571762071
ms.lasthandoff: 02/08/2017

---

# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Xbox One 上的 UWP 最新更新中適用於開發人員的新功能

Xbox One 上的通用 Windows 平台 (UWP) 2016 年 7 月版本包含以下新功能、現有功能的更新，以及錯誤修正。

## <a name="networking-using-tcpudp-sockets-is-now-available"></a>使用 TCP/UDP 通訊端的網路功能現在已可使用  
來自使用傳統 TCP/UDP 通訊端 (WinSock、Windows.Networking.Sockets) 的主機的輸入和輸出網路存取現在已可使用。

## <a name="fiddler-support"></a>Fiddler 支援
您現在可以針對已在 Xbox One 上啟用 UWP 的主機，將 Fiddler 啟用為 Proxy。 Fiddler 可讓您記錄與調查 Xbox 服務與信賴憑證者 Web 服務的所有 HTTP/HTTPS 進出流量。 如需詳細資訊，請參閱[如何在開發 UWP 時使用 Fiddler 搭配 Xbox One](uwp-fiddler.md)。

## <a name="mouse-mode-is-now-enabled-by-default"></a>滑鼠模式現已預設為啟用
滑鼠模式現已預設針對 XAML 和託管的 Web 應用程式啟用。
我們強烈建議您關閉此功能，並針對方向控制器瀏覽進行最佳化。
若要了解如何關閉滑鼠模式，請參閱[如何停用滑鼠模式](how-to-disable-mouse-mode.md)。
如需如何建立適用於 Xbox 的絕佳 App 詳細資訊，請參閱[針對 Xbox 與電視進行設計](../input-and-devices/designing-for-tv.md#mouse-mode)。

## <a name="extended-uwp-api-surface-area-is-now-functional-on-the-console"></a>延伸的 UWP API 介面區域現已可在主機上正常運作
其他的 UWP API 現在可在 Xbox 主機上正常運作。 如需 UWP API 支援的詳細資訊，請參閱 [Xbox 上尚未支援的 UWP 功能](http://go.microsoft.com/fwlink/p/?LinkID=760755)。 

## <a name="background-music-and-audio-capabilities"></a>背景音樂和音訊功能
您現在可以從在背景執行的 App 播放音樂與音訊。

## <a name="xaml-improvements"></a>XAML 改進功能
XAML 平台已經做了下列改良：
-    焦點矩形現在是針對電視 10 呎體驗設定樣式。
-    Xbox 音效現在內嵌於 XAML 平台中。
-    已改進 UI 元素間的 XY 焦點瀏覽。 

## <a name="you-can-now-change-the-size-of-allocated-developer-storage-on-the-console"></a>您現在可以變更主機上已配置開發人員存放區的大小
「開發人員首頁」App 中的新設定可讓您增加或減少主機上已配置開發人員存放區的大小。 如需變更已配置開發人員存放區大小的詳細資訊，請參閱 [Xbox One 工具簡介](introduction-to-xbox-tools.md)。

## <a name="wdp-tool-enhancements"></a>WDP 工具增強功能
已針對適用於 Xbox 的 Windows Device Portal (WDP) 工具進行下列改善：
 - 工具包含額外的主機設定。 如需主機設定的詳細資訊，請參閱 [/ext/settings](wdp-xboxsettings-api.md) 參考主題。 
 - 使用者可以在主機上登入或登出。 如需使用者的詳細資訊，請參閱 [/ext/user](wdp-user-management.md) 參考主題。
 - 您現在可以擷取主機的螢幕擷取畫面。 如需製作螢幕擷取畫面的詳細資訊，請參閱 [/ext/screenshot](wdp-media-capture-api.md) 參考主題。
 - 此工具可以部署 App 的鬆散檔案組建。 如需鬆散檔案組建的詳細資訊，請參閱 [/api/app/packagemanager/register](wdp-loose-folder-register-api.md) 參考主題。
 - 可以從開發電腦的 [檔案總管] 存取主機上的開發人員檔案。 如需透過 [檔案總管] 存取檔案的詳細資訊，請參閱 [/ext/smb/developerfolder](wdp-smb-api.md) 參考主題。

## <a name="see-also"></a>另請參閱
- [已知問題](known-issues.md)
- [Xbox One 上的 UWP](index.md)

