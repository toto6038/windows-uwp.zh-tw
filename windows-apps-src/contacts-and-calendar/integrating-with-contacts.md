---
author: normesta
description: 說明如何在連絡人卡片中動作的旁邊新增您的 app
MSHAttr: PreferredLib:/library/windows/apps
title: 將應用程式連結到連絡人卡片上的動作
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 連絡人, 連絡人卡片, 註解
ms.assetid: 0edabd9c-ecfb-4525-bc38-53f219d744ff
ms.localizationpriority: medium
ms.openlocfilehash: eb1c01a4fe370f899da185dc39b7d3abe6a1904e
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6853971"
---
# <a name="connect-your-app-to-actions-on-a-contact-card"></a>將應用程式連結到連絡人卡片上的動作

您的 app 可以顯示在連絡人卡片或迷你連絡人卡片上的動作旁邊。 使用者可以選擇您的 app 來執行動作，例如開啟個人資料頁面、撥打電話，或傳送訊息。

![連絡人卡片與迷你連絡人卡片](images/all-contact-cards.png)

若要開始，請尋找現有的連絡人或建立新的連絡人。 接下來，請建立「註解」** 和幾個封裝資訊清單項目，以描述您的 app 支援哪些動作。 然後，撰寫執行動作的程式碼。

如需更完整的範例，請參閱[連絡人卡片整合範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)。

## <a name="find-or-create-a-contact"></a>尋找或建立連絡人

如果您的 app 可協助連絡人與其他人聯繫，請搜尋 Windows 以尋找連絡人，然後為他們加上註解。 如果您的 app 可管理連絡人，您可以將他們新增到 Windows 連絡人清單，然後為他們加上註解。

### <a name="find-a-contact"></a>尋找連絡人

使用姓名、電子郵件地址或電話號碼來尋找連絡人。

```cs
ContactStore contactStore = await ContactManager.RequestStoreAsync();

IReadOnlyList<Contact> contacts = null;

contacts = await contactStore.FindContactsAsync(emailAddress);

Contact contact = contacts[0];
```

### <a name="create-a-contact"></a>建立連絡人

如果您的 app 比較像通訊錄，請建立連絡人，然後將他們新增到連絡人清單。

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

## <a name="tag-each-contact-with-an-annotation"></a>使用註解來標記每個連絡人

使用您的 app 可以執行 (例如：視訊通話與傳訊) 的動作 (作業) 清單來標記每個連絡人。

然後，將連絡人的識別碼與您 app 內部用來識別連絡人的識別碼相關聯。

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

annotation.SupportedOperations = ContactAnnotationOperations.Message |
  ContactAnnotationOperations.AudioCall |
  ContactAnnotationOperations.VideoCall |
 ContactAnnotationOperations.ContactProfile;

await annotationList.TrySaveAnnotationAsync(annotation);
```

## <a name="register-for-each-operation"></a>登錄每項作業

在您的封裝資訊清單中，登錄您在註解中列出的每項作業。

透過將通訊協定處理常式新增到資訊清單的 ``Extensions`` 元素來登錄。

```xml
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-contact-profile">
      <uap:DisplayName>TestProfileApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-ipmessaging">
      <uap:DisplayName>TestMsgApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-voip-video">
      <uap:DisplayName>TestVideoApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-voip-call">
      <uap:DisplayName>TestCallApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
</Extensions>
```
您也可以在 Visual Studio 中資訊清單設計工具的 **\[宣告\]** 索引標籤中新增這些處理常式。

![資訊清單設計工具的 [宣告] 索引標籤](images/manifest-designer-protocols.png)

## <a name="find-your-app-next-to-actions-in-a-contact-card"></a>在連絡人卡片中動作的旁邊尋找您的 app

開啟連絡人 app。 您的 app 會顯示在您於註解和封裝資訊清單中指定的每個動作 (作業) 旁邊。

![連絡人卡片](images/a-contact-card.png)

如果使用者針對某項動作選擇您的 app，它就會在使用者下次開啟連絡人卡片時顯示為該動作的預設 app。

## <a name="find-your-app-next-to-actions-in-a-mini-contact-card"></a>在迷你連絡人卡片中動作的旁邊尋找您的 app

在迷你連絡人卡片中，您的 app 會顯示在代表動作的索引標籤中。

![迷你連絡人卡片](images/mini-contact-card.png)

像**郵件**這樣的 app 會開啟迷你連絡人卡片。 您的 app 也可以開啟它們。 這個程式碼會示範做法。

```cs
public async void OpenContactCard(object sender, RoutedEventArgs e)
{
    // Get the selection rect of the button pressed to show contact card.
    FrameworkElement element = (FrameworkElement)sender;

    Windows.UI.Xaml.Media.GeneralTransform buttonTransform = element.TransformToVisual(null);
    Windows.Foundation.Point point = buttonTransform.TransformPoint(new Windows.Foundation.Point());
    Windows.Foundation.Rect rect =
        new Windows.Foundation.Rect(point, new Windows.Foundation.Size(element.ActualWidth, element.ActualHeight));

   // helper method to find a contact just for illustrative purposes.
    Contact contact = await findContact("contoso@contoso.com");

    ContactManager.ShowContactCard(contact, rect, Windows.UI.Popups.Placement.Default);

}
```

若要查看更多使用迷你連絡人卡片的範例，請參閱[連絡人卡片範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCards)。

就像連絡人卡片一樣，每個索引標籤都會記住使用者上次使用的 app，以方便使用者繼續使用您的 app。

## <a name="perform-operations-when-users-select-your-app-in-a-contact-card"></a>在使用者於連絡人卡片中選取您的 app 時執行作業

覆寫您 **App.cs** 檔案中的 [Application.OnActivated](https://msdn.microsoft.com/library/windows/apps/br242330) 方法，並將使用者導覽至您應用程式中的頁面。 [連絡人卡片整合範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)會說明這樣做的其中一種方法。

在頁面後端檔案的程式碼中，複寫 [Page.OnNavigatedTo](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.onnavigatedto.aspx) 方法。 連絡人卡片會透過此方法傳遞作業名稱與連絡人識別碼。

若要開始視訊或語音通話，請參閱此範例：[VoIP 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/VoIP)。 您將會在 [WIndows.ApplicationModel.Calls](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.calls.aspx) 命名空間中發現完整的 API。

若要加快傳訊速度，請參閱 [Windows.ApplicationModel.Chat](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.chat.aspx) 命名空間。

您也可以啟動另一個 app。 這就是此程式碼所執行的動作。

```cs
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    var args = e.Parameter as ProtocolActivatedEventArgs;
    // Display the result of the protocol activation if we got here as a result of being activated for a protocol.

    if (args != null)
    {
        var options = new Windows.System.LauncherOptions();
        options.DisplayApplicationPicker = true;

        options.TargetApplicationPackageFamilyName = “ContosoApp”;

        string launchString = args.uri.Scheme + ":" + args.uri.Query;
        var launchUri = new Uri(launchString);
        await Windows.System.Launcher.LaunchUriAsync(launchUri, options);
    }
}
```

```args.uri.scheme``` 屬性包含作業的名稱，```args.uri.Query``` 屬性則包含使用者的識別碼。
