---
author: TylerMSFT
title: "啟動、繼續和背景工作"
description: "本節描述通用 Windows 平台 (UWP) app 在啟動、暫停、繼續及終止時的反應。"
ms.assetid: 75011D52-1511-4ECF-9DF6-52CBBDB15BD7
ms.sourcegitcommit: a8e6145f7a5c75d3b37277b80b07b0b3ad739d5c
ms.openlocfilehash: ab20c4af5b9a87dc73775d304c314c9861d989d4

---

# 啟動、繼續和背景工作


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本節描述通用 Windows 平台 (UWP) app 在啟動、暫停、繼續及終止時的反應。 其內容涵蓋如何使用協定或延伸來啟用 app，以及如何使用背景工作，讓 UWP app 即使在不在前景的情況下仍能執行。 最後會說明如何將啟動顯示畫面新增到您的 app。

## App 週期

| 主題                                            | 說明                                                                                                     |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [App 週期](app-lifecycle.md)               | 了解 UWP app 的週期，以及當 Windows 啟動、暫停及繼續執行您的 app 時，會發生什麼情況。 |
| [處理 app 預先啟動](handle-app-prelaunch.md) | 了解如何處理 app 預先啟動。                                                                              |
| [處理 app 啟用](activate-an-app.md)     | 了解如何處理 app 啟用。                                                                             |
| [處理 app 暫停](suspend-an-app.md)         | 了解如何在系統暫停您的 app 時，儲存重要的應用程式資料。                                 |
| [處理 app 繼續執行](resume-an-app.md)           | 了解如何在系統繼續執行您的 app 時，重新整理已顯示的內容。                                        |

 

## 啟動 app


