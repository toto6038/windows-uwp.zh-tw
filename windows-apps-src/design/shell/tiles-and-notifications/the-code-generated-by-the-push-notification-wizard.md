---
author: mijacobs
Description: By using a wizard in Visual Studio, you can generate push notifications from a mobile service that was created with Azure Mobile Services.
title: 由推播通知精靈產生的程式碼
ms.assetid: 340F55C1-0DDF-4233-A8E4-C15EF9030785
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6b39211c4b21a68fc0e563f73805805dcf1f4641
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5641426"
---
# <a name="code-generated-by-the-push-notification-wizard"></a>由推播通知精靈產生的程式碼
 

您可以藉由 Visual Studio 中的精靈，從利用 Azure 行動服務建立的行動服務產生推播通知。 Visual Studio 精靈會產生程式碼，協助您開始使用。 這個主題說明精靈如何修改您的專案、所產生的程式碼如何作用、如何使用此程式碼，以及如何進一步充分利用推播通知。 請參閱 [Windows 推播通知服務 (WNS) 概觀](windows-push-notification-services--wns--overview.md)。

## <a name="how-the-wizard-modifies-your-project"></a>精靈如何修改您的專案


推播通知精靈會透過下列方式修改您的專案：

-   新增對行動服務受管理用戶端 (MobileServicesManagedClient.dll) 的參考。 不適用於 JavaScript 專案。
-   根據服務在子資料夾新增檔案，將檔案命名為 push.register.cs、push.register.vb、push.register.cpp 或 push.register.js。
-   在行動服務的資料庫伺服器上建立通道表。 此表格包含將推播通知傳送到應用程式執行個體所需的資訊。
-   建立四個函式的指令碼：delete、insert、read 和 update。
-   使用自訂 API (notifyallusers.js) 建立一個指令碼，將推播通知傳送到所有用戶端。
-   將宣告加入您的應用程式.xaml.cs、App.xaml.vb 或應用程式.xaml.cpp 檔案，或將宣告加入 JavaScript 專案的新檔案 service.js。 這個宣告會宣告 MobileServiceClient 物件，其中包含連接到行動服務時所需的資訊。 您可以使用應用程式.*MyServiceName*Client 名稱，從應用程式的任一頁面存取這個名為 *MyServiceName*Client 的 MobileServiceClient 物件。

services.js 檔案包含下列程式碼：

```js
var <mobile-service-name>Client = new Microsoft.WindowsAzure.MobileServices.MobileServiceClient(
                "https://<mobile-service-name>.azure-mobile.net/",
                "<your client secret>");
```

## <a name="registration-for-push-notifications"></a>推播通知的登錄


在 push.register.\* 中，UploadChannel 方法會登錄要接收推播通知的裝置。 市集會追蹤已安裝應用程式的執行個體，並提供推播通知通道。 請參閱 [**PushNotificationChannelManager**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager)。

用戶端程式碼類似 JavaScript 後端和 .NET 後端兩者。 根據預設，當您新增 JavaScript 後端服務的推播通知時，對 notifyAllUsers 自訂 API 的一個範例呼叫會插入 UploadChannel 方法。

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.MobileServices;
using Newtonsoft.Json.Linq;

namespace App2
{
    internal class mymobileservice1234Push
    {
        public async static void UploadChannel()
        {
            var channel = await Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            try
            {
                await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri);
                await App.mymobileservice1234Client.InvokeApiAsync("notifyAllUsers");
            }
            catch (Exception exception)
            {
                HandleRegisterException(exception);
            }
        }

        private static void HandleRegisterException(Exception exception)
        {
            
        }
    }
}
```

```vb
Imports Microsoft.WindowsAzure.MobileServices
Imports Newtonsoft.Json.Linq

Friend Class mymobileservice1234Push
    Public Shared Async Sub UploadChannel()
        Dim channel = Await Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync()

        Try
            Await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri)
            Await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri, New String() {"tag1", "tag2"})
            Await App.mymobileservice1234Client.InvokeApiAsync("notifyAllUsers")
        Catch exception As Exception
            HandleRegisterException(exception)
        End Try
    End Sub

    Private Shared Sub HandleRegisterException(exception As Exception)

    End Sub
