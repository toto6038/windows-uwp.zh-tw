---
title: 新增朋友圈支援至應用程式
description: 說明如何將朋友圈支援新增至應用程式，及如何釘選與取消釘選連絡人
ms.date: 06/28/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f54cb261f6ef94545d656d5bd4f624622cc6dfff
ms.sourcegitcommit: dafda665fd3d25136194e452e7500b5bab076638
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/06/2019
ms.locfileid: "71982225"
---
# <a name="adding-my-people-support-to-an-application"></a>新增朋友圈支援至應用程式

> [!Note]
> 從 Windows 10 5 月2019更新（版本1903）中，新的 Windows 10 安裝預設不會再顯示「工作列中的人員」。 客戶可以在工作列上按一下滑鼠右鍵，然後按 [顯示工作列上的人員]，來啟用此功能。 不鼓勵開發人員將我的人員支援新增至他們的應用程式，而且應該流覽[Windows 開發人員的 Blog](https://blogs.windows.com/windowsdeveloper/) ，以取得優化 windows 10 應用程式的詳細資訊。

\[朋友圈\] 功能可讓使用者從應用程式直接將連絡人釘選到其工作列，這會建立一個可供使用者透過數種方式互動的新連絡人物件。 本文說明如何新增此功能的支援，讓使用者直接從您的應用程式釘選連絡人。 當釘選連絡人時，如[朋友圈分享](my-people-sharing.md)和[通知](my-people-notifications.md)等新類型的使用者互動便可供使用。

![朋友圈聊天](images/my-people-chat.png)

## <a name="requirements"></a>需求

+ Windows 10 和 Microsoft Visual Studio 2019。 如需安裝詳細資訊，請參閱[開始設定 Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up)。
+ C# 或類似物件導向程式設計語言的基本知識。 若要開始使用 C#，請參閱[建立 "Hello, world" 應用程式](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)。

## <a name="overview"></a>總覽

若要讓您的應用程式能夠使用 \[朋友圈\] 功能時，您必須完成三件事：

1. [在您的應用程式資訊清單中宣告 Windows.sharetarget 啟用合約的支援。](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#declaring-support-for-the-share-contract)
2. [標注使用者可以使用您的應用程式共用的連絡人。](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#annotating-contacts)
3.  支援同時執行您應用程式的多個執行個體。 使用者將您應用程式的完整版用於連絡人面板中時，必須能夠與該版本互動。  他們甚至可以同時將該版本用於多個連絡人面板中。  若要支援此功能，您的應用程式必須能夠同時執行多個檢視。 若要了解做法，請參閱[顯示應用程式的多重檢視](https://docs.microsoft.com/windows/uwp/design/layout/show-multiple-views) (英文) 一文。

當您完成時，您的應用程式將出現在所註解連絡人的連絡人面板中。

## <a name="declaring-support-for-the-contract"></a>宣告合約的支援

若要宣告支援朋友圈合約，請以 Visual Studio 開啟您的應用程式。 在 \[方案總管\] 中，以滑鼠右鍵按一下 \[Package.appxmanifest\]，然後選取 \[開啟方式\]。 從功能表中，選取 \[XML (文字) 編輯器\]，然後按一下 \[確定\]。 對資訊清單進行以下變更：

**之前**

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10">

    <Applications>
        <Application Id="MyApp"
          Executable="$targetnametoken$.exe"
          EntryPoint="My.App">
        </Application>
    </Applications>

```

**之後**

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4">

    <Applications>
        <Application Id="MyApp"
          Executable="$targetnametoken$.exe"
          EntryPoint="My.App">
          <Extensions>
            <uap4:Extension Category="windows.contactPanel" />
          </Extensions>
        </Application>
    </Applications>

```

透過新增上述內容，現在可透過 **windows.ContactPanel** 合約啟動您的應用程式，這讓您能夠與連絡人面板互動。

## <a name="annotating-contacts"></a>註解連絡人

若要允許您應用程式中的連絡人透過 \[朋友圈\] 顯示在工作列中，您必須將這些連絡人寫入 Windows 連絡人存放區。  若要了解如何寫入連絡人，請參閱[連絡人卡片範例](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration) (英文)。

您的應用程式必須也對每位連絡人寫下註解。 註解是您應用程式中與連絡人相關聯的些許資料。 註解必須包含對應到您在其 **ProviderProperties** 成員中所需檢視的可啟用類別，並宣告支援 **ContactProfile** 作業。

當您的應用程式正在執行時，您可以隨時為連絡人加上註解，但通常只要連絡人一新增至 Windows 連絡人存放區，您就應該註解連絡人。

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and contact panel support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactPanelAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations.ContactProfile;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation));
}
```

“appId” 是套件系列名稱，後面加上 ‘!’ 及可啟用類別識別碼。 若要尋找您的套件系列名稱，請使用預設的編輯器開啟 **Package.appxmanifest**，然後尋找 \[封裝\] 索引標籤。在此，“App” 是對應到應用程式啟動檢視的可啟用類別。

## <a name="allow-contacts-to-invite-new-potential-users"></a>允許連絡人邀請新的潛在使用者

根據預設，只有您特別註解的連絡人其連絡人面板中才會顯示您的應用程式。  這是為了避免與無法透過您應用程式互動的連絡人混淆。  如果您想要對您應用程式不知道的連絡人顯示您的應用程式 (例如邀請使用者將該連絡人新增至其帳戶)，您可以將以下內容新增到您的資訊清單：

**之前**

```Csharp
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
      <Extensions>
        <uap4:Extension Category="windows.contactPanel" />
      </Extensions>
    </Application>
</Applications>
```

**之後**

```Csharp
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
      <Extensions>
        <uap4:Extension Category="windows.contactPanel">
            <uap4:ContactPanel SupportsUnknownContacts="true" />
        </uap4:Extension>
      </Extensions>
    </Application>
</Applications>
```

透過這項變更，您的應用程式在連絡人面板中會對使用者已釘選的所有連絡人顯示成可用的選項。  當使用連絡人面板合約啟用您的應用程式時，您應該檢查以了解連絡人是否是您應用程式知道的連絡人。  如果不是，您應該會顯示您應用程式的新使用者體驗。

![朋友圈連絡人面板](images/my-people.png)

## <a name="support-for-email-apps"></a>支援電子郵件應用程式

如果您正在撰寫電子郵件應用程式，便不需要手動註解每位連絡人。  如果您宣告支援連絡人面板和 mailto: protocol，您的應用程式將自動為有電子郵件地址的使用者顯示。

## <a name="running-in-the-contact-panel"></a>在連絡人面板中執行

現在您的應用程式會顯示在部分或所有使用者的連絡人面板中，您必須透過連絡人面板合約處理啟用。

```Csharp
override protected void OnActivated(IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.ContactPanel)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        var rootFrame = new Frame();

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;

        // Navigate to the page that shows the Contact UI.
        rootFrame.Navigate(typeof(ContactPage), e);

        // Ensure the current window is active
        Window.Current.Activate();
    }
}
```

當您的應用程式透過此合約啟用後，將會收到 [ContactPanelActivatedEventArgs 物件](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.activation.contactpanelactivatedeventargs)。  這包含了您應用程式在啟動時不斷嘗試互動的連絡人其識別碼，及 [ContactPanel](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactpanel) 物件。 您應保留對此 ContactPanel 物件的參照，這可讓您與面板互動。

ContactPanel 物件具有兩個您應用程式應聆聽的事件：
+ 當使用者已叫用 UI 元素且該元素要求整個應用程式以其自己的視窗啟動時，便會傳送 **LaunchFullAppRequested** 事件。  您的應用程式負責自我啟動，傳遞所有必要內容。  您可以任意地依您想要的式進行 (例如透過啟動通訊協定)。
+ 當您的應用程式即將關閉時，便會傳送 **Closing event**，讓您可以儲存任何內容。

ContactPanel 物件也允許您設定連絡人面板標頭的背景色彩 (若未設定，則將預設為系統佈景主題)，並以程式設計方式關閉連絡人面板。

## <a name="supporting-notification-badging"></a>支援通知徽章

如果您希望有來自您應用程式且與該連絡人相關的新通知時，釘選到工作列的連絡人可以收到徽章通知，則您必須在 [快顯通知](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)和易懂的[朋友圈通知](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-notifications)中包含 **hint-people** 參數。

![朋友圈通知徽章](images/my-people-badging.png)

若要將聯絡人加上徽章，最上層的快顯通知節點必須包含 hint-people 參數，以指出傳送或相關連絡人。 這個參數可以有以下任何的值：
+ **電子郵件地址** 
    + 例如 mailto:johndoe@mydomain.com
+ **電話號碼** 
    + 例如 tel:888-888-8888
+ **遠端識別碼** 
    + 例如 remoteid:1234

以下是如何找出快顯通知與特定連絡人相關的範例：
```XML
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastText01">
            <text>John Doe posted a comment.</text>
        </binding>
    </visual>
