---
author: muhsinking
title: 從背景工作存取感應器和裝置
description: DeviceUseTrigger 可讓您的通用 Windows app 在背景存取感應器和周邊裝置，即使您的前景應用程式已暫停也一樣。
ms.assetid: B540200D-9FF2-49AF-A224-50877705156B
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，背景工作
ms.localizationpriority: medium
ms.openlocfilehash: 99f853da53302d4080bfa9462da0ec524e8d2064
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2018
ms.locfileid: "5480838"
---
# <a name="access-sensors-and-devices-from-a-background-task"></a>從背景工作存取感應器和裝置




[**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 可讓您的通用 Windows app 在背景存取感應器和周邊裝置，即使您的前景 App 已暫停也一樣。 例如，根據您的 App 在何處執行而定，它能夠使用背景工作，將資料與裝置或監視感應器同步。 為了協助延長電池使用時間並確保可適當取得使用者同意，[**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 的用法受限於本主題中所述的原則。

若要在背景中存取感應器或周邊裝置，請建立使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 的背景工作。 如需示範如何在電腦上完成這個動作的範例，請參閱[自訂 USB 裝置範例](http://go.microsoft.com/fwlink/p/?LinkId=301975 )。 如需手機上的範例，請參閱[背景感應器範例](http://go.microsoft.com/fwlink/p/?LinkId=393307)。

> [!Important]
> **DeviceUseTrigger** 無法與同處理序背景工作搭配使用。 本主題中的資訊僅適用於在跨處理序中執行的背景工作。

## <a name="device-background-task-overview"></a>裝置背景工作概觀

當使用者已看不到您的 App 時，Windows 會將您的 App 暫停或終止，以回收記憶體和 CPU 資源。 這可讓其他 App 在前景中執行，並減少電池耗電量。 發生這個情況時，如果沒有背景工作的協助，所有進行中的資料事件都將遺失。 Windows 提供背景工作觸發程序 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)，讓您的 App 能夠在背景中於裝置和感應器上安全地執行長時間執行的同步和監視操作，即使您的 App 暫停也一樣。 如需 App 週期的詳細資訊，請參閱[啟動、繼續和背景工作](index.md)。 如需背景工作的詳細資訊，請參閱[使用背景工作支援 App](support-your-app-with-background-tasks.md)。

**注意：** 在通用 Windows 應用程式，同步處理在背景中的裝置需要您的使用者已核准您的應用程式的背景同步處理。 裝置也必須透過作用中的 I/O 連接到電腦或與電腦配對，並允許最多 10 分鐘的背景活動。 本主題後續將說明有關原則強制執行的詳細資訊。

### <a name="limitation-critical-device-operations"></a>限制：重要裝置操作

部分重要裝置操作 (例如長時間執行的韌體更新) 無法使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 來執行。 這類操作只能在電腦上執行，而且只能由使用 [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297315) 且具有特殊權限的 App 來執行。 「*具有特殊權限的 App*」是裝置製造商授權執行這些操作的 App。 裝置中繼資料可用來指定要將哪個應用程式 (如果有的話) 指定為裝置的具有特殊權限的應用程式。 如需詳細資訊，請參閱 [Microsoft Store 裝置應用程式的裝置同步和更新](http://go.microsoft.com/fwlink/p/?LinkId=306619)。

## <a name="protocolsapis-supported-in-a-deviceusetrigger-background-task"></a>DeviceUseTrigger 背景工作中支援的通訊協定/API

使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 的背景工作可讓您的 App 透過許多通訊協定/API 進行通訊，但其中的絕大部分不受系統觸發的背景工作支援。 以下受到通用 Windows app 支援。

| 通訊協定         | 通用 Windows app 中的 DeviceUseTrigger                                                                                                                                                    |
|------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| USB              | ![支援這個通訊協定。](images/ap-tools.png)                                                                                                                                            |
| HID              | ![支援這個通訊協定。](images/ap-tools.png)                                                                                                                                            |
| 藍牙 RFCOMM | ![支援這個通訊協定。](images/ap-tools.png)                                                                                                                                            |
| 藍牙 GATT   | ![支援這個通訊協定。](images/ap-tools.png)                                                                                                                                            |
| MTP              | ![支援這個通訊協定。](images/ap-tools.png)                                                                                                                                            |
| 有線網路    | ![支援這個通訊協定。](images/ap-tools.png)                                                                                                                                            |
| Wi-Fi 網路    | ![支援這個通訊協定。](images/ap-tools.png)                                                                                                                                            |
| IDeviceIOControl | ![DeviceServicingTrigger 支援 IDeviceIOControl](images/ap-tools.png)                                                                                                                       |
| 感應器 API      | ![DeviceServicingTrigger 支援通用感應器 API](images/ap-tools.png) (僅限[通用裝置系列](https://msdn.microsoft.com/library/windows/apps/dn894631)中的感應器) |

## <a name="registering-background-tasks-in-the-app-package-manifest"></a>在應用程式套件資訊清單中登錄背景工作

您的應用程式將能使用可做為背景工作一部分執行的程式碼來執行同步和更新操作。 這個程式碼內嵌於實作 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 的 Windows 執行階段類別中 (或在 JavaScript app 專用的 JavaScript 頁面中)。 若要使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 背景工作，您的應用程式必須在前景應用程式的應用程式資訊清單檔案中加以宣告，就像對系統觸發的背景工作所做的一樣。

在這個應用程式套件資訊清單檔案範例中，**DeviceLibrary.SyncContent** 是使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 之背景工作的進入點。

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" EntryPoint="DeviceLibrary.SyncContent">
    <BackgroundTasks>
      <m2:Task Type="deviceUse" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

## <a name="introduction-to-using-deviceusetrigger"></a>使用 DeviceUseTrigger 的簡介

若要使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)，請依照下列基本步驟進行。 如需背景工作的詳細資訊，請參閱[使用背景工作支援 App](support-your-app-with-background-tasks.md)。

1.  您的 App 會在應用程式資訊清單中登錄它的背景工作，並在實作 IBackgroundTask 的 Windows 執行階段類別中，或者在 JavaScript App 專用的 JavaScript 頁面中，內嵌背景工作程式碼。
2.  當您的 App 啟動時，會建立並設定類型為 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 的觸發程序物件，並儲存觸發程序執行個體以供日後使用。
3.  您的應用程式會檢查先前是否已登錄過該背景工作，如果沒有，即會針對觸發程序登錄此背景工作。 請注意，您的 app 無法針對與此觸發程序相關聯的工作設定條件。
4.  當您的 App 需要觸發背景工作時，必須先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) 以檢查 App 是否能夠要求背景工作。
5.  如果 App 能夠要求背景工作，它就會在裝置觸發程序物件上呼叫 [**RequestAsync**](https://msdn.microsoft.com/library/windows/apps/dn297341) 啟用方法。
6.  您的背景工作無法像其他系統背景工作一樣進行流速控制 (沒有 CPU 時間配額)，但是會降低優先順序來執行，讓前景 App 保持可回應狀態。
7.  Windows 接著將根據符合必要原則的觸發程序類型來驗證，包含要求使用者在啟動背景工作之前先同意該操作。
8.  Windows 會監視系統條件和工作執行階段，若其不再符合必要條件，即會在必要時取消該工作。
9.  當背景工作報告進度或完成時，您的 App 將透過已登錄工作上的進度和已完成的事件來接收這些事件。

**重要**時使用[**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)請考量下列重點：

-   Windows8.1 和 Windows Phone 8.1 中首次引入能夠以程式設計方式觸發使用[**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)的背景工作。

-   更新電腦上的周邊裝置時，Windows 會強制執行某些原則，以確保能取得使用者同意。

-   同步和更新周邊裝置時，會強制執行其他原則，以延長使用者的電池使用時間。

-   Windows 可能會在不再符合某些原則需求時，取消使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 的背景工作，其中包含背景時間量的上限 (實際執行時間)。 使用這些背景工作來與周邊裝置進行互動時，請務必將這些原則需求納入考量。

**提示：** 若要查看這些背景工作的運作方式，請下載範例。 如需示範如何在電腦上完成這個動作的範例，請參閱[自訂 USB 裝置範例](http://go.microsoft.com/fwlink/p/?LinkId=301975 )。 如需手機上的範例，請參閱[背景感應器範例](http://go.microsoft.com/fwlink/p/?LinkId=393307)。
 
## <a name="frequency-and-foreground-restrictions"></a>頻率與前景限制

對於 App 能夠初始操作的頻率並無任何限制，但是 App 一次只能執行一個 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 背景工作的操作 (這不會影響其他類型的背景工作)，而且只能在 App 於前景中執行時初始背景工作。 App 不在前景時，便無法使用 **DeviceUseTrigger** 來初始背景工作。 App 無法在第一個背景工作完成之前，初始第二個 **DeviceUseTrigger** 背景工作。

## <a name="device-restrictions"></a>裝置限制

儘管每個 App 受限於只能登錄並執行一個 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 背景工作，但是裝置 (App 執行所在的裝置) 可能允許多個 App 登錄並執行 **DeviceUseTrigger** 背景工作。 根據裝置而定，所有 App 的 **DeviceUseTrigger** 背景工作總數可能有所限制。 這樣有助於在資源受限的裝置上延長電力。 如需詳細資訊，請參閱下表。

透過單一 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 背景工作，您的 App 可以存取不限數量的周邊裝置或感應器 - 只會受限於先前表格中所列的支援 API 和通訊協定。

## <a name="background-task-policies"></a>背景工作原則

Windows 會在 App 使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 背景工作時強制執行原則。 如果不符合這些原則，可能會取消該背景工作。 使用這類型的背景工作來與裝置或感應器進行互動時，請務必將這些原則需求納入考量。

### <a name="task-initiation-policies"></a>工作初始原則

下表說明哪些工作初始原則會套用至通用 Windows app。

| 原則 | 通用 Windows app 中的 DeviceUseTrigger |
|--------|---------------------------------------------|
| 觸發背景工作時，app 是在前景中執行。 | ![原則套用](images/ap-tools.png) |
| 裝置連接到系統 (或處於無線裝置範圍內)。 | ![原則套用](images/ap-tools.png) |
| 裝置能夠使用支援的裝置周邊 API 來存取 App (適用於 USB、HID、藍牙及感應器等項目的 Windows 執行階段 API)。 如果 App 無法存取裝置或感應器，即會拒絕存取背景工作。 | ![原則套用](images/ap-tools.png) |
| 應用程式所提供的背景工作進入點是在應用程式套件資訊清單中進行登錄。 | ![原則套用](images/ap-tools.png) |
| 每個 App 只有一個執行中的 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337) 背景工作。 | ![原則套用](images/ap-tools.png) |
| 裝置 (App 執行所在的裝置) 上的 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337) 背景工作數量尚未到達上限。 | **電腦裝置系列：** 您可以登錄不限數量的工作，並以平行方式執行這些工作。 **行動裝置系列：** 您可以在 512 MB 裝置上登錄 1 個工作，否則可以登錄 2 個工作，並以平行方式執行這些工作。 |
| 使用支援的 API/通訊協定時，App 可以從單一 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337) 背景工作存取的周邊裝置或感應器的數量上限。 | 沒有限制 |
| 當螢幕鎖定時，背景工作每分鐘會耗用 400 毫秒的 CPU 時間 (假設 1GHz CPU)，若螢幕未鎖定，則是每 5 分鐘會耗用相同的 CPU 時間。 無法符合這個原則會導致工作取消。 | ![原則套用](images/ap-tools.png) |

### <a name="runtime-policy-checks"></a>執行階段原則檢查

Windows 會在工作於背景中執行的同時，強制執行下列執行階段原則需求。 如果符合任何執行階段需求停止的情況，Windows 將取消您的裝置背景工作。

下表說明哪些執行階段原則會套用至通用 Windows app。

| 原則檢查 | 通用 Windows app 中的 DeviceUseTrigger |
|--------------|:-------------------------------------------:|
| 裝置連接到系統 (或處於無線裝置範圍內)。 | ![原則檢查套用](images/ap-tools.png) |
| 工作正在對裝置執行正常 I/O (每 5 秒 1 個 I/O)。 | ![原則檢查套用](images/ap-tools.png) |
| 應用程式尚未取消工作。 | ![原則檢查套用](images/ap-tools.png) |
| 實際執行時間限制 – App 的工作可以在背景中執行的總時間。 | **電腦裝置系列：** 10 分鐘。 **行動裝置系列：** 沒有時間限制。 為節省資源，一次只能執行 1 或 2 個工作。 |
| App 尚未結束。 | ![原則檢查套用](images/ap-tools.png) |

## <a name="best-practices"></a>最佳做法

下列為使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 背景工作的 App 的最佳做法。

### <a name="programming-a-background-task"></a>進行背景工作的程式設計

使用 App 的 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 背景工作，可在使用者切換 App 且 Windows 暫停您的前景 App 時，確保任何從前景 App 啟動的同步或監視操作可以繼續在背景中執行。 建議您遵循這整個模型來登錄、觸發及解除登錄背景工作：

1.  呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) 以檢查 App 是否能夠要求背景工作。 這必須在登錄背景工作之前完成。

2.  先登錄背景工作，然後再要求觸發程序。

3.  將進度和完成事件處理常式連接到觸發程序。 當 App 從暫停狀態中返回時，Windows 將為 App 提供任何已排入佇列的進度或完成事件，這些事件可用來判斷背景工作的狀態。

4.  當您觸發 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 背景工作時，請關閉所有開啟的裝置或感應器物件，以釋放這些裝置或感應器，讓您的背景工作能夠開啟並加以使用。

5.  登錄觸發程序。

6.  謹慎考量從背景工作存取裝置或感應器對電力所產生的影響。 例如，感應器的報告間隔執行得太過頻繁，可能導致工作經常處於執行狀態中，這會快速耗盡手機的電力。

7.  當背景工作完成時，請取消登錄該工作。

8.  登錄背景工作類別的取消事件。 登錄取消事件會讓背景工作程式碼在 Windows 或前景 App 取消背景工作時，完全停止執行中的背景工作。

9.  在 App 結束 (非暫停) 時，如果 App 不再需要任何執行中的工作，請取消登錄並取消這些工作。 在資源受限的系統上 (例如，記憶體不足的手機)，這能讓其他 App 使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 背景工作。

    -   當 App 結束時，請取消登錄並取消所有執行中的工作。

    -   當 App 結束時，系統將會取消背景工作，並將任何現有的事件處理常式與現有的背景工作中斷連線。 這能讓您不需要判斷背景工作的狀態。 取消登錄並取消背景工作，讓取消程式碼能夠完全停止背景工作。

### <a name="cancelling-a-background-task"></a>取消背景工作

若要從前景 App 取消正在背景中執行的工作，請在您於 App 中使用的 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) 物件上使用 Unregister 方法來登錄 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 背景工作。 使用 **BackgroundTaskRegistration** 上的 [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869) 方法來取消登錄背景工作，將導致背景工作基礎結構取消您的背景工作。

此外，[**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869) 方法會取得布林值的 True 或 False 值，以指出是否應取消目前執行中的背景工作執行個體，而不需讓它們完成執行。 如需詳細資訊，請參閱 **Unregister** 的 API 參考資料。

除了 [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869)，App 也需要呼叫 [**BackgroundTaskDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504)。 這會通知系統，已完成與背景工作相關聯的非同步操作。
