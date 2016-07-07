---
author: mijacobs
Description: "彈性磚範本是 Windows 10 中的新功能，可讓您使用能夠適應不同螢幕密度的簡易靈活標記語言，設計專屬的磚通知內容。"
title: "建立彈性磚"
ms.assetid: 1246B58E-D6E3-48C7-AD7F-475D113600F9
label: Create adaptive tiles
template: detail.hbs
ms.sourcegitcommit: a6632c7b8fdee5320f35e316abd318193a254c51
ms.openlocfilehash: 6cd4519007d1241cb7c411dade1a092140b598c4

---

# 建立彈性磚





彈性磚範本是 Windows 10 中的新功能，可讓您使用能夠適應不同螢幕密度的簡易靈活標記語言，設計專屬的磚通知內容。 本文會告訴您如何為通用 Windows 平台 (UWP) app 建立彈性動態磚。 如需彈性元素和屬性的完整清單，請參閱[彈性磚結構描述](tiles-and-notifications-adaptive-tiles-schema.md)。

(設計適用於 Windows 10 的通知時，如果您想要，仍然可以使用 [Windows 8 磚範本目錄](https://msdn.microsoft.com/library/windows/apps/hh761491)中的預設範本)。

## 開始使用


**安裝 NotificationsExtensions。** 若您想要使用 C# 而不是 XML 產生通知，請安裝名為 [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) 的 NuGet 套件。 本文中所提供的 C# 範例使用 NotificationsExtensions。

**安裝通知視覺化工具。** 這個免費的 UWP 應用程式可協助您透過在編輯磚時提供即時視覺預覽 (類似 Visual Studio 的 XAML 編輯器/設計檢視)，以設計彈性動態磚。 您可以閱讀[此部落格文章](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/09/22/introducing-notifications-visualizer-for-windows-10.aspx)以取得詳細資訊，您也可以從[這裡](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)下載通知視覺化工具。

## 用法指導方針


彈性範本的設計可跨不同的尺寸規格和通知類型運作。 諸如群組和子群組之類的元素會將內容連結在一起，而且並不代表自己的特定視覺行為。 無論是手機、平板電腦、傳統型裝置或其他裝置，通知的最終外觀應該以將會顯示在其上的特定裝置為基礎。

提示是選用的屬性，可以新增至元素以達到特定的視覺行為。 提示可以是針對裝置或針對通知。

## 基本範例


此範例示範彈性磚範本可以產生的內容。

```XML
<tile>
  <visual>
  
    <binding template="TileMedium">
      ...
    </binding>
  
    <binding template="TileWide">
      <text hint-style="subtitle">Jennifer Parker</text>
      <text hint-style="captionSubtle">Photos from our trip</text>
      <text hint-style="captionSubtle">Check out these awesome photos I took while in New Zealand!</text>
    </binding>
  
    <binding template="TileLarge">
      ...
    </binding>
  
  </visual>
</tile>
```

```CSharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = ...
  
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new TileText()
                    {
                        Text = "Jennifer Parker",
                        Style = TileTextStyle.Subtitle
                    },
  
                    new TileText()
                    {
                        Text = "Photos from our trip",
                        Style = TileTextStyle.CaptionSubtle
                    },
  
                    new TileText()
                    {
                        Text = "Check out these awesome photos I took while in New Zealand!",
                        Style = TileTextStyle.CaptionSubtle
                    }
                }
            }
        },
  
        TileLarge = ...
    }
};
```

**結果：**

![快速範例磚](images/adaptive-tiles-quicksample.png)

## 磚大小


每個磚大小的內容是分別在 XML 承載內的不同 [&lt;binding&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 元素中指定。 將範本屬性設定為下列其中一個值，以選擇目標大小：

-   TileSmall
-   TileMedium
-   TileWide
-   TileLarge (僅適用於桌上型電腦)

對於單一磚通知 XML 承載，為您想要支援的每個磚大小提供 &lt;binding&gt; 元素，如此範例中所示：

```XML
<tile>
  <visual>
  
    <binding template="TileSmall">
      <text>Small</text>
    </binding>
  
    <binding template="TileMedium">
      <text>Medium</text>
    </binding>
  
    <binding template="TileWide">
      <text>Wide</text>
    </binding>
  
    <binding template="TileLarge">
      <text>Large</text>
    </binding>
  
  </visual>
</tile>
```

```CSharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileSmall = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new TileText() { Text = "Small" }
                }
            }
        },
  
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new TileText() { Text = "Medium" }
                }
            }
        },
  
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new TileText() { Text = "Wide" }
                }
            }
        },
  
        TileLarge = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new TileText() { Text = "Large" }
                }
            }
        }
    }
};
```

**結果：**

![彈性磚大小：小、中、寬及大](images/adaptive-tiles-sizes.png)

## 商標


您可以使用通知承載的商標屬性，控制動態磚底部的商標 (顯示名稱和角標誌)。 您可以選擇顯示 "none"、僅顯示 "name"、僅顯示 "logo"，或使用 "nameAndLogo" 來顯示兩者。

**注意：**Windows Mobile 不支援角落標誌，因此在行動裝置上，"logo" 和 "nameAndLogo" 預設會是 "name"。

 

```XML
<visual branding="logo">
  ...
</visual>
```

```CSharp
new TileVisual()
{
    Branding = TileBranding.Logo,
    ...
}

new TileVisual()
{
    Branding = TileBranding.Logo,
    ...
}
```

**結果：**

![彈性磚、名稱和標誌](images/adaptive-tiles-namelogo.png)

可以針對特定磚大小，以下列其中一種方式套用商標：

1. 透過在 [&lt;binding&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 元素上套用屬性
2. 透過在 [&lt;visual&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 元素上套用屬性，這會影響整個通知承載。如果您沒有指定繫結的商標，它將會使用 visual 元素上提供的商標。

```XML
<tile>
  <visual branding="nameAndLogo">
 
    <binding template="TileMedium" branding="logo">
      ...
    </binding>
 
    <!--Inherits branding from visual-->
    <binding template="TileWide">
      ...
    </binding>
 
  </visual>
</tile>
```

```CSharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        Branding = TileBranding.NameAndLogo,
 
        TileMedium = new TileBinding()
        {
            Branding = TileBranding.Logo,
            ...
        },
 
        // Inherits branding from Visual
        TileWide = new TileBinding()
        {
            ...
        }
    }
};
```

**預設商標結果：**

![磚的預設品牌](images/adaptive-tiles-defaultbranding.png)

如果您沒有指定通知承載中的品牌，基底磚的屬性將決定品牌。 如果基底磚顯示顯示名稱，則品牌將會預設為 "name"。 否則若未出現顯示名稱，品牌將會預設為 "none"。

**注意：**這是與 Windows 8.x 不同的地方，其預設商標為 "logo"。

 

## 顯示名稱


您可以使用 **displayName** 屬性來輸入您所選擇的文字字串，來覆寫通知的顯示名稱。 和商標一樣，您可以在 [&lt;visual&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 元素上指定此顯示名稱，這會影響整個通知承載，您也可以在 [&lt;binding&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 元素上指定此顯示名稱，這將只會影響個別的磚。

**已知問題：**在 Windows Mobile 上，如果您為磚指定了 ShortName，系統就不會使用通知中提供的顯示名稱 (一律會顯示 ShortName)。 

```XML
<tile>
  <visual branding="nameAndLogo" displayName="Wednesday 22">
 
    <binding template="TileMedium" displayName="Wed. 22">
      ...
    </binding>
 
    <!--Inherits displayName from visual-->
    <binding template="TileWide">
      ...
    </binding>
 
  </visual>
</tile>
```

```CSharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        Branding = TileBranding.NameAndLogo,
        DisplayName = "Wednesday 22",
 
        TileMedium = new TileBinding()
        {
            DisplayName = "Wed. 22",
            ...
        },
 
        // Inherits DisplayName from Visual
        TileWide = new TileBinding()
        {
            ...
        }
    }
};
```

**結果：**

![彈性磚顯示名稱](images/adaptive-tiles-displayname.png)

## 文字


[&lt;text&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 元素用來顯示文字。 您可以使用此提示修改文字的顯示方式。

```XML
<text>This is a line of text</text></code></pre></td>
</tr>
</tbody>
</table>
```


```CSharp
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
new TileText()
{
    Text = "This is a line of text"
};
```

**結果：**

![彈性磚文字](images/adaptive-tiles-text.png)

## 文字換行


根據預設，文字不會換行，而且將會繼續超出磚的邊緣。 使用 **hint-wrap** 屬性在文字元素上設定文字換行。 您也可以使用 **hint-minLines** 和 **hint-maxLines** (皆接受正整數) 控制最小和最大行數。

```XML
<text hint-wrap="true">This is a line of wrapping text</text></code></pre></td>
</tr>
</tbody>
</table>
```


```CSharp
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
new TileText()
{
    Text = "This is a line of wrapping text",
    Wrap = true
};
```

**結果：**

![含文字換行的彈性磚](images/adaptive-tiles-textwrapping.png)

## 文字樣式


樣式可控制文字元素的字型大小、色彩及粗細。 有許多可用的樣式，包括每個可將不透明度設定為 60% 的樣式的「細微」變化，這通常會使文字色彩變成淺灰色網底。

```XML
<text hint-style="base">Header content</text>
<text hint-style="captionSubtle">Subheader content</text>
```

```CSharp
new TileText()
{
    Text = "Header content",
    Style = TileTextStyle.Base
},
 
new TileText()
{
    Text = "Subheader content",
    Style = TileTextStyle.CaptionSubtle
}
```

**結果：**

![彈性磚文字樣式](images/adaptive-tiles-textstyles.png)

**注意：**如果沒有指定 hint-style，則樣式預設為 caption。

 

**基本文字樣式**

|                                |                           |             |
|--------------------------------|---------------------------|-------------|
| &lt;text hint-style="\*" /&gt; | 字型高度               | 字型寬度 |
| caption                        | 12 個有效像素 (epx) | Regular     |
| body                           | 15 epx                    | Regular     |
| base                           | 15 epx                    | Semibold    |
| subtitle                       | 20 epx                    | Regular     |
| title                          | 24 epx                    | Semilight   |
| subheader                      | 34 epx                    | Light       |
| header                         | 46 epx                    | Light       |

 

**數字的文字樣式變化**

這些變化會減少行高，讓上方和下方的內容會更接近文字。

|                  |
|------------------|
| titleNumeral     |
| subheaderNumeral |
| headerNumeral    |

 

**細微的文字樣式變化**

每個樣式都有可提供文字 60% 不透明度的細微變化，這通常會使文字色彩變成淺灰色網底。

|                        |
|------------------------|
| captionSubtle          |
| bodySubtle             |
| baseSubtle             |
| subtitleSubtle         |
| titleSubtle            |
| titleNumeralSubtle     |
| subheaderSubtle        |
| subheaderNumeralSubtle |
| headerSubtle           |
| headerNumeralSubtle    |

 

## 文字對齊方式


文字可以水平靠左對齊、置中對齊或靠右對齊。 對於由左到右的語言 (如英文)，文字預設為靠左對齊。 對於由右到左的語言 (如阿拉伯文)，文字預設為靠右對齊。 您可以對元素使用 **hint-align** 屬性，來手動設定對齊方式。

```XML
<text hint-align="center">Hello</text></code></pre></td>
</tr>
</tbody>
</table>
```


```CSharp
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
new TileText()
{
    Text = "Hello",
    Align = TileTextAlign.Center
};
```

**結果：**

![彈性磚文字對齊方式](images/adaptive-tiles-textalignment.png)

## 群組與子群組


群組可以讓您從語意宣告群組內的內容相關，而且必須以內容的完整性顯示，才會具有意義。 例如，您可能會有兩個文字元素、一個標頭和一個子標頭，如果只顯示標頭，將不具任何意義。 為子群組內的這些元素分組，將會顯示所有元素 (如果可以容納)，或完全不顯示任何元素 (因為無法容納)。

若要在裝置和螢幕上提供最佳體驗，請提供多個群組。 擁有多個群組可讓您的磚配合較大的螢幕。

**注意：**唯一有效的群組子系是子群組。

 

```XML
...
<binding template="TileWide" branding="nameAndLogo">
  <group>
    <subgroup>
      <text hint-style="subtitle">Jennifer Parker</text>
      <text hint-style="captionSubtle">Photos from our trip</text>
      <text hint-style="captionSubtle">Check out these awesome photos I took while in New Zealand!</text>
    </subgroup>
  </group>
 
  <text />
 
  <group>
    <subgroup>
      <text hint-style="subtitle">Steve Bosniak</text>
      <text hint-style="captionSubtle">Build 2015 Dinner</text>
      <text hint-style="captionSubtle">Want to go out for dinner after Build tonight?</text>
    </subgroup>
  </group>
</binding>
...
```

```CSharp
...
 
TileWide = new TileBinding()
{
    Branding = TileBranding.NameAndLogo,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            CreateGroup(
                from: "Jennifer Parker",
                subject: "Photos from our trip",
                body: "Check out these awesome photos I took while in New Zealand!"),
 
            // For spacing
            new TileText(),
 
            CreateGroup(
                from: "Steve Bosniak",
                subject: "Build 2015 Dinner",
                body: "Want to go out for dinner after Build tonight?")
        }
    }
}
 
...
 
 
private static TileGroup CreateGroup(string from, string subject, string body)
{
    return new TileGroup()
    {
        Children =
        {
            new TileSubgroup()
            {
                Children =
                {
                    new TileText()
                    {
                        Text = from,
                        Style = TileTextStyle.Subtitle
                    },
 
                    new TileText()
                    {
                        Text = subject,
                        Style = TileTextStyle.CaptionSubtle
                    },
 
                    new TileText()
                    {
                        Text = body,
                        Style = TileTextStyle.CaptionSubtle
                    }
                }
            }
        }
    };
}
```

**結果：**

![彈性磚群組以及子群組](images/adaptive-tiles-groups-subgroups.png)

## 子群組 (欄)


子群組也可讓您在群組中，將資料分成多個語意式區段。 對於動態磚，這在視覺上會翻譯成欄。

**hint-weight** 屬性可讓您控制欄寬。 **hint-weight** 的值會表示為可用空間的加權比例，這與 **GridUnitType.Star** 行為相同。 對於等寬的欄，將每個加權指派為 1。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">寬度百分比</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="odd">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="odd">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="even">
<td align="left">總加權：4</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![子群組，甚至欄](images/adaptive-tiles-subgroups01.png)

若要讓一欄是另一欄兩倍的大小，為較小的欄指派加權 1，並為較大的欄指派加權 2。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">寬度百分比</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">33.3%</td>
</tr>
<tr class="odd">
<td align="left">2</td>
<td align="left">66.7%</td>
</tr>
<tr class="even">
<td align="left">總加權：3</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![子群組，一欄是另一欄兩倍的大小](images/adaptive-tiles-subgroups02.png)

如果您希望您的第一欄最多佔 20% 的總寬度，且第二欄最多佔 80% 的總寬度，請將第一個加權指派為 20，並將第二個加權指派為 80。 如果您的總加權等於 100，它們就會成為百分比。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">寬度百分比</td>
</tr>
<tr class="even">
<td align="left">20</td>
<td align="left">20%</td>
</tr>
<tr class="odd">
<td align="left">80</td>
<td align="left">80%</td>
</tr>
<tr class="even">
<td align="left">總加權：100</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![子群組，加權總計為 100](images/adaptive-tiles-subgroups03.png)

**注意：**欄與欄之間會自動加入 8 個像素的邊界。

 

當您有超過兩個子群組時，應該指定 **hint-weight**，這只會接受正整數。 如果您沒有為第一個子群組指定 hint-weight，則會將加權指派為 50。 對於沒有指定的 hint-weight 的下一個子群組，其獲指派的加權將等於 100 減去之前加權的加總，如果結果為零，則獲指派的加權將等於 1。 置於沒有指定的 hint-weight 的其餘子群組，其獲指派的加權為 1。

以下是天氣磚的範例程式碼，可顯示如何完成五個欄寬相等的磚：

```XML
...
<binding template="TileWide" displayName="Seattle" branding="name">
  <group>
    <subgroup hint-weight="1">
      <text hint-align="center">Mon</text>
      <image src="Assets\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Tue</text>
      <image src="Assets\Weather\Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-align="center" hint-style="captionsubtle">38°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Wed</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">59°</text>
      <text hint-align="center" hint-style="captionsubtle">43°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Thu</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">62°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Fri</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">71°</text>
      <text hint-align="center" hint-style="captionsubtle">66°</text>
    </subgroup>
  </group>
</binding>
...
```

```CSharp
...
TileWide = new TileBinding()
{
    DisplayName = "Seattle",
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new TileGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°"),
 
                    CreateSubgroup("Tue", "Cloudy.png", "57°", "38°"),
 
                    CreateSubgroup("Wed", "Sunny.png", "59°", "43°"),
 
                    CreateSubgroup("Thu", "Sunny.png", "62°", "42°"),
 
                    CreateSubgroup("Fri", "Sunny.png", "71°", "66°")
                }
            }
        }
    }
}
...
 
 
private static TileSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new TileSubgroup()
    {
        Weight = 1,
 
        Children =
        {
            new TileText()
            {
                Text = day,
                Align = TileTextAlign.Center
            },
 
            new TileImage()
            {
                Source = new TileImageSource("Assets/Weather/" + image),
                RemoveMargin = true
            },
 
            new TileText()
            {
                Text = highTemp,
                Align = TileTextAlign.Center
            },
 
            new TileText()
            {
                Text = lowTemp,
                Align = TileTextAlign.Center,
                Style = TileTextStyle.CaptionSubtle
            }
        }
    };
}
```

**結果：**

![天氣磚的範例](images/adaptive-tiles-weathertile.png)

## 影像


&lt;image&gt; 元素用來顯示磚通知上的影像。 影像可以內嵌於磚內容內 (預設值)、當做內容後方的背景影像，或當做以動畫方式從通知上方進入的預覽影像。

**注意：**對於[檔案大小和影像尺寸有一些限制](https://msdn.microsoft.com/library/windows/apps/hh781198)。

 

未指定任何額外的行為時，影像將會統一壓縮或延伸以填滿可用的寬度。 下列範例顯示使用兩欄與內嵌影像的磚。 內嵌影像會伸展以填滿欄寬。

```XML
...
<binding template="TileMedium" displayName="Seattle" branding="name">
  <group>
    <subgroup>
      <text hint-align="center">Mon</text>
      <image src="Assets\Apps\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-style="captionsubtle" hint-align="center">42°</text>
    </subgroup>
    <subgroup>
      <text hint-align="center">Tue</text>
      <image src="Assets\Apps\Weather\Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-style="captionSubtle" hint-align="center">38°</text>
    </subgroup>
  </group>
</binding>
...
```

```CSharp
...
TileMedium = new TileBinding()
{
    DisplayName = "Seattle",
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new TileGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°"),
 
                    CreateSubgroup("Tue", "Cloudy.png", "57°", "38°")
                }
            }
        }
    }
}
...
 
 
private static TileSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new TileSubgroup()
    {
        Children =
        {
            new TileText()
            {
                Text = day,
                Align = TileTextAlign.Center
            },
 
            new TileImage()
            {
                Source = new TileImageSource("Assets/Weather/" + image),
                RemoveMargin = true
            },
 
            new TileText()
            {
                Text = highTemp,
                Align = TileTextAlign.Center
            },
 
            new TileText()
            {
                Text = lowTemp,
                Align = TileTextAlign.Center,
                Style = TileTextStyle.CaptionSubtle
            }
        }
    };
}
```

**結果：**

![影像範例](images/adaptive-tiles-images01.png)

放置在 &lt;binding&gt; 根目錄或第一個群組中的影像也會伸展以填滿可用的高度。

### 影像對齊方式

您可以使用 **hint-align** 屬性，將影像設為靠左對齊、置中對齊或靠右對齊。 這也會使影像以原始解析度顯示，而不會自動伸展以填滿寬度。

```XML
...
<binding template="TileLarge">
  <image src="Assets/fable.jpg" hint-align="center"/>
</binding>
...
```

```CSharp
...
TileLarge = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new TileImage()
            {
                Source = new TileImageSource("Assets/fable.jpg"),
                Align = TileImageAlign.Center
            }
        }
    }
}
...
```

**結果：**

![影像對齊方式範例 (靠左、置中、靠右)](images/adaptive-tiles-imagealignment.png)

### 影像邊界

根據預設，內嵌影像在影像上方或下方的任何內容之間有 8 個像素的邊界。 此邊界也可以使用影像的 **hint-removeMargin** 屬性移除。 不過，影像距離磚的邊緣一律會保留 8 個像素的邊界，且子群組 (欄) 在各欄之間一律會保留 8 個像素的邊框距離。

```XML
...
<binding template="TileMedium" branding="none">
  <group>
    <subgroup>
      <text hint-align="center">Mon</text>
      <image src="Assets\Numbers\4.jpg" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-style="captionsubtle" hint-align="center">42°</text>
    </subgroup>
    <subgroup>
      <text hint-align="center">Tue</text>
      <image src="Assets\Numbers\3.jpg" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-style="captionsubtle" hint-align="center">38°</text>
    </subgroup>
  </group>
</binding>
...
```

```CSharp
...
 
TileMedium = new TileBinding()
{
    Branding = TileBranding.None,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new TileGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "4.jpg", "63°", "42°"),
 
                    CreateSubgroup("Tue", "3.jpg", "57°", "38°")
                }
            }
        }
    }
}
 
...
 
 
private static TileSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new TileSubgroup()
    {
        Weight = 1,
 
        Children =
        {
            new TileText()
            {
                Text = day,
                Align = TileTextAlign.Center
            },
 
            new TileImage()
            {
                Source = new TileImageSource("Assets/Numbers/" + image),
                RemoveMargin = true
            },
 
            new TileText()
            {
                Text = highTemp,
                Align = TileTextAlign.Center
            },
 
            new TileText()
            {
                Text = lowTemp,
                Align = TileTextAlign.Center,
                Style = TileTextStyle.CaptionSubtle
            }
        }
    };
}
```

![hint remove margin 範例](images/adaptive-tiles-removemargin.png)

### 影像裁剪

使用 **hint-crop** 屬性可將影像裁剪成圓形，此屬性目前僅支援 "none" (預設) 或 "circle" 的值。

```XML
...
<binding template="TileLarge" hint-textStacking="center">
  <group>
    <subgroup hint-weight="1"/>
    <subgroup hint-weight="2">
      <image src="Assets/Apps/Hipstame/hipster.jpg" hint-crop="circle"/>
    </subgroup>
    <subgroup hint-weight="1"/>
  </group>
 
  <text hint-style="title" hint-align="center">Hi,</text>
  <text hint-style="subtitleSubtle" hint-align="center">MasterHip</text>
</binding>
...
```

```CSharp
...
TileLarge = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        TextStacking = TileTextStacking.Center,
 
        Children =
        {
            new TileGroup()
            {
                Children =
                {
                    new TileSubgroup() { Weight = 1 },
 
                    new TileSubgroup()
                    {
                        Weight = 2,
                        Children =
                        {
                            new TileImage()
                            {
                                Source = new TileImageSource("Assets/Apps/Hipstame/hipster.jpg"),
                                Crop = TileImageCrop.Circle
                            }
                        }
                    },
 
                    new TileSubgroup() { Weight = 1 }
                }
            },
 
 
            new TileText()
            {
                Text = "Hi,",
                Style = TileTextStyle.Title,
                Align = TileTextAlign.Center
            },
 
            new TileText()
            {
                Text = "MasterHip",
                Style = TileTextStyle.SubtitleSubtle,
                Align = TileTextAlign.Center
            }
        }
    }
}
...
```

**結果：**

![影像裁剪範例](images/adaptive-tiles-imagecropping.png)

### 背景影像

若要設定背景影像，將 image 元素放在 &lt;binding&gt; 的根目錄中，並將 placement 屬性設定為 "background"。

```XML
...
<binding template="TileWide">
  <image src="Assets\Mostly Cloudy-Background.jpg" placement="background"/>
  <group>
    <subgroup hint-weight="1">
      <text hint-align="center">Mon</text>
      <image src="Assets\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    ...
  </group>
</binding>
...
```

```CSharp
...
TileWide = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        BackgroundImage = new TileBackgroundImage()
        {
            Source = new TileImageSource("Assets/Mostly Cloudy-Background.jpg")
        },
 
        Children =
        {
            new TileGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°")
                    ...
                }
            }
        }
    }
}
...
 
 
private static TileSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new TileSubgroup()
    {
        Weight = 1,
 
        Children =
        {
            new TileText()
            {
                Text = day,
                Align = TileTextAlign.Center
            },
 
            new TileImage()
            {
                Source = new TileImageSource("Assets/Weather/" + image),
                RemoveMargin = true
            },
 
            new TileText()
            {
                Text = highTemp,
                Align = TileTextAlign.Center
            },
 
            new TileText()
            {
                Text = lowTemp,
                Align = TileTextAlign.Center,
                Style = TileTextStyle.CaptionSubtle
            }
        }
    };
}
```

**結果：**

![背景影像範例](images/adaptive-tiles-backgroundimage.png)

此外，您可以使用 **hint-overlay** 在背景影像上設定黑色重疊，其可接受 0-100 的整數，0 是沒有重疊，而 100 則是完全黑色重疊。 預設值為 20。

```XML
...
<binding template="TileWide" hint-overlay="60">
  <image src="Assets\Mostly Cloudy-Background.jpg" placement="background"/>
  ...
</binding>
...
```

```CSharp
...
 
TileWide = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        BackgroundImage = new TileBackgroundImage()
        {
            Source = new TileImageSource("Assets/Mostly Cloudy-Background.jpg"),
            Overlay = 60
        },
 
        ...
    }
}
 
...
```

**hint-overlay 結果：**

![影像 hint overlay 的範例](images/adaptive-tiles-image-hintoverlay.png)

### 預覽影像

您可以指定從磚的上方「預覽」的影像。 預覽影像使用動畫，從磚的上方向下/向上滑動以進入檢視，之後再向後滑出，以在磚上顯示主要內容。 若要設定預覽影像，將 image 元素放在 &lt;binding&gt; 的根目錄中，並將 placement 屬性設定為 "peek"。

```XML
...
<binding template="TileMedium" branding="name">
  <image placement="peek" src="Assets/Apps/Hipstame/hipster.jpg"/>
  <text>New Message</text>
  <text hint-style="captionsubtle" hint-wrap="true">Hey, have you tried Windows 10 yet?</text>
</binding>
...
```

```CSharp
...
 
TileWide = new TileBinding()
{
    Branding = TileBranding.Name,
 
    Content = new TileBindingContentAdaptive()
    {
        PeekImage = new TilePeekImage()
        {
            Source = new TileImageSource("Assets/Apps/Hipstame/hipster.jpg")
        },
 
        Children =
        {
            new TileText()
            {
                Text = "New Message"
            },
 
            new TileText()
            {
                Text = "Hey, have you tried Windows 10 yet?",
                Style = TileTextStyle.CaptionSubtle,
                Wrap = true
            }
        }
    }
}
 
...
```

![預覽影像的範例](images/adaptive-tiles-imagepeeking.png)

**預覽和背景影像的圓形裁剪**

在預覽和背景影像上使用下列屬性以進行圓形裁剪：

hint-crop="circle"

結果將看起來如下：

![預覽和背景影像的圓形裁剪](images/circlecrop-image.png)

**同時使用預覽和背景影像**

若要在磚通知上同時使用預覽和背景影像，請在您的通知承載中指定預覽影像和背景影像。

結果將看起來如下：

![一起使用的預覽和背景影像](images/peekandbackground.png)

**在預覽影像上使用 hint-overlay**

您可以在預覽影像上使用 **hint-overlay** 增加不透明度，並讓磚的顯示名稱更為清楚。 如果您將 **hint-overlay** 指定到 &lt;binding&gt; 元素上，則會在背景和預覽影像上套用重疊。

您也可以將 **hint-overlay** 套用到有 placement="peek" 或 placement="background" 的 &lt;image&gt; 元素，讓這些影像有各自的不透明度。 如果您沒有指定重疊，則背景影像的不透明度預設為 20%，預覽影像的不透明度預設為 0%。

這個範例顯示 20% 不透明度 (左) 以及 0% 不透明度 (右) 的背景影像：

![預覽影像上的 hint-overlay](images/hintoverlay.png)

## 垂直對齊方式 (文字堆疊)


您也可以使用 [&lt;binding&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 元素和 [&lt;subgroup&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 元素上的 **hint-textStacking** 屬性，控制內容在您的磚上的垂直對齊方式。 根據預設，所有內容都會垂直靠上對齊，但是您也可以將內容靠下對齊或置中對齊。

### binding 元素上的文字堆疊

在 [&gt;binding&lt;](tiles-and-notifications-adaptive-tiles-schema.md) 層級套用時，文字堆疊會將通知內容當做一個整體來設定其垂直對齊方式，在品牌/徽章區域上方的可用垂直空間內對齊。

```XML
...
<binding template="TileMedium" hint-textStacking="center" branding="logo">
  <text hint-style="base" hint-align="center">Hi,</text>
  <text hint-style="captionSubtle" hint-align="center">MasterHip</text>
</binding>
...
```

```CSharp
...
 
TileMedium = new TileBinding()
{
    Branding = TileBranding.Logo,
 
    Content = new TileBindingContentAdaptive()
    {
        TextStacking = TileTextStacking.Center,
 
        Children =
        {
            new TileText()
            {
                Text = "Hi,",
                Style = TileTextStyle.Base,
                Align = TileTextAlign.Center
            },
 
            new TileText()
            {
                Text = "MasterHip",
                Style = TileTextStyle.CaptionSubtle,
                Align = TileTextAlign.Center
            }
        }
    }
}
 
...
```

![binding 元素上的文字堆疊](images/adaptive-tiles-textstack-bindingelement.png)

### 子群組元素上的文字堆疊

在 [&lt;subgroup&gt;](tiles-and-notifications-adaptive-tiles-schema.md) 層級套用時，文字堆疊會設定子群組 (欄) 的垂直對齊方式，在整個群組中的可用垂直空間內對齊。

```XML
...
<binding template="TileWide" branding="nameAndLogo">
  <group>
    <subgroup hint-weight="33">
      <image src="Assets/Apps/Hipstame/hipster.jpg" hint-crop="circle"/>
    </subgroup>
    <subgroup hint-textStacking="center">
      <text hint-style="subtitle">Hi,</text>
      <text hint-style="bodySubtle">MasterHip</text>
    </subgroup>
  </group>
</binding>
...
```

```CSharp
...
 
TileWide = new TileBinding()
{
    Branding = TileBranding.NameAndLogo,
 
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new TileGroup()
            {
                Children =
                {
                    // Image column
                    new TileSubgroup()
                    {
                        Weight = 33,
                        Children =
                        {
                            new TileImage()
                            {
                                Source = new TileImageSource("Assets/Apps/Hipstame/hipster.jpg"),
                                Crop = TileImageCrop.Circle
                            }
                        }
                    },
 
                    // Text column
                    new TileSubgroup()
                    {
                        // Vertical align its contents
                        TextStacking = TileTextStacking.Center,
 
                        Children =
                        {
                            new TileText()
                            {
                                Text = "Hi,",
                                Style = TileTextStyle.Subtitle
                            },
 
                            new TileText()
                            {
                                Text = "MasterHip",
                                Style = TileTextStyle.BodySubtle
                            }
                        }
                    }
                }
            }
        }
    }
}
 
...
```

## 相關主題


* [彈性磚結構描述](tiles-and-notifications-adaptive-tiles-schema.md)
* [GitHub 上的 NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki)
* [特殊磚範本目錄](tiles-and-notifications-special-tile-templates-catalog.md)
 

 







<!--HONumber=Jun16_HO4-->


