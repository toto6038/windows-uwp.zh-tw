---
description: 本文說明如何使用分享協定，在您的通用 Windows 平台 (UWP) app 中接收從另一個應用程式分享的內容。 分享協定可以在使用者叫用分享時，讓您的應用程式成為一個選項。
title: 接收資料
ms.assetid: 0AFF9E0D-DFF4-4018-B393-A26B11AFDB41
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e17b9ddd5833899a83e24d24c74f9c620a28f5c8
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2018
ms.locfileid: "8799428"
---
# <a name="receive-data"></a>接收資料



本文說明如何使用分享協定，在您的通用 Windows 平台 (UWP) app 中接收從另一個應用程式分享的內容。 分享協定可以在使用者叫用分享時，讓您的應用程式成為一個選項。

## <a name="declare-your-app-as-a-share-target"></a>將您的 app 宣告為分享目標

當使用者叫用分享時，系統會顯示可能的目標 app 的清單。 為了要顯示在清單上，您的 app 必須宣告它支援分享協定。 這會讓系統知道您的 app 能夠接收內容。

1.  開啟資訊清單檔案。 這個檔案的命名格式應該像這樣 **package.appxmanifest**。
2.  開啟 **\[宣告\]** 索引標籤。
3.  從 **\[可用宣告\]** 清單中選擇 **\[分享目標\]**，然後選取 **\[新增\]**。

## <a name="choose-file-types-and-formats"></a>選擇檔案類型和格式

接下來，決定您要支援哪些檔案類型和資料格式。 分享 API 支援數種標準格式，例如文字、HTML 及點陣圖。 您也可以指定自訂的檔案類型和資料格式。 如果這樣做，請記住來源 app 必須知道這些類型和格式，否則它們就無法使用這些格式來分享資料。

只註冊您的 app 可以處理的格式。 當使用者叫用分享時，系統只會顯示能夠支援分享資料的目標 app。

設定檔案類型：

1.  開啟資訊清單檔案。 這個檔案的命名格式應該像這樣 **package.appxmanifest**。
2.  在 **\[宣告\]** 頁面的 **\[支援的檔案類型\]** 區段中，選取 **\[加入新的\]**。
3.  輸入想要支援的副檔名，例如 .docx。 您必須加上句點 (.)。 如果想要支援所有檔案類型，請選取 **\[SupportsAnyFileType\]** 核取方塊。

設定資料格式：

1.  開啟資訊清單檔案。
2.  開啟 **\[宣告\]** 頁面的 **\[資料格式\]** 區段，然後選取 **\[加入新的\]**。
3.  輸入支援的資料格式名稱，例如 Text。

## <a name="handle-share-activation"></a>處理分享啟用

