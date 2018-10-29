---
author: TylerMSFT
title: 啟動檔案的預設 app
description: 了解如何啟動檔案的預設應用程式。
ms.assetid: BB45FCAF-DF93-4C99-A8B5-59B799C7BD98
ms.author: twhitney
ms.date: 07/05/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 736018fbf966b547c3dd41e245149d498c1231e3
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "5747925"
---
# <a name="launch-the-default-app-for-a-file"></a>啟動檔案的預設應用程式

**重要 API**

-   [**Windows.System.Launcher.LaunchFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701461)

了解如何啟動檔案的預設 app。 許多應用程式需要使用它們本身無法處理的檔案。 例如，電子郵件 app 會收到各種類型的檔案，因此它們需要可以使用這些檔案類型的預設處理常式啟動這些檔案的方法。 這些步驟示範如何使用 [**Windows.System.Launcher**](https://msdn.microsoft.com/library/windows/apps/br241801) API 來啟動您 app 本身無法處理之檔案的預設處理常式。

## <a name="get-the-file-object"></a>取得檔案物件

首先，為檔案取得 [**Windows.Storage.StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 物件。

如果檔案包含在應用程式的套件中，您可以使用 [**Package.InstalledLocation**](https://msdn.microsoft.com/library/windows/apps/br224681) 屬性來取得 [**Windows.Storage.StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 物件，並使用 [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) 方法來取得 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 物件。

如果檔案位於已知資料夾中，您可以使用 [**Windows.Storage.KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) 類別的屬性來取得 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)，並使用 [**GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) 方法來取得 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 物件。

## <a name="launch-the-file"></a>啟動檔案

Windows 提供數個不同的選項來啟動檔案的預設處理常式。 這些選項在這個圖表和接下來的小節中有更詳細的說明。

| 選項 | 方法 | 說明 |
|--------|--------|-------------|
| 預設啟動 | [**LaunchFileAsync(IStorageFile)**](https://msdn.microsoft.com/library/windows/apps/hh701471) | 使用預設處理常式啟動指定的檔案。 |
| 開啟檔案啟動 | [**LaunchFileAsync(IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) | 啟動指定的檔案，讓使用者透過 [開啟檔案] 對話方塊挑選處理常式。 |
| 使用建議的 app 備用選項啟動 | [**LaunchFileAsync(IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) | 使用預設處理常式啟動指定的檔案。 如果系統上沒有安裝處理常式，則建議使用者使用市集中的應用程式。 |
| 以所需的剩餘檢視啟動 | [**LaunchFileAsync(IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) (僅限 Windows) | 使用預設處理常式啟動指定的檔案。 指定啟動後停留在畫面上的喜好設定，並要求特定視窗大小。 行動裝置系列上不支援 [**LauncherOptions.DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)。 |

### <a name="default-launch"></a>預設啟動

呼叫 [**Windows.System.Launcher.LaunchFileAsync(IStorageFile)**](https://msdn.microsoft.com/library/windows/apps/hh701471) 方法來啟動預設 app。 這個範例使用 [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) 方法來啟動影像檔案 test.png，它包含於 app 套件內。

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.png";
   
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);
   
   if (file != null)
   {
      // Launch the retrieved file
      var success = await Windows.System.Launcher.LaunchFileAsync(file);

      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()
   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.png"
   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)
   
   If file IsNot Nothing Then
      ' Launch the retrieved file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Launch the retrieved file
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Launch the retrieved file
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

### <a name="open-with-launch"></a>開啟檔案啟動

呼叫 [**Windows.System.Launcher.LaunchFileAsync(IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) 方法並將 [**LauncherOptions.DisplayApplicationPicker**](https://msdn.microsoft.com/library/windows/apps/hh701438) 設定為 **true**，以啟動使用者從 [**開啟檔案**] 對話方塊中選取的 app。

建議您在使用者不想選取特定檔案預設的 app 時，使用 [**開啟檔案**] 對話方塊。 例如，如果您的應用程式允許使用者啟動影像檔案，則預設處理常式可能是某個檢視器應用程式。 在某些情況下，使用者可能想編輯影像而非檢視影像。 使用 [**開啟檔案**] 選項搭配 [**AppBar**] 或操作功能表中的其他命令，可以讓使用者開啟 [**開啟檔案**] 對話方塊，並選取想在這類情況中使用的編輯器 app。

![啟動 .png 檔案的 [開啟檔案] 對話方塊。 對話方塊包含一個核取方塊，指定使用者的選擇應該用於所有 .png 檔案還是只用於一個 .png 檔案。 對話方塊包含四個 app 選項來啟動檔案以及一個 [更多選項] 連結。](images/checkboxopenwithdialog.png)

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
      string imageFile = @"images\test.png";
      
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the option to show the picker
      var options = new Windows.System.LauncherOptions();
      options.DisplayApplicationPicker = true;

      // Launch the retrieved file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()

   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.png"

   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)

   If file IsNot Nothing Then
      ' Set the option to show the picker
      Dim options = Windows.System.LauncherOptions()
      options.DisplayApplicationPicker = True

      ' Launch the retrieved file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the option to show the picker
        Windows::System::LauncherOptions launchOptions;
        launchOptions.DisplayApplicationPicker(true);

        // Launch the retrieved file
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the option to show the picker
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->DisplayApplicationPicker = true;

         // Launch the retrieved file
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

**使用建議的 app 備用選項啟動**

在某些情況下，使用者可能尚未安裝處理您要啟動之檔案的 app。 依照預設，Windows 處理這些情況的方法是提供連結，讓使用者在市集上搜尋適當的應用程式。 如果您想提供使用者在此情況下應取得什麼應用程式的特定建議，您可以在啟動檔案時一併傳送該建議。 若要這樣做，請呼叫 [**Windows.System.Launcher.launchFileAsync(IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) 方法，並將 [**LauncherOptions.PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482) 設為您想建議使用者使用的市集 app 套件系列名稱。 然後將 [**LauncherOptions.PreferredApplicationDisplayName**](https://msdn.microsoft.com/library/windows/apps/hh965481) 設為該應用程式的名稱。 Windows 將使用此資訊，以從市集取得建議 app 的特定選項，取代在市集中搜尋 app 的一般選項。

> [!NOTE]
> 您必須設定這兩個選項才能建議應用程式。 只設定其中一個將出現錯誤。

![啟動 .contoso 檔案的 [開啟檔案] 對話方塊。 因為 .contoso 未在電腦上安裝處理常式，所以對話方塊會包含一個選項，這個選項會包含市集圖示以及指示使用者市集中正確處理常式的文字。 對話方塊也包含 [更多選項] 連結。](images/howdoyouwanttoopen.png)

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.contoso";

   // Get the image file from the package's image directory
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the recommended app
      var options = new Windows.System.LauncherOptions();
      options.PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
      options.PreferredApplicationDisplayName = "Contoso File App";

      // Launch the retrieved file pass in the recommended app
      // in case the user has no apps installed to handle the file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()

   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.contoso"

   ' Get the image file from the package's image directory
   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)

   If file IsNot Nothing Then
      ' Set the recommended app
      Dim options = Windows.System.LauncherOptions()
      options.PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
      options.PreferredApplicationDisplayName = "Contoso File App";

      ' Launch the retrieved file pass in the recommended app
      ' in case the user has no apps installed to handle the file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the recommended app
        Windows::System::LauncherOptions launchOptions;
        launchOptions.PreferredApplicationPackageFamilyName(L"Contoso.FileApp_8wknc82po1e");
        launchOptions.PreferredApplicationDisplayName(L"Contoso File App");

        // Launch the retrieved file, and pass in the recommended app
        // in case the user has no apps installed to handle the file.
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.contoso"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the recommended app
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
         launchOptions->PreferredApplicationDisplayName = "Contoso File App";
         
         // Launch the retrieved file pass, and in the recommended app
         // in case the user has no apps installed to handle the file.
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

### <a name="launch-with-a-desired-remaining-view-windows-only"></a>以所需的剩餘檢視啟動 (僅限 Windows)

呼叫 [**LaunchFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701461) 的來源應用程式可要求在檔案啟動後停留在畫面上。 根據預設，Windows 會嘗試將所有可用空間平均分享給來源 app 與用來處理檔案的目標 app。 來源 app 可以使用 [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) 屬性，告知作業系統要讓 app 視窗佔用較多或較少可用空間。 也可以使用 **DesiredRemainingView**，指出來源 app 在檔案啟動後不需要停留在畫面上，且可由目標 app 完全取代。 這個屬性只會指定發出呼叫的 app 的慣用視窗大小。 它不會指定其他可能也同時在螢幕上之 app 的行為。

> [!NOTE]
> Windows 會考量多種不同因素，在判斷來源應用程式的最終視窗大小，例如，來源 app 的喜好設定，在畫面、 螢幕方向，以及等等的應用程式數目。 設定 [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) 並無法保證來源 App 的特定視窗行為。

**行動裝置系列：**[**LauncherOptions.DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)不支援行動裝置系列上。

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.png";
   
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the desired remaining view
      var options = new Windows.System.LauncherOptions();
      options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

      // Launch the retrieved file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the desired remaining view.
        Windows::System::LauncherOptions launchOptions;
        launchOptions.DesiredRemainingView(Windows::UI::ViewManagement::ViewSizePreference::UseLess);

        // Launch the retrieved file.
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the desired remaining view.
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->DesiredRemainingView = Windows::UI::ViewManagement::ViewSizePreference::UseLess;

         // Launch the retrieved file.
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

## <a name="remarks"></a>備註

您的 app 不能選取已啟動的 app。 使用者決定要啟動哪個 app。 使用者可選取通用 Windows 平台 (UWP) app 或 Windows 傳統型應用程式。

啟動檔案時，您的 app 必須是前景 app，也就是說，使用者必須看得到您的 app。 這項需求可讓使用者握有控制權。 為了滿足這項需求，請務必將所有檔案啟動直接繫結到您的應用程式 UI。 最好是讓使用者一律必須採取某些動作才能起始檔案啟動。

如果包含程式碼或指令碼的檔案類型 (例如 .exe、.msi 以及 .js 檔案) 是由作業系統自動執行，您就無法啟動這些檔案類型。 這項限制可以防止使用者遭受可能竄改作業系統的惡意檔案攻擊。 如果包含指令碼的檔案類型 (例如 .docx 檔案) 是由隔離指令碼的應用程式所執行，就可以使用這個方法來啟動。 Microsoft Word 之類的應用程式會防止 .docx 檔案中的指令碼修改作業系統。

如果您嘗試啟動受限制的檔案類型，則啟動將會失敗，並會叫用您的錯誤回呼。 如果您的應用程式處理許多不同類型的檔案，而您預期會碰到這個錯誤，建議您為使用者提供遞補做法。 例如，您可以為使用者提供將檔案儲存到桌面的選項，讓他們從桌面開啟檔案。

## <a name="related-topics"></a>相關主題

### <a name="tasks"></a>工作

* [啟動 URI 的預設 app](launch-default-app.md)
* [處理檔案啟用](handle-file-activation.md)

### <a name="guidelines"></a>指導方針

* [檔案類型與 URI 的指導方針](https://msdn.microsoft.com/library/windows/apps/hh700321)

### <a name="reference"></a>參考資料

* [**Windows.Storage.StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)
* [**Windows.System.Launcher.LaunchFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701461)