End Class
```

```c++
#include "pch.h"
#include "services\mobile services\mymobileservice1234\mymobileservice1234Push.h"

using namespace AzureMobileHelper;

using namespace web;
using namespace concurrency;

using namespace Windows::Networking::PushNotifications;

void mymobileservice1234Push::UploadChannel()
{
    create_task(PushNotificationChannelManager::CreatePushNotificationChannelForApplicationAsync()).
    then([] (PushNotificationChannel^ newChannel) 
    {
        return mymobileservice1234MobileService::GetClient().get_push().register_native(newChannel->Uri->Data());
    }).then([]()
    {
        return mymobileservice1234MobileService::GetClient().invoke_api(L"notifyAllUsers");
    }).then([](task<json::value> result)
    {
        try
        {
            result.wait();
        }
        catch(...)
        {
            HandleExceptionsComingFromTheServer();
        }
    });
}

void mymobileservice1234Push::HandleExceptionsComingFromTheServer()
{
}
```

```js
(function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.addEventListener("activated", function (args) {
        if (args.detail.kind == activation.ActivationKind.launch) {
            Windows.Networking.PushNotifications.PushNotificationChannelManager.createPushNotificationChannelForApplicationAsync()
                .then(function (channel) {
                    mymobileserviceclient1234Client.push.registerNative(channel.Uri, new Array("tag1", "tag2"))
                    return mymobileservice1234Client.push.registerNative(channel.uri);
                })
                .done(function (registration) {
                    return mymobileservice1234Client.invokeApi("notifyAllUsers");
                }, function (error) {
                    // Error

                });
        }
    });
})();
```

推播通知標記對用戶端子集提供限制通知的方式。 您可以使用 registerNative 方法 (或 RegisterNativeAsync 方法) 來登錄所有推播通知但不指定標記，或者您可以藉由提供第二個引數 (標記陣列) 使用標記登錄。 如果您使用一或多個標記登錄，只會收到符合這些標記的通知。

## <a name="server-side-scripts-javascript-backend-only"></a>伺服器端指令碼 (僅限 JavaScript 後端)


如果是使用 JavaScript 後端的行動服務，則會在進行刪除、插入、讀取或更新操作時執行伺服器端指令碼。 指令碼不會實作這些操作，但是當用戶端呼叫 Windows Mobile REST API 觸發這些事件時，就會執行指令碼。 然後指令碼會呼叫 request.execute 或 request.respond 向呼叫內容發出回應，將控制權傳遞給操作本身。 請參閱 [Azure 行動服務 REST API 參考](http://go.microsoft.com/fwlink/p/?linkid=511139)。

伺服器端指令碼提供各種不同函式。 請參閱[在 Azure 行動服務中登錄資料表操作](http://go.microsoft.com/fwlink/p/?linkid=511140)。 如需所有可用函式的參考，請參閱[行動服務伺服器指令碼參考](http://go.microsoft.com/fwlink/p/?linkid=257676)。

系統也會在 Notifyallusers.js 中建立下列自訂 API 程式碼：

```js
exports.post = function(request, response) {
    response.send(statusCodes.OK,{ message : 'Hello World!' })
    
    // The following call is for illustration purpose only
    // The call and function body should be moved to a script in your app
    // where you want to send a notification
    sendNotifications(request);
};

