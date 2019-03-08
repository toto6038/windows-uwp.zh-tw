---
title: 如何在針對 UWP 進行開發時使用 Fiddler 搭配 Xbox One
description: 描述如何使用免費軟體 Fiddler 工具，在 UWP Xbox One 開發人員套件上查看網路流量。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 9c133c77-fe9d-4b81-b4b3-462936333aa3
ms.localizationpriority: medium
ms.openlocfilehash: fae6caf73cb8a5b569193a17e65e5d8b4f582ff2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57652223"
---
# <a name="how-to-use-fiddler-with-xbox-one-when-developing-for-uwp"></a>如何在針對 UWP 進行開發時使用 Fiddler 搭配 Xbox One

Fiddler 是 Web 偵錯 Proxy，它會記錄您的 Xbox One 開發人員套件與網際網路之間所有的 HTTP 與 HTTPS 流量。 您將使用它來記錄與調查 Xbox 服務與信賴憑證者 Web 服務的進出流量，以了解 Web 服務的呼叫並偵錯。 

在一般作業中，透過 Proxy 通訊的主機有通訊遭到 Proxy 修改的風險，並可能使玩家得以作弊。 因此，主機被設計為不允許透過 Proxy 通訊。 您必須在開發人員套件上執行一些特殊的設定步驟來讓它可以使用 Fiddler Proxy，才能搭配 Xbox One 開發人員套件使用 Fiddler。 

Fiddler 是免費軟體，可以從 [Fiddler 網站](https://www.fiddler2.com/fiddler2/)下載。 

Fiddler 可能會影響主機所報告的網路狀態。 如果執行 Fiddler 的電腦停用上游連線，直到主機的驗證到期之前，主機可能無法偵測到這個中斷連線。 如果您正在使用 Fiddler，請務必中斷主機與執行 Fiddler 電腦之間的連線，而不要使用 Fiddler 模擬中斷連線。

### <a name="to-install-and-enable-fiddler-on-your-development-pc"></a>在開發電腦上安裝並啟用 Fiddler
請依照以下步驟安裝並啟用 Fiddler 來監視您的開發人員套件流量：

1. 依照 [Fiddler 網站](https://www.fiddler2.com/fiddler2/)上的指示，在您的開發電腦上安裝 Fiddler。 
2. 啟動 Fiddler 並從 [工具] 功能表選取 [Fiddler 選項]。 
3. 選取 [連線] 索引標籤，然後確定已選取 [允許遠端電腦連線]。 
4. 按一下 [OK] (確定) 接受對設定的變更。 您會看到顯示 Fiddler 必須重新啟動，變更才會生效，且您必須手動設定防火牆的對話方塊。 按一下對話方塊上的 [OK] (確定) 對話方塊，但「還不要重新啟動 Fiddler」。
5. 設定所需的防火牆規則，以允許遠端電腦連線。 啟動 Windows 防火牆控制台小程式。 依序按一下 [進階設定]、[輸入規則]。 尋找名為 "FiddlerProxy" 的規則並捲動到右邊，確認下表中的每項設定都顯示於該規則上。
  
  | 設定           | 偏好值                |
  | ----              | ----                           |
  | 名稱              | FiddlerProxy                   |
  | 群組             | *沒有值* |
  | 設定檔           | 全部                            |
  | Enabled           | 是                            |
  | 動作            | 允許                          |
  | 覆寫          | 否                             |
  | 程式           | *fiddler.exe 路徑*          |
  | LocalAddress      | 任何值                            |
  | RemoteAddress     | 任何值                            |
  | 通訊協定          | TCP                            |
  | 本機連接埠         | 任何值                            |
  | 遠端連接埠        | 任何值                            |
  | 允許的使用者      | 任何值                            |
  | 允許的電腦  | 任何值                            |


6. 執行下列動作，設定 Fiddler 以擷取並解密 HTTPS 流量：
  1. 為啟用最佳效能，按一下按鈕列上的 [資料流] 按鈕，將 Fiddler 設為使用資料流模式。
  2. 在 Fiddler [工具] 功能表中，選取 [Fiddler 選項]，然後按一下 [HTTPS]。
  3. 選取 [解密 HTTPS 流量] 核取方塊。 如果對話方塊詢問是否要將 Windows 設為信任 CA 憑證，按一下 [否]。
  4. 按一下 [Export Root Certificate to Desktop] (匯出根憑證到桌面)。
7. 結束並重新啟動 Fiddler。

### <a name="to-configure-a-dev-kit-to-use-fiddler-as-its-proxy-to-the-internet"></a>設定開發人員套件使用 Fiddler 作為其網際網路的 Proxy

1. 瀏覽至 Xbox 裝置入口網站 UI 中的 [網路] 工具。
2. 瀏覽您匯出到桌面的 Fiddler 根憑證。 
3. 輸入執行 Fiddler 的開發電腦 IP 位址或主機名稱。
4. 輸入 Fiddler 正在接聽的連接埠號碼 (根據預設，Fiddler 會使用連接埠 8888)。 
5. 請按一下 [啟用]。 這會重新啟動您的開發人員套件。

### <a name="to-stop-using-fiddler"></a>停止使用 Fiddler
若要停止使用 Fiddler 作為網際網路的 Proxy (並停止 Fiddler 追蹤所有的開發人員套件網路流量)，請執行下列動作：

1. 瀏覽至 Xbox 裝置入口網站 UI 中的 [網路] 工具。
2. 按一下 [停用]。

> [!NOTE]
> 每部安裝 Fiddler 的電腦都會使用不同的 Fiddler 根憑證。 如果您有多部可能用來提供 Fiddler Proxy 給開發人員套件的電腦，在它們之間切換時您必須選取新的根憑證。 如果您只使用一部電腦，則您只需要在第一次啟用 Fiddler 時選取根憑證即可。 您仍必須指定 IP 位址與連接埠。

## <a name="see-also"></a>請參閱
- [Fiddler 設定 API 參考](wdp-fiddler-api.md)
- [常見問題集](frequently-asked-questions.md)
- [在 Xbox One UWP](index.md)



