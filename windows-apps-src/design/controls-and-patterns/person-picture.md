---
author: mijacobs
description: 顯示個人的虛擬人偶影像 (如果有的話)。要是沒有，則顯示個人的縮寫名或一般字符。
title: 個人圖片控制項
template: detail.hbs
label: Parallax View
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: trestar
design-contact: kimsea
dev-contact: kefodero
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: eadc0763e7f99930bee3d2a388a881c52e89f609
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5839477"
---
# <a name="person-picture-control"></a>個人圖片控制項

個人圖片控制項會顯示個人的虛擬人偶影像 (如果有的話)。要是沒有，則顯示個人的縮寫名或一般字符。 您可以使用控制項來顯示 [Contact 物件](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact) (管理個人連絡資訊的物件)，也可以手動提供連絡人資訊，例如顯示名稱和設定檔圖片。  

> **重要 API**: [PersonPicture 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.personpicture)、[Contact 類別](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact)、[ContactManager 類別](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.ContactManager)

此圖例顯示兩個個人圖片控制項，伴隨兩個顯示使用者名稱的[文字區塊](text-block.md)元素。 
![個人圖片控制項](images/person-picture/person-picture_hero.png)


## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

當您想要表示一個人及其連絡人資訊時，請使用個人圖片。 例如，以下是您可能使用控制項的時機︰
* 顯示目前的使用者
* 顯示通訊錄中的連絡人
* 顯示訊息的寄件者 
* 顯示社交媒體連絡人

圖中顯示連絡人清單中的個人圖片控制項：![個人圖片控制項](images/person-picture/person-picture-control.png)

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/PersonPicture">開啟應用程式並查看 PersonPicture 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-the-person-picture-control"></a>如何使用個人圖片控制項

若要建立個人圖片，您可以使用 PersonPicture 類別。 此範例建立 PersonPicture 控制項，並手動提供個人的顯示名稱、設定檔圖片和縮寫名：

```xaml
<Page
    x:Class="App2.ExamplePage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App2"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <PersonPicture
            DisplayName="Betsy Sherman"
            ProfilePicture="Assets\BetsyShermanProfile.png"
            Initials="BS" />
    </StackPanel>
</Page>
```

## <a name="using-the-person-picture-control-to-display-a-contact-object"></a>使用個人圖片控制項來顯示的 Contact 物件

您可以使用人員選擇器控制項來顯示 [Contact](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact) 物件： 

```xaml
<Page
    x:Class="SampleApp.PersonPictureContactExample"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SampleApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <PersonPicture
            Contact="{x:Bind CurrentContact, Mode=OneWay}" />
            
        <Button Click="LoadContactButton_Click">Load contact</Button>
    </StackPanel>
</Page>
```

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;
using Windows.ApplicationModel.Contacts;

namespace SampleApp
{
    public sealed partial class PersonPictureContactExample : Page, System.ComponentModel.INotifyPropertyChanged
    {
        public PersonPictureContactExample()
        {
            this.InitializeComponent();
        }

        private Windows.ApplicationModel.Contacts.Contact _currentContact; 
        public Windows.ApplicationModel.Contacts.Contact CurrentContact
        {
            get => _currentContact;
            set
            {
                _currentContact = value;
                PropertyChanged?.Invoke(this,
                    new System.ComponentModel.PropertyChangedEventArgs(nameof(CurrentContact)));
            }

        }
        public event System.ComponentModel.PropertyChangedEventHandler PropertyChanged;

        public static async System.Threading.Tasks.Task<Windows.ApplicationModel.Contacts.Contact> CreateContact()
        {

            var contact = new Windows.ApplicationModel.Contacts.Contact();
            contact.FirstName = "Betsy";
            contact.LastName = "Sherman";

            // Get the app folder where the images are stored.
            var appInstalledFolder = 
                Windows.ApplicationModel.Package.Current.InstalledLocation;
            var assets = await appInstalledFolder.GetFolderAsync("Assets");
            var imageFile = await assets.GetFileAsync("betsy.png");
            contact.SourceDisplayPicture = imageFile;

            return contact;
        }

        private async void LoadContactButton_Click(object sender, RoutedEventArgs e)
        {
            CurrentContact = await CreateContact();
        }
    }
}
```

> [!NOTE]
> 為了保持程式碼簡單瞭解，此範例建立新的 Contact 物件。 在實際的 App 中，您可以讓使用者選取連絡人，或使用 [ContactManager](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.ContactManager) 來查詢連絡人清單。 如需擷取和管理連絡人的詳細資訊，請參閱[連絡人和行事曆文章](../../contacts-and-calendar/index.md)。 

## <a name="determining-which-info-to-display"></a>決定要顯示哪些資訊

當您提供 [Contact](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact) 物件時，個人圖片控制項會進行評估，判斷該物件可以顯示哪些資訊。 

如果有可用影像，控制項會依照下列順序，顯示找到的第一個影像：

1. LargeDisplayPicture
1. SmallDisplayPicture
1. 縮圖

您可以將 PreferSmallImage 屬性設定為 true 來變更影像。這樣會讓 SmallDisplayPicture 的優先順序高於 LargeDisplayPicture。

如果沒有影像，控制項會顯示連絡人的名稱或縮寫名。如果沒有任何名稱資料，控制項會顯示連絡人資料，例如電子郵件地址或電話號碼。 

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

* [連絡人和行事曆](../../contacts-and-calendar/index.md)
* [連絡人卡片範例](http://go.microsoft.com/fwlink/p/?LinkId=624040)
