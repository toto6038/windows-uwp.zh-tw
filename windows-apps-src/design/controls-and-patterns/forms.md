---
Description: 在 UWP 應用程式的表單的版面配置指導方針。
title: 表單
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, fluent
ms.openlocfilehash: 8a57f13e168a248569bca1beeceed7b4f6c89f69
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658153"
---
# <a name="forms"></a>表單
表單是一組控制項來收集並送出使用者的資料。 表單通常會用於設定 頁面中，建立帳戶和其他更多的問卷。 

這篇文章討論建立 XAML 配置表單的設計方針。

![表單的範例](images/PivotHeader.png)

## <a name="when-should-you-use-a-form"></a>您應該在何時使用表單？
表單是將專用的頁面來收集會清楚地彼此相關的資料輸入。 當您需要明確地從使用者收集資料時，您應該使用表單。 您可以建立使用者的表單：
- 登入帳戶
- 註冊帳戶
- 變更應用程式設定，例如隱私權或顯示選項
- 填寫調查問卷
- 購買項目
- 提供意見反應

## <a name="types-of-forms"></a>類型的表單

當考慮如何提交並顯示使用者輸入，有兩種類型的表單：

### <a name="1-instantly-updating"></a>1.立即更新
![設定頁面](images/control-examples/toggle-switch-news.png)

當您想要立即查看結果，變更表單中的值的使用者時，請使用立即更新的表單。 比方說，在 [設定] 頁面中，會顯示目前的選取項目，並選取項目所做的變更會立即套用。 若要確認您的應用程式中的變更，您必須[加入事件處理常式](controls-and-events-intro.md)每個輸入的控制項。 如果使用者變更輸入的控制項，然後能夠適當地回應您的應用程式。

### <a name="2-submitting-with-button"></a>2.提交按鈕
另一種格式可讓使用者選擇何時要送出 按鈕按一下的資料。

![行事曆新增新的事件 頁面](images/calendar-form.png)

這種類型的形式提供回應的使用者彈性。 一般而言，這種類型的形式包含更多的自由格式輸入的欄位，而因此接收的回應。 若要確保有效的使用者輸入和格式正確的資料，在提交時，請考慮下列建議：

- 讓它無法使用正確的控制項送出無效的資訊 （亦即，使用 CalendarDatePicker，而不是文字方塊的行事曆日期）。 查看更多有關在您稍後輸入控制項一節中的表單中選取適當的輸入的控制項。
- 當使用文字方塊控制項，提供使用者的所需的輸入格式與提示[PlaceholderText](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox.PlaceholderText)屬性。
- 提供具有適當的使用者螢幕小鍵盤所指出預期的輸入控制項的[InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscope)屬性。
- 標記所需的輸入，將以星號 * 標籤上。
- 停用 [提交] 按鈕，直到所有必要的資訊填入。
- 如果有無效的資料，提交後，將標記與反白顯示的欄位或框線，無效的輸入控制項，並要求使用者重新提交表單。
- 對於其他錯誤，例如網路連線失敗，請務必向使用者顯示適當的錯誤訊息。 


## <a name="layout"></a>配置

若要簡化使用者體驗並確保使用者能夠輸入正確的輸入，考慮下列建議設計的表單的版面配置。 

### <a name="labels"></a>標籤
[標籤](labels.md)應該靠左對齊，並放置在輸入控制項上方。 許多控制項都有內建的標頭屬性，來顯示標籤。 對於沒有 Header 屬性的控制項，或是要對一組控制項加上標籤，則可改用 [TextBlock](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.TextBlock)。

若要[協助工具設計](../accessibility/accessibility.md)，加上標籤的所有個人和群組的這兩種人類看得清楚和螢幕助讀程式的控制項。 

對 字型樣式，使用預設[UWP 型別 ramp](../style/typography.md)。 使用`TitleTextBlockStyle`的頁面標題`SubtitleTextBlockStyle`群組標題和`BodyTextBlockStyle`控制項標籤。

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
若要以視覺化方式彼此分隔控制項群組，請使用[對齊、 邊界和邊框距離](../layout/alignment-margin-padding.md)。 個別的輸入的控制項高度的 80px 而且應該是等間距的 24px 分開的。 輸入的控制項群組應該是等間距的 48px 分開。

![表單群組](images/forms-groups.png)

### <a name="columns"></a>欄
建立資料行，可以減少不必要的泛空白字元，在表單中，特別是使用較大的螢幕大小。 不過，如果您想要建立多重資料行的表單，資料行數目的動作就應該取決於頁面上的輸入控制項的數目和應用程式視窗的螢幕大小。 而不會拖垮含有許多輸入控制項的畫面，請考慮建立多個頁面，為您的表單。  

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

### <a name="responsive-layout"></a>回應式配置
表單應調整大小在螢幕或視窗大小變更，因此使用者不會忽略任何輸入的欄位。 如需詳細資訊，請參閱 <<c0> [ 回應式設計技術](../layout/responsive-design.md)。 比方說，您可能要在檢視中，不論螢幕大小一律保留在表單的特定區域。

