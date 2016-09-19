---
author: normesta
description: "說明如將社交動向摘要整合到連絡人 app 中"
MSHAttr: PreferredLib:/library/windows/apps
title: "提供社交動向摘要給連絡人 app"
translationtype: Human Translation
ms.sourcegitcommit: 767acdc847e1897cc17918ce7f49f9807681f4a3
ms.openlocfilehash: c5b9666d8654a4065bc0e4e400d3e47de4773b8b

---

# 提供社交動向摘要給連絡人 app

將來自您資料庫的社交動向摘要資料整合到連絡人 app 中

您的摘要資料將會顯示在連絡人 app 的 [最新動向]**** 頁面，或連絡人的 [個人資料]**** 頁面中。

使用者可以點選摘要項目來開啟您的 app。

![連絡人 App 中的社交動向摘要](images/social-feeds.png)

若要開始，請建立針對社交動向摘要標記連絡人的前景 app，以及將摘要資料傳送給連絡人 app 的背景代理程式。

如需更完整的範例，請參閱[社交動向資訊範例](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp)。

## 建立前景 app

首先，請建立通用 Windows 平台 (UWP) 專案，然後將 **UWP 適用的 Windows Mobile 擴充功能**新增至專案。

![Mobile 擴充功能](images/mobile-extensions.png)

### 尋找或建立連絡人

您可以使用姓名、電子郵件地址或電話號碼來尋找連絡人。

```cs
ContactStore contactStore = await ContactManager.RequestStoreAsync();

IReadOnlyList<Contact> contacts = null;

contacts = await contactStore.FindContactsAsync(emailAddress);

Contact contact = contacts[0];
```
您也可以建立連絡人，然後將他們新增到連絡人清單。

```cs
Contact contact = new Contact();
contact.FirstName = "TestContact";

ContactEmail email = new ContactEmail();
email.Address = "TestContact@contoso.com";
email.Kind = ContactEmailKind.Other;
contact.Emails.Add(email);

ContactPhone phone = new ContactPhone();
phone.Number = "4255550101";
phone.Kind = ContactPhoneKind.Mobile;
contact.Phones.Add(phone);

ContactStore store = await
    ContactManager.RequestStoreAsync(ContactStoreAccessType.AppContactsReadWrite);

ContactList contactList;

IReadOnlyList<ContactList> contactLists = await store.FindContactListsAsync();

if (0 == contactLists.Count)
    contactList = await store.CreateContactListAsync("TestContactList");
else
    contactList = contactLists[0];

await contactList.SaveContactAsync(contact);
```

### 使用註解來標記每個連絡人

此「註解」**會讓連絡人 app 向您的背景代理程式要求連絡人的摘要資料。

請將連絡人的識別碼與您 app 內部用來識別連絡人的識別碼相關聯，以做為註解的一部份。

```cs
ContactAnnotationStore annotationStore = await
   ContactManager.RequestAnnotationStoreAsync(ContactAnnotationStoreAccessType.AppAnnotationsReadWrite);

ContactAnnotationList annotationList;

IReadOnlyList<ContactAnnotationList> annotationLists = await annotationStore.FindAnnotationListsAsync();
if (0 == annotationLists.Count)
    annotationList = await annotationStore.CreateAnnotationListAsync();
else
    annotationList = annotationLists[0];

ContactAnnotation annotation = new ContactAnnotation();
annotation.ContactId = contact.Id;
annotation.RemoteId = "user22";

annotation.SupportedOperations = ContactAnnotationOperations.SocialFeeds;

await annotationList.TrySaveAnnotationAsync(annotation);

```
### 佈建背景代理程式

