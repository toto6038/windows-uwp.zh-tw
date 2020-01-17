---
Description: UWP 應用程式中表單的版面配置指導方針。
title: 表單
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, fluent
ms.openlocfilehash: e1a5b192ed57d3962b6ba4cbef69e3663bc1e2ec
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683991"
---
# <a name="forms"></a>表單
表單是一組控制項，會收集和提交使用者的資料。 表單通常用於 [設定] 頁面、問卷調查、建立帳戶等等。 

本文討論用於建立表單 XAML 版面配置的設計方針。

![表單範例](images/PivotHeader.png)

## <a name="when-should-you-use-a-form"></a>何時應該使用表單？
表單是一個專用頁面，用於收集彼此清楚相關的資料輸入。 當您需要明確地從使用者收集資料時，您應該使用表單。 您可以為使用者建立表單，以便：
- 登入帳戶
- 註冊帳戶
- 變更應用程式設定，例如隱私權或顯示選項
- 進行調查問卷
- 購買物品
- 提供意見反應

## <a name="types-of-forms"></a>表單類型

在考慮如何提交並顯示使用者輸入時，有兩種類型的表單：

### <a name="1-instantly-updating"></a>1.立即更新
![設定頁面](images/control-examples/toggle-switch-news.png)

當您希望使用者立即查看變更表單值的結果時，請使用立即更新的表單。 比方說，[設定] 頁面中會顯示目前的選取項目，而對選取項目所做的變更都會立即套用。 若要確認您應用程式中的變更，您必須在每個輸入控制項中[新增事件處理常式](controls-and-events-intro.md)。 如果使用者變更輸入控制項，您的應用程式即可適當地回應。

### <a name="2-submitting-with-button"></a>2.透過按鈕提交
另一種類型的表單可讓使用者選擇何時透過按一下按鈕來提交資料。

![行事曆新增新的事件頁面](images/calendar-form.png)

這類型的表單讓使用者具備回應彈性。 一般而言，這類型的表單包含更多自由表單輸入欄位，因此可接收更多樣的回應。 若要在提交時確保有效的使用者輸入和正格式確的資料，請考慮下列建議：

- 使用正確的控制項 (也就是，使用 CalendarDatePicker，而不是行事曆日期的 TextBox)，讓使用者無法提交無效的資訊。 在稍後的「輸入控制項」一節中，查看更多關於在表單中選取適當輸入控制項的資訊。
- 使用 TextBox 控制項時，利用 [PlaceholderText](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox.PlaceholderText) 屬性提供使用者所需輸入格式的提示。
- 以 [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscope) 屬性陳述預期的控制項輸入，以提供使用者適當的螢幕小鍵盤。
- 以標籤上的星號 * 標記必要的輸入。
- 停用提交按鈕，直到填入所有必要資訊為止。
- 如果提交時有無效的資料，請以亮顯的欄位或框線標記具有無效輸入的控制項，並要求使用者重新提交表單。
- 對於其他錯誤 (例如網路連線失敗)，請務必向使用者顯示適當的錯誤訊息。 


## <a name="layout"></a>版面配置

若要輔助使用者體驗並確保使用者能夠輸入正確的輸入，請考慮下列建議來設計表單的版面配置。 

### <a name="labels"></a>標籤
[標籤](labels.md)應該靠左對齊且置於輸入控制項上方。 許多控制項都具備可用來顯示標籤的內建 Header 屬性。 對於沒有 Header 屬性的控制項，或是要對一組控制項加上標籤，則可改用 [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)。

若要[進行協助工具設計](../accessibility/accessibility.md)，請標記所有個別控制項和控制項群組，讓人類讀者和螢幕助讀程式都能清楚明確地使用。 

對於字型樣式，請使用預設 [UWP 字體坡形](../style/typography.md)。 將 `TitleTextBlockStyle` 使用於頁面標題，將 `SubtitleTextBlockStyle` 使用於群組標題，以及將 `BodyTextBlockStyle` 使用於控制項標籤。

<div class="mx-responsive-img">
<table>
<tr><th>可行事項</th><th>禁止事項</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-shortform1col.png" alt="form with top labels"></td>
<td><img src="../controls-and-patterns/images/forms-leftlabel-donot1.png" alt="form with left labels don't"></td>
</tr>
</table>
</div>

### <a name="spacing"></a>間距
若要以視覺化方式讓控制項群組彼此分隔，請使用[對齊、邊界及邊框距離](../layout/alignment-margin-padding.md)。 個別輸入控制項的高度為 80px，且間距應該是 24px。 輸入控制項群組的間距應該是 48px。

![表單群組](images/forms-groups.png)

### <a name="columns"></a>欄
建立欄可減少表單中不必要的空格，特別是在使用的畫面較大時。 不過，如果您想要建立多欄的表單，欄數應該取決於頁面上的輸入控制項數目和應用程式視窗的畫面大小。 請考慮為您的表單建立多個頁面，而不要以許多輸入控制項淹沒畫面。  

