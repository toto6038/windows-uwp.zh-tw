---
title: 朋友圈分享
description: 說明如何新增朋友圈分享的支援
author: muhsinking
ms.author: mukin
ms.date: 06/28/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7084c4dde7bdf2d59842a04fe9fd52bc029c264a
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "6199397"
---
# <a name="my-people-sharing"></a>朋友圈分享

「朋友圈」可讓使用者將連絡人釘選到其工作列上，讓他們能夠輕鬆地從 Windows 中的任何一處保持聯繫，無論其使用何種應用程式連線。 現在使用者可以將內容與其釘選的連絡人分享，做法是將檔案從檔案總管拖曳到其 \[朋友圈\] 釘選。 使用者也可以透過標準分享常用鍵分享給 Windows 連絡人存放區中的任何連絡人。 持續閱讀以了解如何讓您的應用程式成為朋友圈分享目標。

![朋友圈分享面板](images/my-people-sharing.png)

## <a name="requirements"></a>需求

+ Windows 10 和 Microsoft Visual Studio 2017。 如需安裝詳細資訊，請參閱[開始設定 Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up)。
+ C# 或類似物件導向程式設計語言的基本知識。 若要開始使用 C#，請參閱[建立 "Hello, world" 應用程式](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)。

## <a name="overview"></a>概觀

您必須採取三個步驟，才能讓您的應用程式成為朋友圈分享目標：

1. [宣告支援您應用程式資訊清單中的 shareTarget 啟用合約。](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#declaring-support-for-the-share-contract)
2. [為使用者可分享使用您應用程式的連絡人加上註解。](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#annotating-contacts)
3. 支援同時執行應用程式的多個執行個體。  使用者亦將您應用程式的完整版與他人分享時，必須能夠與該版本互動。 他們可以同時將該版本用於多個分享視窗中。 若要支援此功能，您的應用程式必須能夠同時執行多個檢視。 若要了解做法，請參閱[顯示應用程式的多重檢視](https://docs.microsoft.com/en-us/windows/uwp/layout/show-multiple-views) (英文) 一文。

當您已完成此作業時，您的應用程式將在 \[朋友圈分享\] 視窗中顯示成分享目標，該視窗的啟動方式有兩種：
1. 透過分享共用鍵選擇連絡人。
2. 將檔案拖放到釘選到工作列上的連絡人。

## <a name="declaring-support-for-the-share-contract"></a>宣告支援分享協定

若要宣告支援您的應用程式成為分享目標，請先在 Visual Studio 中開啟您的應用程式。 在 \[方案總管\]**** 中，以滑鼠右鍵按一下 \[Package.appxmanifest\]****，然後選取 \[開啟方式\]****。 從功能表中，選取 \[XML (文字) 編輯器\]****，然後按一下 \[確定\]****。 接著，對資訊清單進行以下變更：


**之前**
```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
    </Application>
</Applications>
```

**之後**

```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
        <Extensions>
            <uap:Extension Category="windows.shareTarget">
                <uap:ShareTarget Description="Share with MyApp">
                    <uap:SupportedFileTypes>
                        <uap:SupportsAnyFileType/>
                    </uap:SupportedFileTypes>
                    <uap:DataFormat>Text</uap:DataFormat>
                    <uap:DataFormat>Bitmap</uap:DataFormat>
                    <uap:DataFormat>Html</uap:DataFormat>
                    <uap:DataFormat>StorageItems</uap:DataFormat>
                    <uap:DataFormat>URI</uap:DataFormat>
                </uap:ShareTarget>
            </uap:Extension>
         </Extensions>
    </Application>
</Applications>
```

此程式碼會新增所有檔案與資料格式的支援，但您可以選擇指定支援哪些檔案類型與資料格式 (請參閱[ShareTarget 類別文件](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget) (英文) 以取得詳細資訊)。

## <a name="annotating-contacts"></a>註解連絡人

若要允許 \[朋友圈分享\] 視窗將您的應用程式顯示成您連絡人的分享目標，您必須將連絡人寫入 Windows 連絡人存放區。 若要了解如何寫入連絡人，請參閱[連絡人卡片整合範例](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration) (英文)。 

您的應用程式當分享給連絡人時如需顯示成朋友圈分享目標，則必須寫入該連絡人的註解中。 註解是您應用程式中與連絡人相關聯的些許資料。 註解必須包含對應到您在其 **ProviderProperties** 成員中所需檢視的可啟用類別，並宣告支援 **Share** 作業。

當您的應用程式正在執行時，您可以隨時為連絡人加上註解，但通常只要連絡人一新增至 Windows 連絡人存放區，您就應該註解連絡人。

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and Share support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactShareAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations::Share;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation);
}
```

“appId” 是套件系列名稱，後面加上 ‘!’ 及可啟用類別識別碼。 若要尋找您的套件系列名稱，開啟**Package.appxmanifest**使用預設的編輯器，然後尋找 \ [封裝 \] 索引標籤。在這裡，「 應用程式 」 是對應到分享目標檢視的可啟用類別。

## <a name="running-as-a-my-people-share-target"></a>做為朋友圈分享目標執行

最後，若要執行應用程式，請覆寫您應用程式主要類別中的 [OnShareTargetActivated](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Application#Windows_UI_Xaml_Application_OnShareTargetActivated_Windows_ApplicationModel_Activation_ShareTargetActivatedEventArgs_) 方法，以處理分享目標啟用。 [ShareTargetActivatedEventArgs.ShareOperation.Contacts](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation#Properties) 屬性將包含正要與其分享的連絡人，如果這是標準分享作業 (不是朋友圈分享) 則將為空白。

```Csharp
protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    bool isPeopleShare = false;
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
    {
        // Make sure the current OS version includes the My People feature before
        // accessing the ShareOperation.Contacts property
        isPeopleShare = (args.ShareOperation.Contacts.Count > 0);
    }

    if (isPeopleShare)
    {
        // Show share UI for MyPeople contact(s)
    }
    else
    {
        // Show standard share UI for unpinned contacts
    }
}
```

## <a name="see-also"></a>請參閱
+ [新增朋友圈支援](my-people-support.md)
+ [ShareTarget 類別](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget)
+ [連絡人卡片整合範例](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)
