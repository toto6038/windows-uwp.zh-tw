---
author: mcleanbyron
ms.assetid: 9ca1f880-2ced-46b4-8ea7-aba43d2ff863
description: "了解 Microsoft Store Services SDK 中 Microsoft Advertising 程式庫之目前版本的已知問題。"
title: "Microsoft Advertising 程式庫的已知問題"
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: 7d0eeda4deac304fb9b573b6ed206a191f037a3e

---

# Microsoft Advertising 程式庫的已知問題




本主題會列出 Microsoft Store Services SDK (適用於 UWP App)，以及適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK (適用於 Windows 8.1 和 Windows Phone 8.x App) 中 Microsoft Advertising 程式庫之目前版本的已知問題。

## 需要適用於通用 Windows App 的 Visual Studio Tools 才能安裝 Microsoft Store Services SDK

若要使用 Visual Studio 2015 安裝 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)，您必須安裝適用於通用 Windows App 的 Visual Studio Tools 1.1 版或更新版本。 如需詳細資訊，請參閱 Visual Studio [版本資訊](http://go.microsoft.com/fwlink/?LinkID=624516)。

## Windows Phone 8.x Silverlight 專案

適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK 對於 Windows Phone 8.x Silverlight 專案的支援有限。 如需詳細資訊，請參閱[在您的 App 中顯示廣告](display-ads-in-your-app.md#silverlight_support)。

若要取得適用於 Windows Phone 8.x Silverlight 專案的 Microsoft Advertising 組件，請安裝[適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)，在 Visual Studio 中開啟您的專案，然後移至 [專案]**** > [加入已連接服務]**** > [Ad Mediator]**** 以自動下載組件。 完成後，若您不想使用廣告流量分配，請從您的專案中移除 Ad Mediator 參考。 如需詳細資訊，請參閱 [Windows Phone Silverlight 中的 AdControl](adcontrol-in-windows-phone-silverlight.md)。

## AdControl 介面於 XAML 中為未知

[AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 的 XAML 標記可能會不正確地顯示藍色曲線，意指該介面為未知。 這只會在以 x86 為目標時發生，並可以忽略它。

## 來自先前廣告要求的 lastError

如果有來自先前廣告要求的剩餘 **lastError**，事件可能會在下一個廣告呼叫期間被觸發兩次。 雖然仍然會做出新的廣告要求，並可能產生有效的廣告，此行為可能會造成混淆。

## 手機上的插入式廣告和瀏覽按鈕

在具有軟體 [返回]****、[開始]**** 及 [搜尋]**** 按鈕的手機 (或模擬器) 上，插入式廣告影片的倒數計時器和點選按鈕可能會被遮住。

## 最近建立的廣告未被提供到您的 App

如果您最近有建立廣告 (一天之內)，它可能無法立即可用。 如果廣告的編輯內容已受到核准，則會在廣告伺服器處理完畢，且該廣告能做為詳細目錄提供時提供。

## 您的 App 中沒有顯示廣告

有很多原因會使您看不見廣告，包括網路錯誤。 其他原因可能包含：

* 在 Windows 開發人員中心選取大小大於或小於您 App 程式碼之 **AdControl** 大小的廣告單位。

* 如果您在執行實際 App 時使用[測試模式值](test-mode-values.md)做為您的廣告單位識別碼，則廣告將不會出現。

* 如果您在過去半個小時之內建立新的廣告單位識別碼，在伺服器將新資料傳播至整個系統之前，您可能看不見廣告。 先前已顯示過廣告的現有識別碼應該會立即顯示廣告。

如果您可以在 App 中看見測試廣告，便代表您的程式碼運作正常並可以顯示廣告。 如果您遭遇到問題，請連絡[產品支援](https://go.microsoft.com/fwlink/p/?LinkId=331508)。 在該頁面上，請選擇 [App 內廣告]****。

您也可以在[論壇](http://go.microsoft.com/fwlink/p/?LinkId=401266)中張貼問題。

## 您的 App 中顯示測試廣告而不是實際廣告

就算您是預期實際廣告，仍有可能會顯示測試廣告。 這可能會在下列案例中發生：

* Microsoft Advertising 無法驗證或找不到在應用程式市集中使用的實際應用程式識別碼。 在此情況下，當使用者建立廣告單位時，廣告單位的狀態一開始可能會是實際運作 (非測試)，但將會在第一次廣告要求後的 6 小時內移至測試狀態。 如果在 10 天內沒有任何來自測試 App 的要求，它將會變更回實際運作的狀態。

* 側載 App 或在模擬器中執行的 App 將不會顯示實際廣告。

當實際廣告單位正在提供測試廣告時，廣告單位的狀態會在 Windows 開發人員中心中顯示 [作用中並正在提供測試廣告]****。 目前這並不適用於手機 App。

## 廣告單位識別碼和應用程式識別碼的過時測試值已無法運作

下列適用於 Windows Phone Silverlight App 的測試值已經過時，並已無法運作。 如果您的現有專案中有使用這些測試值，請使用[測試模式值 (英文)](test-mode-values.md) 中提供的值來更新您的專案。

| 應用程式識別碼  |  廣告單位識別碼    |
|-----------------|----------------|
| test_client     |  Image320_50   |
| test_client     |  Image300_50   |
| test_client     |  TextAd   |
| test_client     |  Image480_80   |

<span id="reference_errors"/>
## 專案中因目標為 [任何 CPU] 所造成的參考錯誤

使用 Microsoft Advertising 程式庫時，您在專案中將無法以 [任何 CPU]**** 為目標。 如果您的專案以 [任何 CPU]**** 平台為目標，您在新增類似下列的參照之後可能會看見警告。

![referenceerror\-solutionexplorer](images/13-19629921-023c-42ec-b8f5-bc0b63d5a191.jpg)

如果要移除這項警告，請將您的專案更新成使用架構特定的建置輸出 (例如 **x86**)。 使用 [組態管理員]**** 來針對偵錯和發行組態設定平台目標。

![configurationmanagerwin10](images/13-87074274-c10d-4dbd-9a06-453b7184f8de.png)

當您針對市集提交建立應用程式套件 (如下列影像所示)，請務必包含您想要做為目標的架構。 如果您想要在 x64 OS 上執行 x86 組建，您可以選擇略過 x64。

![projectstorecreateapppackages](images/13-a99b05a4-8917-4c53-822e-2548fadf828a.png)

![createapppackages](images/13-16280cb1-a838-42b9-9256-eac7f33f5603.png)

## JavaScript/HTML App 中的 Z 軸順序

JavaScript/HTML App 不能將元素置於 Z 軸順序的保留 MAX-10 範圍內。 唯一的例外是插斷覆疊，例如 Skype App 的輸入呼叫通知。

<span id="bkmk-ui"/>
## 請不要使用邊界。

設定由 **AdControl** 從其父類別繼承的邊界相關屬性，將會造成廣告位置錯誤。

## 其他資訊


如需最新已知問題的詳細資訊，或是張貼 Microsoft Advertising 程式庫的相關問題，請造訪[論壇](http://go.microsoft.com/fwlink/p/?LinkId=401266)。

## 支援


若要針對 Microsoft Advertising 程式庫的問題連絡產品支援，請造訪[支援頁面](https://go.microsoft.com/fwlink/p/?LinkId=331508)並選擇 [App 內廣告]****。

 

 



<!--HONumber=Sep16_HO2-->


