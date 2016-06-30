---
author: Xansky
description: "本主題示範如何啟動 \\[撰寫 SMS\\] 對話方塊，讓使用者傳送 SMS 訊息。 您可以在顯示該對話方塊之前，使用資料預先填入 SMS 的欄位。 在使用者點選 \\[傳送\\] 按鈕之前，不會將訊息傳送出去。"
title: "傳送 SMS 訊息"
ms.assetid: 4D7B509B-1CF0-4852-9691-E96D8352A4D6
keywords: contacts, SMS, send
translationtype: Human Translation
ms.sourcegitcommit: 1395e342bb6ad6a2d4fa347f1797aeafd7a524a6
ms.openlocfilehash: 70dfce318d37d6790585b0fa5da50963f95495dc

---

# 傳送 SMS 訊息

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本主題示範如何啟動 \[撰寫 SMS\] 對話方塊，讓使用者傳送 SMS 訊息。 您可以在顯示該對話方塊之前，使用資料預先填入 SMS 的欄位。 在使用者點選 \[傳送\] 按鈕之前，不會將訊息傳送出去。

## 啟動 \[撰寫 SMS\] 對話方塊

建立一個新的 ChatMessage 物件，然後設定您要在 \[撰寫電子郵件\] 對話方塊中預先填入的資料。 呼叫 [**ShowComposeSmsMessageAsync**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.chat.chatmessagemanager.showcomposesmsmessageasync) 以顯示該對話方塊。

```cs
private async void ComposeSms(Windows.ApplicationModel.Contacts.Contact recipient,
    string messageBody,
    StorageFile attachmentFile,
    string mimeType)
{
    var chatMessage = new Windows.ApplicationModel.Chat.ChatMessage();
    chatMessage.Body = messageBody;

    if (attachmentFile != null)
    {
        var stream = Windows.Storage.Streams.RandomAccessStreamReference.CreateFromFile(attachmentFile);

        var attachment = new Windows.ApplicationModel.Chat.ChatMessageAttachment(
            mimeType,
            stream);

        chatMessage.Attachments.Add(attachment);
    }

    var phone = recipient.Phones.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactPhone>();
    if (phone != null)
    {
        chatMessage.Recipients.Add(phone.Number);
    }
    await Windows.ApplicationModel.Chat.ChatMessageManager.ShowComposeSmsMessageAsync(chatMessage);
}
```

## 摘要與後續步驟

本主題已經示範如何啟動 \[撰寫 SMS\] 對話方塊。 如需有關選取連絡人做為 SMS 訊息收件者的資訊，請參閱[選取連絡人](selecting-contacts.md)。 請從 GitHub 下載[通用 Windows app 範例](http://go.microsoft.com/fwlink/p/?linkid=619979)，以查看更多如何使用背景工作來傳送和接收 SMS 訊息的範例。

## 相關主題

* [選取連絡人](selecting-contacts.md)



<!--HONumber=Jun16_HO4-->


