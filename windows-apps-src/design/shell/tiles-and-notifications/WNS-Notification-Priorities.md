---
title: WNS 通知優先順序
description: 可在通知上設定之各種優先順序的描述
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10，uwp，WinRT API，WNS
localizationpriority: medium
ms.openlocfilehash: 3310b34b2748bd684e46e04775c973680f8e03a9
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282237"
---
# <a name="wns-notification-priorities"></a>WNS 通知優先順序
藉由使用簡單標頭對 WNS 張貼訊息來設定通知的優先順序，您可以控制在以電池區分的情況下傳遞通知的方式。

## <a name="power-on-windows"></a>Windows 上的電源
當有更多使用者只在備有電池的裝置上工作時，將電源使用量降到最低已成為所有應用程式的標準需求。 如果應用程式耗用的能源超過其所提供的值，使用者可能會卸載應用程式。 雖然 Windows 作業系統會盡可能減少電池的電源使用量，但應用程式必須負責有效率地運作。 

WNS 優先順序是一種將非關鍵性工作移出電池的方式。 WNS 優先順序會告訴系統應該立即傳遞哪些通知，而哪些可以等到裝置插到電源來源為止。 透過這些提示，系統可以將通知傳遞給使用者和應用程式最有價值的確切時間。 

## <a name="power-modes-on-the-device"></a>裝置上的電源模式
每個 Windows 裝置都會透過各種電源模式（電池、電池保護和收費）來運作，而使用者在不同的電源模式下，會預期應用程式有不同的行為。 當裝置開啟時，應該傳遞所有通知。 在電池保護模式中，只應傳遞最重要的通知。 當裝置插入電源時，可以完成同步或非時間關鍵作業。

Windows 不知道哪些通知對任何使用者或應用程式很重要，因此系統完全依賴應用程式來設定其通知的正確優先順序。 

## <a name="priorities"></a>重點
有四個優先順序可供應用程式在傳送推播通知時使用。 優先順序是針對個別通知而設定，可讓您選擇要立即傳遞的通知（例如，IM 訊息），以及可以等候的通知（例如，連絡人相片更新）。

優先順序如下： 

|    Priority    |    使用者覆寫    |    描述    |    範例    |
|----------------|---------------------|-------------------|---------------|
|    高    |    是–使用者可以封鎖來自應用程式的所有通知，或防止應用程式在電池保護模式下受到節流。    |    當裝置可以接收通知時，必須立即提供的最重要通知。 如 VoIP 呼叫或應喚醒裝置的重大警示，則屬於此類別。    |    VoIP 呼叫，時間緊迫警示    |
|    中等    |    是–使用者可以封鎖來自應用程式的所有通知，或防止應用程式在電池保護模式下受到節流。    |    這些是不重要的專案，不需要立即發生的事情，但是如果使用者不是在背景中執行，就會感到苦惱。    |    次要電子郵件帳戶同步處理、動態磚更新。    |
|    低    |    是–使用者可以封鎖來自應用程式的所有通知，或防止應用程式在電池保護模式下受到節流。    |    只有當使用者使用裝置或背景活動合理時，才有意義的通知。 這些會經過快取，而且在使用者登入或插入其裝置之前，都不會進行處理。    |    連絡人狀態（線上/離線）    |
|    非常低     |    否–它無法防止極低優先順序的通知在電池保護模式下受到節流。    |    這幾乎與低優先順序相同，不同之處在于使用者無法覆寫電池保護原則。 這些通知永遠都不會以節電的方式提供。    |    同步處理服務的同步檔案。    |

請注意，許多應用程式在其生命週期中會有不同優先順序的通知。 由於優先順序是根據每個通知來設定，因此這不是問題。 VoIP 應用程式可以傳送來電的高優先順序通知，然後在連絡人上線時，以低優先順序來追蹤。 

## <a name="setting-the-priority"></a>設定優先順序

透過 POST 要求的額外標頭來設定通知要求的優先順序，`X-WNS-PRIORITY`。 這是介於1和4之間的整數值，其對應至優先順序： 

| 優先順序名稱 | X-WNS-優先順序值 | 預設值： |
|---------------|----------------------|------------------|
| 高 | 1 | 快顯通知 |
| Meduim | 2 | 磚和徽章 |
| 低 | 3 | 原始 |
| 非常低 | 4 |  |

若要回溯相容，則不需要設定優先順序。 如果應用程式未設定其通知的優先順序，系統會提供預設優先順序。 預設值會顯示在上圖中，並符合現有 Windows 版本的行為。 

## <a name="detailed-listing-of-desktop-behavior"></a>桌面行為的詳細清單 

如果您要將應用程式傳送到許多不同的 Windows Sku，通常最好遵循上一節中的圖表。 

以下列出每個優先權的更具體建議行為。 這並不保證每個裝置都能完全依照圖表來執行。 Oem 可自由設定不同的行為，但大部分都接近此圖表。 

| 裝置狀態    | 優先順序高    |    優先順序中等        | 優先順序低    |    優先順序非常低    |
|-------------------------------------------------------|----------------------------------------------------|----------------------------------------------------|----------------------------------------------------|--------------------------|
|    開啟或插入畫面    |    構建    |    構建    |    構建    |    構建    |
|    螢幕關閉和使用電池    |    構建    |    如果使用者豁免：提供其他： batch     |    如果使用者豁免：提供其他：快取 *    |    快取    |
|    啟用電池保護    |    如果使用者豁免：提供其他：快取    |    如果使用者豁免：提供其他：快取    |    如果使用者豁免：提供其他：快取    |    快取     |
|    已啟用電池 + 節電功能 + 螢幕關閉    |    如果使用者豁免：提供其他：快取    |    如果使用者豁免：提供其他：快取    |    如果使用者豁免：提供其他：快取    |    快取    |

請注意，根據預設，低優先順序的通知將會針對螢幕關閉，而僅針對以 Windows Phone 為基礎的裝置提供電池。 這是為了 maintian 與預先存在之 MPNS 原則的相容性。 另請注意，第四個和第五個數據列相同，只是呼叫不同的案例。

若要使用節電功能來豁免應用程式，使用者必須前往 [設定] 中的 [依應用程式電池使用量]，然後選取 [允許應用程式執行背景工作]。 此使用者選項會從電池保護豁免應用程式，以達到高、中和低優先順序的通知。 您也可以呼叫[BACKGROUNDEXECUTIONMANAGER API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_) ，以程式設計方式要求使用者的許可權。  

## <a name="related-topics"></a>相關主題
- [Windows 推播通知服務 (WNS) 概觀](windows-push-notification-services--wns--overview.md)
- [要求在背景中執行的許可權](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_)
