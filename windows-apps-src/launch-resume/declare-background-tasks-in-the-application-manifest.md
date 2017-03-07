---
author: TylerMSFT
title: "在應用程式資訊清單中宣告背景工作"
description: "在應用程式資訊清單中，透過宣告背景工作為延伸的方式，啟用它們的使用。"
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 364edc93c52d3c7c8cbe5f1a85c8ca751eb44b35
ms.lasthandoff: 02/07/2017

---

# <a name="declare-background-tasks-in-the-application-manifest"></a>在應用程式資訊清單中宣告背景工作


\[ 針對 Windows 10 上的 UWP 應用程式更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


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

    **注意**  請確認列出您要使用的每一個觸發程序類型，否則背景工作將不會使用未宣告的觸發程序類型進行登錄 ([**Register**](https://msdn.microsoft.com/library/windows/apps/br224772) 方法將會失敗並擲回例外狀況)。

    這個程式碼片段範例指出系統事件觸發程序和推播通知的用法：

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
    <BackgroundTasks>
        <Task Type="systemEvent" />
        <Task Type="pushNotification" />
    </BackgroundTasks>
</Extension>
```


## <a name="add-additional-background-task-extensions"></a>新增其他背景工作擴充功能

針對每一個由應用程式登錄的額外背景工作類別，請重複步驟 2。

下列範例是取自[背景工作範例]( http://go.microsoft.com/fwlink/p/?linkid=227509)的完整 Application 元素。 這將示範兩種背景工作類別的使用，總共有 3 種觸發程序類型。 請複製這個範例的 Extensions 區段，並視需要修改它，以在應用程式資訊清單中宣告背景工作。

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

## <a name="declare-your-background-task-to-run-in-a-different-process"></a>宣告要在不同處理程序中執行的背景工作

Windows 10 版本 1507 中的新功能可讓您在與 BackgroundTaskHost.exe (背景工作預設在其中執行的處理程序) 不同的處理程序中執行背景工作。  有兩個選項︰在與您前景應用程式相同的處理程序中執行；在與其他來自相同應用程式之背景工作執行個體不同的 BackgroundTaskHost.exe 執行個體中執行。  

### <a name="run-in-the-foreground-application"></a>在前景應用程式中執行

以下是一個範例 XML，當中宣告與前景應用程式在相同處理程序中執行的背景工作。 請注意，`Executable` 屬性：

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="ExecModelTestBackgroundTasks.ApplicationTriggerTask" Executable="$targetnametoken$.exe">
        <BackgroundTasks>
            <Task Type="systemEvent" />
        </BackgroundTasks>
    </Extension>
</Extensions>
```

> [!Note]
> 請只將 Executable 元素與需要它的背景工作 (例如 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)) 搭配使用。  

### <a name="run-in-a-different-background-host-process"></a>在不同的背景主機處理程序中執行

以下是一個範例 XML，當中宣告在 BackgroundTaskHost.exe 處理程序中執行的背景工作，但該處理程序是與其他來自相同 App 的背景工作執行個體不同的執行個體。 請注意 `ResourceGroup` 屬性，此屬性可識別哪些背景工作會一起執行。

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


## <a name="related-topics"></a>相關主題


* [偵錯背景工作](debug-a-background-task.md)
* [登錄背景工作](register-a-background-task.md)
* [背景工作的指導方針](guidelines-for-background-tasks.md)

