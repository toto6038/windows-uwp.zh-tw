---
author: TylerMSFT
title: "啟動、繼續和背景工作"
description: "本節描述通用 Windows 平台 (UWP) app 在啟動、暫停、繼續及終止時的反應。"
ms.assetid: 75011D52-1511-4ECF-9DF6-52CBBDB15BD7
translationtype: Human Translation
ms.sourcegitcommit: a21b2e9bb41e951660916bbbdb09b0bd3e5ecf2d
ms.openlocfilehash: 7667cfb9671a7517a394f6f691aef4c305c02087

---

# <a name="launching-resuming-and-background-tasks"></a>啟動、繼續和背景工作

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本節包含下列各項的相關資訊：

- 通用 Windows 平台 (UWP) app 在啟動、暫停、繼續及終止時的反應。
- 如何使用 URI 或檔案啟用來啟動 app。
- 如何使用 App 服務，這可讓您的通用 Windows 平台 (UWP) app 與其他 app 共用資料和功能。
- 如何使用背景工作，讓 UWP app 即使本身不在前景中仍能執行。
- 如何探索連線的裝置、啟動另一部裝置上的 app，以及與遠端裝置上的 app 服務通訊，讓您能夠建立在裝置間橫向流動的使用者經驗。
- 如何新增和設定 app 的啟動顯示畫面。

## <a name="the-app-lifecycle"></a>App 週期

本節詳細說明 Windows 10 通用 Windows 平台 (UWP) app 從啟用到關閉為止的整個週期。

| 主題 | 說明 |
|-------|-------------|
| [App 週期](app-lifecycle.md)               | 了解 UWP app 的週期，以及當 Windows 啟動、暫停及繼續執行您的 app 時，會發生什麼情況。 |
| [處理 app 預先啟動](handle-app-prelaunch.md) | 了解如何處理 app 預先啟動。                                                                              |
| [處理 app 啟用](activate-an-app.md)     | 了解如何處理 app 啟用。                                                                             |
| [處理 app 暫停](suspend-an-app.md)         | 了解如何在系統暫停您的 app 時，儲存重要的應用程式資料。                                 |
| [處理 app 繼續執行](resume-an-app.md)           | 了解如何在系統繼續執行您的 app 時，重新整理已顯示的內容。                                        |
| [當 app 移至背景時釋出記憶體](reduce-memory-usage.md)           | 了解如何減少 app 使用的記憶體量，如此處於背景狀態時才不會遭到終止。                                        |

## <a name="launch-apps"></a>啟動 App

[使用 URI 啟動 App](launch-app-with-uri.md) 一節詳細說明如何使用統一資源識別項 (URI)，從一個 app 中啟動另一個 app。

| 主題 | 說明 |
|-------|-------------|
| [啟動 URI 的預設 App](launch-default-app.md) | 了解如何啟動統一資源識別項 (URI) 的預設 app。 URI 可讓您啟動另一個 app 來執行特定工作。 本主題也提供許多內建於 Windows 之 URI 配置的概觀。 |
| [處理 URI 啟用](handle-uri-activation.md) | 了解如何登錄 app，以使它成為統一資源識別項 (URI) 配置名稱的預設處理常式。 |
| [啟動 App 以取得結果](how-to-launch-an-app-for-results.md) | 了解如何從某個 app 啟動另一個 app，以及在這兩者間交換資料的方式。 這稱為「啟動 app 以取得結果」。 |
| [使用 ms-tonepicker URI 配置來選擇及儲存音調](launch-ringtone-picker.md) | 本主題說明 ms-tonepicker URI 配置，以及如何使用它來顯示音調選擇器以選取音調、儲存音調，以及取得音調的易記名稱。 |
| [啟動 Windows 設定 App](launch-settings-app.md) | 了解如何從您的 app 啟動 Windows 設定 app。 本主題描述 ms-settings URI 配置。 使用此 URI 配置，可將 Windows 設定 app 啟動到特定的設定頁面。 |
| [啟動 Windows 市集應用程式](launch-store-app.md) | 本主題描述 ms-windows-store URI 配置。 您的 App 可以使用此 URI 配置，將 Windows 市集應用程式啟動到市集中的特定頁面。 |
| [啟動 Windows 地圖 App](launch-maps-app.md) | 了解如何從您的 app 啟動 Windows 地圖 app。 |
| [啟動連絡人 App](launch-people-apps.md) | 本主題描述 ms-people URI 配置。 您的 app 可以使用此 URI 配置來啟動連絡人 app，以執行特定動作。 |
| [透過 App URI 處理常式支援網站至 App 連結](web-to-app-linking.md) | 使用 app URI 處理常式，讓使用者持續使用您的 app。 |

[透過檔案啟用啟動 App](launch-app-from-file.md) 一節詳細說明如何設定您的 app 在開啟特定類型的檔案時啟動。

| 主題 | 說明 |
|-------|-------------|
| [啟動檔案的預設 App](launch-the-default-app-for-a-file.md) | 了解如何啟動檔案的預設 app。 |
| [處理檔案啟用](handle-file-activation.md) | 了解如何登錄您的 app，以使它成為某種檔案類型的預設處理常式。 |

請參閱後續內容中與啟動 app 相關的其他主題。

