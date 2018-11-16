---
author: TylerMSFT
title: 在應用程式資訊清單中宣告背景工作
description: 在 App 資訊清單中宣告背景工作為擴充功能，以允許使用背景工作。
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp，背景工作
ms.localizationpriority: medium
ms.openlocfilehash: 343cca5b89dbe5fd7e1309b9487e8939218203d0
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2018
ms.locfileid: "6835106"
---
# <a name="declare-background-tasks-in-the-application-manifest"></a>在應用程式資訊清單中宣告背景工作




**重要 API**

-   [**BackgroundTasks 結構描述**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

在應用程式資訊清單中，透過宣告背景工作為延伸的方式，啟用它們的使用。

> [!Important]
>  本文是針對跨處理序背景工作所撰寫。 同處理序背景工作不會在資訊清單中進行宣告。

跨處理序背景工作必須在應用程式資訊清單中進行宣告，否則您的 App 將無法註冊背景工作 (將會擲回例外狀況)。 此外，跨處理序背景工作必須在應用程式資訊清單中進行宣告才能通過認證。

這個主題假設您已建立了一或多個背景工作類別，而且您的 App 註冊要執行的每一項背景工作以回應至少一個觸發程序。

## <a name="add-extensions-manually"></a>手動新增延伸


開啟 app 資訊清單 (Package.appxmanifest)，然後移至 Application 元素。 建立 Extensions 元素 (如果還不存在時)。

下列的程式碼片段取自[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)：

```xml
<Application Id="App"
   ...
   <Extensions>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
       <BackgroundTasks>
         <Task Type="systemEvent" />
         <Task Type="timer" />
       </BackgroundTasks>
     </Extension>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
       <BackgroundTasks>
         <Task Type="systemEvent"/>
       </BackgroundTasks>
     </Extension>
   </Extensions>
 </Application>
```

## <a name="add-a-background-task-extension"></a>新增背景工作延伸  

宣告您的第一個背景工作。

將這個程式碼複製到 Extensions 元素 (您將在下列步驟新增屬性)。

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="">
      <BackgroundTasks>
        <Task Type="" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```

1.  將 EntryPoint 屬性變更成具有登錄背景工作時您的程式碼用來做為進入點的相同字串 (**namespace.classname**)。

    在此範例中，進入點是 ExampleBackgroundTaskNameSpace.ExampleBackgroundTaskClassName：

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTaskClassName">
       <BackgroundTasks>
         <Task Type="" />
       </BackgroundTasks>
    </Extension>
</Extensions>
```

2.  變更 Task Type 屬性清單以表示使用這個背景工作的工作登錄類型。 如果使用多個觸發程序類型來登錄背景工作，請針對每一個觸發程序類型，新增其他 Task 元素與 Type 屬性。

    **注意：** 請確定列出的每一個觸發程序類型您要使用，或使用未宣告的觸發程序類型 （[**註冊**](https://msdn.microsoft.com/library/windows/apps/br224772)方法將會失敗並擲回例外狀況） 不會登錄背景工作。

    這個程式碼片段範例指出系統事件觸發程序和推播通知的用法：

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
    <BackgroundTasks>
        <Task Type="systemEvent" />
        <Task Type="pushNotification" />
    </BackgroundTasks>
</Extension>
```

### <a name="add-multiple-background-task-extensions"></a>新增多個背景工作擴充功能

針對每一個由應用程式註冊的其他背景工作類別，重複步驟 2。

下列範例是取自[背景工作範例]( http://go.microsoft.com/fwlink/p/?linkid=227509)的完整 Application 元素。 這將示範兩種背景工作類別的使用，總共有 3 種觸發程序類型。 複製此範例的 Extensions 區段，並視需要進行修改，以在應用程式資訊清單中宣告背景工作。

```xml
<Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="BackgroundTask.App">
        <uap:VisualElements
          DisplayName="BackgroundTask"
          Square150x150Logo="Assets\StoreLogo-sdk.png"
          Square44x44Logo="Assets\SmallTile-sdk.png"
          Description="BackgroundTask"

          BackgroundColor="#00b2f0">
          <uap:LockScreen Notification="badgeAndTileText" BadgeLogo="Assets\smalltile-Windows-sdk.png" />
            <uap:SplashScreen Image="Assets\Splash-sdk.png" />
            <uap:DefaultTile DefaultSize="square150x150Logo" Wide310x150Logo="Assets\tile-sdk.png" >
                <uap:ShowNameOnTiles>
                    <uap:ShowOn Tile="square150x150Logo" />
                    <uap:ShowOn Tile="wide310x150Logo" />
                </uap:ShowNameOnTiles>
            </uap:DefaultTile>
        </uap:VisualElements>

      <Extensions>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
          <BackgroundTasks>
            <Task Type="systemEvent" />
            <Task Type="timer" />
          </BackgroundTasks>
        </Extension>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
          <BackgroundTasks>
            <Task Type="systemEvent"/>
          </BackgroundTasks>
        </Extension>
      </Extensions>
    </Application>
</Applications>
```

## <a name="declare-where-your-background-task-will-run"></a>宣告背景工作執行所在位置

您可以指定背景工作執行所在位置︰

* 預設會在 BackgroundTaskHost.exe 處理序中執行。
* 在前景應用程式所在的相同處理序中。
* 使用 `ResourceGroup` 將多背景工作放在相同的主控處理序，或分別放在不同的處理序。
* 使用 `SupportsMultipleInstances` 在新處理序中執行背景處理程序，這個新的處理序會在每次引發新的觸發程序時取得本身的資源限制 (記憶體、CPU)。

### <a name="run-in-the-same-process-as-your-foreground-application"></a>在前景應用程式所在的那個處理序中執行

以下是宣告背景工作的範例 XML，這個背景工作與前景應用程式在相同處理序中執行。

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="ExecModelTestBackgroundTasks.ApplicationTriggerTask">
        <BackgroundTasks>
            <Task Type="systemEvent" />
        </BackgroundTasks>
    </Extension>
</Extensions>
```

當您指定 **EntryPoint** 時，應用程式會在觸發程序引發時收到對指定之方法的回呼。 如果沒有指定 **EntryPoint**，應用程式則會透過 [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) 收到回呼。  如需詳細資訊，請參閱[建立和註冊同處理序的背景工作](create-and-register-an-inproc-background-task.md)。

### <a name="specify-where-your-background-task-runs-with-the-resourcegroup-attribute"></a>使用 ResourceGroup 屬性來指定背景工作執行所在的位置。

以下是一個範例 XML，當中宣告在 BackgroundTaskHost.exe 處理程序中執行的背景工作，但該處理程序是與其他來自相同 App 的背景工作執行個體不同的執行個體。 注意 `ResourceGroup` 屬性，此屬性可識別哪些背景工作會在一起執行。

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.SessionConnectedTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimeZoneTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="timer" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.ApplicationTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.MaintenanceTriggerTask" ResourceGroup="foobar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```

### <a name="run-in-a-new-process-each-time-a-trigger-fires-with-the-supportsmultipleinstances-attribute"></a>每次觸發程序透過 SupportsMultipleInstances 屬性引發時，在新的處理序中執行

此範例會宣告在新處理序中執行的背景工作，這個新的處理序在每次引發新的觸發程序時取得本身的資源限制 (記憶體、CPU)。 注意啟用此行為的 `SupportsMultipleInstances` 使用方式。 若要使用此屬性中，您必須鎖定 SDK 版本 '10.0.15063' (Windows 10 Creators Update) 或更高版本。

```xml
<Package
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application ...>
            ...
            <Extensions>
                <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask">
                    <BackgroundTasks uap4:SupportsMultipleInstances=“True”>
                        <Task Type="timer" />
                    </BackgroundTasks>
                </Extension>
            </Extensions>
        </Application>
    </Applications>
```

> [!NOTE]
> 您無法連同 `SupportsMultipleInstances` 一起指定 `ResourceGroup` 或 `ServerName`。

## <a name="related-topics"></a>相關主題

* [偵錯背景工作](debug-a-background-task.md)
* [登錄背景工作](register-a-background-task.md)
* [背景工作的指導方針](guidelines-for-background-tasks.md)