請確定將執行您 app 的裝置上有 [SocialInfoContract](https://msdn.microsoft.com/library/windows/apps/dn706146.aspx) API 協定可用。

如果有，就請佈建背景代理程式。

```cs
if (Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
"Windows.ApplicationModel.SocialInfo.SocialInfoContract",
1))
{
    bool isProvisionSuccessful = await Windows.ApplicationModel.SocialInfo.Provider.SocialInfoProviderManager.ProvisionAsync();

    // Throw an exception if the app could not be provisioned
    if (!isProvisionSuccessful)
    {
        throw new Exception("Could not provision the app with the SocialInfoProviderManager");
    }
}
```
## 建立背景代理程式

背景代理程式為「Windows 執行階段元件」，可回應來自連絡人 app 的摘要要求。

在您的代理程式中，您將會透過向連絡人 app 提供來自您資料庫的摘要資料以回應那些要求。

### 建立 Windows 執行階段元件

將 **Windows 執行階段元件 (通用 Windows)** 專案新增到您的解決方案。

![Windows 執行階段元件](images/windows-runtime-component.png)

### 將背景代理程式登錄為一項 app 服務

透過將通訊協定處理常式新增到資訊清單的 ``Extensions`` 元素來登錄。

```xml
<Extensions>
  <uap:Extension Category="windows.appService" EntryPoint="SocialFeeds.BackgroundAgent">
    <uap:AppService Name="com.microsoft.windows.social-feeds" />
  </uap:Extension>
</Extensions>
```
您也可以在 Visual Studio 中資訊清單設計工具的 [宣告]**** 索引標籤中新增這些處理常式。

![資訊清單設計工具中的 app 服務](images/manifest-designer-app-service.png)

### 向連絡人 app 要求作業

詢問連絡人 app 接下來需要哪種類型的資料。 連絡人 app 將會向您提供指示它需要哪些摘要資料的代碼來回應您的要求。

此表格描述每項摘要：

| 摘要 | 說明 |
|-------|-------------|
| 首頁 | 連絡人 app [最新動向] 頁面中顯示的摘要 |
| 連絡人 | 連絡人 [最新動向] 頁面中顯示的摘要 |
| 儀表板 | 個人資料圖片旁邊的連絡人卡片中顯示的摘要。 |
<br>
您將會透過要求「作業」**來向連絡人 app 提出要求。 實作 [IBackgroundTask](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.aspx) 介面並複寫 [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 方法。

在 [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 方法中，向連絡人 app 傳送兩個機碼值組。 其中一個包含通訊協定版本，另一個包含作業類型。

然後接聽來自連絡人 app 的回應。 該回應將包含代碼。

```cs
public sealed class BackgroundAgent : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {
        BackgroundTaskDeferral backgroundTaskDeferral = taskInstance.GetDeferral();

        AppServiceTriggerDetails triggerDetails = taskInstance.TriggerDetails as AppServiceTriggerDetails;

        AppServiceConnection appServiceConnection = triggerDetails.AppServiceConnection;

        bool continueProcessing = true;

        while (continueProcessing)
        {
            // Get next operation.
            ValueSet fields =
                new ValueSet();

            fields.Add("Version",
                (1 << 16) | 0);

            fields.Add(
                "Type", 0x10);

            AppServiceResponse response =
                await appServiceConnection.SendMessageAsync(fields);

            if (response == null || response.Status != AppServiceResponseStatus.Success)
            { /* throw exception */ }

            UInt32 type = (UInt32)response.Message["Type"];

            switch (type)
            {
                //Get Next Operation.
                case 0x10:
                    break;
                //Download Home Feed Operation.
                case 0x11:
                    break;
                //Download Contact Feed Operation.
                case 0x13:
                    break;
                //Download Dashboard Feed Operation.
                case 0x15:
                    break;
                //Operation Result.
                case 0x80:
                    break;
                //Good Bye.
                case 0xF1:
                    continueProcessing = false;
                    break;
            }
        }
        // Complete the task deferral
        backgroundTaskDeferral.Complete();

        backgroundTaskDeferral = null;
        appServiceConnection = null;
    }

}
```

請參閱 [AppServiceResponse.Message](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceresponse.message.aspx) 屬性的 ``Type`` 元素來取得該代碼。 以下是代碼的完整清單。

| 類型| 說明 |
|-----|-------------|
| 0x10 | 向連絡人 app 要求下一項作業的要求。 |
| 0x11 | 來自連絡人 app，用來提供主要使用者首頁摘要的要求。 |
| 0x13 | 來自連絡人 app，用來取得所選連絡人的連絡人摘要的要求。 |
| 0x15 | 來自連絡人 app，用來取得所選連絡人的儀表板項目的要求。 |
| 0x80 | 指示作業已經完成。 這會通知連絡人 app 資料現在可供使用。 |
| 0xF1 | 來自連絡人 app 並指示它不需要任何其他作業的訊息。 現在可以關閉背景代理程式。 |
<br>
[AppServiceResponse.Message](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceresponse.message.aspx) 屬性也會傳回描述回應的其他機碼值組集合。 以下是機碼值組的清單。

| 機碼 | 類型 | 說明 |
|-----|------|-------------|
| 版本 | UINT32 | (必要) 識別訊息通訊協定的版本。 較高的 16 位元是主要版本，較低的 16 位元是次要版本。 |
| 類型 | UINT32 | (必要) 要執行的作業類型。 前一個範例是使用 Type 機碼來判斷連絡人 app 要求哪些作業。
| OperationId | UINT32 | 作業的識別碼。 |
| OwnerRemoteId | 字串 | 您的 app 內部用來識別該連絡人的識別碼。 |
| LastFeedItemTimeStamp | 字串 | 上一個所擷取摘要項目的識別碼。 |
| LastFeedItemTimeStamp | DateTime | 上一個所擷取摘要項目的時間戳記。 |
| ItemCount | UINT32 | 連絡人 app 所要求項目的數目。 |
| IsFetchMore | 布林值 | 決定何時更新內部快取。 |
| ErrorCode | UINT32 | 與背景代理程式作業關聯的錯誤代碼。 |
<br>
### 向連絡人 app 提供資料摘要

``0x11``、``0x13`` 或 ``0x15`` 的 **Type** 值是連絡人 app 對摘要資料的要求。  

接下來幾個程式碼片段說明向連絡人 app 提供資料的方式。

> [!NOTE]
> 這些程式碼片段是來自[社交動向資訊範例](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp)。 它們包含對範例中其他位置所定義介面、類別及成員的參考。 請搭配此主題中的其他範例使用這些程式碼片段來了解工作流程，且如果您有興趣進一步區分至介面、類別與類型的堆疊，請參閱樣本。

**取得連絡人摘要項目**

```cs
public override async Task DownloadFeedAsync()
{
    // Get the contact feeds
    IEnumerable<FeedItem> contactFeedItems =
        InMemorySocialCache.Instance.GetContactFeeds(OwnerRemoteId, ItemCount);

    // Check if the platform supports the SocialInfo APIs
    if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
         "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
    {

        return;
    }

    // Create the social feed updater.
    SocialFeedUpdater feedUpdater = await SocialInfoProviderManager.CreateSocialFeedUpdaterAsync(
        SocialFeedKind.ContactFeed,
        SocialFeedUpdateMode.Replace,
        OwnerRemoteId);

    // Translate the sample feed into Social info feed items.
    foreach (FeedItem fi in contactFeedItems)
    {
        SocialFeedItem item = new SocialFeedItem();

        item.Timestamp = fi.Timestamp;
        item.RemoteId = fi.Id;
        item.TargetUri = fi.TargetUri;
        item.Author.DisplayName = fi.AuthorDisplayName;
        item.Author.RemoteId = fi.AuthorId;
        item.PrimaryContent.Title = fi.Title;
        item.PrimaryContent.Message = fi.Message;

        if (fi.ImageUri != null)
        {
            item.Thumbnails.Add(new SocialItemThumbnail()
            {
                TargetUri = fi.TargetUri,
                ImageUri = fi.ImageUri
            });
        }

        feedUpdater.Items.Add(item);
    }

    await feedUpdater.CommitAsync();
}
```

**取得儀表板項目**

```cs
public override async Task DownloadFeedAsync()
{
    // Get the dashboard feed item from your database.
    FeedItem dashboardFeedItem = InMemorySocialCache.Instance.GetDashboardFeed(OwnerRemoteId);

    if (dashboardFeedItem != null)
    {
        // Check if the platform supports the social info APIs.
        if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
             "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
        {

            return;
        }

        SocialDashboardItemUpdater dashboard =
            await SocialInfoProviderManager.CreateDashboardItemUpdaterAsync(OwnerRemoteId);

        dashboard.Content.Message = dashboardFeedItem.Message;
        dashboard.Content.Title = dashboardFeedItem.Title;
        dashboard.Timestamp = dashboardFeedItem.Timestamp;

        // The TargetUri of the dashboard always has to be set.
        dashboard.TargetUri = dashboardFeedItem.TargetUri;

        // For a thumbnail, there must always be a target Uri.
        if ((dashboardFeedItem.ImageUri != null) && (dashboardFeedItem.TargetUri != null))
        {
            dashboard.Thumbnail = new SocialItemThumbnail()
            {
                ImageUri = dashboardFeedItem.ImageUri,
                TargetUri = dashboardFeedItem.TargetUri
            };
        }

        await dashboard.CommitAsync();
    }
}
```

**取得首頁摘要項目**

```cs
public override async Task DownloadFeedAsync()
{
    // Query the "database" for the home feeds.
    IEnumerable<FeedItem> homeFeedItems =
        InMemorySocialCache.Instance.GetHomeFeeds(OwnerRemoteId, ItemCount);

    // Check if the platform supports the social info APIs.
    if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
         "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
    {

        return;
    }

    // Create the social feed updater.
    SocialFeedUpdater feedUpdater = await SocialInfoProviderManager.CreateSocialFeedUpdaterAsync(
        SocialFeedKind.HomeFeed,
        SocialFeedUpdateMode.Replace,
        OwnerRemoteId);

    // Generate each of the feed items.
    foreach (FeedItem fi in homeFeedItems)
    {
        SocialFeedItem item = new SocialFeedItem();

        item.Timestamp = fi.Timestamp;
        item.RemoteId = fi.Id;
        item.TargetUri = fi.TargetUri;
        item.Author.DisplayName = fi.AuthorDisplayName;
        item.Author.RemoteId = fi.AuthorId;
        item.PrimaryContent.Title = fi.Title;
        item.PrimaryContent.Message = fi.Message;

        if (fi.ImageUri != null)
        {
            item.Thumbnails.Add(new SocialItemThumbnail()
            {
                TargetUri = fi.TargetUri,
                ImageUri = fi.ImageUri
            });
        }

        feedUpdater.Items.Add(item);
    }

    await feedUpdater.CommitAsync();
}
```

### 向連絡人 app 回傳成功或失敗通知

在 Try Catch 區塊中封裝您的呼叫，然後在您已經提供摘要資料之後向連絡人 app 回傳成功或失敗訊息。

```cs
try
{
    // Calls to methods that fetch data and populate feed.
}
catch (Exception exception)
{
    errorCode = (UInt32)exception.HResult;
}

// Send status back to the people app.
ValueSet fields =
    new ValueSet();

fields.Add("ErrorCode", errorCode);

UInt32 operationID = (UInt32)response.Message["OperationId"];

fields.Add("OperationId", operationID);

await this.mAppServiceConnection.SendMessageAsync(fields);

```



<!--HONumber=Aug16_HO4-->


