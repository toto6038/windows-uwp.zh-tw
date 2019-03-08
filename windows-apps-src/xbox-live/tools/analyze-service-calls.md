---
title: Xbox Live 的追蹤分析器
description: 了解如何使用 Xbox Live 追蹤 Analyzer 來檢閱您的項目所進行的服務呼叫。
ms.assetid: b4490fae-d554-403d-bbbc-601af38af0ef
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 一個 xbox、 服務呼叫、 測試、 追蹤分析器
ms.localizationpriority: medium
ms.openlocfilehash: fa8ca37842edfbeaab0063cd953f3a34358a82da
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623463"
---
# <a name="xbox-live-trace-analyzer"></a>Xbox Live 的追蹤分析器

Xbox Live 服務 API 現在可讓標題開發人員擷取所有服務呼叫，並接著呼叫模式的任何違規分析它們離線。 使用提供的新功能在 xbtrace 命令列工具中，或透過通訊協定啟用更進階的案例，就可以啟動服務的呼叫追蹤。 也支援啟用追蹤直接從標題的程式碼的服務呼叫。 名為 「 Xbox Live 追蹤分析器 」 (XBLTraceAnalyzer.exe) 的離線分析工具可從 Xbox Live 的工具套件的一部分[ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools)。


## <a name="gather-logs-and-analyze-the-service-calls"></a>收集記錄和分析服務呼叫

下列步驟所需收集的記錄檔，其中包含您的服務呼叫的記錄並分析使用 Xbox Live 追蹤分析器。

1.  建置您使用新版的 Xbox Live 服務 API 都會包含在 7 月 2015年或較新版本的 Xbox Developer Kit (XDK) 中的標題。
2.  修改您的標題，以啟用追蹤，如下所述。
3.  部署您的標題。
4.  啟動標題，並讓 Xbox Live 服務的至少一個呼叫，以初始化 Xbox Live 服務 API。
5.  在您想要分析您的標題中點開始追蹤。
6.  停止追蹤。
7.  在開發電腦上執行的 Xbox Live 追蹤分析器工具，並檢視輸出。

## <a name="starting-and-stopping-tracing"></a>啟動和停止追蹤

有三種方式可以啟動和停止追蹤：

1.  您可以呼叫一組的 Xbox Live 服務 Api，直接從您的標題。
2.  您可以使用*xbtrace*命令列工具。
3.  您可以使用透過的通訊協定啟用*應用程式管理 (xbapp.exe)* 命令列工具。


### <a name="starting-and-stopping-tracing-directly-from-your-title"></a>啟動和停止追蹤，直接從您的標題

若要開始追蹤直接從您的標題，您必須執行下列作業：

1.  在 `Microsoft::Xbox::Services::Experimental`命名空間，設定`EnableServiceCallTracking`屬性`ServiceCallTrackerSettings`類別設為 true。
2.  呼叫`StartServiceCallTracking()`若要開始追蹤服務呼叫。
3.  呼叫`StopServiceCallTracking()`停止追蹤服務呼叫。
4.  追蹤停止之後，產生的追蹤檔從開發人員暫存磁碟機複製在主控台上回到您的電腦使用其中一種*檔案複製 (xbcp.exe)* 或*Xbox 一個網路上的芳鄰*以使用 Xbox Live 追蹤分析器來分析它。

### <a name="starting-and-stopping-tracing-by-using-the-xbtrace-command-line-tool"></a>啟動和停止追蹤使用 xbtrace 命令列工具

開始追蹤的最方便且簡單的方式是使用 xboxliveservices 追蹤型別使用 xbtrace 命令列工具。 當您使用 xbTrace 時，產生的追蹤檔就會為您複製回您的電腦。

啟動和停止追蹤使用 xbtrace 依賴通訊協定啟用。 您必須使用之前 xbtrace 啟動和停止追蹤，初始化通訊協定啟用藉由呼叫`RegisterForProtcolActivation`方法`ServiceCallTrackerSettings`類別。

下列範例示範如何啟動和停止使用 xbTrace 的 Xbox Live 服務追蹤：

    xbtrace start xboxliveservices
    xbtrace stop


請記住，必須執行您的標題，而且您可以啟動和停止追蹤與 xbtrace 之前，必須先初始化通訊協定啟用。 追蹤停止之後，xbtrace 將追蹤檔案複製到開發電腦，並將它放在其名稱包含"xbtrace 」 和 「 時間戳記的目錄。 這個目錄的名稱可以使用來覆寫\[etlfile\] xbtrace 的選項。

<a name="starting-and-stopping-tracing-by-using-protocol-activation"></a>啟動和停止使用通訊協定啟用的追蹤
----------------------------------------------------------
也可以使用 「 xbApp 產品上市 」 的通訊協定啟動功能控制追蹤。 您必須知道您的標題 titleid 啟動和停止透過通訊協定啟用的追蹤。 您的標題資訊清單檔中，您可以找到您標題的識別碼。 追蹤被透過包含 「 serviceCallTracking"參數的 Uri。 下列範例示範如何啟動和停止追蹤項目的標題 id 是 12345678︰

    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=start"
    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=stop"

當您使用通訊協定啟用時，產生的追蹤檔案會儲存在主控台上的開發人員暫存磁碟機上。 您必須將檔案複製回您的電腦使用 xbcp 或 Xbox 一個網路上的芳鄰。 檔案不會自動複製回到電腦上，因為它是使用 xbtrace 時。

通訊協定啟用可讓您設定其他的追蹤參數，例如詳細資訊。 支援四種詳細層級： 無訊息、 診斷、 詳細和最小。 下列範例示範如何設定的詳細資訊層級：

    xbapp launch "ms-xbl-12345678://serviceCallTracking?verbosity=diagnostic"

## <a name="analyze-the-trace-file"></a>分析追蹤檔案

追蹤檔案複製回您的電腦之後，您可以使用 GNDP 中的 Xbox Live 追蹤分析器工具來分析您的標題使用的 Xbox Live 服務。 請參閱描述如何叫用的工具和解譯輸出的遊戲開發人員網路上的 Xbox Live 追蹤分析器工具所隨附的文件。 您也可以使用命令列選項-執行 XBLTraceAnalyzer.exe 嗎？ 或者-h，若要檢視命令列說明。
