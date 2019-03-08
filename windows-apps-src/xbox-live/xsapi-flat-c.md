---
title: Xbox Live C Api
description: 深入了解一般的 C API 模型可供您與 Xbox Live 服務互動。
ms.date: 06/05/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 c、 xsapi
ms.localizationpriority: medium
ms.openlocfilehash: a1c73661b561d586f9e28957c7caa6a1b1f9cb03
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597523"
---
# <a name="introduction-to-the-xbox-live-c-apis"></a>Xbox Live C Api 簡介

在 2018 年 6 月日 XSAPI 加入新的一般 C API 層。 這個新的 API 層可解決一些與 c + + 和 WinRT API 層級發生的問題。

從 C API 還未涵蓋所有 XSAPI 功能，但開發其他功能。 所有 3 個 API 層級、 C、 c + + 和 WinRT 會繼續受到支援，並已新增一段時間的其他功能。

> [!NOTE]
> C Api 目前只適用於使用 Xbox Developer Kit (XDK) 的標題。 在此階段不支援 UWP 遊戲。

## <a name="features-covered-by-the-c-apis"></a>C Api 所涵蓋的功能

從 C API 目前支援下列功能和服務：

- 成就
- 顯示線上狀態
- 設定檔
- 社交活動
- 身分證管理員

## <a name="benefits-of-the-c-api-for-xsapi"></a>XSAPI 的 C API 的優點

- 可讓控制記憶體配置呼叫 XSAPI 時的標題。
- 允許完整控制執行緒處理時呼叫 XSAPI 的標題。
- 使用新的 HTTP 程式庫，libHttpClient，專為遊戲開發人員所設計。

您可以使用與 c + + XSAPI C Api，但您無法使用 c + + Api 取得先前所列出的優點。

### <a name="managing-memory-allocations"></a>管理記憶體配置

使用新的 C API，您現在可以指定每當嘗試配置記憶體時，會呼叫 XSAPI 函式回呼。 如果您未指定函式回呼，XSAPI 會使用標準的記憶體配置常式。

若要手動指定記憶體常式，您可以執行下列作業：

- 在遊戲開始：
  - 呼叫`XblMemSetFunctions(memAllocFunc, memFreeFunc)`指定指派和釋放記憶體配置回呼。
  - 呼叫`XblInitialize()`初始化程式庫執行個體。  
- 在執行遊戲時：
  - 呼叫任何一項新的 C Api XSAPI 中的配置或可用記憶體會造成 XSAPI 呼叫指定的記憶體處理回呼。  
- 當結束遊戲：
  - 呼叫`XblCleanup()`回收 XSAPI 程式庫相關聯的所有資源。
  - 清除您的遊戲自訂記憶體管理員。

### <a name="managing-asynchronous-threads"></a>管理非同步執行緒

從 C API 導入了新的非同步執行緒呼叫的執行緒模型的完整控制權可讓開發人員的模式。 如需詳細資訊，請參閱 < [XSAPI 呼叫模式一般 C 層非同步呼叫](flatc-async-patterns.md)。

## <a name="migrating-code-to-use-c-xsapi"></a>若要使用 C XSAPI 移轉程式碼

XSAPI C Api 都可以搭配 XSAPI c + + Api，在專案中，因此我們建議您一次移轉一項功能。

C Api 和 c + + Api 是通用的核心，只是與不同進入點，其實只是精簡型包裝函式，因此功能應該維持不變。 不過，只有 C Api 可以利用自訂的記憶體和執行緒的管理功能。

> [!IMPORTANT]
> 您不能混用 XSAPI WinRT Api，與 C Api。

## <a name="where-to-view-the-c-apis"></a>若要檢視 C Api 的位置

- [C API 標頭檔](https://github.com/Microsoft/xbox-live-api/tree/master/Include/xsapi-c)
- [使用新的 C Api 的範例程式碼](https://github.com/Microsoft/xbox-live-api/tree/master/InProgressSamples/Social/Xbox/C)
- [libHttpClient](https://github.com/Microsoft/libHttpClient)
