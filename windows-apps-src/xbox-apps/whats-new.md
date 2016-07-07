---
author: v-angraf
title: "Xbox One 上的 UWP 新功能"
description: "Xbox One app 上的 UWP 重點新功能。"
area: Xbox
ms.sourcegitcommit: 59019f209729b56e02ebdbdfd53a8fbf835c69f7
ms.openlocfilehash: dfa94ad42a79d0f6b3f72fbf2efe9ce043532c56

---

# Xbox One 上的 UWP 2016 年 6 月開發人員預覽的新功能

Xbox One 上的通用 Windows 平台 (UWP) 2016 年 6 月開發人員預覽版本包含以下新功能、現有功能的更新與錯誤修正。

## 滑鼠模式現已預設為啟用
針對 XAML 和託管的 Web 應用程式，滑鼠模式現已預設為啟用。
強烈建議您關閉此功能，並針對方向控制器瀏覽最佳化。
若要了解如何關閉滑鼠模式，請參閱[如何停用滑鼠模式](how-to-disable-mouse-mode.md)。
如需如何建立適用於 Xbox 的絕佳 App 詳細資訊，請參閱[針對 Xbox 與電視進行設計](https://msdn.microsoft.com/en-us/windows/uwp/input-and-devices/designing-for-tv?f=255&MSPPError=-2147217396#mouse-mode)。

## 延伸的 UWP API 介面區域現已可在主機上正常運作
其他的 UWP API 現在可在 Xbox 主機上正常運作。 如需 UWP API 支援的詳細資訊，請參閱 [Xbox 上尚未支援的 UWP 功能](http://go.microsoft.com/fwlink/?LinkID=760755)。 

## 背景音樂和音訊功能
您現在可以從在背景執行的 App 播放音樂與音訊。

## XAML 改進功能
XAML 平台已經做了下列改良：
-   焦點矩形現在是針對電視 10 呎體驗設定樣式。
-   Xbox 音效現在內嵌於 XAML 平台中。
-   已改進 UI 元素間的 XY 焦點瀏覽。 

## 您現在可以變更主機上已配置開發人員存放區的大小
「開發人員首頁」App 中的新設定可讓您增加或減少主機上已配置開發人員存放區的大小。 如需變更已配置開發人員存放區大小的詳細資訊，請參閱 [Xbox One 工具簡介](introduction-to-xbox-tools.md)。

## WDP 工具增強功能
已針對適用於 Xbox 的 Windows Device Portal (WDP) 工具進行下列改善：
 - 工具包含額外的主機設定。 如需主機設定的詳細資訊，請參閱 [/ext/settings](wdp-xboxsettings-api.md) 參考主題。 
 - 使用者可以在主機上登入或登出。 如需使用者的詳細資訊，請參閱 [/ext/user](wdp-user-management.md) 參考主題。
 - 您現在可以擷取主機的螢幕擷取畫面。 如需製作螢幕擷取畫面的詳細資訊，請參閱 [/ext/screenshot](wdp-media-capture-api.md) 參考主題。
 - 此工具可以部署 App 的鬆散檔案組建。 如需鬆散檔案組建的詳細資訊，請參閱 [/api/app/packagemanager/register](wdp-loose-folder-register-api.md) 參考主題。
 - 可以從開發電腦的 [檔案總管] 存取主機上的開發人員檔案。 如需透過 [檔案總管] 存取檔案的詳細資訊，請參閱 [/ext/smb/developerfolder](wdp-smb-api.md) 參考主題。

## 另請參閱
- [Xbox One 上的 UWP](index.md)
- [已知問題](known-issues.md)



<!--HONumber=Jun16_HO4-->


