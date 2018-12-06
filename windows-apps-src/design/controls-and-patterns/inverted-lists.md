---
Description: Use an inverted list to add new items at the bottom.
title: 反轉清單
label: Inverted lists
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 52c1d63d-69c1-48d6-a234-6f39296e4bfd
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 12f86c0d4f8980cea375b9a0a8a6876510c795b0
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8740056"
---
# <a name="inverted-lists"></a>反轉清單

 

您可以在聊天體驗中，使用清單檢視來呈現交談，並利用視覺上有明顯區別的項目來代表傳送者/接收者。  使用不同的色彩和水平對齊來區分傳送者/接收者的訊息，可協助使用者在交談中快速定位。

> **重要 API**：[ListView 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)、[ItemsStackPanel 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx)、[ItemsUpdatingScrollMode 屬性](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode.aspx)
 
您通常需要以由下而上 (不是由上而下) 的顯示方式來呈現清單。  當新訊息送達並新增至結尾時，先前的訊息會向上滑動，以便挪出空間讓使用者注意到最新送達的訊息。  不過，如果使用者已經往上捲動來檢視先前的回覆，則新訊息的送達不應造成視覺轉變來干擾他們的焦點。

![聊天 App 的反轉清單](images/listview-inverted.png)

## <a name="create-an-inverted-list"></a>建立反轉清單

若要建立反轉清單，使用清單檢視搭配 [ItemsStackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx) 做為其項目面板。 在 ItemsStackPanel 上，將 [ItemsUpdatingScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode.aspx) 設為 [KeepLastItemInView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsupdatingscrollmode.aspx)。

> [!IMPORTANT]
> 從 Windows 10 版本 1607 開始，已有 **KeepLastItemInView** 列舉值可供使用。 在舊版 Windows 10 上執行您的應用程式時，您無法使用這個值。

這個範例示範如何使清單檢視項目對齊底部，並指出對項目進行變更時，應將最後一個項目保持在檢視中。
 
 **XAML**
 ```xaml
<ListView>
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsStackPanel VerticalAlignment="Bottom"
                             ItemsUpdatingScrollMode="KeepLastItemInView"/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
</ListView>
```

## <a name="dos-and-donts"></a>可行與禁止事項

- 將來自傳送者/接收者的訊息對齊於相對的兩端，以便讓使用者清楚進行交談流程。
- 如果使用者已經在交談結尾等待下一則訊息，請利用動畫方式顯示現有訊息以顯示最新訊息。
- 如果使用者正在閱讀的並非交談結尾，就不要移動項目來干擾使用者的焦點。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 由下而上清單範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBottomUpList)