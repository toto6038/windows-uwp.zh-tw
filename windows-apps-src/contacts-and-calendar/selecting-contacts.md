---
description: 在整個 Windows.ApplicationModel.Contacts 命名空間中，有好幾種方法可以用來選取連絡人。
title: 選取連絡人
ms.assetid: 35FEDEE6-2B0E-4391-84BA-5E9191D4E442
關鍵字：連絡人, 選取
關鍵字：選取單一連絡人
關鍵字：選取多個連絡人
關鍵字：連絡人, 選取多個
關鍵字：選取特定的連絡人資料
關鍵字：連絡人, 選取特定資料
關鍵字：連絡人, 選取特定欄位
---

# 選取連絡人

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


在整個 [**Windows.ApplicationModel.Contacts**](https://msdn.microsoft.com/library/windows/apps/BR225002) 命名空間中，有好幾種方法可以用來選取連絡人。 我們會在這裡示範如何選取單一連絡人或多位連絡人，也會示範如何設定連絡人選擇器，只抓取您 app 所需的連絡人資訊。

## 設定連絡人選擇器

建立一個 [**Windows.ApplicationModel.Contacts.ContactPicker**](https://msdn.microsoft.com/library/windows/apps/BR224913) 執行個體，並將它指派給一個變數。

```cs
var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
```

## 設定選取模式 (選擇性)

根據預設，連絡人選擇器會抓取使用者選取之連絡人的所有可用資料。 [
            **SelectionMode**](https://msdn.microsoft.com/library/windows/apps/BR224913-selectionmode) 屬性讓您設定連絡人選擇器只抓取您應用程式需要的資料欄位。 如果您只需要可用連絡人資料的子集，這是使用連絡人選擇器較有效率的方式。

首先，將 [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/BR224913-selectionmode) 屬性設定成 **Fields**：

```cs
contactPicker.SelectionMode = Windows.ApplicationModel.Contacts.ContactSelectionMode.Fields;
```

然後，使用 [**desiredFieldsWithContactFieldType**](https://msdn.microsoft.com/library/windows/apps/BR224913-desiredfieldswithcontactfieldtype) 屬性指定您想要連絡人選擇器抓取的欄位。 這個範例會設定連絡人選擇器抓取電子郵件地址：

``` cs
contactPicker.DesiredFieldsWithContactFieldType.Add(Windows.ApplicationModel.Contacts.ContactFieldType.Email);
```

## 啟動選擇器

```cs
Contact contact = await contactPicker.PickContactAsync();
```

如果您想讓使用者選取一或多位連絡人，請使用 [**pickContactsAsync**](https://msdn.microsoft.com/library/windows/apps/BR224913-pickcontactsasync)。

```cs
public IList&lt;Contact&gt; contacts;
contacts = await contactPicker.PickContactsAsync();
```

## 處理連絡人

當選擇器傳回時，檢查使用者是否已經選取任何連絡人。 如果有的話，處理連絡人資訊。

這個範例示範如何處理單一連絡人。 在這裡，我們將抓取連絡人的姓名並複製到名為 *OutputName* 的 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 控制項中。

```cs
if (contact != null)
{
    OutputName.Text = contact.DisplayName;
}
else
{
    rootPage.NotifyUser(&quot;No contact was selected.&quot;, NotifyType.ErrorMessage);
}
```

這個範例示範如何處理多位連絡人。

```cs
if (contacts != null &amp;&amp; contacts.Count &gt; 0)
{
    foreach (Contact contact in contacts)
    {
        // Do something with the contact information.
    }
}
```

## 完整範例 (單一連絡人)

這個範例使用連絡人選擇器來抓取單一連絡人的姓名，以及電子郵件地址、位置或電話號碼。

```cs
// ...
using Windows.ApplicationModel.Contacts;
// ...

private async void PickAContactButton-Click(object sender, RoutedEventArgs e)
{
    ContactPicker contactPicker = new ContactPicker();

    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.Email);
    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.Address);
    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.PhoneNumber);

    Contact contact = await contactPicker.PickContactAsync();

    if (contact != null)
    {
        OutputFields.Visibility = Visibility.Visible;
        OutputEmpty.Visibility = Visibility.Collapsed;

        OutputName.Text = contact.DisplayName;

        AppendContactFieldValues(OutputEmails, contact.Emails);
        AppendContactFieldValues(OutputPhoneNumbers, contact.Phones);
        AppendContactFieldValues(OutputAddresses, contact.Addresses);
    }
    else
    {
        OutputEmpty.Visibility = Visibility.Visible;
        OutputFields.Visibility = Visibility.Collapsed;
    }
}

private void AppendContactFieldValues&lt;T&gt;(TextBlock content, IList&lt;T&gt; fields)
{
    if (fields.Count &gt; 0)
    {
        StringBuilder output = new StringBuilder();

        if (fields[0].GetType() == typeof(ContactEmail))
        {
            foreach (ContactEmail email in fields as IList&lt;ContactEmail&gt;)
            {
                output.AppendFormat(&quot;Email: {0} ({1})\n&quot;, email.Address, email.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactPhone))
        {
            foreach (ContactPhone phone in fields as IList&lt;ContactPhone&gt;)
            {
                output.AppendFormat(&quot;Phone: {0} ({1})\n&quot;, phone.Number, phone.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactAddress))
        {
            List&lt;String&gt; addressParts = null;
            string unstructuredAddress = &quot;&quot;;

            foreach (ContactAddress address in fields as IList&lt;ContactAddress&gt;)
            {
                addressParts = (new List&lt;string&gt; { address.StreetAddress, address.Locality, address.Region, address.PostalCode });
                unstructuredAddress = string.Join(&quot;, &quot;, addressParts.FindAll(s =&gt; !string.IsNullOrEmpty(s)));
                output.AppendFormat(&quot;Address: {0} ({1})\n&quot;, unstructuredAddress, address.Kind);
            }
        }

        content.Visibility = Visibility.Visible;
        content.Text = output.ToString();
    }
    else
    {
        content.Visibility = Visibility.Collapsed;
    }
}
```

## 完整範例 (多位連絡人)

這個範例使用連絡人選擇器來抓取多位連絡人，然後將他們新增到名為 `OutputContacts` 的 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 控制項。

```cs
MainPage rootPage = MainPage.Current;
public IList&lt;Contact&gt; contacts;

private async void PickContactsButton-Click(object sender, RoutedEventArgs e)
{
    var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
    contactPicker.CommitButtonText = &quot;Select&quot;;
    contacts = await contactPicker.PickContactsAsync();

    // Clear the ListView.
    OutputContacts.Items.Clear();

    if (contacts.Count &gt; 0)
    {
        OutputContacts.Visibility = Windows.UI.Xaml.Visibility.Visible;
        OutputEmpty.Visibility = Visibility.Collapsed;

        foreach (Contact contact in contacts)
        {
            // Add the contacts to the ListView.
            OutputContacts.Items.Add(new ContactItemAdapter(contact));
        }
    }
    else
    {
        OutputEmpty.Visibility = Visibility.Visible;
    }         
}
```

``` cs
public class ContactItemAdapter
{
    public string Name { get; private set; }
    public string SecondaryText { get; private set; }

    public ContactItemAdapter(Contact contact)
    {
        Name = contact.DisplayName;
        if (contact.Emails.Count &gt; 0)
        {
            SecondaryText = contact.Emails[0].Address;
        }
        else if (contact.Phones.Count &gt; 0)
        {
            SecondaryText = contact.Phones[0].Number;
        }
        else if (contact.Addresses.Count &gt; 0)
        {
            List&lt;string&gt; addressParts = (new List&lt;string&gt; { contact.Addresses[0].StreetAddress, 
              contact.Addresses[0].Locality, contact.Addresses[0].Region, contact.Addresses[0].PostalCode });
              string unstructuredAddress = string.Join(&quot;, &quot;, addressParts.FindAll(s =&gt; !string.IsNullOrEmpty(s)));
            SecondaryText = unstructuredAddress;
        }
    }
}
```

## 摘要與後續步驟

現在您已基本了解如何使用連絡人選擇器來抓取連絡人資訊。 請從 GitHub 下載[通用 Windows app 範例](http://go.microsoft.com/fwlink/p/?linkid=619979)，以查看更多如何使用連絡人和連絡人選擇器的範例。



<!--HONumber=Mar16_HO1-->