</toast>
```

> [!NOTE]
> 如果您的應用程式使用 [ContactStore API](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactstore) 並使用 [StoredContact.RemoteId](https://docs.microsoft.com/en-us/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId) 屬性將儲存在 PC 上的連絡人與遠端儲存的連絡人連結在一起，則 RemoteId 屬性的值必須是固定且唯一的。 這表示遠端識別碼必須能一致地識別單一使用者帳戶，且應包含唯一的標記以保證不會與 PC 上其他連絡人的遠端識別碼衝突，包括其他應用程式擁有的連絡人。
> 如果您應用程式使用的遠端識別碼不保證固定且唯一，您可以使用本主題稍後顯示的 RemoteIdHelper 類別，先將唯一標記新增至您所有的遠端識別碼，再新增到系統。 或者您可以選擇完全不使用 RemoteId 屬性，而是建立自訂延伸屬性，讓連絡人的遠端識別碼儲存在此屬性中。

## <a name="the-pinnedcontactmanager-class"></a>PinnedContactManager 類別

[PinnedContactManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager) 用於管理將哪些連絡人釘選到工作列上。 此類別可讓您釘選與取消釘選連絡人、判斷連絡人是否已釘選，並判定目前執行應用程式的系統是否支援釘選到特定介面上。

您可以使用 **GetDefault** 方法擷取 PinnedContactManager 物件：

```Csharp
PinnedContactManager pinnedContactManager = PinnedContactManager.GetDefault();
```

## <a name="pinning-and-unpinning-contacts"></a>釘選與取消釘選連絡人
現在您可以使用剛剛建立的 PinnedContactManager 釘選與取消釘選連絡人。 **RequestPinContactAsync** 與 **RequestUnpinContactAsync** 方法會將確認對話方塊提供給使用者，因此必須從應用程式單一執行緒 Apartment (ASTA，或 UI) 執行緒呼叫這些方法。

```Csharp
async void PinContact (Contact contact)
{
    await pinnedContactManager.RequestPinContactAsync(contact,
                                                      PinnedContactSurface.Taskbar);
}

async void UnpinContact (Contact contact)
{
    await pinnedContactManager.RequestUnpinContactAsync(contact,
                                                        PinnedContactSurface.Taskbar);
}
```

您可以同時釘選多個連絡人：

```Csharp
async Task PinMultipleContacts(Contact[] contacts)
{
    await pinnedContactManager.RequestPinContactsAsync(
        contacts, PinnedContactSurface.Taskbar);
}
```

> [!Note]
> 目前沒有批次作業可取消釘選連絡人。

**注意：** 

## <a name="see-also"></a>另請參閱
+ [朋友圈分享](my-people-sharing.md)
+ [我的人員通知](my-people-notifications.md)
+ [Channel 9 將我的人員支援新增至應用程式的影片](https://channel9.msdn.com/Events/Build/2017/P4056)
+ [我的人員整合範例](https://aka.ms/mypeoplebuild2017)
+ [連絡人卡片範例](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)
+ [PinnedContactManager 類別檔](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager)
+ [將應用程式連結到連絡人卡片上的動作](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/integrating-with-contacts)
