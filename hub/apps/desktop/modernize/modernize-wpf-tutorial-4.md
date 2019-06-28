---
description: 本教學課程會示範如何新增 UWP XAML 使用者介面，建立 MSIX 套件，以及您的 WPF 應用程式中納入其他現代的元件。
title: 新增 Windows 10 使用者活動及通知
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、 uwp、 windows form、 wpf、 xaml 群島
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 8443ac25ba678986046b967a90a8899eaffb76aa
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420115"
---
# <a name="part-4-add-windows-10-user-activities-and-notifications"></a>第 4 部分：新增 Windows 10 使用者活動及通知

這是示範如何將範例 WPF 傳統型應用程式現代化名為 Contoso 費用的教學課程的第四個部分。 如需教學課程、 必要條件和指示，下載範例應用程式的概觀，請參閱[教學課程：將 WPF 應用程式現代化](modernize-wpf-tutorial.md)。 本文假設您已完成[第 3 部分](modernize-wpf-tutorial-3.md)。

在本教學課程的先前部分中，您新增 UWP XAML 控制項新增至應用程式使用 XAML 群島。 為這個產品，您也啟用應用程式，以呼叫任何的 WinRT API。 這會開啟應用程式使用 Windows 10，不只是 UWP XAML 控制項所提供的許多其他功能的機會。

在本教學課程的虛構案例中，Contoso 開發團隊決定應用程式中新增兩項新功能： 活動和通知。 此部分的教學課程會示範如何實作這些功能。

## <a name="add-a-user-activity"></a>新增使用者活動

在 Windows 10 應用程式可以追蹤例如開啟檔案，或特定的頁面會顯示使用者所執行的活動。 這些活動就可以供時間表，在 Windows 10 1803，版中引進的功能可讓使用者快速回到過去並繼續先前啟動的活動。

![Windows 時間表影像](images/wpf-modernize-tutorial/WindowsTimeline.png)

