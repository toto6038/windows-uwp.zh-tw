---
author: mcleblanc
description: 當您開始移植程序時，會有兩個選項。
title: 將 Windows 執行階段 8.x 專案移植到 UWP 專案
ms.assetid: 2dee149f-d81e-45e0-99a4-209a178d415a
---

# 將 Windows 執行階段 8.x 專案移植到 UWP 專案

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


當您開始移植程序時，會有兩個選項。 其中一個是編輯現有專案檔案的複本，包括應用程式套件資訊清單 (若要了解該選項，請參閱[將 app 移轉至通用 Windows 平台 (UWP)](https://msdn.microsoft.com/library/mt148501.aspx) 中關於更新您專案檔案的資訊)。 另一個選項是在 Visual Studio 中建立新的 Windows 10 專案，並將檔案複製到其中。 本主題的第一節會說明第二個選項，但本主題的其餘部分則會包含其他適用於這兩個選項的資訊。 您也可以選擇將新的 Windows 10 專案和您的現有專案保留在同一個方案中，並使用共用專案來共用原始程式碼檔案。 或者，您可以在方案中保留新專案，並在 Visual Studio 中使用連結檔案的功能共用原始程式碼檔案。

## 建立專案並將檔案複製到其中

這些步驟著重在 Visual Studio 中建立新的 Windows 10 專案，並將檔案複製到其中的選項。 一些有關您建立的專案數量以及要複製哪些檔案的特定內容，取決於[如果您有通用 8.1 應用程式](w8x-to-uwp-root.md#if-you-have-an-81-universal-windows-app)與其中各小節中所述的因素與決策。 這些步驟假設最簡單的情況。

1.  啟動 Microsoft Visual Studio 2015，並建立新的空白應用程式 (Windows 通用) 專案。 如需詳細資訊，請參閱[使用範本快速建立您的 Windows 市集應用程式 (C#、C++、Visual Basic)](https://msdn.microsoft.com/library/windows/apps/hh768232)。 新專案建置的應用程式套件 (appx 檔案) 將在所有裝置系列執行。
2.  在通用 8.1 應用程式專案中，找出您想要重複使用的所有原始程式碼檔案及視覺資產檔案。 使用 [檔案總管]，將資料模型、檢視模型、視覺資產、資源字典、資料夾結構，以及任何您想要重複使用的其他項目複製到新專案。 視需要在磁碟上複製或建立子資料夾。
3.  將檢視 (例如 MainPage.xaml 和 MainPage.xaml.cs) 一併複製到新專案。 同樣地，請視需要建立新的子資料夾，然後從專案移除現有的檢視。 但是在您覆寫或移除 Visual Studio 產生的檢視之前，請保留一份複本，因為可能稍後可用來供參考。 移植通用 8.1 應用程式的第一個階段著重在美化外觀以及能在裝置系列上運作良好。 稍後，您會將重點放在確認檢視能隨所有尺寸規格適當調整，也可以新增任何調適型程式碼，以充分利用特定的裝置系列。
4.  在 [方案總管]**** 中，確定 [顯示所有檔案]**** 已切換成開啟。 選取您複製的檔案，在這些檔案上按一下滑鼠右鍵，然後按一下 [加入至專案]****。 這將會自動包含它們的容器資料夾。 然後您可以視需要將 [**顯示所有檔案**] 切換成關閉。 如果您想要的話，也可以選擇替代的工作流程，就是先在 Visual Studio [**方案總管**] 中建立任何必要的子資料夾，然後使用 [**加入現有項目**]命令。 仔細檢查您視覺資產的 [建置動作]**** 是否已設定為 [內容]****，而 [複製到輸出目錄]**** 是否已設定為 [不要複製]****。
5.  您在這個階段可能會看到一些建置錯誤。 但如果您知道需要變更什麼，就可以使用 Visual Studio 的 **Find and Replace** 命令針對原始碼進行大量變更；還可以在 Visual Studio 的命令式程式碼編輯器中使用內容功能表上的 **Resolve** 和 **Organize Usings** 命令，對更多目標項目做出變更。

## 儘可能重複使用標記與程式碼

您將發現重構一些和/或新增調適型程式碼 (其會在下面說明)，可讓您儘可能重複使用可跨所有裝置系列運作的標記與程式碼。 以下是更多詳細資料。

-   所有裝置系列都通用的檔案不需要特殊考量。 那些檔案將由所有裝置系列上執行的應用程式使用。 這包括 XAML 標記檔案、命令式原始程式碼檔案以及資產檔案。
-   您的應用程式能夠偵測到執行的裝置系列，並瀏覽到專為該裝置系列設計的檢視。 如需詳細資訊，請參閱[偵測執行您 app 的平台](w8x-to-uwp-input-and-sensors.md#detecting-the-platform)。
-   有項可能很有用的類似技術，就是在別無他法時，為標記檔案或 **ResourceDictionary** 檔案 (或包含該檔案的資料夾) 指定特殊名稱，如此一來，只有在特定裝置系列執行您的 App 時，才會於執行階段自動載入。 此技術會在 [Bookstore1](w8x-to-uwp-case-study-bookstore1.md#an-optional-adjustment) 案例研究中說明。
-   如果您只需要支援 Windows 10，應該能移除通用 8.1 應用程式原始程式碼中的許多條件式編譯指示詞。 請參閱本主題中的[條件式編譯與調適型程式碼](#reviewing-conditional-compilation)。
-   若要使用並非所有裝置系列都適用的功能 (例如，印表機、掃描器或相機按鈕)，您可以撰寫調適型程式碼。 請參閱本主題中[條件式編譯與調適型程式碼](#reviewing-conditional-compilation)的第三個範例。
-   如果您想要支援 Windows 8.1、Windows Phone 8.1 及 Windows 10，可以將三個專案保留在同一個方案中，並使用共用專案共用程式碼。 或者，您可以在專案間共用原始程式碼檔案。 做法如下：在 Visual Studio 中，於 [方案總管]**** 中的專案上按一下滑鼠右鍵，選取 [加入現有項目]****，選取要共用的檔案，然後按一下 [加入做為連結]****。 將您的原始程式碼檔案儲存在檔案系統上的通用資料夾中，這是連結到那些檔案的專案可看見它們的資料夾。 同時別忘了將它們新增到原始檔控制。
-   若要在二進位層級重複使用，而不是在原始程式碼層級重複使用，請參閱[在 C# 和 Visual Basic 中建立 Windows 執行階段元件](http://msdn.microsoft.com/library/windows/apps/xaml/br230301.aspx)。 另外也有「可攜式類別庫」，這些類別庫支援適用於 Windows 8.1、Windows Phone 8.1 及 Windows 10 應用程式 (.NET Core) 的 .NET Framework 以及完整 .NET Framework 中提供的 .NET API 子集。 「可攜式類別庫」組件與這些平台皆二進位相容。 請使用 Visual Studio 來建立針對「可攜式類別庫」設計的專案。 請參閱[使用可攜式類別庫進行跨平台開發](http://msdn.microsoft.com/library/gg597391.aspx)。

## 擴充功能 SDK

通用 8.1 應用程式已經呼叫的大部分 Windows 執行階段 API 已經在稱為通用裝置系列的一組 API 中實作。 但是有些實作於擴充功能 SDK，而且 Visual Studio 只能辨識您的應用程式目標裝置系列或任何您參考的擴充功能 SDK 實作的 API。

如果您收到有關找不到命名空間或型別或成員的編譯錯誤，原因可能在此。 開啟 API 參考文件中的 API 主題，並瀏覽到＜需求＞一節：文中會告訴您實作裝置系列為何。 如果那不是您的目標裝置系列，則您必須擁有該裝置系列之擴充功能 SDK 的參考，您的專案才能使用 API。

按一下 [專案]**** &gt; [加入參考]**** &gt; [Windows 通用]**** &gt; [擴充功能]****，然後選取適當的擴充功能 SDK。 例如，如果您想要呼叫的 API 只能在行動裝置系列中使用且是在版本 10.0.x.y 引進，請選取 [適用於 UWP 的 Windows 行動擴充功能]****。

如此便會將下列參考加入您的專案檔案：

```XML
<ItemGroup>
    <SDKReference Include="WindowsMobile, Version=10.0.x.y">
        <Name>Windows Mobile Extensions for the UWP</Name>
    </SDKReference>
</ItemGroup>
```

名稱與版本號碼會和 SDK 安裝位置中的資料夾相符。 例如，上述資訊符合這個資料夾名稱：

`\Program Files (x86)\Windows Kits\10\Extension SDKs\WindowsMobile\10.0.x.y`

除非您的 app 是專為實作 API 的裝置系列所設計，否則必須在呼叫之前，使用 [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) 類別來測試該 API 是否存在 (這稱為調適型程式碼)。 每當您的應用程式執行時都將會評估此條件，但只會將有 API 存在的裝置評估為 true，也才可供呼叫。 務必在檢查通用 API 是否存在之後，才使用擴充功能 SDK 與調適型程式碼。 下一節會提供一些範例。

另請參閱[應用程式套件資訊清單](#appxpackage)。

## 條件式編譯與調適型程式碼

如果您使用條件式編譯 (搭配使用 C# 前置處理器指示詞)，以便您的程式碼檔案在 Windows 8.1 與 Windows Phone 8.1 上運作，有鑑於 Windows 10 中完成的交集工作，您現在可以檢閱所做的條件式編譯。 交集表示在 Windows 10 應用程式中，有些條件式可以全然移除。 其他有關執行階段檢查的變更，如下列範例所示。

**注意** 如果您想要以單一程式碼檔案支援 Windows 8.1、Windows Phone 8.1 及 Windows 10，您也可以執行這個動作。 如果您在專案的屬性頁面中查看 Windows 10 專案，您會看到專案將 WINDOWS\_UAP 定義為條件式編譯符號。 因此您能夠用來與 WINDOWS\_APP 及 WINDOWS\_PHONE\_APP 一起使用。 這些範例示範說明較簡單的情況，移除通用 8.1 應用程式的條件式編譯，然後取代為 Windows 10 應用程式的對等程式碼。

第一個範例示範 **PickSingleFileAsync** API (這只適用於 Windows 8.1) 與 **PickSingleFileAndContinue** API (這只適用於 Windows Phone 8.1) 的使用模式。

```csharp
#if WINDOWS_APP
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAsync
#else
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAndContinue
#endif // WINDOWS_APP
```

Windows 10 在 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) API 交集，因此您的程式碼可簡化如下：

```csharp
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAsync
```

我們在這個範例中處理硬體的返回按鈕—但僅限於 Windows Phone。

```csharp
#if WINDOWS_PHONE_APP
        Windows.Phone.UI.Input.HardwareButtons.BackPressed += this.HardwareButtons_BackPressed;
#endif // WINDOWS_PHONE_APP

...

#if WINDOWS_PHONE_APP
    void HardwareButtons_BackPressed(object sender, Windows.Phone.UI.Input.BackPressedEventArgs e)
    {
        // Handle the event.
    }
#endif // WINDOWS_PHONE_APP
```

在 Windows 10 中，返回按鈕事件是通用的概念。 實作於硬體或軟體的返回按鈕都會引發 [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596) 事件，因此要處理的就是這個事件。

```csharp
    Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested +=
        this.ViewModelLocator_BackRequested;

...

private void ViewModelLocator_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    // Handle the event.
}
```

最後的這個範例類似於前一個範例。 我們在此處理的是硬體相機按鈕—但同樣僅限編譯至 Windows Phone 應用程式套件的程式碼。

```csharp
#if WINDOWS_PHONE_APP
    Windows.Phone.UI.Input.HardwareButtons.CameraPressed += this.HardwareButtons_CameraPressed;
#endif // WINDOWS_PHONE_APP

...

#if WINDOWS_PHONE_APP
void HardwareButtons_CameraPressed(object sender, Windows.Phone.UI.Input.CameraEventArgs e)
{
    // Handle the event.
}
#endif // WINDOWS_PHONE_APP
```

在 Windows 10 中，硬體相機按鈕是行動裝置系列專屬的概念。 因為應用程式套件將在所有裝置上執行，所以我們將編譯階段條件變更為使用調適型程式碼的執行階段條件。 為此，我們使用 [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) 類別查詢執行階段是否有 [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) 類別存在。 **HardwareButtons** 定義在行動裝置的擴充功能 SDK，因此我們必須將該 SDK 的參照新增到專案，以供編譯這個程式碼。 不過請注意，處理常式只會在實作行動裝置擴充功能 SDK 中定義的裝置類型上執行，那就是行動裝置系列。 因此這個程式碼實質上等同於通用 8.1 程式碼，因為它很謹慎地僅使用現有的功能 (雖然是以不同的方式達成)。

```csharp
    // Note: Cache the value instead of querying it more than once.
    bool isHardwareButtonsAPIPresent = Windows.Foundation.Metadata.ApiInformation.IsTypePresent
        ("Windows.Phone.UI.Input.HardwareButtons");

    if (isHardwareButtonsAPIPresent)
    {
        Windows.Phone.UI.Input.HardwareButtons.CameraPressed +=
            this.HardwareButtons_CameraPressed;
    }

    ...

private void HardwareButtons_CameraPressed(object sender, Windows.Phone.UI.Input.CameraEventArgs e)
{
    // Handle the event.
}
```

另請參閱[偵測執行您 app 的平台](w8x-to-uwp-input-and-sensors.md#detecting-the-platform)。

## 應用程式套件資訊清單

[Windows 10 中的變更功能](https://msdn.microsoft.com/library/windows/apps/dn705793)主題會針對 Windows 10 列出對套件資訊清單結構描述參考所做的變更，包括已新增、移除及變更的元素。 如需結構描述中所有元素、屬性和類型的參考資訊，請參閱[元素階層](https://msdn.microsoft.com/library/windows/apps/dn934819)。 如果您正在移植 Windows Phone 市集應用程式，則需確保已移植應用程式資訊清單中的 **pm:PhoneIdentity** 元素符合您正在移植之 app 的應用程式資訊清單中的元素 (請參閱 [**pm:PhoneIdentity**](https://msdn.microsoft.com/library/windows/apps/dn934763) 主題，以取得完整詳細資料)。

您專案 (包括任何擴充功能 SDK 參考) 中的設定，會決定您應用程式能呼叫的 API 介面區。 但客戶實際上能夠透過市集安裝您應用程式的裝置集，是由您的應用程式套件資訊清單決定。 如需詳細資訊，請參閱 [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) 中的範例。

您可以編輯應用程式套件資訊清單，來設定各種宣告、功能及某些功能需要的其他設定。 您可以使用 Visual Studio 應用程式套件資訊清單編輯器來編輯它。 如果未顯示 [方案總管]****，請從 [檢視]**** 功能表選擇它。 按兩下 [Package.appxmanifest]****。 資訊清單編輯器視窗隨即開啟。 選取適當的索引標籤來進行變更，然後儲存。

下一個主題是[疑難排解](w8x-to-uwp-troubleshooting.md)。

## 相關主題

* [開發適用於通用 Windows 平台的應用程式](http://msdn.microsoft.com/library/dn975273.aspx)
* [使用範本快速建立您的 Windows 市集應用程式 (C#、C++、Visual Basic)。](https://msdn.microsoft.com/library/windows/apps/hh768232)
* [建立 Windows 執行階段元件](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [可攜式類別庫的跨平台開發](http://msdn.microsoft.com/library/gg597391.aspx)



<!--HONumber=May16_HO2-->


