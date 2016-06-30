---
author: Xansky
description: "示範如何啟動 \\[撰寫電子郵件\\] 對話方塊，讓使用者傳送電子郵件訊息。 您可以在顯示該對話方塊之前，使用資料預先填入電子郵件的欄位。 在使用者點選 \\[傳送\\] 按鈕之前，不會將訊息傳送出去。"
title: "傳送電子郵件"
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: contacts, email, send
ms.sourcegitcommit: 252e144b2436f047f7b0849bb6e5aee87b2e3464
ms.openlocfilehash: ff0d2cd86f8ba422db7a26565c52293a3e21a018

---

# 傳送電子郵件

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


示範如何啟動 \[撰寫電子郵件\] 對話方塊，讓使用者傳送電子郵件訊息。 您可以在顯示該對話方塊之前，使用資料預先填入電子郵件的欄位。 在使用者點選 \[傳送\] 按鈕之前，不會將訊息傳送出去。

**本文內容**

-   [啟動 \[撰寫電子郵件\] 對話方塊](#launch-the-compose-email-dialog)
-   [摘要與後續步驟](#summary-and-next-steps)
-   [相關主題](#related-topics)

## 啟動 \[撰寫電子郵件\] 對話方塊

建立一個新的 EmailMessage 物件，然後設定您要在 \[撰寫電子郵件\] 對話方塊中預先填入的資料。 呼叫 [**ShowComposeNewEmailAsync**](https://msdn.microsoft.com/library/windows/apps/Dn631269) 以顯示該對話方塊。

``` cs
private async Task ComposeEmail(Windows.ApplicationModel.Contacts.Contact recipient, 
    string messageBody, 
    StorageFile attachmentFile)
{
    var emailMessage = new Windows.ApplicationModel.Email.EmailMessage();
    emailMessage.Body = messageBody;

    if (attachmentFile != null)
    {
        var stream = Windows.Storage.Streams.RandomAccessStreamReference.CreateFromFile(attachmentFile);

        var attachment = new Windows.ApplicationModel.Email.EmailAttachment(
            attachmentFile.Name,
            stream);

        emailMessage.Attachments.Add(attachment);
    }

    var email = recipient.Emails.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactEmail>();
    if (email != null)
    {
        var emailRecipient = new Windows.ApplicationModel.Email.EmailRecipient(email.Address);
        emailMessage.To.Add(emailRecipient);
    }

    await Windows.ApplicationModel.Email.EmailManager.ShowComposeNewEmailAsync(emailMessage);
        
}
```

## 摘要與後續步驟

本主題已經示範如何啟動 \[撰寫電子郵件\] 對話方塊。 如需有關如何選取連絡人做為電子郵件訊息收件者的資訊，請參閱[選取連絡人](selecting-contacts.md)。 請參閱 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) 來選取要做為電子郵件附件的檔案。

## 相關主題

* [選取連絡人](selecting-contacts.md)
* [如何在呼叫檔案選擇器之後繼續執行您的 Windows Phone app](https://msdn.microsoft.com/library/windows/apps/xaml/Dn614994)
 

 







<!--HONumber=Jun16_HO4-->


