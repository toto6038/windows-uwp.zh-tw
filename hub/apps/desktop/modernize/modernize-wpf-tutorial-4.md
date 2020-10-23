---
description: 本教學課程示範如何將活動和通知功能新增至應用程式。
title: 新增 Windows 10 使用者活動及通知
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 3acc5638115932f6536eccb3be5e7222ef53fbb7
ms.sourcegitcommit: 0c4bbaf1c119a84002748cdcf02e1449835559c3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2020
ms.locfileid: "92133051"
---
# <a name="part-4-add-windows-10-user-activities-and-notifications"></a>第 4 部分：新增 Windows 10 使用者活動及通知

這是教學課程的第四部分，示範如何讓名為 Contoso Expenses 的範例 WPF 傳統型應用程式現代化。 如需教學課程概觀、必要條件和下載範例應用程式的指示，請參閱[教學課程：讓 WPF 應用程式現代化](modernize-wpf-tutorial.md)。 本文假設您已經完成[第 3 部分](modernize-wpf-tutorial-3.md)。

在本教學課程的前幾個部分，您已使用 XAML Islands 將 UWP XAML 控制項新增至應用程式。 其附帶產生的結果就是，您還啟用了應用程式來呼叫任何 WinRT API。 這讓應用程式有機會使用 Windows 10 所提供的許多其他功能，而不只是 UWP XAML 控制項。

在本教學課程的虛構案例中，Contoso 開發小組已決定將兩項新功能新增至應用程式：活動和通知。 本教學課程的這個部分會說明如何實作這些功能。

## <a name="add-a-user-activity"></a>新增使用者活動

在 Windows 10 中，應用程式可以追蹤使用者所執行的活動，例如開啟檔案或顯示特定頁面。 這些活動接著會透過「時間軸」提供，這是 Windows 10 1803 版引進的功能，可讓使用者快速地回到過去，並繼續他們先前開始的活動。

![Windows 時間軸映像](images/wpf-modernize-tutorial/WindowsTimeline.png)