使用追蹤使用者活動[Microsoft Graph](https://developer.microsoft.com/graph/)。 不過，當您建置的 Windows 10 應用程式時，您不需要直接與 Microsoft Graph 提供的 REST 端點互動。 相反地，您可以使用一組方便 WinRT Api。 我們要在 Contoso 費用報銷應用程式中使用這些 WinRT Api，來追蹤每次使用者開啟應用程式內的經費支出，並讓使用者能夠建立活動使用自適性卡片。

### <a name="introduction-to-adaptive-cards"></a>自適性卡片的簡介

本節提供的簡短概觀[自適性卡片](https://docs.microsoft.com/adaptive-cards/)。 如果您不需要這項資訊，您可以略過這個，然後直接前往[加入 自適性卡片](#add-an-adaptive-card)指示。

自適性卡片讓開發人員以通用且一致的方式交換卡內容。 自適性卡片是由內容，其中可以包括文字、 影像、 動作和多個定義的 JSON 承載所描述。

自適性卡片會定義只要內容並沒有視覺外觀的內容。 在接收到自適性卡片的平台可以轉譯的內容，使用最適當的樣式。 自適性卡片的設計的方式是透過[轉譯器](https://docs.microsoft.com/adaptive-cards/rendering-cards/getting-started)，可接受 JSON 承載，並將它轉換成原生 UI。 比方說，UI 可能是 XAML 在 WPF 或 UWP 應用程式中，AXML for Android 的應用程式或 HTML 網站或 bot 聊天。

以下是簡單的自適性卡片裝載的範例。

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

下圖顯示此 JSON 呈現方式以不同的方式，由 ta 小組頻道、 Cortana 和 Windows 的通知。

![自適性卡片轉譯映像](images/wpf-modernize-tutorial/AdaptiveCards.png)

自適性卡片時間軸中扮演著重要角色，因為它是 Windows 呈現活動的方式。 顯示在時間軸內每個縮圖為實際自適性卡片。 因此，當您要建立您的應用程式內的使用者活動，系統會要求您提供自適性卡片來呈現它。

> [!NOTE]
> 腦力激盪的自適性卡片設計的好方法使用[線上的設計工具](https://adaptivecards.io/designer/)。 您必須設計具有建置組塊 （影像、 文字、 資料行等等） 的資訊卡，並取得相對應 JSON 的機會。 了解最後的設計之後，您可以使用一個稱為程式庫[自適性卡片](https://www.nuget.org/packages/AdaptiveCards/)讓您更輕鬆地建置您的自適性卡片使用C#類別，而不是純 JSON，這可能很難偵錯和建置。

### <a name="add-an-adaptive-card"></a>新增調適性的卡片

1. 以滑鼠右鍵按一下**ContosoExpenses.Core**方案總管 中專案，然後選擇**管理 NuGet 封裝**。

2. 在 [ **NuGet 套件管理員**] 視窗中，按一下**瀏覽**。 搜尋`Newtonsoft.Json`封裝並安裝最新的可用版本。 這是受歡迎的 JSON 操作程式庫，您會使用它來協助的 mainipulate 自適性卡片所需的 JSON 字串。

    ![NewtonSoft.Json NuGet 套件](images/wpf-modernize-tutorial/JsonNetNuGet.png)

    > [!NOTE]
    > 如果您未安裝`Newtonsoft.Json`個別封裝、 自適性卡片程式庫會參考較舊版本的`Newtonsoft.Json`不支援.NET Core 3.0 的封裝。

2. 在 [ **NuGet 套件管理員**] 視窗中，按一下**瀏覽**。 搜尋`AdaptiveCards`封裝並安裝最新的可用版本。

    ![自適性卡片 NuGet 封裝](images/wpf-modernize-tutorial/AdaptiveCardsNuGet.png)

3. 在 [**方案總管] 中**，以滑鼠右鍵按一下**ContosoExpenses.Core**專案，選擇**新增]-> [類別**。 將類別命名為**TimelineService.cs**然後按一下**確定**。

4. 在  **TimelineService.cs**檔案中，將下列陳述式新增至檔案頂端。

    ```csharp
    using AdaptiveCards;
    using ContosoExpenses.Data.Models;
    ```

5. 從檔案中宣告的命名空間的變更`ContosoExpenses.Core`至`ContosoExpenses`。

5. 將下列方法加入`TimelineService`類別。

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

這個方法會接收**費用**呈現費用和它相關的所有資訊的物件建置新**AdaptiveCard**物件。 方法會將下列新增至卡片：

- 標題，其使用成本的描述。
- 映像，也就是 contoso 公司標誌。
- 成本的數量。
- 成本的日期。

前 3 個項目會分割成兩個不同的資料行中，以便可以並排放置 Contoso 標誌及費用詳細資料。 建立物件之後，方法會傳回對應的 JSON 字串，借助**ToJson**方法。

### <a name="define-the-user-activity"></a>定義使用者活動

既然您已定義的自適性卡片，您可以建立以它為基礎的使用者活動。

1. 將下列陳述式加入至頂端**TimelineService.cs**檔案：

    ```csharp
    using Windows.ApplicationModel.UserActivities;
    using System.Threading.Tasks;
    using Windows.UI.Shell;
    ```

    > [!NOTE]
    > 這些是 UWP 命名空間。 這些解析，因為`Microsoft.Toolkit.Wpf.UI.Controls`您在步驟 2 中安裝的 NuGet 封裝包含的參考`Microsoft.Windows.SDK.Contracts`套件，可讓**ContosoExpenses.Core**專案參考的 WinRT Api，即使它是.NET核心 3 專案。

2. 下列欄位將宣告加入至`TimelineService`類別。

    ```csharp
    private UserActivityChannel _userActivityChannel;
    private UserActivity _userActivity;
    private UserActivitySession _userActivitySession;
    ```

3. 將下列方法加入`TimelineService`類別。

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

4. 變更儲存到**TimelineService.cs**。

#### <a name="about-the-code"></a>關於程式碼

`AddToTimeline`方法會先取得**UserActivityChannel** ，才能儲存使用者活動的物件。 然後它會建立新的使用者活動使用**GetOrCreateUserActivityAsync**方法，它需要的唯一識別碼。 如此一來，如果活動已存在，應用程式可以更新它;否則，它會建立一個新。 要傳遞的識別項取決於您要建置的應用程式類型：

* 如果您想要一律更新相同的活動，讓時間軸只會顯示最新，您可以使用固定識別項 (例如**費用**)。
* 如果您想要追蹤的另一為每個活動，以便在時間軸會顯示所有人都所示，您可以使用動態識別碼。

在此案例中，應用程式將會追蹤每個開啟的費用為不同的使用者活動，讓程式碼會建立每個識別項使用關鍵字**費用-** 後面費用的唯一識別碼。

此方法會建立之後**UserActivity**物件，它會填入下列資訊的物件：

* **ActivationUri**當使用者按一下時間軸中的活動上叫用。 程式碼會使用稱為的自訂通訊協定**contosoexpenses**應用程式將稍後處理。
* **VisualElements**物件，其中包含一組定義活動的視覺外觀的屬性。 此程式碼會設定**3** （這是顯示在時間軸中的項目上方的標題） 和**內容**。 

這是您稍早定義的自適性卡片其中扮演的角色。 應用程式會傳遞您設計的自適性卡片先前做為內容的方法。 不過，Windows 10 會使用不同的物件來代表相較於所使用的卡片`AdaptiveCards`NuGet 套件。 因此，此方法會重新建立卡片使用**CreateAdaptiveCardFromJson**方法所公開**AdaptiveCardBuilder**類別。 此方法會建立使用者活動之後，它會儲存活動，並建立新的工作階段。

當使用者按一下時間軸中的活動時**contosoexpenses: / /** 通訊協定將會啟動，且 URL 會包含應用程式必須擷取所選的費用資訊。 為選擇性的工作中，您可以實作通訊協定啟用，讓應用程式適當地回應在使用者使用時間軸。

### <a name="integrate-the-application-with-timeline"></a>整合應用程式與時間軸

既然您已經建立互動的類別與時間軸，我們可以開始使用它來增強應用程式的經驗。 若要使用的最佳位置**AddToTimeline**方法所公開**TimelineService**類別是當使用者開啟費用的詳細資料頁面。

1. 在  **ContosoExpenses.Core**專案中，展開**Viewmodel**資料夾，然後開啟**ExpenseDetailViewModel.cs**檔案。 這是支援的費用詳細資料視窗中的 ViewModel。

2. 找出的公用建構函式**ExpenseDetailViewModel**類別，並在建構函式的結尾新增下列程式碼。 只要開啟 [費用] 視窗中，此方法會呼叫**AddToTimeline**方法並傳遞目前的費用。 **TimelineService**類別會使用此資訊來建立使用者活動使用的費用資訊。

    ```csharp
    TimelineService timeline = new TimelineService();
    timeline.AddToTimeline(expense);
    ```

    當您完成時，建構函式應該如下所示。

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

3. 按 F5 以建置並執行應用程式偵錯工具。 從清單中的員工，然後選擇 費用。 在詳細資料頁面上，記下的費用、 日期和數量的描述。

4. 按下**啟動 + TAB**開啟時間軸。

5. 向下捲動直到您看到一節目前開啟的應用程式清單**今天稍早已**。 本節說明一些最新的使用者活動。 按一下 **查看所有活動**連結**今天稍早已**標題。

6. 確認您看到新的卡片，您只選取應用程式中的費用的相關資訊。

    ![Contoso 費用時間軸](images/wpf-modernize-tutorial/ContosoExpensesTimeline.png)

7. 如果您現在開啟其他費用，您會看到新的卡片會新增為使用者的活動。 請記住，此程式碼會使用不同的識別碼，每個活動，因此它會建立針對每個費用的卡片您開啟應用程式中。

8. 關閉應用程式。

## <a name="add-a-notification"></a>新增通知

第二個 Contoso 開發小組想要新增的功能是，每當新的費用儲存至資料庫時，會將對使用者顯示的通知。 若要這樣做，您可以利用內建通知系統的 Windows 10，這公開給開發人員透過 WinRT Api。 此通知系統有許多優點：

- 通知會與 OS 的其餘部分一致的。
- 也就是可採取動作。
- 它們取得儲存在行動作業中心，因此它們可以稍後檢閱。

若要新增至應用程式的通知：

1. 在 [**方案總管] 中**，以滑鼠右鍵按一下**ContosoExpenses.Core**專案，選擇**新增]-> [類別**。 將類別命名為**NotificationService.cs**然後按一下**確定**。

2. 在  **NotificationService.cs**檔案中，將下列陳述式新增至檔案頂端。

    ```csharp
    using Windows.Data.Xml.Dom;
    using Windows.UI.Notifications;
    ```

3. 從檔案中宣告的命名空間的變更`ContosoExpenses.Core`至`ContosoExpenses`。

4. 將下列方法加入`NotificationService`類別。

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

    快顯通知會以 XML 承載，其中可以包括文字、 影像、 動作和更多。 您可以找到所有支援的項目[此處](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema)。 此程式碼，非常簡單的結構描述使用兩行文字： 標題和本文。 程式碼定義 XML 承載，並將它在載入後**XmlDocument**物件，它會包裝在 XML **ToastNotification**物件，並示範使用**ToastNotificationManager**類別。

5. 在  **ContosoExpenses.Core**專案中，展開**Viewmodel**資料夾，然後開啟**AddNewExpenseViewModel.cs**檔案。 

6. 找出`SaveExpenseCommand`方法，以儲存新的支出使用者按下按鈕時，會觸發。 下列程式碼加入之後呼叫這個方法，`SaveExpense`方法。

    ```csharp
    NotificationService notificationService = new NotificationService();
    notificationService.ShowNotification(expense.Description, expense.Cost);
    ```

    當您完成時`SaveExpenseCommand`方法應該看起來像這樣。

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

7. 按 F5 以建置並執行應用程式偵錯工具。 從清單中的員工，然後按一下**加入新的支出** 按鈕。 完成表單，然後按下的所有欄位**儲存**。

8. 您會收到下列例外狀況。

    ![快顯通知錯誤](images/wpf-modernize-tutorial/ToastNotificationError.png)

這個例外狀況會引起，Contoso 費用報銷應用程式還沒有套件識別資料。 某些 WinRT Api，包括通知 API，需要封裝識別碼，才能在應用程式中使用。 UWP 應用程式預設收到套件身分識別，因為它們只透過 MSIX 封裝散發。 其他類型的 Windows 應用程式，包括 WPF 應用程式，也能夠透過 MSIX 套件，以取得封裝識別碼。 本教學課程的下一個部分會探討如何執行這項操作。

## <a name="next-steps"></a>後續步驟

此時在教學課程中，您已成功新增使用者活動與 Windows 時間軸，整合的應用程式，您也已經加入至應用程式的使用者建立新的費用時，會觸發通知。 不過，因為應用程式需要使用通知 API 的套件識別通知無法尚未運作。 若要了解如何建置應用程式，以取得封裝識別碼，並獲得其他部署優勢 MSIX 套件，請參閱[第 5 部分：封裝和部署使用 MSIX](modernize-wpf-tutorial-5.md)。