// The following code should be moved to appropriate script in your app where notification is sent
function sendNotifications(request) {
    var payload = '<?xml version="1.0" encoding="utf-8"?><toast><visual><binding template="ToastText01">' +
        '<text id="1">Sample Toast</text></binding></visual></toast>';
    var push = request.service.push; 
    push.wns.send(null,
        payload,
        'wns/toast', {
            success: function (pushResponse) {
                console.log("Sent push:", pushResponse);
            }
        });
}
```

sendNotifications 函式會以快顯通知的方式傳送單一通知。 您也可以使用其他類型的推播通知。

**提示：** 如何在編輯指令碼時取得協助的相關資訊，請參閱[針對伺服器端 JavaScript 啟用 IntelliSense](http://go.microsoft.com/fwlink/p/?LinkId=309275)。

 

## <a name="push-notification-types"></a>推播通知類型


Windows 可支援推播通知以外的通知。 如需有關通知的一般資訊，請參閱[選擇通知傳遞方法](choosing-a-notification-delivery-method.md)。

快顯通知的使用方式非常簡單，您可以檢閱通道表上的 Insert.js 程式碼中為您產生的範例。 如果您計劃使用磚或徽章通知，必須為磚和徽章建立 XML 範本，也必須指定範本中的封裝資訊編碼方式。 請參閱[使用磚、徽章以及快顯通知](https://msdn.microsoft.com/library/windows/apps/xaml/hh868259)。

因為 Windows 會回應推播通知，所以能在應用程式未執行時處理大部分的這類通知。 例如，即使本機郵件應用程式並未執行，推播通知也能讓使用者得知有新的郵件訊息。 Windows 處理快顯通知的方式是顯示訊息，例如文字訊息的第一行。 Windows 處理磚或徽章通知的方式是更新應用程式的動態磚，以反映新郵件訊息數。 您可以利用這種方式提示應用程式使用者檢查新資訊。 您的應用程式可以在執行時收到原始通知，而您可以使用這類通知將資料傳送給應用程式。 如果應用程式未執行，您可以設定背景工作來監視推播通知。

您應該根據通用 Windows 平台 (UWP) 應用程式的指導方針來使用推播通知，因為這些通知會用盡使用者的資源，而且過度使用也可能造成困擾。 請參閱[推播通知的指導方針和檢查清單](https://msdn.microsoft.com/library/windows/apps/hh761462)。

如果您利用推播通知更新動態磚，也應該遵循[磚與徽章的指導方針和檢查清單](https://msdn.microsoft.com/library/windows/apps/hh465403)中的指導方針。

## <a name="next-steps"></a>後續步驟


### <a name="using-the-windows-push-notification-services-wns"></a>使用 Windows 推播通知服務 (WNS)

若行動服務提供的彈性不足、您想要以 C# 或 Visual Basic 撰寫伺服器程式碼，或是您已經有雲端服務並想要透過它傳送推播通知，則可以直接呼叫 Windows 推播通知服務 (WNS)。 直接呼叫 WNS 時，可以從您自己的雲端服務 (例如，監視來自資料庫或其他 Web 服務之資料的工作者角色) 傳送推播通知。 您的雲端服務必須透過 WNS 進行驗證，才能將推播通知傳送到您的應用程式。 請參閱[如何使用 Windows 推播通知服務進行驗證 (JavaScript)](https://msdn.microsoft.com/library/windows/apps/hh465407) 或 [(C#/C++/VB)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868206)。

您也可以在行動服務中執行排定的工作，以傳送推播通知。 請參閱[在行動服務中排程週期性工作](http://go.microsoft.com/fwlink/p/?linkid=301694)。

**警告**當您一次執行推播通知精靈時，不會執行精靈來針對其他行動服務新增註冊碼第二次。 針對單一專案多次執行精靈時，所產生的註冊碼會造成重複呼叫 [**CreatePushNotificationChannelForApplicationAsync**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync) 方法，進而導致發生執行階段例外狀況。 如果您想要為多個行動服務註冊推播通知，請執行一次精靈，然後重新寫入註冊碼，以確保不會同時執行對 **CreatePushNotificationChannelForApplicationAsync** 的呼叫。 例如，您可以將精靈在 push.register.\* 中產生的註冊碼 (包括對 **CreatePushNotificationChannelForApplicationAsync** 的呼叫) 移到 OnLaunched 事件外以達到這個目的，但其中的細節將取決於您應用程式的架構。

 

## <a name="related-topics"></a>相關主題


* [Windows 推播通知服務 (WNS) 概觀](windows-push-notification-services--wns--overview.md)
* [原始通知概觀](raw-notification-overview.md)
* [連線到 Microsoft Azure 行動服務 (JavaScript)](https://msdn.microsoft.com/library/windows/apps/dn263160)
* [連線到 Microsoft Azure 行動服務 (C#/C++/VB)](https://msdn.microsoft.com/library/windows/apps/xaml/dn263175)
* [快速入門：為行動服務加入推播通知 (JavaScript)](https://msdn.microsoft.com/library/windows/apps/dn263163)
 

 