| URI 和檔案啟用                                                                         | 說明                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [啟動 app 以取得結果](how-to-launch-an-app-for-results.md)                               | 了解如何從某個 app 啟動另一個 app，以及在這兩者間交換資料的方式。                                                                                             |
| [啟動 URI 的預設 app](launch-default-app.md)                                      | 了解如何啟動統一資源識別項 (URI) 的預設 app。                                                                                               |
| [處理 URI 啟用](handle-uri-activation.md)                                              | 了解 app 如何透過登錄成為 URI 配置名稱的預設處理常式。                                                                                          |
| [啟動檔案的預設 app](launch-the-default-app-for-a-file.md)                      | 了解如何啟動檔案類型的預設 app。                                                                                                                       |
| [處理檔案啟用](handle-file-activation.md)                                            | 了解您的 app 如何登錄為檔案類型的預設處理常式。                                                                                                  |
| [檔案類型與 URI 的指導方針](https://msdn.microsoft.com/library/windows/apps/hh700321) | 只要了解 UWP app 和所支援之檔案類型和通訊協定間的關係，您就可以為使用者提供更一致、更順暢的使用經驗。 |
| [保留檔案和 URI 配置名稱](reserved-uri-scheme-names.md)                             | 此主題列出並不適用於您的 app 的保留檔案和 URI 配置名稱。                                                                                |
| [啟動 Windows 設定 app](launch-settings-app.md)                                      | 了解如何啟動 Windows 設定 app。                                                                                                                              |
| [啟動 Windows 市集 app](launch-store-app.md)                                            | 了解如何啟動 Windows 市集 app。                                                                                                                                 |
| [啟動 Windows 地圖 app](launch-maps-app.md)                                              | 了解如何啟動 Windows 地圖 app。                                                                                                                                  |

 

## 背景工作和服務



| 主題                                                                                                            | 說明                                                                                                                                                                                   |
|------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [使用背景工作支援 app](support-your-app-with-background-tasks.md)                             | 本節中的主題說明如何藉由以背景工作回應觸發程序，在背景執行您的輕量型程式碼。                                                       |
| [從背景工作存取感應器和裝置](access-sensors-and-devices-from-a-background-task.md)       | [ **DeviceUseTrigger** ](https://msdn.microsoft.com/library/windows/apps/dn297337) 可讓您的通用 Windows 應用程式在背景存取感應器和周邊裝置，即使您的前景 App 已暫停也一樣。 |
| [背景工作的指導方針](guidelines-for-background-tasks.md)                                           | 確保您的 app 符合執行背景工作的需求。                                                                                                                          |
| [建立和使用 app 服務](how-to-create-and-consume-an-app-service.md)                                | 了解如何撰寫可為其他 UWP app 提供服務的 UWP app，以及如何取用這些服務。                                                                                  |
| [建立並註冊背景工作](create-and-register-a-background-task.md)                               | 建立背景工作類別並註冊它，即使您的 app 不在前景也能執行。                                                                                                 |
| [偵錯背景工作](debug-a-background-task.md)                                                           | 了解如何偵錯背景工作，包括 Windows 事件記錄檔中的背景工作啟用和偵錯追蹤。                                                                        |
| [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md) | 在 app 資訊清單中，透過宣告背景工作為延伸的方式，啟用它們的使用。                                                                                                       |
| [處理已取消的背景工作](handle-a-cancelled-background-task.md)                                     | 了解如何讓可辨識取消要求並停止工作的背景工作，使用永續性儲存體向 app 回報取消。                                     |
| [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)           | 了解 app 如何辨識背景工作的進度與完成度。                                                                                                                     |
| [登錄背景工作](register-a-background-task.md)                                                     | 了解如何建立可重複用來安全登錄大多數背景工作的函式。                                                                                                  |
| [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)             | 了解如何建立回應 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 事件的背景工作。                                                                         |
| [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)                                        | 了解如何排程一次性的背景工作，或執行定期的背景工作。                                                                                                          |
| [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)                 | 了解如何設定條件以控制背景工作的執行時間。                                                                                                                  |
| [在背景傳輸資料](https://msdn.microsoft.com/library/windows/apps/mt280377)                                           | 使用背景傳輸 API 在背景中複製檔案。                                                                                                                              |
| [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)                       | 使用背景工作來更新含有最新內容的 app 動態磚。                                                                                                                      |
| [使用維護觸發程序](use-a-maintenance-trigger.md)                                                       | 了解如何在裝置使用 AC 電源時，使用 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) 類別於背景中執行輕量型程式碼。                             |

 

## 新增啟動顯示畫面


所有 UWP app 都必須具備啟動顯示畫面，也就是啟動顯示畫面影像與背景色彩 (兩者都可以自訂) 的組合。

在使用者啟動 app 時，會立即顯示您的啟動顯示畫面。 這會在初始化 app 資源時，提供立即的回饋給使用者。 只要 app 準備好與使用者互動，啟動顯示畫面就會關閉。

設計良好的啟動顯示畫面可讓您的 app 更具吸引力。 這裡有個簡單明瞭的啟動顯示畫面：

![啟動顯示畫面範例中縮放比例 75% 的啟動顯示畫面的螢幕擷取畫面。](images/regularsplashscreen.png)

這個啟動顯示畫面結合了綠色背景色彩與透明的 PNG。

將影像與背景色彩放在一起組成啟動顯示畫面時，不論安裝 app 的裝置為何，都可以讓啟動顯示畫面保持美觀。 啟動顯示畫面顯示時，只會變更背景的大小來適應不同的螢幕大小。 您的影像會永遠保持不變。

此外，您可以使用 [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) 類別自訂 app 的啟動經驗。 您可以放置延長式啟動顯示畫面，建立這個畫面是為了讓您的 app 有更多的時間可以完成其他工作，像是準備 app UI 或是完成網路作業。 您也可以使用 **SplashScreen** 類別，在關閉啟動顯示畫面時通知您，這樣您就可以開始進入動畫。

| 主題                                                                          | 說明                                                                                                                                                                                       |
|--------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [新增啟動顯示畫面](add-a-splash-screen.md)                                 | 設定 app 的啟動顯示畫面影像與背景色彩                                                                                                                                          |
| [延長顯示啟動顯示畫面](create-a-customized-splash-screen.md) | 您可以為 app 建立延長式啟動顯示畫面，讓啟動顯示畫面的顯示時間變長。 這個延長的畫面是模仿您 app 啟動時所顯示的啟動顯示畫面，您可以自訂這個畫面。 |

 

 

 



<!--HONumber=Jun16_HO4-->


