---
Description: 如果您的應用程式使用廣告流量分配或顯示來自 Microsoft Advertising 的橫幅或影片插入式廣告，請使用 [創造營收] &gt; [利用廣告獲利] 頁面來管理廣告的使用方式。
title: 利用廣告獲利
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
---

# 利用廣告獲利


如果您的應用程式使用 **AdMediatorControl**、**AdControl** 或 **InterstitialAd** 控制項來顯示橫幅或影片插入式廣告，請使用 [創造營收]**** &gt; [利用廣告獲利]**** 頁面來管理廣告的使用方式。

## Windows 廣告流量分配


如果您的 app 使用廣告流量分配，請使用這個區段來設定流量分配設定，並針對 app 使用的每個廣告網路新增必要參數。 如需這個區段中選項的詳細資訊，請參閱[送出您的應用程式並設定廣告流量分配](https://msdn.microsoft.com/library/windows/apps/mt219689)。

廣告流量分配支援可讓您透過為來自多個廣告網路的橫幅和要求進行流量分配，讓您的 app 內廣告獲得最佳收益。 如需廣告流量分配的詳細資訊，請參閱[使用廣告流量分配獲得最佳收益](https://msdn.microsoft.com/library/windows/apps/mt219691)。

## COPPA 相容性

基於兒童線上隱私權保護法 (「COPPA」) 的立法宗旨，如果您的 app 是針對 13 歲以下的兒童，則您必須通知 Microsoft。 如果您使用開發人員中心來通知 Microsoft 有關您 app 是針對 13 歲以下的兒童，當傳送廣告到您 app 時，Microsoft 會採取步驟來停用其行為廣告服務。 如果您的 app 是針對 13 歲以下的兒童，基於 COPPA 的規範，您必須承擔某些義務。

如需有關 COPPA 規範義務的詳細資訊，請參閱[本頁](http://go.microsoft.com/fwlink/p/?linkid=536558)。

## Microsoft 聯盟廣告

如果您想要在應用程式中顯示 Microsoft 聯盟廣告，請勾選此區段中的方塊。 如果您勾選此方塊，沒有來自其他廣告網路的廣告可用時，將提供市集中產品的廣告 (包括音樂、遊戲、電影、應用程式、硬體和軟體) 給您的應用程式。 當使用者在指定的屬性視窗內按一下市集中的廣告和匯流排產品時，您就會獲得核可購買項目的佣金。

如果您變更此選取項目，不需要重新發佈您的 app，變更就會生效。 如需 Microsoft 聯盟廣告的詳細資訊，請參閱[關於聯盟廣告](about-affiliate-ads.md)。

> **注意**：如果您的應用程式使用廣告流量分配 (也就是使用 **AdMediatorControl** 來顯示廣告)，則只有在廣告流量分配設定為顯示來自 Microsoft 的廣告時，您的應用程式才可顯示聯盟廣告。

## 社群廣告

如果您想要聯合促銷您的應用程式與其他開發人員的 app，請勾選此區段中的方塊。 如果您勾選此方塊，然後[建立社群廣告活動](create-an-ad-campaign-for-your-app.md)，您的 app 將會顯示其他同樣建立社群廣告活動的開發人員所發佈之 app 的廣告，而他們的 app 的廣告將會顯示在您的 app 中。 社群廣告是免費的，只會在沒有其他廣告網路的廣告可用時顯示。

如果您變更此選取項目，不需要重新發佈您的 app，變更就會生效。 如需社群廣告的詳細資訊，請參閱[關於社群廣告](about-community-ads.md)。

## Microsoft Advertising 廣告單元

使用本區段來建立 Microsoft Advertising 廣告單元。 您只需要在下列案例中建立廣告單元：

-   您的 app 會使用 [AdControl](https://msdn.microsoft.com/library/mt313154.aspx) 物件顯示來自 Microsoft Advertising 的橫幅廣告。
-   您的 app 會使用 [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx) 物件顯示來自 Microsoft Advertising 的影片插入式廣告。

若要為這些案例建立廣告單元：

1.  為廣告單元命名。
2.  選取廣告單元類型 ([橫幅]**** 或 [插入式影片]****)。
3.  選取裝置類型 ([行動裝置]**** 或 [電腦/平板電腦]****)。
4.  按一下 [建立廣告單元]****。

您的廣告單元會出現在此區段底部的表格中。 您會看到每個廣告單元的**應用程式識別碼**和**廣告單元識別碼**。 若要在 app 中顯示廣告，您需要在程式碼中使用這些值：

-   如果您的 app 顯示橫幅廣告，請將這些值指派給 [AdControl](https://msdn.microsoft.com/library/mt313154.aspx) 物件的 [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) 和 [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) 屬性。
-   如果您的 app 顯示影片插入式廣告，請將這些值傳遞到 [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx) 物件的 [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) 方法。

> **注意** 如果您的 app 使用廣告流量分配顯示來自 Microsoft Advertising 的橫幅廣告 (也就是，它會使用 **AdMediatorControl** 物件)，則您不需要求廣告單元。 在這個案例中，系統會自動產生 Microsoft Advertising 廣告單元。

 

 

 


<!--HONumber=Mar16_HO5-->


