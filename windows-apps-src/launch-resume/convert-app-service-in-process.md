---
author: TylerMSFT
title: "轉換 App 服務，以便與其主控 App 在相同處理序中執行"
description: "將在個別背景處理序中執行的應用程式服務程式碼，轉換成和您應用程式服務提供者在相同處理序內執行的程式碼。"
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 30aef94b-1b83-4897-a2f1-afbb4349696a
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 1fea72237a9ac7d18fb415d5957f959542a833e8
ms.lasthandoff: 02/08/2017

---

# <a name="convert-an-app-service-to-run-in-the-same-process-as-its-host-app"></a>轉換應用程式服務，以便與其主控應用程式在相同處理序中執行

[AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx) 可讓另一個應用程式喚醒您在背景中的 app，並開始與其直接通訊。

引進同處理序的應用程式服務後，兩個執行中的前景應用程式可以透過應用程式服務連線直接通訊。 應用程式服務現在可以和前景應用程式在相同的處理序中執行，讓 App 之間更容易通訊，而且不再需要將服務程式碼分成不同的專案。

需要兩處變更，才能將跨處理序模型的應用程式服務轉變成同處理序模型。 第一處是變更資訊清單。

> ```xml
>  <uap:Extension Category="windows.appService">
>          <uap:AppService Name="InProcessAppService" />
>  </uap:Extension>
> ```

移除 `EntryPoint` 屬性。 現在會使用 [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) 回撥，當成叫用應用程式服務時的回撥方法。

第二處變更是，將服務邏輯從其個別的背景工作專案移入可從 **OnBackgroundActivated()** 呼叫的方法中。

現在可以您的應用程式可以直接執行應用程式服務。  例如：

> ``` cs
> private AppServiceConnection appServiceconnection;
> private BackgroundTaskDeferral appServiceDeferral;
> protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
> {
>     base.OnBackgroundActivated(args);
>     IBackgroundTaskInstance taskInstance = args.TaskInstance;
>     AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
>     appServiceDeferral = taskInstance.GetDeferral();
>     taskInstance.Canceled += OnAppServicesCanceled;
>     appServiceConnection = appService.AppServiceConnection;
>     appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
>     appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
> }
>
> private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
> {
>     AppServiceDeferral messageDeferral = args.GetDeferral();
>     ValueSet message = args.Request.Message;
>     string text = message["Request"] as string;
>              
>     if ("Value" == text)
>     {
>         ValueSet returnMessage = new ValueSet();
>         returnMessage.Add("Response", "True");
>         await args.Request.SendResponseAsync(returnMessage);
>     }
>     messageDeferral.Complete();
> }
>
> private void OnAppServicesCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
> {
>     appServiceDeferral.Complete();
> }
>
> private void AppServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
> {
>     appServiceDeferral.Complete();
> }
> ```

在上述程式碼中，`OnBackgroundActivated` 方法負責處理應用程式服務啟用。 已登錄透過 [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx) 通訊時所需的所有事件，並已儲存工作延遲物件，以便在應用程式間的通訊完成時，標示為完成。

當 app 收到要求，然後讀取提供的 [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx)，查看是否會出現 `Key` 和 `Value` 字串。 如果有的話，則應用程式服務會將一組 `Response` 和 `True` 字串值傳回 **AppServiceConnection** 另一端的 app。

在[建立和取用 App 服務](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service?f=255&MSPPError=-2147217396)，深入了解連線並與其他 App 通訊。

