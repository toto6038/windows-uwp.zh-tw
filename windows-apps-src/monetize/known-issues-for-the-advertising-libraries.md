---
author: Xansky
ms.assetid: 9ca1f880-2ced-46b4-8ea7-aba43d2ff863
description: 了解 Microsoft Advertising SDK 目前版本的已知問題。
title: 應用程式內廣告的已知問題與疑難排解
ms.author: mhopkins
ms.date: 04/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, 廣告, 通知, 已知問題, 疑難排解
ms.localizationpriority: medium
ms.openlocfilehash: 1ca7949b3092b03500f25249ce1af3832a9e61ba
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2018
ms.locfileid: "5443401"
---
# <a name="known-issues-and-troubleshooting-for-ads-in-apps"></a>應用程式內廣告的已知問題與疑難排解

本主題列出 Microsoft Advertising SDK 目前版本的已知問題。 如需其他疑難排解指導方針，請參閱下列主題。

* [HTML 和 JavaScript 疑難排解指南](html-and-javascript-troubleshooting-guide.md)
* [XAML 和 C# 的疑難排解指南](xaml-and-c-troubleshooting-guide.md)

## <a name="adcontrol-interface-unknown-in-xaml"></a>AdControl 介面於 XAML 中為未知

[AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 的 XAML 標記可能會不正確地顯示藍色曲線，意指該介面為未知。 這只會在以 x86 為目標時發生，並可以忽略它。

## <a name="lasterror-from-previous-ad-request"></a>來自先前廣告要求的 lastError

如果有來自先前廣告要求的剩餘 **lastError**，事件可能會在下一個廣告呼叫期間被觸發兩次。 雖然仍然會做出新的廣告要求，並可能產生有效的廣告，此行為可能會造成混淆。

## <a name="interstitial-ads-and-navigation-buttons-on-phones"></a>手機上的插入式廣告和瀏覽按鈕

在具有軟體 **\[返回\]**、**\[開始\]** 及 **\[搜尋\]** 按鈕的手機 (或模擬器) 上，插播式廣告的倒數計時器和點選按鈕可能會被遮住。

## <a name="recently-created-ads-are-not-being-served-to-your-app"></a>最近建立的廣告未被提供到您的 App

如果您最近有建立廣告 (一天之內)，它可能無法立即可用。 如果廣告的編輯內容已受到核准，則會在廣告伺服器處理完畢，且該廣告能做為詳細目錄提供時提供。

## <a name="no-ads-are-shown-in-your-app"></a>您的 App 中沒有顯示廣告

有很多原因會使您看不見廣告，包括網路錯誤。 其他原因可能包含：

* 在 Windows 開發人員中心選取大小大於或小於您 App 程式碼之 **AdControl** 大小的廣告單位。

* 如果您在執行實際 App 時使用[測試模式值](set-up-ad-units-in-your-app.md#test-ad-units)做為您的廣告單位識別碼，則廣告將不會出現。

* 如果您在過去半個小時之內建立新的廣告單位識別碼，在伺服器將新資料傳播至整個系統之前，您可能看不見廣告。 先前已顯示過廣告的現有識別碼應該會立即顯示廣告。

如果您可以在 App 中看見測試廣告，便代表您的程式碼運作正常並可以顯示廣告。 如果您遭遇到問題，請連絡[產品支援](https://developer.microsoft.com/en-us/windows/support)。 在該頁面上，請選擇 **\[應用程式內廣告\]**。

您也可以在[論壇](http://go.microsoft.com/fwlink/p/?LinkId=401266)中張貼問題。

## <a name="test-ads-are-showing-in-your-app-instead-of-live-ads"></a>您的 App 中顯示測試廣告而不是實際廣告

就算您是預期實際廣告，仍有可能會顯示測試廣告。 這可能會在下列案例中發生：

* 我們的廣告平台無法驗證或找不到在 Microsoft Store 中使用的實際應用程式識別碼。 在此情況下，當使用者建立廣告單位時，廣告單位的狀態一開始可能會是實際運作 (非測試)，但將會在第一次廣告要求後的 6 小時內移至測試狀態。 如果在 10 天內沒有任何來自測試 App 的要求，它將會變更回實際運作的狀態。

* 側載 App 或在模擬器中執行的 App 將不會顯示實際廣告。

當實際廣告單位正在提供測試廣告時，廣告單位的狀態會在 Windows 開發人員中心中顯示 **\[作用中並正在提供測試廣告\]**。 目前這並不適用於手機 App。


<span id="reference_errors"/>

## <a name="reference-errors-caused-by-targeting-any-cpu-in-your-project"></a>專案中因目標為 [任何 CPU] 所造成的參考錯誤

使用 Microsoft Advertising SDK 時，您在專案中將無法以 **\[任何 CPU\]** 為目標。 如果您的專案以 **\[任何 CPU\]** 平台為目標，您在新增類似下列的參照之後可能會看見警告。

![referenceerror\-solutionexplorer](images/13-19629921-023c-42ec-b8f5-bc0b63d5a191.jpg)

如果要移除這項警告，請將您的專案更新成使用架構特定的建置輸出 (例如 **x86**)。 使用 **\[組態管理員\]** 來針對偵錯和發行組態設定平台目標。

![configurationmanagerwin10](images/13-87074274-c10d-4dbd-9a06-453b7184f8de.png)

當您針對 Microsoft Store 提交建立應用程式套件 (如下列影像所示)，請務必包含您想要做為目標的架構。 如果您想要在 x64 OS 上執行 x86 組建，您可以選擇略過 x64。

![projectstorecreateapppackages](images/13-a99b05a4-8917-4c53-822e-2548fadf828a.png)

![createapppackages](images/13-16280cb1-a838-42b9-9256-eac7f33f5603.png)

## <a name="z-order-in-javascripthtml-apps"></a>JavaScript/HTML App 中的 Z 軸順序

JavaScript/HTML App 不能將元素置於 Z 軸順序的保留 MAX-10 範圍內。 唯一的例外是插斷覆疊，例如 Skype App 的輸入呼叫通知。

<span id="bkmk-ui"/>

## <a name="do-not-use-borders"></a>請不要使用邊界。

設定由 **AdControl** 從其父類別繼承的邊界相關屬性，將會造成廣告位置錯誤。

## <a name="more-information"></a>其他資訊

如需最新已知問題的詳細資訊，或是張貼 Microsoft Advertising SDK 的相關問題，請造訪[論壇](http://go.microsoft.com/fwlink/p/?LinkId=401266)。

 

 
