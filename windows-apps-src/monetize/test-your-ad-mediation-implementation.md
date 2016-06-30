---
author: mcleanbyron
ms.assetid: 54ECD653-7FC2-4A95-AC5A-972C4FB5A54B
description: "提交應用程式之前，建議您先測試您的廣告流量分配實作。"
title: "測試您的廣告流量分配實作"
translationtype: Human Translation
ms.sourcegitcommit: ec7ce299545de8e5c167e1934fb9a0b4f4370948
ms.openlocfilehash: 0805ed5462a4b100b837ed9c11ec2d9e7caabc34

---

# 測試您的廣告流量分配實作


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

提交應用程式之前，建議您先測試您的廣告流量分配實作。

## 使用測試廣告網路設定值進行測試


如果您透過在 Visual Studio 中啟動您專案的 \[已連接服務\]，在沒有輸入廣告網路設定的情況下執行 app，當您在開發電腦 (適用於通用 Windows 平台 (UWP) 和 Windows 8.1 XAML app) 或在模擬器或裝置 (適用於 Windows Phone app) 上執行 app 時，廣告流量分配將自動使用測試設定值。 這樣可以在輸入廣告網路所需的參數之前，讓您快速地測試 app 並確定它的程式碼撰寫正確。

廣告網路會依順序輪替，某一個網路顯示之後再顯示另一個網路，並且每個網路的顯示時間都相同。 請務必耐心等待以執行幾個循環，讓您可以檢視所有廣告網路，以降低可能發生任何暫時性連線問題的機率。

將會為支援這些廣告的廣告網路顯示測試廣告。 請注意，測試廣告有時可能看起來像錯誤。 請務必檢閱您的事件，以判斷是否發生錯誤。

> **注意：**當測試 Windows Phone Silverlight App 時，Google AdMob 一律會傳回「**無效的要求**」錯誤，因為它不會使用測試中繼資料。 若要驗證您的 Google AdMob 實作，您必須輸入所需的參數，如下一節所述。

 

如果您使用了[新增和使用廣告流量分配控制項](add-and-use-the-ad-mediator-control.md)中所示的事件處理程式碼，就會在主控台輸出中顯示所有錯誤。

## 使用您的廣告網路設定值進行測試


使用測試設定資料測試您的應用程式之後，您可以使用用於發佈到 Windows 市集之應用程式版本的廣告網路設定值來測試您的應用程式。

首先，請開啟 \[新增已連接服務\] 視窗 (Visual Studio 2015) 或 \[服務管理員\] 視窗 (Visual Studio 2013) 然後設定每個廣告網路，如新增和使用廣告流量分配控制項中所描述。 為每個廣告網路輸入所需的參數。

現在，您已經準備好測試您的 App。 請務必讓 App 執行夠長的時間，以確認每個廣告網路都可正常顯示廣告。 送出您的 App 之前，請檢查是否有任何的例外狀況並修正程式碼錯誤。

當送出 App 套件到 Windows 開發人員中心儀表板時，您在 Visual Studio 中輸入的設定值會自動填入到 \[利用廣告賺取獲利\] 儀表板頁面。 您可以在 Windows 市集中修改這些值，來設定您應用程式的廣告網路行為。 如需詳細資訊，請參閱[送出您的應用程式並設定廣告流量分配](submit-your-app-and-configure-ad-mediation.md)。

## 相關主題

* [選取和管理廣告網路](select-and-manage-your-ad-networks.md)
* [新增和使用廣告流量分配控制項](add-and-use-the-ad-mediator-control.md)
* [送出您的 App 並設定廣告流量分配](submit-your-app-and-configure-ad-mediation.md)
* [疑難排解廣告流量分配](troubleshoot-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