| 主題 | 說明 |
|-------|-------------|
| [保留檔案和 URI 配置名稱](reserved-uri-scheme-names.md) | 此主題列出您的 app 無法使用的保留檔案和 URI 配置名稱。 |
| [使用自動播放功能來自動啟動](auto-launching-with-autoplay.md) | 當使用者將裝置連接至電腦時，您可以使用「自動播放」，讓您的 app 成為一個選項。 這些裝置包含非磁碟區型裝置 (例如相機或媒體播放裝置) 或磁碟區型裝置 (例如 USB 隨身碟、SD 記憶卡或 DVD)。 |

## <a name="app-services"></a>App 服務

[App 服務](app-services.md)一節說明如何將 app 服務整合到您的 UWP app，以允許在 app 之間共用資料和功能。

| 主題 | 說明 |
|-------|-------------|
| [建立和取用 App 服務](how-to-create-and-consume-an-app-service.md) | 了解如何撰寫可為其他通用 Windows 平台 (UWP) app 提供服務的 UWP app，以及如何取用這些服務。 |
| [轉換 App 服務，以便與其主控 App 在相同處理序中執行](convert-app-service-in-process.md) | 將在個別背景處理序中執行的 app 服務程式碼，轉換成和您 app 服務提供者在相同處理序內執行的程式碼。 |

## <a name="background-tasks"></a>背景工作

[背景工作](support-your-app-with-background-tasks.md)一節將示範如何在背景中執行輕量型程式碼來回應觸發程序。

| 主題 | 說明 |
|-------|-------------|
| [從背景工作存取感應器和裝置](access-sensors-and-devices-from-a-background-task.md)       | [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 可讓您的通用 Windows app 在背景存取感應器和周邊裝置，即使您的前景 app 已暫停也一樣。 |
| [背景工作的指導方針](guidelines-for-background-tasks.md)                                           | 確保您的 app 符合執行背景工作的需求。                                                                                                                          |
| [建立及註冊跨處理序的背景工作](create-and-register-a-background-task.md)                               | 建立及註冊在與 App 不同處理序中執行的背景工作，以便在 App 不在前景時也能執行。                                                                                                 |
| [建立及註冊同處理序的背景工作](create-and-register-an-inproc-background-task.md)                               | 建立及註冊與前景 App 在相同處理序中執行的背景工作。                                                                                                 |
| [將跨處理序背景工作轉換成同處理序背景工作](convert-out-of-process-background-task.md)                               | 了解如何將跨處理序背景工作轉換成在與前景 App 相同處理序中執行的同處理序背景工作。
| [偵錯背景工作](debug-a-background-task.md)                                                           | 了解如何偵錯背景工作，包括 Windows 事件記錄檔中的背景工作啟用和偵錯追蹤。                                                                        |
| [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md) | 在應用程式資訊清單中，透過宣告背景工作為延伸的方式，啟用它們的使用。                                                                                                       |
| [處理已取消的背景工作](handle-a-cancelled-background-task.md)                                     | 了解如何讓可辨識取消要求並停止工作的背景工作，使用永續性儲存體向 app 回報取消。                                     |
| [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)           | 了解 app 如何辨識背景工作的進度與完成度。                                                                                                                     |
| [登錄背景工作](register-a-background-task.md)                                                     | 了解如何建立可重複用來安全登錄大多數背景工作的函式。                                                                                                  |
| [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)             | 了解如何建立回應 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 事件的背景工作。                                                                         |
| [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)                                        | 了解如何排程一次性的背景工作，或執行定期的背景工作。                                                                                                          |
| [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)                 | 了解如何設定條件以控制背景工作的執行時間。                                                                                                                  |
| [在背景傳輸資料](https://msdn.microsoft.com/library/windows/apps/mt280377)                                           | 使用背景傳輸 API 在背景中複製檔案。                                                                                                                              |
| [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)                       | 使用背景工作來更新含有最新內容的 app 動態磚。                                                                                                                      |
| [使用維護觸發程序](use-a-maintenance-trigger.md)                                                       | 了解如何在裝置使用 AC 電源時，使用 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) 類別於背景中執行輕量型程式碼。                             |

## <a name="remote-systems"></a>遠端系統

[已連線的 App 和裝置 (專案 "Rome")](connected-apps-and-devices.md) 一節說明如何使用遠端系統平台來探索遠端裝置、啟動遠端裝置上的 App，以及與遠端裝置上的 App 服務通訊。

| 主題 | 說明 |
|-------|-------------|
| [探索遠端裝置](discover-remote-devices.md)  | 了解如何探索能夠連線的裝置。 |
| [啟動遠端裝置上的 app](launch-a-remote-app.md) | 了解如何啟動遠端裝置上的 app。  |
| [與遠端應用程式服務通訊](communicate-with-a-remote-app-service.md) | 了解如何與遠端裝置上的 app 互動。 |

## <a name="splash-screens"></a>啟動顯示畫面

[啟動顯示畫面](splash-screens.md)一節說明如何設定應用程式的啟動顯示畫面。

| 主題 | 說明 |
|-------|-------------|
| [新增啟動顯示畫面](add-a-splash-screen.md) | 設定 app 的啟動顯示畫面影像與背景色彩 |
| [延長顯示啟動顯示畫面](create-a-customized-splash-screen.md) | 您可以為 app 建立延長式啟動顯示畫面，讓啟動顯示畫面的顯示時間變長。 這個延長的畫面是模仿您 app 啟動時所顯示的啟動顯示畫面，您可以自訂這個畫面。 |



<!--HONumber=Dec16_HO1-->


