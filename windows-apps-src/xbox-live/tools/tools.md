---
title: Xbox Live 的開發工具
description: 了解可協助開發和測試您的 Xbox Live 啟用標題的工具。
ms.assetid: 380a29bf-41a7-4817-9c57-f48f2b824b52
ms.date: 6/13/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 工具、 播放程式重設、 即時追蹤分析器，LTA，xbox live 帳戶工具，
ms.localizationpriority: medium
ms.openlocfilehash: ad43ecbd3bfd266d4a237253380bca223302fe54
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623513"
---
# <a name="development-tools-for-xbox-live"></a>Xbox Live 的開發工具

本節說明可用來協助您開發的 Xbox Live 的各種工具。 有許多工具都位於[Xbox Live 開發人員工具 GitHub](https://github.com/Microsoft/xbox-live-developer-tools)存放庫。 您也可以使用[開發人員工具程式庫](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools)來建立您自己自訂的工具。 所有的獨立開發人員工具下載位置[ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools)。

> [!NOTE]
> 下載中包含的 MatchSim 和 XboxLiveCompute 工具僅適用於受管理的合作夥伴，或在註冊協力廠商[ ID@Xbox ](https://www.xbox.com/Developers/id)程式。 若要深入了解可用的開發人員計劃，請參閱[開發人員計劃概觀](https://docs.microsoft.com/windows/uwp/xbox-live/developer-program-overview)。 

## <a name="global-storage"></a>全域儲存體
全域的標題儲存體用來儲存所有使用者都可以讀取，例如名冊、 對應、 挑戰或美工圖案資源的資料。 它是一種[標題儲存體](../storage-platform/xbox-live-title-storage/xbox-live-title-storage.md)。 通用的儲存體工具用來管理測試沙箱中的全域的標題儲存體。 仍然必須將資料發佈為透過合作夥伴中心或 Xbox 開發人員入口網站 (XDP) 的零售版。 此工具會透過命令列的一部分[開發工具](https://aka.ms/xboxliveuwptools)zip。 您可以使用建立自訂的工具[開發人員工具程式庫](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools)。

## <a name="multiplayer-session-history-viewer"></a>多人連線的工作階段歷程記錄檢視器
多人遊戲工作階段歷程記錄檢視器可讓您能夠檢視的所有變更歷程記錄時間軸，透過多人連線的工作階段文件的歷程記錄 （包括已刪除的文件）。 使用此工具會提供您更深入的了解隨時間變更發生 MPSD 工作階段的文件。 它是以獨立的工具中提供[開發工具](https://aka.ms/xboxliveuwptools)zip。

## <a name="player-data-reset"></a>玩家資料重設
播放程式重設資料 」 工具可用來重設測試沙箱中的播放程式的資料。 您可以如; 重設資料成就、 排行榜、 統計資料和標題的歷程記錄。 此工具會透過命令列的一部分[開發工具](https://aka.ms/xboxliveuwptools)zip。 您可以使用建立自訂的工具[開發人員工具程式庫](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools)。

## <a name="xbox-live-developer-account"></a>Xbox Live 的開發人員帳戶
Xbox Live 開發人員帳戶工具用來管理開發人員帳戶的驗證。 它需要與其他需要開發人員認證，例如播放程式重設和全域儲存體的開發人員工具互動。 此工具會透過命令列的一部分[開發工具](https://aka.ms/xboxliveuwptools)zip。

## <a name="xbox-live-trace-analyzer"></a>Xbox Live 的追蹤分析器
使用[Xbox Live 追蹤分析器](analyze-service-calls.md)，您可以擷取所有服務呼叫，並接著呼叫模式的任何違規分析它們離線。 追蹤服務的呼叫可啟用使用 xbtrace 命令列工具，或透過多個通訊協定啟用進階的案例。 也支援啟用追蹤直接從標題的程式碼的服務呼叫。 此工具會透過命令列的一部分[開發工具](https://aka.ms/xboxliveuwptools)zip。

## <a name="xbox-live-account-tool"></a>Xbox Live 帳戶工具  
[Xbox Live 帳戶工具](xbox-live-account-tool.md)設計來協助您設定現有的測試帳戶，以測試遊戲的案例。 例如，您可以使用 Xbox Live 帳戶工具來變更帳戶的玩家代號、 或快速將 1000年粉絲新增至帳戶的好友名單。 此工具會透過命令列的一部分[開發工具](https://aka.ms/xboxliveuwptools)zip。

## <a name="config-as-source"></a>做為來源的組態
[設定為來源](https://github.com/Microsoft/xbox-live-developer-tools/blob/master/CONFIGASSOURCE.md)是一套以容納進階的使用者，藉由將整合到我們的組態服務提供正式支援的工具和 Api 的 Microsoft 開發的工具。 您在合作夥伴中心，包括服務範圍從排行榜，成就，而 web 服務和信賴憑證者的合作對象的標題通常會設定這些 Xbox Live 服務。 許多遊戲開發人員而言，使用合作夥伴中心已足夠。 對於進階使用者，不過，還是會渴望將一般設定工作整合到他們自己的處理程序和工具。  做為來源的設定被要支援這些案例提供的命令列工具和新的 Api，以支援您現有的工作流程和管線的自訂整合。 此工具會透過命令列的一部分[開發工具](https://aka.ms/xboxliveuwptools)zip。