![form 焦點](images/forms-focus2.png)

### <a name="tab-stops"></a>定位停駐點
使用者可以使用鍵盤來瀏覽控制項[定位停駐點](../input/keyboard-interactions.md#tab-stops)。 根據預設，控制項的定位順序會反映在 XAML 中建立的順序。 若要覆寫預設行為，請變更**IsTabStop**或是**TabIndex**控制項的屬性。 

![控制項在表單上的索引標籤焦點](images/forms-focus1.png)

## <a name="input-controls"></a>輸入的控制項
輸入的控制項是允許使用者將資訊輸入表單的 UI 項目。 表單可以加入一些通用控制項下方列出的以及何時使用它們的相關資訊。

### <a name="text-input"></a>文字輸入
控制項 | 用法 | 範例
 - | - | -
[TextBox](text-box.md) | 擷取一或多個文字行 | 名稱、 自由格式的回應或意見反應
[PasswordBox](password-box.md) | 藉由隱藏字元收集私人資料 | 社會安全號碼 (SSN) 的密碼 Pin，信用卡資訊 
[AutoSuggestBox](auto-suggest-box.md) | 輸入一組對應的資料從建議清單顯示使用者 | 資料庫搜尋，郵寄給： 行，先前的查詢
[RichEditBox](rich-edit-box.md) | 編輯格式化的文字、 超連結與映像的文字檔 | 上傳檔案、 預覽及應用程式中編輯

### <a name="selection"></a>選項
控制項 | 用法 | 範例
- | - | - 
| [CheckBox](checkbox.md) | 選取或取消選取一或多個動作項目 | 同意條款及條件、 新增選用項目、 選取所有適用
[RadioButton](radio-button.md) | 從兩個或多個選項中選取其中一個選項 | 挑選出貨等方法的類型。
[ToggleSwitch](toggles.md) | 選擇其中兩個互斥的選項 | 開啟/關閉

> **注意**：如果有五個或多個選取項目，請使用清單控制項。

### <a name="lists"></a>清單
控制項 | 用法 | 範例
- | - | -
[ComboBox](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists.md#drop-down-lists) | 壓縮的狀態啟動，並展開以顯示可選取的項目清單 | 選取 從一長串的項目，例如州或國家/地區
[ListView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#list-views) | 分類項目，並指派群組標頭、 拖放項目、 規劃內容，並重新排列項目 | Rank 選項
[GridView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#grid-views) | 排列，並瀏覽映像為基礎的集合 | 挑選一張照片，色彩、 顯示佈景主題

### <a name="numeric-input"></a>輸入數字
控制項 | 用法 | 範例
- | - | -
[滑桿](slider.md) | 連續的數字值的範圍從選取的數字 | 百分比、 磁碟區、 播放速度
[評等](rating.md) | 使用顆星評分 | 客戶回函

### <a name="date-and-time"></a>日期和時間

控制項 | 用法 
- | - 
[CalendarView](calendar-view.md) | 挑選單一日期或從最上層顯示的行事曆的日期範圍 
[CalendarDatePicker](calendar-date-picker.md) | 挑選單一內容行事曆的日期 
[DatePicker](date-picker.md) | 挑選單一當地語系化的日期時內容的資訊並不重要
[TimePicker](time-picker.md) | 挑選單一時間值

### <a name="additional-controls"></a>其他控制項 
為 UWP 控制項的完整清單，請參閱[函式的控制項索引](controls-by-function.md)。

對於更複雜和自訂 UI 控制項，看看可從公司的 UWP 資源這類[Telerik](https://www.telerik.com/)， [SyncFusion](https://www.syncfusion.com/products/uwp)， [DevExpress](https://www.devexpress.com/Products/NET/Controls/Win10Apps/)， [Infragistics](https://www.infragistics.com/products/universal-windows-platform)， [ComponentOne](https://www.componentone.com/Studio/Platform/UWP)，以及[ActiPro](https://www.actiprosoftware.com/products/controls/universal)。

## <a name="one-column-form-example"></a>一個資料行表單範例
這個範例會使用 Acrylic[主從式](master-details.md)[清單檢視](lists.md)並[NavigationView](navigationview.md)控制項。
![另一個 「 表單 」 範例的螢幕擷取畫面](images/FormExample2.png)
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

## <a name="two-column-form-example"></a>表單的兩個資料行範例
這個範例會使用[Pivot](pivot.md)控制項， [Acrylic](../style/acrylic.md)背景，以及[CommandBar](app-bars.md)除了輸入控制項。
![Form 範例的螢幕擷取畫面](images/FormExample.png)
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
![客戶訂單資料庫螢幕擷取畫面](images/customerorderform.png)以了解如何將表單輸入以連接**Azure**資料庫中，請參閱完全實作的表單中，請參閱[客戶訂單資料庫](https://github.com/Microsoft/Windows-appsample-customers-orders-database)應用程式範例。

## <a name="related-topics"></a>相關主題
- [輸入的控制項](controls-and-events-intro.md)
- [Typography](../style/typography.md)
