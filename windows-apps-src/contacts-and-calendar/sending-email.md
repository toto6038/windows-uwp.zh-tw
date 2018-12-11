---
description: 示範如何啟動 [撰寫電子郵件] 對話方塊，讓使用者傳送電子郵件訊息。 您可以在顯示該對話方塊之前，使用資料預先填入電子郵件的欄位。 在使用者點選 [傳送] 按鈕之前，不會將訊息傳送出去。
title: 傳送電子郵件
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: 連絡人, 電子郵件, 傳送
ms.date: 10/11/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1593ab8b547a464492a35aa7d49d38f667a8210b
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8927402"
---
# <a name="send-email"></a>傳送電子郵件

示範如何啟動 [撰寫電子郵件] 對話方塊，讓使用者傳送電子郵件訊息。 您可以在顯示該對話方塊之前，使用資料預先填入電子郵件的欄位。 在使用者點選 [傳送] 按鈕之前，不會將訊息傳送出去。

**本文內容**

-   [啟動 [撰寫電子郵件] 對話方塊](#launch-the-compose-email-dialog)
-   [摘要與後續步驟](#summary-and-next-steps)
-   [相關主題](#related-topics)

## <a name="launch-the-compose-email-dialog"></a>啟動 [撰寫電子郵件] 對話方塊

建立一個新的 [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/Dn631270) 物件，然後設定您要在 [撰寫電子郵件] 對話方塊中預先填入的資料。 呼叫 [**ShowComposeNewEmailAsync**](https://msdn.microsoft.com/library/windows/apps/Dn631269) 以顯示該對話方塊。

``` cs
private async Task ComposeEmail(Windows.ApplicationModel.Contacts.Contact recipient,
    string subject, string messageBody)
{
    var emailMessage = new Windows.ApplicationModel.Email.EmailMessage();
    emailMessage.Body = messageBody;

    var email = recipient.Emails.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactEmail>();
    if (email != null)
    {
        var emailRecipient = new Windows.ApplicationModel.Email.EmailRecipient(email.Address);
        emailMessage.To.Add(emailRecipient);
        emailMessage.Subject = subject;
    }

    await Windows.ApplicationModel.Email.EmailManager.ShowComposeNewEmailAsync(emailMessage);
}
```

>[!NOTE]
> 您使用[EmailAttachment](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email.emailattachment)類別新增到電子郵件的附件只會出現在 [郵件] app。 如果使用者已設定為其預設的郵件程式的任何其他郵件程式，撰寫視窗會顯示不含附件。 這是已知的問題。

## <a name="summary-and-next-steps"></a>摘要與後續步驟

本主題已經示範如何啟動 [撰寫電子郵件] 對話方塊。 如需有關如何選取連絡人做為電子郵件訊息收件者的資訊，請參閱[選取連絡人](selecting-contacts.md)。 請參閱 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) 來選取要做為電子郵件附件的檔案。

## <a name="related-topics"></a>相關主題

* [選取連絡人](selecting-contacts.md)
* [如何在呼叫檔案選擇器之後繼續執行您的 Windows Phone app](https://msdn.microsoft.com/library/windows/apps/xaml/Dn614994)
 

 
