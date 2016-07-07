---
author: TylerMSFT
title: "偵錯背景工作"
description: "了解如何偵錯背景工作，包括 Windows 事件記錄檔中的背景工作啟用和偵錯追蹤。"
ms.assetid: 24E5AC88-1FD3-46ED-9811-C7E102E01E9C
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 1b49c3f06456eef2f3e56227ecb5af5fc991fb13

---

# 偵錯背景工作


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [Windows.ApplicationModel.Background](https://msdn.microsoft.com/library/windows/apps/br224847)

了解如何偵錯背景工作，包括 Windows 事件記錄檔中的背景工作啟用和偵錯追蹤。

本主題假設您已經有需要偵錯背景工作的應用程式。

## 請確定已正確設定背景工作專案


-   在 C# 與 C++ 中，確定主要專案參照背景工作專案。 如果此參照未就緒，背景工作將不會包含在應用程式套件中。
-   在 C\# 與 C++ 中，確定背景工作專案的 \[輸出類型\] 為「Windows 執行階段元件」。
-   背景類別必須在套件資訊清單的進入點屬性中進行宣告。

## 手動觸發背景工作以偵錯背景工作程式碼


您可以透過 Microsoft Visual Studio 手動觸發背景工作。 然後就可以逐步檢查程式碼並進行偵錯。

1.  在 C# 背景類別的 Run 方法中設置中斷點，和/或使用 [**System.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/xaml/hh441592.aspx) 編寫偵錯輸出。

    在 C++ 背景類別的 Run 函式中設置中斷點，和/或使用 [**OutputDebugString**](https://msdn.microsoft.com/library/windows/desktop/aa363362) 編寫偵錯輸出。

2.  在偵錯工具中執行您的應用程式，然後使用 \[週期事件\] 工具列中的暫停下拉式功能表來觸發背景工作。 這個下拉式清單會顯示可由 Visual Studio 啟用的背景工作名稱。

    若要能夠運作，背景工作必須已經註冊且必須仍在等候觸發程序。 例如，如果背景工作是以一次性的 TimeTrigger 註冊，且該觸發程序已經引發，則透過 Visual Studio 啟動工作將不會有作用。

    
            **注意** 使用 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) 或 [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) 的背景工作以及使用有 [**SmsReceived**](https://msdn.microsoft.com/library/windows/apps/br224839) 觸發程序類型的 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) 的背景工作，都無法以這個方式啟用。     

    ![偵錯背景工作](images/debugging-activation.png)

3.  當背景工作啟用時，偵錯工具會連結到工作，並在 VS 顯示偵錯輸出。

## 偵錯背景工作啟用


背景工作啟用必須仰賴下列三件正確符合要求的項目： 此程序說明如何檢查並確定這些項目皆符合要求。

-   背景工作類別的名稱與命名空間
-   在套件資訊清單中指定的進入點屬性
-   您的 app 在註冊背景工作時指定的進入點

1.  使用 Visual Studio 記下背景工作的進入點：

    -   在 C# 與 C++ 中，記下背景工作專案中所指定的背景工作類別的名稱與命名空間。

2.  使用資訊清單設計工具來檢查已在套件資訊清單中正確宣告背景工作：

    -   在 C# 與 C++ 中，進入點屬性必須符合類別名稱後的背景工作命名空間。 例如：RuntimeComponent1.MyBackgroundTask。
    -   也必須指定與工作搭配使用的所有觸發程序類型。
    -   除非您使用 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) 或 [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543)，否則「切勿」指定可執行檔。

3.  僅 Windows。 查看 Windows 使用的進入點來啟動背景工作、啟用偵錯追蹤，以及使用 Windows 事件記錄檔。

    如果您遵照此程序但事件日誌顯示背景工作發生錯誤進入點或觸發程序，表示您的 app 未正確註冊背景工作。 如需此工作的協助，請參閱[註冊背景工作](register-a-background-task.md)。

    1.  移至 \[開始\] 畫面並搜尋 eventvwr.exe，開啟事件檢視器。
    2.  在事件檢視器中，移至 \[應用程式及服務記錄檔\] \[Microsoft\] \[Windows\] \[BackgroundTaskInfrastructure\]。
    3.  在動作窗格中，選取 \[檢視\] \[顯示分析與偵錯記錄檔\]，啟用診斷記錄。
    4.  選取 \[診斷記錄檔\]，然後按一下 \[啟用記錄\]。
    5.  現在嘗試使用應用程式再次註冊並啟動背景工作。
    6.  檢視診斷記錄檔，以取得詳細的錯誤資訊。 當中包含為背景工作註冊的進入點。

![背景工作偵錯資訊的事件檢視器](images/event-viewer.png)

## 背景工作和 Visual Studio 套件部署


如果使用背景工作的應用程式是利用 Visual Studio 所部署，且隨後更新了在資訊清單設計工具中指定的版本 (主要和/或次要)，則後續透過 Visual Studio 重新部署應用程式時，可能會使應用程式的背景工作停止。 這可以透過下列方式來修正：

-   執行與套件一起產生的指令碼，使用 Windows PowerShell (而非 Visual Studio) 來部署已更新的應用程式。
-   如果您已經使用 Visual Studio 部署應用程式，而應用程式的背景工作現在停止了，請重新啟動或登出/登入，讓應用程式的背景工作再次運作。
-   您可以選取 \[永遠重新安裝我的封裝\] 偵錯選項，避免在 C# 專案中發生這個狀況。
-   等待 app 準備就緒再進行最終部署，以遞增套件版本 (不要在偵錯期間變更)。

## 備註


-   確認您的 app 會先檢查現有背景工作註冊，以免再次註冊背景工作。 多次登錄相同背景工作會在每次觸發背景工作時多次執行該背景工作，因而造成無法預期的結果。
-   在 Windows 上，如果背景工作需要鎖定畫面存取，請確定先將 app 置於鎖定畫面後，再嘗試偵錯背景工作。 如需為具有鎖定畫面功能的 app 指定資訊清單選項的詳細資訊，請參閱[在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)。
-   背景工作登錄參數都是在登錄時驗證。 如果有任一個登錄參數無效，就會傳回錯誤。 確認您的 app 能夠妥善處理背景工作註冊失敗的狀況；反之，如果 app 依賴有效的驗證物件，嘗試註冊工作之後，可能會當機。

如需使用 VS 偵錯背景工作的詳細資訊，請參閱[如何在 Windows 市集應用程式中觸發暫停、繼續以及背景事件](https://msdn.microsoft.com/library/windows/apps/xaml/hh974425.aspx)。

## 相關主題

* [建立並註冊背景工作](create-and-register-a-background-task.md)
* [註冊背景工作](register-a-background-task.md)
* [在 app 資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [背景工作的指導方針](guidelines-for-background-tasks.md)
* [如何在 Windows 市集 app 觸發暫停、繼續以及背景事件](https://msdn.microsoft.com/library/windows/apps/xaml/hh974425.aspx)
* [利用 Visual Studio 程式碼分析來分析 Windows 的 Windows 市集應用程式的程式碼品質](https://msdn.microsoft.com/library/windows/apps/xaml/hh441471.aspx)

 

 



<!--HONumber=Jun16_HO5-->


