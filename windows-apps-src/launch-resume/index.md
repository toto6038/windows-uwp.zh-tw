---
title: 啟動、繼續和背景工作
description: 本節描述通用 Windows 平台 (UWP) app 在啟動、暫停、繼續及終止時的反應。
ms.assetid: 75011D52-1511-4ECF-9DF6-52CBBDB15BD7
ms.date: 10/04/2017
ms.topic: article
keywords: windows 10，uwp，背景工作，應用程式服務連線的裝置，遠端系統
ms.localizationpriority: medium
ms.openlocfilehash: d12113329381c6602edf87a11fc1cc6b822dab4e
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8481322"
---
# <a name="launching-resuming-and-background-tasks"></a>啟動、繼續和背景工作


本節包含下列各項的相關資訊：

- 通用 Windows 平台 (UWP) 應用程式在啟動、暫停、繼續及終止時的反應。
- 如何使用 URI 或檔案啟用來啟動應用程式。
- 如何使用應用程式服務，這可讓您的通用 Windows 平台 (UWP) 應用程式與其他應用程式共用資料和功能。
- 如何使用背景工作，讓 UWP 應用程式即使本身不在前景中仍能執行。
- 如何探索連線的裝置、啟動另一部裝置上的應用程式，以及與遠端裝置上的應用程式服務通訊，讓您能夠建立在裝置間橫向流動的使用者經驗。
- 如何選擇適當的技術來延伸和組件化您的 App。
- 如何新增和設定應用程式的啟動顯示畫面。
- 如何撰寫程式透過使用者可從 Microsoft Store 安裝的套件延伸您的應用程式。

## <a name="the-app-lifecycle"></a>應用程式週期

本節詳細說明 Windows 10 通用 Windows 平台 (UWP) 應用程式從啟用到關閉為止的整個週期。

| 主題 | 描述 |
|-------|-------------|
| [應用程式週期](app-lifecycle.md)               | 了解 UWP 應用程式的週期，以及當 Windows 啟動、暫停及繼續執行您的應用程式時，會發生什麼情況。 |
| [處理應用程式預先啟動](handle-app-prelaunch.md) | 了解如何處理應用程式預先啟動。                                                                              |
| [處理應用程式啟用](activate-an-app.md)     | 了解如何處理應用程式啟用。                                                                             |
| [處理應用程式暫停](suspend-an-app.md)         | 了解如何在系統暫停您的應用程式時，儲存重要的應用程式資料。                                 |
| [處理應用程式繼續執行](resume-an-app.md)           | 了解如何在系統繼續執行您的應用程式時，重新整理已顯示的內容。                                        |
| [當應用程式移至背景時釋出記憶體](reduce-memory-usage.md) | 了解如何減少應用程式使用的記憶體數量，如此在背景狀態時才不會遭到終止。|
| [透過延長執行延後應用程式暫停](run-minimized-with-extended-execution.md) | 了解如何使用延伸執行來保持您的應用程式在最小化時繼續執行 |

## <a name="launch-apps"></a>啟動應用程式

| 主題 | 描述 |
|-------|-------------|
| [建立通用 Windows 平台主控台應用程式](console-uwp.md) | 了解如何撰寫在主控台視窗中執行的通用 Windows 平台應用程式。 |
| [建立多重執行個體 UWP 應用程式](multi-instance-uwp.md) | 了解如何撰寫多重執行個體通用 Windows 平台應用程式。 |

[使用 URI 啟動應用程式](launch-app-with-uri.md) 一節詳細說明如何使用統一資源識別項 (URI) 啟動應用程式。

