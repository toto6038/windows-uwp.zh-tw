---
author: mcleanbyron
ms.assetid: 772DEBF2-1578-4330-9C14-70BCC6F55005
description: "Microsoft 提供廣告流量分配支援，可透過為來自多個廣告網路的橫幅和要求進行流量分配，讓您的應用程式內廣告獲得最佳收益。"
title: "使用廣告流量分配來獲得最佳收益"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c0669e35b285ee7dfeda0c039d8455a4237960f5

---

#  使用廣告流量分配來獲得最佳收益


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Microsoft 提供廣告流量分配支援，可透過為來自多個廣告網路的橫幅和要求進行流量分配，讓您的應用程式內廣告獲得最佳收益。 不同的廣告網路可能會有自己的優點，有一些有較高的有效千次曝光費用 (eCPM)，或是在特定市場中有比其他廣告網路更高的投放率 (在您的 app 要求時所提供的廣告百分比)。 使用單一廣告網路時，您最後可能會有一堆未獲得投放的廣告要求，導致您損失潛在的收益。 廣告流量分配可藉由確定一律會顯示即時廣告，協助您獲得最佳收益。

廣告流量分配支援是透過 [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk) 而提供的。 在您下載 SDK 並將廣告流量分配者控制項新增到應用程式之後，就能在開發人員中心更新流量分配組態，以指定每個網路的使用頻率。 您可以依市場進行最佳化，以便在適當的地區使用最有效的廣告網路。 而且，您可以變更各個廣告網路的使用方式，而不必重新發佈您的 app。

## 開始使用廣告流量分配


請依照下列步驟來安裝和設定 app 中的廣告流量分配：

1.  檢閱廣告流量分配支援的廣告網路和專案類型清單、為您想要使用的廣告網路設定帳戶，然後依照來自每個廣告網路的指示來將 app 上架。 如需詳細資訊，請參閱[選取和管理廣告網路](select-and-manage-your-ad-networks.md)。

2.  使用 Visual Studio 2015 或 Visual Studio 2013 安裝 [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk)。

3.  在 Visual Studio 中，開啟您的專案或建立新專案。 開啟要裝載廣告的頁面，並將 **AdMediatorControl** 拖曳到該頁面。 如果您使用 Microsoft Advertising，請調整控制項的高度與寬度以符合支援的廣告大小。 如需詳細資訊，請參閱[新增和使用廣告流量分配者控制項](add-and-use-the-ad-mediator-control.md)。

4.  執行 [已連接服務]**** 來選擇您想要設為目標的廣告網路，並為每個廣告網路設定必要參數。 如需詳細資訊，請參閱[新增和使用廣告流量分配者控制項](add-and-use-the-ad-mediator-control.md)。

5.  在您的 app 中測試廣告流量分配實作。 如需詳細資訊，請參閱[測試您的廣告流量分配實作](test-your-ad-mediation-implementation.md)。

6.  將應用程式送出至 Windows 開發人員中心儀表板，並在儀表板中設定您的廣告流量分配設定。 如需詳細資訊，請參閱[送出您的應用程式並設定廣告流量分配](submit-your-app-and-configure-ad-mediation.md)。

7.  在開發人員中心儀表板上檢閱廣告流量分配報告。 如需詳細資訊，請參閱[廣告流量分配報告](https://msdn.microsoft.com/library/windows/apps/mt148521)。

## 在不使用廣告流量分配的情況下使用 Microsoft Advertising


如果您不想使用廣告流量分配，或者廣告流量分配目前不支援您的專案類型，您仍然可以提供來自 Microsoft 的橫幅廣告，而不需使用廣告流量分配。 如需詳細資訊，請參閱 [XAML 和 .NET 中的 AdControl](https://msdn.microsoft.com/library/mt313186.aspx) 和 [HTML 5 和 JavaScript 中的 AdControl](https://msdn.microsoft.com/library/mt313130.aspx)。

## 相關主題

* [選取和管理廣告網路](select-and-manage-your-ad-networks.md)
* [新增和使用廣告流量分配控制項](add-and-use-the-ad-mediator-control.md)
* [測試您的廣告流量分配實作](test-your-ad-mediation-implementation.md)
* [送出您的 App 並設定廣告流量分配](submit-your-app-and-configure-ad-mediation.md)
* [疑難排解廣告流量分配](troubleshoot-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