使用 [Microsoft Graph](https://developer.microsoft.com/graph/) 可追蹤使用者活動。 不過，當您在建置 Windows 10 應用程式時，您不需要直接與 Microsoft Graph 所提供的 REST 端點互動。 相反地，您可以使用一組方便的 WinRT API。 我們將在 Contoso Expenses 應用程式中使用這些 WinRT API，以追蹤使用者每次在應用程式中開啟費用，並使用調適型卡片讓使用者建立活動。

### <a name="introduction-to-adaptive-cards"></a>調適型卡片簡介

本節提供[調適型卡片](/adaptive-cards/)的簡要概觀。 如果您不需要此資訊，可予以略過並直接前往[新增調適型卡片](#add-an-adaptive-card)指示。

調適型卡片可讓開發人員以通用且一致的方式交換卡片內容。 調適型卡片是由定義其內容的 JSON 承載所描述，其中可包含文字、影像、動作等等。

調適型卡片只定義內容，而不是內容的視覺化外觀。 接收調適型卡片的平台可以使用最適當的樣式來呈現內容。 調適型卡片是透過[轉譯器](/adaptive-cards/rendering-cards/getting-started)設計，該轉譯器可接受 JSON 承載並將其轉換成原生 UI。 例如，WPF 或 UWP 應用程式的 UI 可以是 XAML，AXML 適用於 Android 應用程式，或 HTML 適用於網站或 Bot 聊天。

以下是簡單調適型卡片承載的範例。

```json
{
    "type": "AdaptiveCard",
    "body": [
        {
            "type": "Container",
            "items": [
                {
                    "type": "TextBlock",
                    "size": "Medium",
                    "weight": "Bolder",
                    "text": "Publish Adaptive Card schema"
                },
                {
                    "type": "ColumnSet",
                    "columns": [
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "Image",
                                    "style": "Person",
                                    "url": "https://pbs.twimg.com/profile_images/3647943215/d7f12830b3c17a5a9e4afcc370e3a37e_400x400.jpeg",
                                    "size": "Small"
                                }
                            ],
                            "width": "auto"
                        },
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "TextBlock",
                                    "weight": "Bolder",
                                    "text": "Matt Hidinger",
                                    "wrap": true
                                },
                                {
                                    "type": "TextBlock",
                                    "spacing": "None",
                                    "text": "Created {{DATE(2017-02-14T06:08:39Z,SHORT)}}",
                                    "isSubtle": true,
                                    "wrap": true
                                }
                            ],
                            "width": "stretch"
                        }
                    ]
                }
            ]
        }
    ],
    "actions": [
        {
            "type": "Action.ShowCard",
            "title": "Set due date",
            "card": {
                "type": "AdaptiveCard",
                "style": "emphasis",
                "body": [
                    {
                        "type": "Input.Date",
                        "id": "dueDate"
                    },
                    {
                        "type": "Input.Text",
                        "id": "comment",
                        "placeholder": "Add a comment",
                        "isMultiline": true
                    }
                ],
                "actions": [
                    {
                        "type": "Action.OpenUrl",
                        "title": "OK",
                        "url": "http://adaptivecards.io"
                    }
                ],
                "$schema": "http://adaptivecards.io/schemas/adaptive-card.json"
            }
        },
        {
            "type": "Action.OpenUrl",
            "title": "View",
            "url": "http://adaptivecards.io"
        }
    ],
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.0"
}
```

下圖顯示如何經由 Teams 通道、Cortana 和 Windows 通知，以不同的方式轉譯此 JSON。

![調適型卡片轉譯映像](images/wpf-modernize-tutorial/AdaptiveCards.png)

調適型卡片在「時間軸」中扮演重要的角色，因為這是 Windows 轉譯活動的方式。 「時間軸」內顯示的每個縮圖實際上都是調適型卡片。 因此，當您要在應用程式內建立使用者活動時，系統會要求您提供調適型卡片來轉譯該活動。

> [!NOTE]
> 使用 [線上設計工具](https://adaptivecards.io/designer/)是集體討論調適型卡片設計的絕佳方式。 您將有機會使用建構元素 (影像、文字、資料行等) 來設計卡片，並取得對應的 JSON。 產生最終設計的想法之後，您可使用名為 [調適型卡片](https://www.nuget.org/packages/AdaptiveCards/)的程式庫，更輕鬆地使用 C# 類別 (而非一般 JSON，其可能會難以進行偵錯和建置) 來建立調適型卡片。

### <a name="add-an-adaptive-card"></a>新增調適型卡片

1. 以滑鼠右鍵按一下 [方案總管] 中的 [ContosoExpenses.Core]  專案，然後選擇 [管理 NuGet 套件]  。

2. 在 [NuGet 套件管理員]  視窗中，按一下 [瀏覽]  。 搜尋 `Newtonsoft.Json` 套件，並安裝最新的可用版本。 這是熱門的 JSON 操作程式庫，您將用來協助操作調適型卡片所需的 JSON 字串。

    ![Newtonsoft.JSON Nuget 套件](images/wpf-modernize-tutorial/JsonNetNuGet.png)

    > [!NOTE]
    > 如果您未個別安裝 `Newtonsoft.Json` 套件，則調適型卡片程式庫會參考不支援 .NET Core 3.0 的舊版 `Newtonsoft.Json` 套件。

2. 在 [NuGet 套件管理員]  視窗中，按一下 [瀏覽]  。 搜尋 `AdaptiveCards` 套件，並安裝最新的可用版本。

    ![調適型卡片 NuGet 套件](images/wpf-modernize-tutorial/AdaptiveCardsNuGet.png)

3. 在 [方案總管]  中，以滑鼠右鍵按一下 [ContosoExpenses.Core]  專案，然後選擇 [新增] -> [類別]  。 將類別命名為 **TimelineService.cs**，然後按一下 [確定]  。

4. 在 **TimelineService.cs** 檔案中，將下列陳述式新增至檔案頂端。

    ```csharp
    using AdaptiveCards;
    using ContosoExpenses.Data.Models;
    ```

5. 將檔案中宣告的命名空間從 `ContosoExpenses.Core` 變更為 `ContosoExpenses`。

5. 將下列方法新增至 `TimelineService` 類別。

   ```csharp
    private string BuildAdaptiveCard(Expense expense)
    {
        AdaptiveCard card = new AdaptiveCard("1.0");

        AdaptiveTextBlock title = new AdaptiveTextBlock
        {
            Text = expense.Description,
            Size = AdaptiveTextSize.Medium,
            Wrap = true
        };

        AdaptiveColumnSet columnSet = new AdaptiveColumnSet();
        AdaptiveColumn photoColumn = new AdaptiveColumn
        {
            Width = "auto"
        };

        AdaptiveImage image = new AdaptiveImage
        {
            Url = new Uri("https://appmodernizationworkshop.blob.core.windows.net/contosoexpenses/Contoso192x192.png"),
            Size = AdaptiveImageSize.Small,
            Style = AdaptiveImageStyle.Default
        };
        photoColumn.Items.Add(image);

        AdaptiveTextBlock amount = new AdaptiveTextBlock
        {
            Text = expense.Cost.ToString(),
            Weight = AdaptiveTextWeight.Bolder,
            Wrap = true
        };

        AdaptiveTextBlock date = new AdaptiveTextBlock
        {
            Text = expense.Date.Date.ToShortDateString(),
            IsSubtle = true,
            Spacing = AdaptiveSpacing.None,
            Wrap = true
        };

        AdaptiveColumn expenseColumn = new AdaptiveColumn
        {
            Width = "stretch"
        };
        expenseColumn.Items.Add(amount);
        expenseColumn.Items.Add(date);

        columnSet.Columns.Add(photoColumn);
        columnSet.Columns.Add(expenseColumn);

        card.Body.Add(title);
        card.Body.Add(columnSet);

        string json = card.ToJson();
        return json;
    }
    ```

#### <a name="about-the-code"></a>關於程式碼

此方法可接收 **Expense** 物件，其中包含要轉譯之費用的所有相關資訊，並建立新的 **AdaptiveCard** 物件。 此方法可將下列項目新增到卡片：

- 標題，其使用費用的描述。
- 影像，這是 Contoso 標誌。
- 費用的金額。
- 費用的日期。

最後 3 個元素會分割成兩個不同的資料行，如此一來，Contoso 標誌和費用詳細資料就可以並排放置。 建立物件之後，此方法會在 **ToJson** 方法的協助之下，傳回對應的 JSON 字串。

### <a name="define-the-user-activity"></a>定義使用者活動

您現在已定義調適型卡片，即可根據該卡片來建立使用者活動。

1. 將下列陳述式新增到 **TimelineService.cs** 檔案的頂端：

    ```csharp
    using Windows.ApplicationModel.UserActivities;
    using System.Threading.Tasks;
    using Windows.UI.Shell;
    ```

    > [!NOTE]
    > 這些都是 UWP 命名空間。 這些命名空間都會解析，因為您在步驟 2 中安裝的 `Microsoft.Toolkit.Wpf.UI.Controls` NuGet 套件包含 `Microsoft.Windows.SDK.Contracts` 套件的參考，其可讓 **ContosoExpenses.Core** 專案參考 WinRT API，即使是 .NET Core 3 專案也一樣。

2. 將下列欄位宣告新增到 `TimelineService` 類別。

    ```csharp
    private UserActivityChannel _userActivityChannel;
    private UserActivity _userActivity;
    private UserActivitySession _userActivitySession;
    ```

3. 將下列方法新增至 `TimelineService` 類別。

    ```csharp
    public async Task AddToTimeline(Expense expense)
    {
        _userActivityChannel = UserActivityChannel.GetDefault();
        _userActivity = await _userActivityChannel.GetOrCreateUserActivityAsync($"Expense-{expense.ExpenseId}");

        _userActivity.ActivationUri = new Uri($"contosoexpenses://expense/{expense.ExpenseId}");
        _userActivity.VisualElements.DisplayText = "Contoso Expenses";

        string json = BuildAdaptiveCard(expense);

        _userActivity.VisualElements.Content = AdaptiveCardBuilder.CreateAdaptiveCardFromJson(json);

        await _userActivity.SaveAsync();
        _userActivitySession?.Dispose();
        _userActivitySession = _userActivity.CreateSession();
    }
    ```

4. 將變更儲存至 **TimelineService.cs**。

#### <a name="about-the-code"></a>關於程式碼

`AddToTimeline` 方法會先取得儲存使用者活動所需的 **UserActivityChannel** 物件。 然後，其會使用 **GetOrCreateUserActivityAsync** 方法 (需要唯一識別碼) 來建立新的使用者活動。 如此一來，如果活動已經存在，應用程式即可加以更新；否則會建立新活動。 要傳遞的識別碼取決於您所建置的應用程式種類：

* 如果您想要一律更新相同的活動，讓「時間軸」只會顯示最新的活動，您可使用固定識別碼 (例如 **Expenses**)。
* 如果您想要以不同的方式追蹤每個活動，讓「時間軸」會顯示所有活動，您可使用動態識別碼。

在此案例中，應用程式會將每個開啟的費用視為不同的使用者活動來追蹤，所以程式碼會使用關鍵字 **Expense-** ，後面接著唯一的費用識別碼來建立每個識別碼。

在方法建立 **UserActivity** 物件之後，其會在物件中填入下列資訊：

* 當使用者按一下「時間軸」中的活動時所叫用的 **ActivationUri**。 此程式碼會使用名為 **contosoexpenses** 的自訂通訊協定，以供應用程式稍後處理。
* **VisualElements** 物件，其中包含一組可定義活動視覺外觀的屬性。 此程式碼會設定 **DisplayText** (這是顯示在「時間軸」中輸入頂端的標題) 和 **Content**。 

這是您稍早定義的調適型卡片扮演角色的地方。 應用程式會將您稍早設計的調適型卡片傳遞至方法。 不過，相較於 `AdaptiveCards` NuGet 套件所使用的物件，Windows 10 會使用不同的物件來代表卡片。 因此，此方法會使用 **AdaptiveCardBuilder** 類別所公開的 **CreateAdaptiveCardFromJson** 方法來重新建立卡片。 在方法建立使用者活動之後，其會儲存活動並建立新的工作階段。

當使用者按一下「時間軸」中的活動時，將會啟用 **contosoexpenses://** 通訊協定，而 URL 會包含應用程式擷取選定費用所需的資訊。 選擇性工作：您可實作通訊協定啟用，以便應用程式能在使用者使用「時間軸」時適當地回應。

### <a name="integrate-the-application-with-timeline"></a>整合應用程式與時間軸

您現已建立可與「時間軸」互動的類別，我們即可開始使用該類別來增強應用程式的體驗。 使用 **TimelineService** 類別所公開 **AddToTimeline** 方法的最佳位置，就是當使用者開啟費用的詳細資料頁面時。

1. 在 **ContosoExpenses.Core** 專案中，展開 **ViewModels** 資料夾，然後開啟 **ExpenseDetailViewModel.cs** 檔案。 這是可支援費用詳細資料視窗的 ViewModel。

2. 找出 **ExpenseDetailViewModel** 類別的公用建構函式，並在建構函式的結尾新增下列程式碼。 每當費用視窗開啟時，此方法就會呼叫 **AddToTimeline** 方法並傳遞目前的費用。 **TimelineService** 類別會使用此資訊來建立使用費用資訊的使用者活動。

    ```csharp
    TimelineService timeline = new TimelineService();
    timeline.AddToTimeline(expense);
    ```

    應如當您完成時，建構函式應該如下所示。

    ```csharp
    public ExpensesDetailViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        var expense = databaseService.GetExpense(storageService.SelectedExpense);

        ExpenseType = expense.Type;
        Description = expense.Description;
        Location = expense.Address;
        Amount = expense.Cost;

        TimelineService timeline = new TimelineService();
        timeline.AddToTimeline(expense);
    }
    ```

3. 按 F5 以在偵錯工具中建置和執行應用程式。 從清單中選擇員工，然後選擇一筆費用。 在詳細資料頁面中，記下費用的描述、日期和金額。

4. 按 [開始 + TAB]  以開啟「時間軸」。

5. 向下捲動目前開啟的應用程式清單，直到您看到標題為 [今日稍早]  的區段為止。 本節顯示一些最新的使用者活動。 按一下 [今日稍早]  標題旁的 [查看所有活動]  連結。

6. 確認您看到新的卡片，以及您剛才在應用程式中所選費用的相關資訊。

    ![Contoso Expenses 時間軸](images/wpf-modernize-tutorial/ContosoExpensesTimeline.png)

7. 如果您現在開啟其他費用，將會看到正新增為使用者活動的新卡片。 請記住，此程式碼會針對每個活動使用不同的識別碼，因此會針對您在應用程式中開啟的每筆費用建立卡片。

8. 關閉應用程式。

## <a name="add-a-notification"></a>新增通知

Contoso 開發小組想要新增的第二個功能，就是每當新費用儲存到資料庫時會向使用者顯示的通知。 若要這麼做，您可利用 Windows 10 內建的通知系統，其會透過 WinRT API 向開發人員公開。 此通知系統有許多優點：

- 通知與作業系統的其餘部分一致。
- 其可採取動作，
- 而且會儲存在 [控制中心]，以便稍後進行檢閱。

若要將通知新增至應用程式：

1. 在 [方案總管]  中，以滑鼠右鍵按一下 [ContosoExpenses.Core]  專案，然後選擇 [新增] -> [類別]  。 將類別命名為 **NotificationService.cs**，然後按一下 [確定]  。

2. 在 **NotificationService.cs** 檔案中，將下列陳述式新增至檔案頂端。

    ```csharp
    using Windows.Data.Xml.Dom;
    using Windows.UI.Notifications;
    ```

3. 將檔案中宣告的命名空間從 `ContosoExpenses.Core` 變更為 `ContosoExpenses`。

4. 將下列方法新增至 `NotificationService` 類別。

    ```csharp
    public void ShowNotification(string description, double amount)
    {
        string xml = $@"<toast>
                          <visual>
                            <binding template='ToastGeneric'>
                              <text>Expense added</text>
                              <text>Description: {description} - Amount: {amount} </text>
                            </binding>
                          </visual>
                        </toast>";

        XmlDocument doc = new XmlDocument();
        doc.LoadXml(xml);

        ToastNotification toast = new ToastNotification(doc);
        ToastNotificationManager.CreateToastNotifier().Show(toast);
    }
    ```

    快顯通知會以 XML 承載表示，其可包含文字、影像、動作等等。 您可以[在此](/windows/uwp/design/shell/tiles-and-notifications/toast-schema)找到所有支援的元素。 此程式碼會使用非常簡單的結構描述，其中包含兩行文字：標題和主體。 在程式碼定義 XML 承載並將其載入 **XmlDocument** 物件之後，就會將 XML 包裝在 **ToastNotification** 物件中，並使用 **ToastNotificationManager** 類別加以顯示。

5. 在 **ContosoExpenses.Core** 專案中，展開 **ViewModels** 資料夾，然後開啟 **AddNewExpenseViewModel.cs** 檔案。 

6. 找出 `SaveExpenseCommand` 方法，其會在使用者按下按鈕以儲存新費用時觸發。 將以下程式碼新增至這個方法，正好在 `SaveExpense` 方法的呼叫之後。

    ```csharp
    NotificationService notificationService = new NotificationService();
    notificationService.ShowNotification(expense.Description, expense.Cost);
    ```

    當您完成時，`SaveExpenseCommand` 方法應該如下所示。

    ```csharp
    private RelayCommand _saveExpenseCommand;
    public RelayCommand SaveExpenseCommand
    {
        get
        {
            if (_saveExpenseCommand == null)
            {
                _saveExpenseCommand = new RelayCommand(() =>
                {
                    Expense expense = new Expense
                    {
                        Address = Address,
                        City = City,
                        Cost = Cost,
                        Date = Date,
                        Description = Description,
                        EmployeeId = storageService.SelectedEmployeeId,
                        Type = ExpenseType
                    };

                    databaseService.SaveExpense(expense);

                    NotificationService notificationService = new NotificationService();
                    notificationService.ShowNotification(expense.Description, expense.Cost);

                    Messenger.Default.Send<UpdateExpensesListMessage>(new UpdateExpensesListMessage());
                    Messenger.Default.Send<CloseWindowMessage>(new CloseWindowMessage());
                }, () => IsFormFilled
                );
            }

            return _saveExpenseCommand;
        }
    }
    ```

7. 按 F5 以在偵錯工具中建置和執行應用程式。 從清單中選擇員工，然後按一下 [新增費用]  按鈕。 完成表單中的所有欄位，然後按 [儲存]  。

8. 您會收到下列例外狀況。

    ![快顯通知錯誤](images/wpf-modernize-tutorial/ToastNotificationError.png)

此例外狀況是由 Contoso Expenses 應用程式尚未擁有套件識別資料的這個事實所造成。 有些 WinRT API (包括通知 API) 必須先具備套件識別資料，才能在應用程式中使用。 UWP 應用程式預設會收到套件識別資料，因為其只能透過 MSIX 套件散發。 其他類型的 Windows 應用程式 (包括 WPF 應用程式) 也可透過 MSIX 套件來部署，以取得套件識別資料。 本教學課程的下一個部分將探討如何執行這項操作。

## <a name="next-steps"></a>接下來的步驟

目前在此教學課程中，您已成功將使用者活動新增至與 Windows 時間軸整合的應用程式，而且也將通知新增至使用者建立新費用時所觸發的應用程式。 不過，通知尚未運作，因為應用程式需要套件識別資料才能使用通知 API。 若要了解如何建置應用程式的 MSIX 套件，以取得套件識別資料並取得其他部署優點，請參閱[第 5 部分：使用 MSIX 進行封裝和部署](modernize-wpf-tutorial-5.md)。