當使用者選取您的 app 時 (通常是從分享 UI 中可用的目標 app 清單中選取)，會引發 [**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Application.OnShareTargetActivated(Windows.ApplicationModel.Activation.ShareTargetActivatedEventArgs)) 事件。 您的 app 必須處理此事件，才能處理使用者想要分享的資料。

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    // Code to handle activation goes here. 
} 
```

使用者想要分享的資料包含在 [**ShareOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation) 物件中。 您可以使用此物件來檢查其包含的資料格式。

```cs
ShareOperation shareOperation = args.ShareOperation;
if (shareOperation.Data.Contains(StandardDataFormats.Text))
{
    string text = await shareOperation.Data.GetTextAsync();

    // To output the text from this example, you need a TextBlock control
    // with a name of "sharedContent".
    sharedContent.Text = "Text: " + text;
} 
```

## <a name="report-sharing-status"></a>報告分享狀態

在某些情形下，您的 app 需要時間來處理想要分享的資料。 範例包括使用者分享檔案或影像的集合。 這些項目比簡單文字字串更大，所以需要更長的處理時間。

```cs
shareOperation.ReportStarted(); 
```

呼叫 [**ReportStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportStarted) 後，不要預期使用者與您的 App 進行更多互動。 因此，除非使用者可以關閉 App，否則不應該呼叫它。

使用延伸分享時，使用者有可能會在 App 從 DataPackage 物件取得所有資料前，就關閉來源 App。 因此，建議您讓系統知道 app 已取得所需的資料。 如此一來，系統就可以視需要暫停或終止來源 app。

```cs
shareOperation.ReportSubmittedBackgroundTask(); 
```

如果出現錯誤，可以呼叫 [**ReportError**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportError(System.String)) 傳送錯誤訊息給系統。 當使用者檢查分享狀態時就會看到這個訊息。 在這個時候，您的 app 會關閉並結束分享。 使用者需要再次啟動並分享內容到您的 app。 根據您的情況，您可以決定特定錯誤沒有嚴重到需要結束分享作業。 在這個情況中，您可以選擇不要呼叫 **ReportError** 而繼續分享。

```cs
shareOperation.ReportError("Could not reach the server! Try again later."); 
```

最後，當您的 app 順利處理完分享內容時，應該呼叫 [**ReportCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportCompleted) 讓系統知道。

```cs
shareOperation.ReportCompleted();
```

當使用這些方法時，您通常會依照上述順序呼叫這些方法，而且不要呼叫它們超過一次。 不過，有時目標應用程式可能會先呼叫 [**ReportDataRetrieved**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportDataRetrieved)，之後才呼叫 [**ReportStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportStarted)。 例如，應用程式可能會在啟用處理常式的工作期間抓取資料，但直到使用者選取 **\[分享\]** 按鈕後才會呼叫 **ReportStarted**。

## <a name="return-a-quicklink-if-sharing-was-successful"></a>如果分享成功，則傳回 QuickLink

當使用者選取您的 app 來接收內容時，建議您建立一個 [**QuickLink**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.QuickLink)。 **QuickLink** 就像捷徑，可讓使用者方便與您的應用程式分享資訊。 例如，您可以建立一個 **QuickLink**，開啟已預先設定朋友電子郵件地址的新電子郵件訊息。

**QuickLink** 必須有標題、圖示和識別碼。當使用者點選 [分享] 常用鍵時，就會出現標題 (如「Email Mom」) 和圖示。 識別碼是您的 app 用來存取任何自訂資訊的物件，例如電子郵件地址或登入認證。 當您的 app 建立 **QuickLink** 時，app 會呼叫 [**ReportCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportCompleted) 來將 **QuickLink** 傳回系統。

**QuickLink** 實際上不會儲存資料。 而是會含有一個識別碼，在選取時傳送到您的 app。 您的 app 要負責儲存 **QuickLink** 的識別碼及對應的使用者資料。 當使用者點選 **QuickLink** 時，您可以透過 [**QuickLinkId**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.QuickLinkId) 屬性取得它的 ID。

```cs
async void ReportCompleted(ShareOperation shareOperation, string quickLinkId, string quickLinkTitle)
{
    QuickLink quickLinkInfo = new QuickLink
    {
        Id = quickLinkId,
        Title = quickLinkTitle,

        // For quicklinks, the supported FileTypes and DataFormats are set 
        // independently from the manifest
        SupportedFileTypes = { "*" },
        SupportedDataFormats = { StandardDataFormats.Text, StandardDataFormats.Uri, 
                StandardDataFormats.Bitmap, StandardDataFormats.StorageItems }
    };

    StorageFile iconFile = await Windows.ApplicationModel.Package.Current.InstalledLocation.CreateFileAsync(
            "assets\\user.png", CreationCollisionOption.OpenIfExists);
    quickLinkInfo.Thumbnail = RandomAccessStreamReference.CreateFromFile(iconFile);
    shareOperation.ReportCompleted(quickLinkInfo);
}
```

## <a name="see-also"></a>另請參閱 

* [App 間通訊](index.md)
* [分享資料](share-data.md)
* [OnShareTargetActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onsharetargetactivated.aspx)
* [ReportStarted](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted.aspx)
* [ReportError](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reporterror.aspx)
* [ReportCompleted](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportcompleted.aspx)
* [ReportDataRetrieved](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportdataretrieved.aspx)
* [ReportStarted](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted.aspx)
* [QuickLink](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.quicklink.aspx)
* [QuickLInkId](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.quicklink.id.aspx)
