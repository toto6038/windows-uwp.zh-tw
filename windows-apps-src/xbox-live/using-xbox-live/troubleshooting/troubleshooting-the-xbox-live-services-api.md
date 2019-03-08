---
title: 疑難排解 Xbox Live 服務 API
description: 了解如何將記錄額外的錯誤資訊，同時使用 Xbox Live Api 進行疑難排解。
ms.assetid: 3827bba1-902f-4f2d-ad51-af09bd9354c4
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 疑難排解、 錯誤、 記錄檔
ms.localizationpriority: medium
ms.openlocfilehash: 67735d83e5ff301ee4434a6917fce814cabe8309
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621413"
---
# <a name="troubleshooting-the-xbox-live-apis"></a>疑難排解的 Xbox Live 的 Api

## <a name="code"></a>程式碼

很難診斷失敗，使用從 Xbox Live 服務 API 層級的錯誤。 額外的錯誤資訊，例如符合 rest 限制的所有呼叫的記錄，可能是可供伺服器使用。 若要接聽這項資料，將連結回應記錄器並啟用偵錯追蹤。 回應記錄可讓您看到 HTTP 流量和 web 服務回應碼，這通常是像是 Fiddler 追蹤一樣，毫無用處。

### <a name="c"></a>C++

下列程式碼範例會啟用回應記錄，並將偵錯錯誤層級設定為 （您也可以設定偵錯錯誤層級錯誤以顯示僅追蹤失敗的呼叫，或關閉停用追蹤） 的詳細資訊。 在 Visual Studio 中執行您的專案時，所產生的偵錯輸出會傳送至 [輸出] 窗格。  

```cpp

        // Set up debug tracing to the Output window in Visual Studio.
            xbox::services::system::xbox_live_services_settings::get_singleton_instance()->set_diagnostics_trace_level(
                xbox_services_diagnostics_trace_level::verbose
                );
```

您也可以選擇偵錯輸出重新導向至您自己的記錄檔就像這樣：

```cpp

        // Set up debug tracing of the Xbox Live Services API traffic to the game UI.
        m_xboxLiveContext->Settings->EnableServiceCallRoutedEvents = true;
        m_xboxLiveContext->Settings->ServiceCallRouted += ref new
        Windows::Foundation::EventHandler<Microsoft::Xbox::Services::XboxServiceCallRoutedEventArgs^>(
            [=] ( Platform::Object^, Microsoft::Xbox::Services::XboxServiceCallRoutedEventArgs^ args )
            {
                gameUI->Log(L"[URL]: " + args->HttpMethod + " " + args->Url->AbsoluteUri);
                gameUI->Log(L"");
                gameUI->Log(L"[Response]: " + args->HttpStatus.ToString() + " " + args->ResponseBody);
            });

```