| 主題 | 描述 |
|-------|-------------|
| [啟動 URI 的預設應用程式](launch-default-app.md) | 了解如何啟動統一資源識別項 (URI) 的預設應用程式。 URI 可讓您啟動另一個應用程式來執行特定工作。 本主題也提供許多內建於 Windows 之 URI 配置的概觀。 |
| [處理 URI 啟用](handle-uri-activation.md) | 了解如何登錄應用程式，以使它成為統一資源識別項 (URI) 配置名稱的預設處理常式。 |
| [啟動應用程式以取得結果](how-to-launch-an-app-for-results.md) | 了解如何從某個應用程式啟動另一個應用程式，以及在這兩者間交換資料的方式。 這稱為「啟動應用程式以取得結果」。 |
| [使用 ms-tonepicker URI 配置來選擇及儲存音調](launch-ringtone-picker.md) | 本主題說明 ms-tonepicker URI 配置，以及如何使用它來顯示音調選擇器以選取音調、儲存音調，以及取得音調的易記名稱。 |
| [啟動 Windows 設定應用程式](launch-settings-app.md) | 了解如何從您的應用程式啟動 Windows 設定應用程式。 本主題描述 ms-settings URI 配置。 使用此 URI 配置可將 Windows 設定應用程式啟動到特定的設定頁面。 |
| [啟動 Microsoft Store 應用程式](launch-store-app.md) | 本主題描述 ms-windows-store URI 配置。 您的應用程式可以使用此 URI 配置，將 UWP 應用程式啟動到 Microsoft Store 中的特定頁面。 |
| [啟動 Windows 地圖應用程式](launch-maps-app.md) | 了解如何從您的應用程式啟動 Windows 地圖應用程式。 |
| [啟動連絡人應用程式](launch-people-apps.md) | 本主題描述 ms-people URI 配置。 您的應用程式可以使用此 URI 配置來啟動連絡人應用程式，以執行特定動作。 |
| [透過應用程式 URI 處理常式支援網站至應用程式連結](web-to-app-linking.md) | 使用應用程式 URI 處理常式，讓使用者持續使用您的應用程式。 |

[透過檔案啟用啟動應用程式](launch-app-from-file.md) 一節詳細說明如何設定您的應用程式在開啟特定類型的檔案時啟動。

| 主題 | 描述 |
|-------|-------------|
| [啟動檔案的預設應用程式](launch-the-default-app-for-a-file.md) | 了解如何啟動檔案的預設應用程式。 |
| [處理檔案啟用](handle-file-activation.md) | 了解如何登錄您的應用程式，以使它成為某種檔案類型的預設處理常式。 |

請參閱後續內容中與啟動應用程式相關的其他主題。

| 主題 | 描述 |
|-------|-------------|
| [繼續使用者活動，甚至是在各個裝置之間](useractivities.md) | 透過從使用者離開之處啟動您的應用程式，讓使用者重新使用您的應用程式，即使是在不同的裝置上。 |
| [使用自動播放功能來自動啟動](auto-launching-with-autoplay.md) | 當使用者將裝置連接至電腦時，您可以使用「自動播放」，讓您的 app 成為一個選項。 這些裝置包含非磁碟區型裝置 (例如相機或媒體播放裝置) 或磁碟區型裝置 (例如 USB 隨身碟、SD 記憶卡或 DVD)。 |
| [保留檔案和 URI 配置名稱](reserved-uri-scheme-names.md) | 此主題列出並不適用於您的應用程式的保留檔案和 URI 配置名稱。 |

## <a name="app-services-and-extensions"></a>應用程式服務與延伸模組

[應用程式服務與延伸模組](app-services.md)一節說明如何將應用程式服務整合到您的 UWP 應用程式，以允許在應用程式之間共用資料和功能。

| 主題 | 描述 |
|-------|-------------|
| [建立和取用 App 服務](how-to-create-and-consume-an-app-service.md) | 了解如何撰寫可為其他通用 Windows 平台 (UWP) app 提供服務的 UWP app，以及如何取用這些服務。 |
| [轉換 App 服務，以便與其主控 App 在相同處理序中執行](convert-app-service-in-process.md) | 將在個別背景處理序中執行的 app 服務程式碼，轉換成和您 app 服務提供者在相同處理序內執行的程式碼。 |
| [透過應用程式服務、延伸模組和套件延伸您的應用程式](extend-your-app-with-services-extensions-packages.md) | 決定使用哪一種技術來延伸及組件化應用程式，並取得每種技術的簡短概述。 |
| [建立和使用應用程式延伸模組](how-to-create-an-extension.md) | 撰寫和裝載通用 Windows 平台 (UWP) 應用程式延伸模組可讓您透過使用者可從 Microsoft Store 安裝的套件來延伸應用程式。 |

## <a name="background-tasks"></a>背景工作

[背景工作](support-your-app-with-background-tasks.md)一節將示範如何在背景中執行輕量型程式碼來回應觸發程序。

