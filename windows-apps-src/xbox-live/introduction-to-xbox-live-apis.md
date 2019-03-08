---
title: Xbox Live API 簡介
description: 深入了解各種 API 模型可供您與 Xbox Live 服務互動。
ms.assetid: 5918c3a2-6529-4f07-b44d-51f9861f91ec
ms.date: 06/05/2018
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1f52e379b524952c3361b432a577a7137b02155b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657693"
---
# <a name="introduction-to-xbox-live-apis"></a>Xbox Live API 簡介

## <a name="use-xbox-live-services"></a>使用 Xbox Live 服務

有兩種方式可取得的 Xbox Live 服務的資訊：

- 使用一個名為 Xbox Live 服務 API 的用戶端端 API (**XSAPI**)
- 呼叫**Xbox Live 的 REST 端點**直接

使用 Xbox Live 服務 API 的優點 (**XSAPI**) 包括：

- 驗證、 編碼和 HTTP 傳送和接收的詳細資料會為您處理。
- 引數，以及資料從傳回，包裝函式 API 會處理在原生資料類型;因此，您不需要執行 JSON 編碼和解碼。
- 直接呼叫 web 服務牽涉到多個非同步的步驟，封裝 API 的包裝函式;這可讓標題的程式碼更容易讀取和寫入。
- 某些功能，例如撰寫的遊戲事件，僅供以 XSAPI。

使用的優點**Xbox Live 的 REST 端點**直接包括：

- 從 web 服務呼叫 Xbox Live 端點的能力
- 呼叫中未包括在 XSAPI 端點的能力。  XSAPI 只包含我們相信，遊戲將會使用，因此如果沒有任何項目遺漏可讓我們知道透過論壇的 Api。
- 經由 REST 端點，您可以使用某些功能可能沒有對應的 XSAPI 包裝函式。

您的遊戲和應用程式不會僅限於使用這些方法的其中一項目。 您可以使用 XSAPI 包裝函式，並仍 REST 端點直接呼叫如有需要。

## <a name="xbox-live-services-api-overview"></a>Xbox Live 服務 API 概觀 ##

Xbox Live 服務 API (**XSAPI**) 會公開三個集合的用戶端 Api，可支援各種不同的客戶案例：

- [XSAPI WinRT API](#xsapi-winrt-based-api)
- [C + + 11 XSAPI 型 API](#xsapi-c11-based-api)
- [XSAPI C 型 API](#xsapi-c-based-api) (**自 2018 年 6 月起的新**)

比較 Api:

### <a name="xsapi-winrt-based-api"></a>XSAPI WinRT 型 API

- 支援應用程式以 C + + /CX 中， C#，和 JavaScript。
    - C + + /CX 是 Microsoft c + + 擴充功能可以很容易 WinRT 程式設計並不會使用 ^ 與 WinRT 指標。
- 支援以 Xbox One XDK 的平台和通用 Windows 平台 (UWP) x86、 x64 和 ARM 架構為目標的應用程式。
- 錯誤的處理透過例外狀況中的所有語言包括 C + + /CX。
- C + + /cli WinRT 也支援。  詳細資訊，關於 C + + /cli WinRT，請參閱 [https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/](https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/)

以下是範例呼叫 XSAPI WinRT API，使用 C + + /cli WinRT:

```c++
winrt::Windows::Xbox::System::User cppWinrtUser = winrt::Windows::Xbox::System::User::Users().GetAt(0);
winrt::Microsoft::Xbox::Services::XboxLiveContext xblContext(cppWinrtUser);
```

如果您想要混用 C + + /CX 和 C + + /cli 做為您的 WinRT 是移轉程式碼，您可以這樣做過，但會比較複雜一點。  
以下是範例呼叫 XSAPI WinRT API，使用 C + + /cli WinRT 提供 C + + /CX 使用者 ^ 物件。

```c++
::Windows::Xbox::System::User^ user1 = ::Windows::Xbox::System::User::Users->GetAt(0);
winrt::Windows::Xbox::System::User cppWinrtUser(nullptr);
winrt::copy_from(cppWinrtUser, reinterpret_cast<winrt::ABI::Windows::Xbox::System::IUser*>(user1));
winrt::Microsoft::Xbox::Services::XboxLiveContext xblContext(cppWinrtUser);
```


### <a name="xsapi-c11-based-api"></a>C + + 11 XSAPI 型 API

- 使用跨平台 ISO 標準 C + + 11
- 使用 c + + 撰寫的支援應用程式
- 支援以 Xbox One XDK 的平台和通用 Windows 平台 (UWP) x86、 x64 和 ARM 架構為目標的應用程式。
- 透過 std::error_code 未處理錯誤。
- C + + 11 基礎的 API 是建議的 API，可用於 c + + 的遊戲引擎更好的效能和更容易偵錯。
- 如果您是在 Xbox Live 創作者計劃，包含 XSAPI 之前，標頭會定義 XBOX_LIVE_CREATORS_SDK。 這會限制為僅供開發人員在 Xbox Live 創作者計劃中的 API 介面區，並變更該登入方法來處理在創作者計劃中的項目。  例如：

```c++
#define XBOX_LIVE_CREATORS_SDK
#include "xsapi\services.h"
```

- C + + /cli WinRT 也支援。  詳細資訊，關於 C + + /cli WinRT，請參閱 [https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/](https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/)

若要使用 C + + /cli WinRT 使用 XSAPI c + + API 時，包含 XSAPI 標頭之前定義 XSAPI_CPPWINRT。  例如：

```c++
#define XSAPI_CPPWINRT
#include "xsapi\services.h"
```

以下是範例呼叫 XSAPI c + + API 使用 C + + /cli WinRT:

```c++
winrt::Windows::Xbox::System::User cppWinrtUser = winrt::Windows::Xbox::System::User::Users().GetAt(0);
std::shared_ptr<xbox::services::xbox_live_context> xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>(cppWinrtUser);
```

### <a name="xsapi-c-based-api"></a>XSAPI C 型 API

- 可讓控制記憶體配置呼叫 XSAPI 時的標題。
- 允許完整控制執行緒處理時呼叫 XSAPI 的標題。
- 使用新的 HTTP 程式庫，libHttpClient，專為遊戲開發人員所設計。

如需詳細資訊，請參閱 < [Xbox Live C Api 簡介](xsapi-flat-c.md)。
