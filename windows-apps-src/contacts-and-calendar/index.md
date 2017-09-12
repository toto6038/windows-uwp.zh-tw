---
author: normesta
description: "如何在 UWP app 中使用連絡人和行事曆資訊。"
title: "連絡人和行事曆"
ms.assetid: b7e53ab5-2828-4fb7-8656-2bec70b3467f
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 連絡人, 行事曆, 約會, 電子郵件訊息"
ms.openlocfilehash: 2d90823c60f9a86bcff2228763911aaf7f5049a1
ms.sourcegitcommit: 378382419f1fda4e4df76ffa9c8cea753d271e6a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2017
---
# <a name="contacts-and-calendar"></a>連絡人和行事曆

\[ 針對 Windows10 上的 UWP app 更新。 如需 Windows8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

您可以讓使用者存取他們的連絡人和約會，以便彼此分享內容、電子郵件、行事曆資訊或訊息，或是共用您所設計的任何功能。

若要查看您的 app 存取連絡人和約會的幾種不同方式，請參閱這些主題：

| 主題 | 說明 |
|-------|-------------|
| [選取連絡人](selecting-contacts.md) | 在整個 [<strong>Windows.ApplicationModel.Contacts</strong>](https://msdn.microsoft.com/library/windows/apps/BR225002) 命名空間中，有好幾種方法可以用來選取連絡人。 我們會在這裡示範如何選取單一連絡人或多位連絡人，也會示範如何設定連絡人選擇器，只抓取您 app 所需的連絡人資訊。 |
| [傳送電子郵件](sending-email.md) | 示範如何啟動 [撰寫電子郵件] 對話方塊，讓使用者傳送電子郵件訊息。 您可以在顯示該對話方塊之前，使用資料預先填入電子郵件的欄位。 在使用者點選 [傳送] 按鈕之前，不會將訊息傳送出去。 |
| [傳送 SMS 訊息](sending-an-sms-message.md) | 本主題示範如何啟動 [撰寫 SMS] 對話方塊，讓使用者傳送 SMS 訊息。 您可以在顯示該對話方塊之前，使用資料預先填入 SMS 的欄位。 在使用者點選 [傳送] 按鈕之前，不會將訊息傳送出去。 |
| [管理約會](managing-appointments.md) | 您可以透過 [<strong>Windows.ApplicationModel.Appointments</strong>](https://msdn.microsoft.com/library/windows/apps/Dn263359) 命名空間，在使用者的行事曆 app 建立和管理約會。 這裡，我們將示範如何建立約會、將約會新增到行事曆 app、在行事曆 app 替換約會，以及從行事曆 app 移除約會。 同時還會示範如何顯示行事曆 app 的時間範圍，以及建立約會週期物件。 |
| [將 App 連結到連絡人卡片上的動作](integrating-with-contacts.md) | 說明如何讓您的 app 顯示在連絡人卡片或迷你連絡人卡片上的動作旁邊。 使用者可以選擇您的 app 來執行動作，例如開啟個人資料頁面、撥打電話，或傳送訊息。 |

 

## <a name="related-topics"></a>相關主題

* [約會 API 範例](http://go.microsoft.com/fwlink/p/?linkid=309836)
* [連絡人管理員 API 範例](http://go.microsoft.com/fwlink/p/?LinkID=310079)
* [連絡人選擇器應用程式範例](http://go.microsoft.com/fwlink/p/?linkid=231575)
* [處理連絡人動作範例](http://go.microsoft.com/fwlink/p/?LinkID=320151)
* [連絡人卡片整合範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)