| 主題 | 描述 |
|-------|-------------|
| [背景工作的指導方針](guidelines-for-background-tasks.md)                                       | 確保您的應用程式符合執行背景工作的需求。 |
| [從背景工作存取感應器和裝置](access-sensors-and-devices-from-a-background-task.md)   | [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 可讓您的通用 Windows 應用程式在背景存取感應器和周邊裝置，即使您的前景應用程式已暫停也一樣。 |
| [建立和註冊同處理序背景工作](create-and-register-an-inproc-background-task.md)       | 建立和註冊與前景應用程式在相同處理序中執行的背景工作。 |
| [建立和註冊跨處理序背景工作](create-and-register-a-background-task.md)           | 建立及註冊在與應用程式不同處理序中執行的背景工作，以便在應用程式不在前景時也能執行。 |
| [將跨處理序背景工作移植到同處理序背景工作](convert-out-of-process-background-task.md) | 了解如何跨處理序背景工作移植到您的前景應用程式在相同處理序中執行同處理序背景工作。|
| [偵錯背景工作](debug-a-background-task.md)                                                       | 了解如何偵錯背景工作，包括 Windows 事件記錄檔中的背景工作啟用和偵錯追蹤。 |
| [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md) | 在 App 資訊清單中宣告背景工作為擴充功能，以允許使用背景工作。 |
| [群組背景工作註冊](group-background-tasks.md)                                             | 使用群組隔離背景工作註冊。 |
| [處理已取消的背景工作](handle-a-cancelled-background-task.md)                                 | 了解如何讓可辨識取消要求並停止工作的背景工作，使用永續性儲存體向應用程式回報取消。 |
| [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)       | 了解應用程式如何辨識背景工作的進度與完成度。 |
| [最佳化背景活動](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) |了解如何以減少使用背景能源與互動的背景活動的使用者設定。 |
| [註冊背景工作](register-a-background-task.md)                                                 | 了解如何建立可重複用來安全登錄大多數背景工作的函式。 |
| [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)         | 了解如何建立回應 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 事件的背景工作。 |
| [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)                                    | 了解如何排程一次性的背景工作，或執行定期的背景工作。 |
| [在背景無限期執行](run-in-the-background-indefinetly.md)                                    | 使用功能在背景無限期執行背景工作或延伸執行工作階段。 |
| [從您的應用程式觸發背景工作](trigger-background-task-from-app.md) | 了解如何使用 [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) 從您的應用程式啟動背景工作。|
| [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)             | 了解如何設定條件以控制背景工作的執行時間。 |
| [在背景傳輸資料](https://msdn.microsoft.com/library/windows/apps/mt280377)                 | 使用背景傳輸 API 在背景中複製檔案。 |
| [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)                   | 使用背景工作來更新含有最新內容的應用程式動態磚。 |
| [使用維護觸發程序](use-a-maintenance-trigger.md)                                                   | 了解如何在裝置使用 AC 電源時，使用 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) 類別於背景中執行輕量型程式碼。 |

## <a name="remote-systems"></a>遠端系統

[已連線的應用程式和裝置 (專案 Rome)](connected-apps-and-devices.md) 一節說明如何使用遠端系統平台來探索遠端裝置、啟動遠端裝置上的應用程式，以及與遠端裝置上的應用程式服務通訊。

| 主題 | 說明 |
|-------|-------------|
| [探索遠端裝置](discover-remote-devices.md)  | 了解如何探索能夠連線的裝置。 |
| [啟動遠端裝置上的應用程式](launch-a-remote-app.md) | 了解如何啟動遠端裝置上的應用程式。  |
| [與遠端應用程式服務通訊](communicate-with-a-remote-app-service.md) | 了解如何與遠端裝置上的 app 互動。 |
| [透過遠端工作階段連接裝置](remote-sessions.md) | 將裝置加入遠端工作階段，以建立跨多部裝置的共用體驗。 |

## <a name="splash-screens"></a>啟動顯示畫面

[啟動顯示畫面](splash-screens.md)一節說明如何設定應用程式的啟動顯示畫面。

| 主題 | 描述 |
|-------|-------------|
| [新增啟動顯示畫面](add-a-splash-screen.md) | 設定 app 的啟動顯示畫面影像與背景色彩 |
| [延長顯示啟動顯示畫面](create-a-customized-splash-screen.md) | 您可以為 app 建立延長式啟動顯示畫面，讓啟動顯示畫面的顯示時間變長。 這個延長的畫面是模仿您 app 啟動時所顯示的啟動顯示畫面，您可以自訂這個畫面。 |
