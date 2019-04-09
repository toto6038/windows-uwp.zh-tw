---
title: WNS 通知優先順序
description: 您可以設定通知的各種不同優先順序的描述
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10 uwp、 WinRT API WNS
localizationpriority: medium
ms.openlocfilehash: 2719c3228c95075eb2a940d12b6c91049b67f524
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291785"
---
# <a name="wns-notification-priorities"></a>WNS 通知優先順序
通知的優先順序，以簡單的標頭設為 WNS 張貼訊息，您可以控制在電池敏感的情況下傳遞通知的方式。

## <a name="power-on-windows"></a>在 Windows 上的電源
因為只電池供電的裝置上使用更多使用者，減少電源使用量已成為所有應用程式的標準需求。 如果應用程式會耗用更多的能源比它們所提供的值，則使用者可能會解除安裝應用程式。 雖然 Windows 作業系統在盡可能減少電池的電源使用量，是為了有效地工作的應用程式的責任。 

WNS 優先順序是一種方式移動關閉電池非關鍵性的工作。 WNS 優先順序告訴的系統應該立即傳遞的通知，而且它可以等到裝置插入電源。 使用這些提示，系統將通知傳送它們是最有價值的使用者和應用程式的確切時間。 

## <a name="power-modes-on-the-device"></a>在裝置上的電源模式
每個 Windows 裝置會透過各種不同的電源模式 （電池、 省電模式及費用），以及使用者期望應用程式的不同行為，在不同的電源模式。 當裝置開啟時，應傳遞所有通知。 在電池省電模式中，您應該傳遞最重要的通知。 當裝置插入電源時，可以完成同步處理或非時間關鍵的作業。

Windows 不知道哪一個通知是任何使用者或應用程式，非常重要的因此系統會完全依賴應用程式，以設定其通知的適當優先順序。 

## <a name="priorities"></a>優先順序
有適用於應用程式以傳送推播通知時使用的四種優先順序。 優先權設定個別通知，可讓您選擇的通知需要立即傳遞 （例如 IM 訊息），以及哪些可等候 （例如，請連絡相片更新）。

優先順序是： 

|    Priority    |    使用者覆寫    |    描述    |    範例    |
|----------------|---------------------|-------------------|---------------|
|    高    |    是-使用者可以封鎖從應用程式的所有通知，或可以防止應用程式進行節流處理電池省電模式。    |    必須立即在任何情況時傳遞裝置可以接收通知的最重要通知。 像是 VoIP 電話或重大警示應喚醒裝置歸類到此類別。    |    VoIP 電話，時間-重大警示    |
|    中等    |    是-使用者可以封鎖從應用程式的所有通知，或可以防止應用程式進行節流處理電池省電模式。    |    這些是不是很重要的是，不需要立即發生的事情的項目，但會困擾的使用者，如果不在背景中執行。    |    次要的電子郵件帳戶同步處理，動態磚更新。    |
|    低    |    是-使用者可以封鎖從應用程式的所有通知，或可以防止應用程式進行節流處理電池省電模式。    |    才有意義或背景活動很合理時使用者使用裝置的通知。 這些是快取，而且不會處理直到使用者登入或在其裝置的插頭。    |    連絡人的狀態 （線上/離線）    |
|    非常低     |    否，它無法防止從電池省電模式中進行節流處理的優先順序非常低通知。    |    這是幾乎完全相同，除了使用者以外的低優先順序無法覆寫電池保護裝置原則。 在省電模式中時，將永遠不會傳遞這些通知。    |    同步處理服務的同步處理檔案。    |

請注意，許多應用程式必須在其生命週期的不同優先順序的通知。 因為的優先權會設定每個通知為基礎，這並不是問題。 VoIP 應用程式可以傳送連入呼叫高優先順序通知，然後依照它以低優先順序一個連絡人上線時。 

## <a name="setting-the-priority"></a>設定優先權

通知要求上設定的優先順序透過額外的標頭的 POST 要求， `X-WNS-PRIORITY`。 這是介於 0 到 3 之間整數值對應到優先順序： 

| 優先權名稱 | X WNS 優先順序值 | 預設值： |
|---------------|----------------------|------------------|
| 高 | 1 | 快顯通知 |
| Meduim | 2 | 磚與徽章 |
| 低 | 3 | 原始 |
| 非常低 | 4 |  |

若要是為了與舊版相容，優先順序不需要設定。 如果應用程式不會設定其通知的優先順序，系統會提供預設優先權。 預設值會顯示在上方的圖表，而且符合現有版本 Windows 的行為。 

## <a name="detailed-listing-of-desktop-behavior"></a>桌面的行為的詳細的清單 

如果您跨許多不同的 Sku 的 Windows 來傳送您的應用程式，最好是通常遵循上一節中的圖表。 

針對每個優先順序更特定的建議的行為如下所示。 這並不保證將完全根據圖表工作之每個裝置。 Oem 可自行設定的行為不同，但是大部分都是接近此圖表。 

| 裝置狀態    | 優先順序：高    |    優先順序：中等        | 優先順序：低    |    優先順序：非常低    |
|-------------------------------------------------------|----------------------------------------------------|----------------------------------------------------|----------------------------------------------------|--------------------------|
|    螢幕上或插入    |    傳遞    |    傳遞    |    傳遞    |    傳遞    |
|    螢幕關閉和電池    |    傳遞    |    如果使用者豁免： 傳遞 Else： 批次     |    如果使用者豁免： 傳遞 Else： 快取 *    |    快取    |
|    啟用省電模式    |    如果使用者豁免： 傳遞 Else： 快取    |    如果使用者豁免： 傳遞 Else： 快取    |    如果使用者豁免： 傳遞 Else： 快取    |    快取     |
|    電池 + 省電模式啟用 + 畫面    |    如果使用者豁免： 傳遞 Else： 快取    |    如果使用者豁免： 傳遞 Else： 快取    |    如果使用者豁免： 傳遞 Else： 快取    |    快取    |

請注意，預設會針對螢幕關閉傳遞低優先順序服務通知和僅適用於 Windows Phone 的電池架構的裝置。 這是為了維護與預先存在的 MPNS 原則的相容性。 也請注意，第四個和第五個資料列都相同，只會呼叫不同的案例。

若要豁免省電模式中的應用程式，使用者必須移至 「 電池使用方式的應用程式 」 設定中，並選取 「 執行背景工作的允許應用程式 」。 此使用者選取豁免不高、 中度和低優先順序服務通知省電模式的應用程式。 您也可以呼叫[BackgroundExecutionManager API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_)向以程式設計方式取得使用者的權限。  

## <a name="related-topics"></a>相關主題
- [Windows 推播通知服務 (WNS) 概觀](windows-push-notification-services--wns--overview.md)
- [若要在背景中執行的要求權限](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_)
