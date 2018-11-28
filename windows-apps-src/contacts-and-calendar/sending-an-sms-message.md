---
description: 本主題示範如何啟動 [撰寫 SMS] 對話方塊，讓使用者傳送 SMS 訊息。 您可以在顯示該對話方塊之前，使用資料預先填入 SMS 的欄位。 在使用者點選 [傳送] 按鈕之前，不會將訊息傳送出去。
title: 傳送 SMS 訊息
ms.assetid: 4D7B509B-1CF0-4852-9691-E96D8352A4D6
keywords: 連絡人, SMS, 傳送
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 427eb1b895269727d82e42d5abc3ae1f1da1a35d
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7840970"
---
# <a name="send-an-sms-message"></a>傳送 SMS 訊息

本主題示範如何啟動 [撰寫 SMS] 對話方塊，讓使用者傳送 SMS 訊息。 您可以在顯示該對話方塊之前，使用資料預先填入 SMS 的欄位。 在使用者點選 [傳送] 按鈕之前，不會將訊息傳送出去。

若要呼叫這個程式碼，宣告您的套件資訊清單中的**聊天**、 **smsSend**，以及**chatSystem**功能。 這些是[受限制的功能](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#special-and-restricted-capabilities)，但您可以在您的應用程式中使用它們。 只有當您想要將您的應用程式發行至市集，您會需要核准。 請參閱[帳戶類型、 位置和費用](https://docs.microsoft.com/windows/uwp/publish/account-types-locations-and-fees)。

## <a name="launch-the-compose-sms-dialog"></a>啟動 [撰寫 SMS] 對話方塊

建立一個新的 [**ChatMessage**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.chat.chatmessage) 物件，然後設定您要在 [撰寫電子郵件] 對話方塊中預先填入的資料。 呼叫 [**ShowComposeSmsMessageAsync**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.chat.chatmessagemanager.showcomposesmsmessageasync) 以顯示該對話方塊。

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

您可以使用下列程式碼，以判斷是否正在執行您的應用程式的裝置為能夠傳送簡訊訊息。

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.ApplicationModel.Chat"))
{
   // Call code here.
}
```

## <a name="summary-and-next-steps"></a>摘要與後續步驟

本主題已經示範如何啟動 [撰寫 SMS] 對話方塊。 如需有關選取連絡人做為 SMS 訊息收件者的資訊，請參閱[選取連絡人](selecting-contacts.md)。 請從 GitHub 下載[通用 Windows app 範例](http://go.microsoft.com/fwlink/p/?linkid=619979)，以查看更多如何使用背景工作來傳送和接收 SMS 訊息的範例。

## <a name="related-topics"></a>相關主題

* [選取連絡人](selecting-contacts.md)
