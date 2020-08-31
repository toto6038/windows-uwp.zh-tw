---
title: WNS 通知優先順序
description: 您可以在通知上設定的各種優先順序描述
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10、uwp、WinRT API、WNS
localizationpriority: medium
ms.openlocfilehash: 3b6642054f9c63a03764267e5886b67fd4a9ac7d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169212"
---
# <a name="wns-notification-priorities"></a>WNS 通知優先順序
藉由以簡單的標頭將通知的優先順序設定為 WNS 張貼訊息，您可以控制在電池敏感性情況下傳遞通知的方式。

## <a name="power-on-windows"></a>Windows 上的電源
由於有更多使用者只會在支援電池的裝置上運作，因此將電源使用量降至最低已成為所有應用程式的標準需求。 如果應用程式所耗用的能源超過其所提供的值，使用者可能會卸載應用程式。 雖然 Windows 作業系統盡可能減少電池的電源使用量，但應用程式必須負責有效率地工作。 

WNS 優先順序是一種將非關鍵性工作移出電池的方式。 WNS 優先權可告知系統應立即傳遞的通知，以及等待裝置插入電源的電源。 藉由這些提示，系統可以將通知傳遞給使用者和應用程式最有價值的確切時間。 

## <a name="power-modes-on-the-device"></a>裝置上的電源模式
每個 Windows 裝置都能透過各種不同的電源模式來運作 (電池、省電模式和收費) ，且使用者預期應用程式會以不同的電源模式進行不同的行為。 裝置開啟時，應傳遞所有通知。 在省電模式模式中，只應傳遞最重要的通知。 當裝置插入電源時，可以完成同步或非時間重要的作業。

Windows 不知道哪些通知對任何使用者或應用程式來說很重要，因此系統完全依賴應用程式來為其通知設定正確的優先順序。 

## <a name="priorities"></a>優先順序
傳送推播通知時，有四個可供應用程式使用的優先順序。 優先權是在個別通知上設定，可讓您選擇要立即傳遞的通知 (例如，IM 訊息) ，以及可等候 (例如，將相片更新) 。

優先順序如下： 

|    優先順序    |    使用者覆寫    |    說明    |    範例    |
|----------------|---------------------|-------------------|---------------|
|    高    |    是–使用者可以封鎖來自應用程式的所有通知，也可以防止應用程式在省電模式模式下進行節流。    |    在任何情況下，如果裝置可以接收通知，就必須立即傳遞的最重要通知。 像是 VoIP 呼叫或應該喚醒裝置的重大警示，都屬於此類別。    |    VoIP 呼叫，時間緊迫警示    |
|    中    |    是–使用者可以封鎖來自應用程式的所有通知，也可以防止應用程式在省電模式模式下進行節流。    |    這些都是不重要的專案，不需要立即執行的專案，但如果使用者不是在背景中執行，就會感到苦惱。    |    次要電子郵件帳戶同步、動態磚更新。    |
|    低    |    是–使用者可以封鎖來自應用程式的所有通知，也可以防止應用程式在省電模式模式下進行節流。    |    只有當使用者使用裝置或背景活動有意義時，才會有意義的通知。 這些會在使用者登入或插入其裝置之前進行快取和處理。    |    連絡人狀態 (線上/離線)     |
|    非常低     |    否–它無法防止在省電模式模式下節流的低優先順序通知。    |    這與低優先順序幾乎相同，不同之處在于使用者無法覆寫省電模式原則。 這些通知將永遠不會在省電模式中傳遞。    |    同步處理服務的檔案。    |

請注意，許多應用程式在整個生命週期中都會有不同優先順序的通知。 因為優先權是根據每個通知來設定的，所以這不是問題。 VoIP 應用程式可以針對來電傳送高優先順序的通知，然後在連絡人上線時以低優先順序的方式進行追蹤。 

## <a name="setting-the-priority"></a>設定優先順序

您可以透過 POST 要求上的額外標頭來設定通知要求的優先順序 `X-WNS-PRIORITY` 。 這是介於1和4之間的整數值，對應至優先順序： 

| 優先順序名稱 | X-WNS-優先順序值 | 預設值： |
|---------------|----------------------|------------------|
| 高 | 1 | 快顯通知 |
| Meduim | 2 | 磚與徽章 |
| 低 | 3 | Raw |
| 非常低 | 4 |  |

為了回溯相容，不需要設定優先順序。 如果應用程式未設定其通知的優先順序，系統將會提供預設優先順序。 預設值會顯示在上圖中，並符合現有 Windows 版本的行為。 

## <a name="detailed-listing-of-desktop-behavior"></a>桌面行為的詳細清單 

如果您要將應用程式傳送到許多不同的 Windows Sku，通常最好依照上一節中的圖表操作。 

以下列出每個優先順序更明確的建議行為。 這並不保證每個裝置都能完全依照圖表運作。 Oem 可以用不同的方式設定行為，但大部分都接近此圖表。 

| 裝置狀態    | 優先順序：高    |    優先順序：中        | 優先順序：低    |    優先順序：非常低    |
|-------------------------------------------------------|----------------------------------------------------|----------------------------------------------------|----------------------------------------------------|--------------------------|
|    螢幕開啟或插入    |    傳遞    |    傳遞    |    傳遞    |    傳遞    |
|    螢幕關閉和電池電力    |    傳遞    |    如果使用者豁免：傳遞其他：批次     |    如果使用者豁免：傳遞 Else： cache *    |    快取    |
|    啟用電池保護    |    如果使用者豁免：傳遞 Else： cache    |    如果使用者豁免：傳遞 Else： cache    |    如果使用者豁免：傳遞 Else： cache    |    快取     |
|    開啟電池 + 省電模式啟用 + 關閉螢幕    |    如果使用者豁免：傳遞 Else： cache    |    如果使用者豁免：傳遞 Else： cache    |    如果使用者豁免：傳遞 Else： cache    |    快取    |

請注意，根據預設，低優先順序通知將會針對僅限 Windows Phone 的裝置，提供給螢幕關閉和電池。 這是為了 maintian 與預先存在的 MPNS 原則的相容性。 另請注意，第四個和第五個數據列相同，只是呼叫不同的案例。

若要在省電模式中豁免應用程式，使用者必須移至 [設定] 中的 [應用程式的電池使用量]，然後選取 [允許應用程式執行背景工作]。 此使用者選取專案會從省電模式豁免應用程式，以取得高、中和低優先順序的通知。 您也可以呼叫 [BACKGROUNDEXECUTIONMANAGER API](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_) ，以程式設計方式要求使用者的許可權。  

## <a name="related-topics"></a>相關主題
- [Windows 推播通知服務 (WNS) 概觀](windows-push-notification-services--wns--overview.md)
- [要求在背景中執行的許可權](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_)