---
Description: A button gives the user a way to trigger an immediate action.
title: 連絡人卡片
ms.date: 03/07/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: kele
design-contact: tbd
dev-contact: tbd
doc-status: not-published
ms.localizationpriority: medium
ms.openlocfilehash: b04a8f616e9f6c7726a222f4a7264b9580ddee3a
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8211929"
---
# <a name="contact-card"></a>連絡人卡片

連絡人卡片會顯示[連絡人](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) (UWP 用來代表人員和企業的機制) 的連絡資訊，例如姓名、電話號碼和地址。  連絡人卡片也可讓使用者編輯連絡資訊。 您可以選擇顯示精簡的連絡人卡片，或是包含額外資訊的完整連絡人卡片。

> **重要 API**：[ShowContactCard 方法](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowFullContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_Foundation_Rect_)、[ShowFullContactCard 方法](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_ApplicationModel_Contacts_FullContactCardOptions_)、[IsShowContactCardSupported 方法](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported)、[Contact 類別](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact)  

有兩種顯示連絡人卡片的方式：  
* 做為標準連絡人卡片，出現在的飛出視窗中，並且可消失關閉 (在使用者按一下連絡人卡片外部時消失)。 
* 做為完整連絡人卡片，佔用大部分空間，但無法消失關閉 (使用者必須按一下 **\[關閉\]** 才能將其關閉)。 


<figure>
    <img src="images/contact-card/contact-card-standard.png" alt="The full contact card">
    <figcaption>標準連絡人卡片</figcaption>
</figure>

<figure>
    <img src="images/contact-card/contact-card-full.png" alt="The full contact card">
    <figcaption>完整連絡人卡片</figcaption>
</figure>


## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

當您想要顯示連絡人的連絡資訊時，請使用連絡人卡片。 如果您只想要顯示連絡人的名稱和圖片，請使用[個人圖片控制項](person-picture.md)。 


<!-- TODO: Add examples back when the contact card has been added. -->

<!-- ## Examples

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>If you have the <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> app installed, click here to <a href="xamlcontrolsgallery:/item/Button">open the app and see the Button in action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Get the source code (GitHub)</a></li>
    </ul>
</td>
</tr>
</table> -->

## <a name="show-a-standard-contact-card"></a>顯示標準連絡人卡片

1. 您通常會因為使用者按一下某個項目 (按鈕或者[個人圖片控制項](person-picture.md)) 而顯示連絡人卡片。 我們並不想要隱藏元素。 為了避免隱藏，我們需要建立描述元素位置及大小的 [Rect](/uwp/api/windows.foundation.rect)。 

    我們來建立為我們這樣做的公用程式函式，稍後會用到。
    ```csharp
    // Gets the rectangle of the element 
    public static Rect GetElementRectHelper(FrameworkElement element) 
    { 
        // Passing "null" means set to root element. 
        GeneralTransform elementTransform = element.TransformToVisual(null); 
        Rect rect = elementTransform.TransformBounds(new Rect(0, 0, element.ActualWidth, element.ActualHeight)); 
        return rect; 
    } 

    ```

2. 呼叫 [ContactManager.IsShowContactCardSupported](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported) 方法，以判斷您是否可以顯示連絡人卡片。 如果不支援，則會顯示錯誤訊息  (此範例假設您要顯示連絡人卡片來回應按一下事件)。
    ```csharp
    // Contact and Contact Managers are existing classes 
    private void OnUserClickShowContactCard(object sender, RoutedEventArgs e) 
    { 
        if (ContactManager.IsShowContactCardSupported()) 
        { 

    ```

3. 使用您在步驟 1 建立的公用程式函式，取得引發事件的控制項範圍 (這樣連絡人卡片就不會蓋住它)。

    ```csharp
            Rect selectionRect = GetElementRect((FrameworkElement)sender); 
    ```

4. 取得您要顯示的 [Contact](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) 物件。 此範例只是建立簡單的連絡人，但您的程式碼應該擷取實際的連絡人。 

    ```csharp
                // Retrieve the contact to display
                var contact = new Contact(); 
                var email = new ContactEmail(); 
                email.Address = "jsmith@contoso.com"; 
                contact.Emails.Add(email); 
    ```
5. 呼叫 [ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowFullContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_Foundation_Rect_) 方法顯示連絡人卡片。 

    ```csharp
            ContactManager.ShowFullContactCard(
                contact, selectionRect, Placement.Default); 
        } 
    } 
    ```

以下是完整的程式碼範例：

```csharp
// Gets the rectangle of the element 
public static Rect GetElementRect(FrameworkElement element) 
{ 
    // Passing "null" means set to root element. 
    GeneralTransform elementTransform = element.TransformToVisual(null); 
    Rect rect = elementTransform.TransformBounds(new Rect(0, 0, element.ActualWidth, element.ActualHeight)); 
    return rect; 
} 
 
// Display a contact in response to an event
private void OnUserClickShowContactCard(object sender, RoutedEventArgs e) 
{ 
    if (ContactManager.IsShowContactCardSupported()) 
    { 
        Rect selectionRect = GetElementRect((FrameworkElement)sender);

        // Retrieve the contact to display
        var contact = new Contact(); 
        var email = new ContactEmail(); 
        email.Address = "jsmith@contoso.com"; 
        contact.Emails.Add(email); 
    
        ContactManager.ShowContactCard(
            contact, selectionRect, Placement.Default); 
    } 
} 

```

## <a name="show-a-full-contact-card"></a>顯示完整連絡人卡片

若要顯示完整的連絡人卡片，請呼叫 [ShowFullContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_ApplicationModel_Contacts_FullContactCardOptions_) 方法，而不是 [ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowFullContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_Foundation_Rect_)。

```csharp
private void onUserClickShowContactCard() 
{ 
   
    Contact contact = new Contact(); 
    ContactEmail email = new ContactEmail(); 
    email.Address = "jsmith@hotmail.com"; 
    contact.Emails.Add(email); 
 
 
    // Setting up contact options.     
    FullContactCardOptions fullContactCardOptions = new FullContactCardOptions(); 
 
    // Display full contact card on mouse click.   
    // Launch the People’s App with full contact card  
    fullContactCardOptions.DesiredRemainingView = ViewSizePreference.UseLess; 
     
 
    // Shows the full contact card by launching the People App. 
    ContactManager.ShowFullContactCard(contact, fullContactCardOptions); 
} 

```

## <a name="retrieving-real-contacts"></a>擷取「真實」的連絡人

本文中的範例會建立簡單的連絡人。 在實際應用程式中，您可能會想要擷取現有的連絡人。 如需相關指示，請參閱[連絡人和行事曆](/windows/uwp/contacts-and-calendar/)。




## <a name="related-articles"></a>相關文章
- [連絡人和行事曆](/windows/uwp/contacts-and-calendar/)
- [連絡人卡片範例](http://go.microsoft.com/fwlink/p/?LinkId=624040)
- [連絡人圖片控制項](/windows/uwp/controls-and-patterns/person-picture/)
