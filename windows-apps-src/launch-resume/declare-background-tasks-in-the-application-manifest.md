---
author: mcleblanc
title: 在 app 資訊清單中宣告背景工作
description: 在應用程式資訊清單中，透過宣告背景工作為延伸的方式，啟用它們的使用。
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
---

# 在應用程式資訊清單中宣告背景工作


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**BackgroundTasks 結構描述**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

在 app 資訊清單中，透過宣告背景工作為延伸的方式，啟用它們的使用。

背景工作必須要在 app 資訊清單中進行宣告，否則您的 app 將無法登錄背景工作 (將會擲回例外狀況)。 此外，在應用程式資訊清單中必須宣告背景工作，才能通過認證。

這個主題假設您已建立了一或多個背景工作類別，而且您的 app 登錄要執行的每一項背景工作以回應至少一個觸發程序。

## 手動新增延伸


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

## 新增背景工作延伸


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

    **注意** 請確認列出您要使用的每一個觸發程序類型，否則背景工作將不會使用未宣告的觸發程序類型進行登錄 ([**Register**](https://msdn.microsoft.com/library/windows/apps/br224772) 方法將會失敗並擲回例外狀況)。

    這個程式碼片段範例表示系統事件觸發程序和推播通知的用法：

    ```xml
                <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
                  <BackgroundTasks>
                    <Task Type="systemEvent" />
                    <Task Type="pushNotification" />
                  </BackgroundTasks>
                </Extension>
    ```

    > **注意** 通常應用程式會在稱為 "BackgroundTaskHost.exe" 的特殊程序中執行。 您可以將 Executable 元素新增至 Extension 元素，讓背景工作能夠在應用程式內容中執行。 Executable 元素只可搭配需要該元素的背景工作使用，例如 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)。    

## 新增其他背景工作延伸


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

## 相關主題

* [偵錯背景工作](debug-a-background-task.md)
* [登錄背景工作](register-a-background-task.md)
* [背景工作的指導方針](guidelines-for-background-tasks.md)


<!--HONumber=May16_HO2-->