<div class="mx-responsive-img">
<table>
<tr><th>可行事項</th><th>禁止事項</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-2cols.png" alt="form with 2 columns"></td>
<td><img src="../controls-and-patterns/images/forms-2cols-bad.png" alt="form with 2 bad columns"></td>
</tr>
<tr><td><img src="../controls-and-patterns/images/forms-3cols.png" alt="form with 3 columns"></td></tr>
</table>

</div>

### <a name="responsive-layout"></a>回應式版面配置
表單應隨著畫面或視窗大小變更來調整大小，使用者才不會忽略任何輸入欄位。 如需詳細資訊，請參閱[回應式設計技術](../layout/responsive-design.md)。 例如，不論畫面大小為何，您可能都想要在檢視中一律保留特定的表單區域。

![表單焦點](images/forms-focus2.png)

### <a name="tab-stops"></a>定位停駐點
使用者可以使用鍵盤來瀏覽具有[定位停駐點](../input/keyboard-interactions.md#tab-stops)的控制項。 根據預設，控制項的定位順序會反映其在 XAML 中建立的順序。 若要覆寫預設行為，請變更控制項的 **IsTabStop** 或 **TabIndex** 屬性。 

![表單中控制項的定位焦點](images/forms-focus1.png)

## <a name="input-controls"></a>輸入控制項
輸入控制項是允許使用者在表單中輸入資訊的 UI 元素。 以下列出可新增到表單的一些通用控制項，以及何時使用它們的相關資訊。

### <a name="text-input"></a>文字輸入
控制 | 用法 | 範例
 - | - | -
[TextBox](text-box.md) | 擷取一或多個文字行 | 名稱、自由格式回應或意見反應
[PasswordBox](password-box.md) | 藉由隱藏字元來收集私人資料 | 密碼、社會安全號碼 (SSN)、PIN、信用卡資訊 
[AutoSuggestBox](auto-suggest-box.md) | 當使用者輸入資料時，從一組對應的資料中顯示建議清單 | 資料庫搜尋、「收件者:」行、先前的查詢
[RichEditBox](rich-edit-box.md) | 編輯包含格式化文字、超連結及影像的文字檔 | 在應用程式中上傳檔案、預覽及編輯

### <a name="selection"></a>選項
控制 | 用法 | 範例
- | - | - 
| [CheckBox](checkbox.md) | 選取或取消選取一或多個動作項目 | 同意條款及條件、新增選用項目、選取所有適用的項目
[RadioButton](radio-button.md) | 從兩個以上的選項中選取一個選項 | 挑選類型、出貨方法等。
[ToggleSwitch](toggles.md) | 選擇兩個互斥選項之一 | 開啟/關閉

> **注意**：如果有五個或更多選取項目，請使用清單控制項。

### <a name="lists"></a>清單
控制 | 用法 | 範例
- | - | -
[ComboBox](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists.md#drop-down-lists) | 一開始為精簡狀態，展開可顯示可選取的項目清單 | 從一長串的項目中選取，例如州或國家/地區
[ListView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#list-views) | 將項目分類並指派群組標頭、拖放項目、規劃內容，以及重新排序項目 | 排名選項
[GridView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#grid-views) | 排列並瀏覽以影像為基礎的集合 | 挑選相片、色彩、顯示佈景主題

### <a name="numeric-input"></a>數字輸入
控制 | 用法 | 範例
- | - | -
[滑桿](slider.md) | 從連續數值範圍中選取數字 | 百分比、音量、播放速度
[評分](rating.md) | 使用星星評分 | 客戶意見反應

### <a name="date-and-time"></a>日期和時間

控制 | 用法 
- | - 
[CalendarView](calendar-view.md) | 從一律顯示的行事曆中挑選單一日期或日期範圍 
[CalendarDatePicker](calendar-date-picker.md) | 從關聯式行事曆中挑選單一日期 
[DatePicker](date-picker.md) | 如果關聯式資訊不重要，請挑選當地語系化的單一日期
[TimePicker](time-picker.md) | 挑選單一時間值

### <a name="additional-controls"></a>其他控制項 
如需 UWP 控制項的完整清單，請參閱[依函式排序的控制項索引](controls-by-function.md)。

如需更多複雜和自訂 UI 控制項，請查看各家公司所提供的 UWP 資源，例如 [Telerik](https://www.telerik.com/)、[SyncFusion](https://www.syncfusion.com/uwp-ui-controls)、[DevExpress](https://www.devexpress.com/Products/NET/Controls/Win10Apps/)、[Infragistics](https://www.infragistics.com/products/universal-windows-platform)、[ComponentOne](https://www.componentone.com/Studio/Platform/UWP) 及 [ActiPro](https://www.actiprosoftware.com/products/controls/universal)。

## <a name="one-column-form-example"></a>一欄表單範例
這個範例會使用 Acrylic [主要/詳細資料](master-details.md)[清單檢視](lists.md)和 [NavigationView](navigationview.md) 控制項。
![另一個表單範例的螢幕擷取畫面](images/FormExample2.png)
```xaml
<StackPanel>
    <TextBlock Text="New Customer" Style="{StaticResource TitleTextBlockStyle}"/>
    <Button Height="100" Width="100" Background="LightGray" Content="Add photo" Margin="0,24" Click="AddPhotoButton_Click"/>
    <TextBox x:Name="Name" Header= "Name" Margin="0,24,0,0" MaxLength="32" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
    <TextBox x:Name="PhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="15" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
    <TextBox x:Name="Email" Header="Email" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <RelativePanel>
        <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" MaxLength="50" Width="200" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
        <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0"  Width="100" RelativePanel.RightOf="City">
             <x:String>WA</x:String>
        </ComboBox>
    </RelativePanel>
    <TextBox x:Name="ZipCode" PlaceholderText="Zip Code" Margin="0,24,0,0" MaxLength="6" Width="100" HorizontalAlignment="Left" InputScope="Number" />
    <StackPanel Orientation="Horizontal">
        <Button Content="Save" Margin="0,24" Click="SaveButton_Click"/>
        <Button Content="Cancel" Margin="24" Click="CancelButton_Click"/>
    </StackPanel>  
</StackPanel>
```

## <a name="two-column-form-example"></a>兩欄表單範例
除了輸入控制項以外，這個範例會使用 [Pivot](pivot.md) 控制項、[Acrylic](../style/acrylic.md) 背景以及 [CommandBar](app-bars.md)。
![表單範例的螢幕擷取畫面](images/FormExample.png)
```xaml
<Grid>
    <Pivot Background="{ThemeResource SystemControlAccentAcrylicWindowAccentMediumHighBrush}" >
        <Pivot.TitleTemplate>
            <DataTemplate>
                <Grid>
                    <TextBlock Text="Company Name" Style="{ThemeResource HeaderTextBlockStyle}"/>
                </Grid>
            </DataTemplate>
        </Pivot.TitleTemplate>
        <PivotItem Header="Orders" Margin="0"/>    
        <PivotItem Header="Customers" Margin="0">
            <!--Form Example-->
            <Grid Background="White">
                <RelativePanel>
                    <StackPanel x:Name="Customer" Margin="20">
                        <TextBox x:Name="CustomerName" Header= "Customer Name" Margin="0,24,0,0" MaxLength="320" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
                        <TextBox x:Name="CustomerPhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
                        <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="AlphanumericFullWidth" />
                        <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="AlphanumericFullWidth" />
                        <RelativePanel>
                            <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" MaxLength="50" Width="200" HorizontalAlignment="Left" InputScope="Text" />
                            <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0"  Width="100" RelativePanel.RightOf="City">
                                <x:String>WA</x:String>
                            </ComboBox>
                        </RelativePanel>
                        <TextBox x:Name="ZipCode" PlaceholderText="Zip Code" Margin="0,24,0,0" MaxLength="6" Width="100" HorizontalAlignment="Left" InputScope="Number" />
                    </StackPanel>
                    <StackPanel x:Name="Associate" Margin="20" RelativePanel.RightOf="Customer">
                        <TextBox x:Name="AssociateName" Header= "Associate" Margin="0,24,0,0" MaxLength="320" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
                        <TextBox x:Name="AssociatePhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
                        <DatePicker x:Name="TargetInstallDate" Header="Target install Date" HorizontalAlignment="Left" Margin="0,24,0,0"></DatePicker>
                        <TimePicker x:Name="InstallTime" Header="Install Time" HorizontalAlignment="Left" Margin="0,24,0,0"></TimePicker>
                    </StackPanel>
                </RelativePanel>
            </Grid>
        </PivotItem>
        <PivotItem Header="Invoices"/>
        <PivotItem Header="Stock"/>
        <Pivot.RightHeader>
            <CommandBar OverflowButtonVisibility="Collapsed" Background="Transparent">
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit" />
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
            </CommandBar>
        </Pivot.RightHeader>
    </Pivot>
</Grid>
```

## <a name="customer-orders-database-sample"></a>客戶訂單資料庫範例
![客戶訂單資料庫螢幕擷取畫面](images/customerorderform.png) 若要了解如何將表單輸入連線到 **Azure** 資料庫並查看完全實作的表單，請參閱[客戶訂單資料庫](https://github.com/Microsoft/Windows-appsample-customers-orders-database)應用程式範例。

## <a name="related-topics"></a>相關主題
- [輸入控制項](controls-and-events-intro.md)
- [印刷樣式](../style/typography.md)
