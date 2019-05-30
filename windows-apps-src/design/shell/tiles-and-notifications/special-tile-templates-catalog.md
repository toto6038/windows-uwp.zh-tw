---
Description: 特殊的磚範本是獨特的範本，它們可能具有動畫效果，或只是能讓您執行使用彈性磚無法達成的工作。
title: 特殊磚範本
ms.assetid: 1322C9BA-D5B2-45E2-B813-865884A467FF
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 739abc139eabc9f773938f55c15d3e18aaf562ce
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365967"
---
# <a name="special-tile-templates"></a>特殊磚範本
 

特殊的磚範本是獨特的範本，它們可能具有動畫效果，或只是能讓您執行使用彈性磚無法達成的工作。 每個特殊的磚範本已特別建置適用於 Windows 10 圖示 圖格 範本中，除了傳統的特殊範本已更新適用於 Windows 10。 本文章涵蓋三個特殊的磚範本：圖示，相片和人員。

## <a name="iconic-tile-template"></a>圖示磚範本


圖示範本 (也稱為「IconWithBadge」範本) 可讓您在磚的中心顯示一個小型影像。 Windows 10 手機和平板電腦/傳統型支援範本。

![小型和中型郵件磚](images/iconic-template-mail-2sizes.png)

### <a name="how-to-create-an-iconic-tile"></a>如何建立圖示磚

下列步驟說明您需要知道建立圖示的圖格，適用於 Windows 10 的所有項目。 就高層級而言，您需要您的圖示影像資產，然後使用圖示範本將通知傳送到磚，最後傳送提供要在磚上顯示的數字的徽章通知。

![圖示磚的開發人員流程](images/iconic-template-dev-flow.png)

**步驟 1：建立以 PNG 格式的影像資產**

為您的磚建立圖示資產，並將它們與您的其他資產一起放在專案資源中。 建立一個最低限度為 200 x 200 像素的圖示，這適用於手機和桌上型電腦上的小型和中型磚。 若要提供最佳的使用者體驗，請為每個大小建立一個圖示。 不需要在這些資產上填補。 請參閱下方影像中的調整大小詳細資訊。

以 PNG 格式儲存具有透明度的圖示資產。 在 Windows Phone 上，每個非透明的像素會顯示成白色 (RGB 255, 255, 255)。 為求簡單一致，也請針對桌面圖示使用白色。

平板電腦、膝上型電腦和桌上型電腦上的 Windows 10 僅支援正方形圖示資產。 Phone 支援正方形的資產和高度大於寬度的資產 (最多為 2:3 的寬度:高度比率)，這對於手機圖示等的影像很有用。

![手機和桌上型電腦上小型和中型磚的圖示大小調整](images/iconic-template-sizing-info.png)

![含/不含徽章的資產大小調整](images/assetguidance24.png)

對於正方形資產，會自動於容器內置中：

![含/不含徽章的正方形資產大小調整](images/assetguidance25.png)

對於非正方形資產，會自動水平/垂直置中並貼齊至容器寬度/高度：

![含/不含徽章的非正方形資產大小調整](images/assetguidance26a.png)

![含/不含徽章的非正方形資產大小調整](images/assetguidance26b.png)

**步驟 2：建立您的基底並排顯示**

您可以在主要和次要磚上使用圖示範本。 如果您正在次要磚上使用它，您必須先建立次要磚或使用已釘選次要磚。 主要磚以隱含方式釘選，且一律會傳送通知。

**步驟 3：傳送通知給您的磚**

雖然此步驟可能會因為通知在本機傳送或透過伺服器推播而不同，但您傳送的 XML 承載仍相同。 若要傳送本機磚通知，請為您的磚 (主要或次要磚) 建立 [**TileUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater)，然後將通知傳送到使用圖示磚範本的磚，如下所示。 在理想的情況下，您也應該使用[彈性磚範本](create-adaptive-tiles.md)包含寬形和大型磚大小的繫結。

以下是 XML 承載的範例程式碼：

```xml
<tile>
  <visual>

    <binding template="TileSquare150x150IconWithBadge">
      <image id="1" src="Iconic.png" alt="alt text"/>
    </binding>
    
    <binding template="TileSquare71x71IconWithBadge">
      <image id="1" src="Iconic.png" alt="alt text"/>
    </binding>

  </visual>
</tile>
```

這個圖示範本的 XML 承載會使用指向您在步驟 1 中建立的影像的影像元素。 現在您的磚已準備好在您的圖示旁顯示徽章，只剩下傳送徽章通知。

**步驟 4：將識別碼通知傳送至您的磚**

和步驟 3 一樣，此步驟可能會因為通知在本機傳送或透過伺服器推播而不同，但您傳送的 XML 承載仍相同。 若要傳送本機徽章通知，請為您的磚 (主要或次要磚) 建立 [**BadgeUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.BadgeUpdater)，然後傳送包含您所需的值的徽章通知 (或清除徽章)。

以下是 XML 承載的範例程式碼：

```xml
<badge value="2"/>
```

磚的徽章會適當地更新。

**步驟 5：總結**

