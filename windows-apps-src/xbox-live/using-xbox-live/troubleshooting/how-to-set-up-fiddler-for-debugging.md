---
title: 疑難排解使用 Fiddler 的 Xbox Live
description: 了解如何使用 Fiddler 疑難排解 Xbox Live 服務呼叫。
ms.assetid: 7d76e444-027b-4659-80d5-5b2bf56d199e
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 fiddler、 服務呼叫、 進行疑難排解
ms.localizationpriority: medium
ms.openlocfilehash: 84c6717a4f9f5aff9fd3ff1f68c870fdd9174865
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606713"
---
# <a name="troubleshooting-xbox-live-using-fiddler"></a>疑難排解使用 Fiddler 的 Xbox Live

Fiddler 是 web 偵錯 proxy，它會記錄您的裝置與網際網路之間的所有 HTTP 和 HTTPS 流量。 您將使用它來記錄和檢查 Xbox Live 服務與信賴憑證者的合作對象 web 服務，以了解及偵錯 web 服務呼叫的流量。

## <a name="for-windows-uwp-pc-apps"></a>針對 Windows PC UWP 應用程式

1. 請確定目前的使用者位於電腦上的系統管理員群組
1. 下載從的 Fiddler [https://www.telerik.com/fiddler](https://www.telerik.com/fiddler)
1. 請確定您選取的 「 建置適用於.NET 4 」 的版本
1. 安裝之後，請移至 工具-> Fiddler 選項，並啟用擷取 HTTPS Connect 和解密 HTTPS 流量。  執行階段和 Xbox LIVE 服務之間的所有通訊會使用 SSL 來都加密。  未選取這個選項不會看到任何有用的資訊。  接受 Fiddler 彈出 （應該 5 個對話方塊包括 UAC） 的所有對話
1. 請移至"WinConfig 」、 「 豁免 All"，並 [儲存變更]。  否則將無法運作 Fiddler，市集應用程式。
1. 現在如果您執行您的應用程式應該會看到要求/回應之間的應用程式、 執行階段和 Xbox LIVE。

若要偵錯 UWP OS 層級呼叫通常不需要但可以是登入和遊戲中的事件，進行偵錯時很有幫助您需要設定 Fiddler 擷取 WinHTTP 流量。
這可以透過來完成：
```cpp
    netsh winhttp set proxy 127.0.0.1:8888 "<-loopback>"
```
在連接埠 8888 符合 Fiddlers 工具，您已設定的連接埠 |選項 |其預設值是 8888 連接索引標籤。
若要復原這種情況，請執行：
```cpp
    netsh winhttp reset proxy
```

## <a name="for-xbox-one-uwp-based-projects"></a>為基礎的 Xbox One UWP 專案

遵循以下的步驟 [https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/uwp-fiddler](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/uwp-fiddler)

## <a name="for-xbox-one-xdk-based-projects"></a>以 Xbox One XDK 為基礎的專案

在一般作業中，透過 Proxy 通訊的主機有通訊遭到 Proxy 修改的風險，並可能使玩家得以作弊。 因此，主控台會設計不是允許透過 proxy 進行通訊。 您必須在開發人員套件上執行一些特殊的設定步驟來讓它可以使用 Fiddler Proxy，才能搭配 Xbox One 開發人員套件使用 Fiddler。

Fiddler 是免費軟體，可以從 [Fiddler 網站](https://www.telerik.com/fiddler/)下載。

Fiddler 可能會影響主機所報告的網路狀態。 如果執行 Fiddler 的電腦停用上游連線，直到主機的驗證到期之前，主機可能無法偵測到這個中斷連線。 如果您正在使用 Fiddler，請務必中斷主機與執行 Fiddler 電腦之間的連線，而不要使用 Fiddler 模擬中斷連線。 仍更好的請使用網路負荷工具讓模擬用於測試的中斷連線。

安裝並啟用在開發電腦上的 Fiddler

請遵循下列步驟來安裝並啟用 Fiddler 來監視流量從您的開發套件。

1. 依照 [Fiddler 網站](https://www.telerik.com/fiddler/)上的指示，在您的開發電腦上安裝 Fiddler。
1. 啟動 Fiddler，並從 工具 功能表中選取 Fiddler 選項。
1. 選取 [連線] 索引標籤，並確定已核取 [允許的遠端電腦連線] 核取方塊。
1. 按一下 [確定] 接受您的設定變更。 您會看到顯示 Fiddler 必須重新啟動，變更才會生效，且您必須手動設定防火牆的對話方塊。 在此對話方塊中，按一下 [確定]，但不是會重新啟動 Fiddler 尚未。
1. 設定所需的防火牆規則，以允許遠端電腦連線。 啟動 Windows 防火牆控制台小程式。 按一下 [進階的設定]，然後輸入規則。 尋找名為"FiddlerProxy 」，此規則並捲動到右方時，驗證每個下列設定會出現該規則。

| 設定          | 偏好值                |
|------------------|--------------------------------|
| 名稱             | FiddlerProxy                   |
| 群組            | （未設定值群組） |
| 設定檔          | 全部                            |
| Enabled          | 是                            |
| 動作           | 允許                          |
| 覆寫         | 否                             |
| 程式          | fiddler.exe 路徑            |
| LocalAddress     | 任何值                            |
| RemoteAddress    | 任何值                            |
| 通訊協定         | TCP                            |
| 本機連接埠        | 任何值                            |
| 遠端連接埠       | 任何值                            |
| 允許的使用者     | 任何值                            |
| 允許的電腦 | 任何值                            |


1. 設定 Fiddler 以擷取並解密 HTTPS 流量。
    1. 為了達到最佳效能，設定 Fiddler 以按鈕列上的 [Stream] 按鈕，即可使用資料流處理模式。
    1. 在 Fiddler 中，選取 工具，然後 Fiddler 選項 中，則 HTTPS。
    1. 檢查 [解密 HTTPS 流量] 核取方塊。 如果訊息方塊會詢問是否要設定 Windows 信任 CA 憑證，請按一下 [否]。
    1. 按一下 [桌面] 按鈕匯出根憑證。
1. 結束 Fiddler，然後重新啟動它。

### <a name="to-configure-a-dev-kit-to-use-fiddler-as-its-proxy-to-the-internet"></a>設定開發人員套件使用 Fiddler 作為其網際網路的 Proxy
在開發套件上的 fiddler 組態已簡化在舊版本中所使用的方法。

1. 複製 Fiddler 根憑證匯出到桌面，做為開發人員套件``` xs:\Microsoft\Cert\FiddlerRoot.cer```。  您可以使用下列命令：  ```xbcp [local Fiddler Root directory]\FiddlerRoot.cer xs:\Microsoft\Cert\FiddlerRoot.cer```
1. 建立名為文字檔```ProxyAddress.txt```、 IP 位址或主機名稱的開發電腦執行 Fiddler 和連接埠號碼 Fiddler 接聽做為唯一的內容檔案中的位置。 來設定格式的名稱/IP 位址和連接埠，如下所示：「 主機： 連接埠 」。 （根據預設，Fiddler 使用連接埠 8888）。例如，"10.124.220.250:8888"或者"my_dev_pc.contoso.com:8888 」。 這個檔案複製到開發套件，為 xs:\Microsoft\Fiddler\ProxyAddress.txt。  您可以使用下列命令：  ```xbcp [local Proxy Address file directory]\ProxyAddress.txt xs:\Microsoft\Fiddler\ProxyAddress.txt```
1. 重新啟動開發套件，鍵入```xbreboot```到命令提示字元

### <a name="to-stop-using-fiddler"></a>停止使用 Fiddler

若要停止使用 Fiddler，做為 proxy 到網際網路，並因此追蹤所有在 Fiddler 中的開發套件的網路流量，只是重新命名或刪除 「 開發人員套件 」，在 FiddlerRoot.cer 檔案，然後再重新啟動開發套件。

### <a name="how-it-works"></a>運作方式

Xs:\Microsoft\Cert\FiddlerRoot.cer 檔案及 xs:\Microsoft\Fiddler\ProxyAddress.txt 檔案在開機時，會導致開發套件，將自己設定成使用 ProxyAddress.txt 中指定的 proxy 伺服器。 如果遺漏任一個檔案，然後開發人員套件不會設定本身，以透過 Fiddler proxy 運作。

每部安裝 Fiddler 的電腦都會使用不同的 Fiddler 根憑證。 如果您有一個以上的電腦可能會用來為您的開發套件中提供的 Fiddler proxy，您可以複製每個電腦的 Fiddler 的根憑證，到 xs:\Microsoft\Cert 目錄中，只要其中一個名為 FiddlerRoot.cer 即可。 正在驗證的 Fiddler proxy 時，會檢查所有的憑證目錄中的憑證項目。 在任何電腦的位址位於 ProxyAddress.txt Fiddler 執行個體將用於做為 proxy，只要其憑證存在的憑證目錄中。