下列影像說明各種 API 與承載如何與圖示磚範本的每個層面相關聯。 [磚通知](https://docs.microsoft.com/previous-versions/windows/apps/hh779724(v=win.10)) (包含這些 &lt;binding[元素) 是用來指定圖示範本和影像資產；&gt;徽章通知](https://docs.microsoft.com/previous-versions/windows/apps/hh779719(v=win.10))指定數值；磚屬性控制磚的顯示名稱及色彩等。

![與圖示磚範本相關聯的 API 與承載](images/iconic-template-properties-info.png)

## <a name="photos-tile-template"></a>相片磚範本


相片磚範本可讓您在動態磚上顯示相片的幻燈片秀。 範本適用於所有磚大小 (包括小型)，而且在每個磚大小上的行為都相同。 下列範例示範使用相片範本的中型磚的五個畫面格。 範本包含縮放和淡入與淡出動畫，而且會循環顯示所選相片並無限循環。

![使用相片磚範本的影像幻燈片秀](images/photo-tile-template-image01.jpg)

### <a name="how-to-use-the-photos-template"></a>如何使用相片範本

如果您安裝了 [Notification 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，使用相片範本就很輕鬆。 雖然您可以使用原始 XML，但強烈建議您使用程式庫，這樣您就不需要為產生有效的 XML 或 XML 逸出內容操心。

Windows Phone 在一個幻燈片秀中最多顯示 9 張相片；平板電腦、膝上型電腦和桌上型電腦最多顯示 12 張。

如需傳送磚通知的詳細資訊，請參閱[傳送通知一文](index.md)。


```xml
<!--
 
To use the Photos template...
 
 - On any adaptive tile binding (like TileMedium or TileWide)
   - Set the hint-presentation attribute to "photos"
   - Add up to 12 images as children of the binding.
    
-->
 
<tile>
  <visual>
     
    <binding template="TileMedium" hint-presentation="photos">
       
      <image src="Assets/1.jpg" />
      <image src="ms-appdata:///local/Images/2.jpg"/>
      <image src="http://msn.com/images/3.jpg"/>
       
      <!--TODO: Can have 12 images total-->
       
    </binding>
     
    <!--TODO: Add bindings for other tile sizes-->
     
  </visual>
</tile>
```

```csharp
/*
 
To use the Photos template...
 
 - On any TileBinding object
   - Set Content property to new instance of TileBindingContentPhotos
   - Add up to 12 images to Images property of TileBindingContentPhotos.
 
*/
 
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentPhotos()
            {
                Images =
                {
                    new TileBasicImage() { Source = "Assets/1.jpg" },
                    new TileBasicImage() { Source = "ms-appdata:///local/Images/2.jpg" },
                    new TileBasicImage() { Source = "http://msn.com/images/3.jpg" }
 
                    // TODO: Can have 12 images total
                }
            }
        }
 
        // TODO: Add other tile sizes
    }
};
```

## <a name="people-tile-template"></a>連絡人磚範本


Windows 10 中的連絡人應用程式使用特殊的磚範本，會在磚上垂直或水平滑動的圓形中顯示影像集合。 這個磚範本已經在 Windows 10 建置 10572，與任何人都可以使用其應用程式中的 歡迎使用。

連絡人磚範本適用於下列大小的磚：

**中型磚** (TileMedium)

![中型連絡人磚](images/people-tile-medium.png)

 

**寬形磚** (TileWide)

![寬形連絡人磚](images/people-tile-wide.png)

 

**大型磚 (僅限桌上型電腦)** (TileLarge)

![大型連絡人磚](images/people-tile-large.png)

 

如果您使用 [Notification 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，若要使用連絡人磚範本，只需要為您的 *TileBinding* 內容建立一個新的 *TileBindingContentPeople* 物件即可。 *TileBindingContentPeople* 類別有一個供您新增影像的 Images 屬性。

如果您使用原始的 XML，請將 *hint-presentation* 設定為 "people"，並將您的影像新增為繫結元素的子系。

下列 C# 程式碼範例假設您使用的是 [Notification 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)。

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentPeople()
            {
                Images =
                {
                    new TileBasicImage() { Source = "Assets/ProfilePics/1.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/2.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/3.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/4.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/5.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/6.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/7.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/8.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/9.jpg" }
                }
            }
        }
    }
};
```

```xml
<tile>
  <visual>
 
    <binding template="TileMedium" hint-presentation="people">
      <image src="Assets/ProfilePics/1.jpg"/>
      <image src="Assets/ProfilePics/2.jpg"/>
      <image src="Assets/ProfilePics/3.jpg"/>
      <image src="Assets/ProfilePics/4.jpg"/>
      <image src="Assets/ProfilePics/5.jpg"/>
      <image src="Assets/ProfilePics/6.jpg"/>
      <image src="Assets/ProfilePics/7.jpg"/>
      <image src="Assets/ProfilePics/8.jpg"/>
      <image src="Assets/ProfilePics/9.jpg"/>
    </binding>
 
  </visual>
</tile>
```

為提供最佳的使用者經驗，建議您針對每個磚大小提供下列數目的相片：

-   中型磚：9 的相片
-   寬形磚：15 的相片
-   大型磚：20 的相片

設定此數目的相片可以保留一些空的圓形，在視覺上磚就不會顯得過於雜亂。 您可以隨意調整相片數目，以設定最適合您的外觀。

若要傳送通知，請參閱[選擇通知傳遞方法](choosing-a-notification-delivery-method.md)。

## <a name="related-topics"></a>相關主題


* [在 GitHub 上的完整程式碼範例](https://github.com/WindowsNotifications/quickstart-people-tile-template)
* [通知程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)
* [磚、徽章及通知](index.md)
* [建立自動調整圖格](create-adaptive-tiles.md)
* [磚內容結構描述](../tiles-and-notifications/tile-schema.md)
 

 




