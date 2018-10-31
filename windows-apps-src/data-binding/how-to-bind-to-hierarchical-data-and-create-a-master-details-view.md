---
author: mcleblanc
ms.assetid: 0C69521B-47E0-421F-857B-851B0E9605F2
title: 繫結階層式資料並建立主要/詳細資料檢視
description: 您可以將項目控制項繫結到已繫結成一個鏈的 CollectionViewSource 執行個體，以建立階層式資料的多層主要/詳細資料 (又稱為清單/詳細資料) 檢視。
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 60d283f41c495f9612311e4b9b9da3df1a44d498
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5838244"
---
# <a name="bind-hierarchical-data-and-create-a-masterdetails-view"></a>繫結階層式資料並建立主要/詳細資料檢視



> **注意：** 另請參閱[主要/詳細資料範例](http://go.microsoft.com/fwlink/p/?linkid=619991)。

您可以將項目控制項繫結到已繫結成一個鏈的 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 執行個體，以建立階層式資料的多層主要/詳細資料 (又稱為清單/詳細資料) 檢視。 在本主題中，我們儘可能使用 [{x:Bind} 標記延伸](https://msdn.microsoft.com/library/windows/apps/Mt204783)，必要時也使用更有彈性 (但效能較低) 的 [{Binding} 標記延伸](https://msdn.microsoft.com/library/windows/apps/Mt204782)。

通用 Windows 平台 (UWP) app 有一個常見的結構，當使用者在主要清單中做選擇時，將會瀏覽至不同的詳細資料頁面。 當您想在階層中的每一層，為每個項目提供豐富的視覺表示時，這就很有用。 另一種作法是在單一頁面中顯示多層資料。 當您想要顯示一些簡單的清單，讓使用者快速深入查看有興趣的項目時，這就很有用。 本主題描述如何實作這種互動。 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 執行個體會追蹤每個階層式層級上目前選取的項目。

我們將建立一個運動團隊階層的檢視，分為聯盟、分組和團隊清單，並且包含團隊詳細資料檢視。 當您從任一清單中選取一個項目時，後續的檢視會自動更新。

![運動階層的主要/詳細資料檢視](images/xaml-masterdetails.png)

## <a name="prerequisites"></a>先決條件

這個主題假設您知道如何建立基本的 UWP app。 如需有關建立第一個 UWP app 的指示，請參閱[使用 C# 或 Visual Basic 建立您的第一個 UWP app](https://msdn.microsoft.com/library/windows/apps/Hh974581)。

## <a name="create-the-project"></a>建立專案

建立新的**空白應用程式 (Windows 通用)** 專案。 命名為 "MasterDetailsBinding"。

## <a name="create-the-data-model"></a>建立資料模型

將新的類別加入到專案，命名為 ViewModel.cs，然後在其中加入下列程式碼。 這將是您的繫結來源類別。

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace MasterDetailsBinding
{
    public class Team
    {
        public string Name { get; set; }
        public int Wins { get; set; }
        public int Losses { get; set; }
    }

    public class Division
    {
        public string Name { get; set; }
        public IEnumerable<Team> Teams { get; set; }
    }

    public class League
    {
        public string Name { get; set; }
        public IEnumerable<Division> Divisions { get; set; }
    }

    public class LeagueList : List<League>
    {
        public LeagueList()
        {
            this.AddRange(GetLeague().ToList());
        }

        public IEnumerable<League> GetLeague()
        {
            return from x in Enumerable.Range(1, 2)
                   select new League
                   {
                       Name = "League " + x,
                       Divisions = GetDivisions(x).ToList()
                   };
        }

        public IEnumerable<Division> GetDivisions(int x)
        {
            return from y in Enumerable.Range(1, 3)
                   select new Division
                   {
                       Name = String.Format("Division {0}-{1}", x, y),
                       Teams = GetTeams(x, y).ToList()
                   };
        }

        public IEnumerable<Team> GetTeams(int x, int y)
        {
            return from z in Enumerable.Range(1, 4)
                   select new Team
                   {
                       Name = String.Format("Team {0}-{1}-{2}", x, y, z),
                       Wins = 25 - (x * y * z),
                       Losses = x * y * z
                   };
        }
    }
}
```

## <a name="create-the-view"></a>建立檢視

接著，從代表標記頁面的類別中公開繫結來源類別。 作法是將 **LeagueList** 類型的屬性加入到 **MainPage**。

```cs
namespace MasterDetailsBinding
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new LeagueList();
        }
        public LeagueList ViewModel { get; set; }
    }
}
```

最後，將 MainPage.xaml 檔案的內容取代為下列標記，其中宣告三個 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 執行個體，並將它們繫結成一個鏈。 然後，後續的控制項就可以繫結到階層中位於適當層級的 **CollectionViewSource**。

```xml
<Page
    x:Class="MasterDetailsBinding.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MasterDetailsBinding"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Page.Resources>
        <CollectionViewSource x:Name="Leagues"
            Source="{x:Bind ViewModel}"/>
        <CollectionViewSource x:Name="Divisions"
            Source="{Binding Divisions, Source={StaticResource Leagues}}"/>
        <CollectionViewSource x:Name="Teams"
            Source="{Binding Teams, Source={StaticResource Divisions}}"/>

        <Style TargetType="TextBlock">
            <Setter Property="FontSize" Value="15"/>
            <Setter Property="FontWeight" Value="Bold"/>
        </Style>

        <Style TargetType="ListBox">
            <Setter Property="FontSize" Value="15"/>
        </Style>

        <Style TargetType="ContentControl">
            <Setter Property="FontSize" Value="15"/>
        </Style>

    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <StackPanel Orientation="Horizontal">

            <!-- All Leagues view -->

            <StackPanel Margin="5">
                <TextBlock Text="All Leagues"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Leagues}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- League/Divisions view -->

            <StackPanel Margin="5">
                <TextBlock Text="{Binding Name, Source={StaticResource Leagues}}"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Divisions}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- Division/Teams view -->

            <StackPanel Margin="5">
                <TextBlock Text="{Binding Name, Source={StaticResource Divisions}}"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Teams}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- Team view -->

            <ContentControl Content="{Binding Source={StaticResource Teams}}">
                <ContentControl.ContentTemplate>
                    <DataTemplate>
                        <StackPanel Margin="5">
                            <TextBlock Text="{Binding Name}" 
                                FontSize="15" FontWeight="Bold"/>
                            <StackPanel Orientation="Horizontal" Margin="10,10">
                                <TextBlock Text="Wins:" Margin="0,0,5,0"/>
                                <TextBlock Text="{Binding Wins}"/>
                            </StackPanel>
                            <StackPanel Orientation="Horizontal" Margin="10,0">
                                <TextBlock Text="Losses:" Margin="0,0,5,0"/>
                                <TextBlock Text="{Binding Losses}"/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ContentControl.ContentTemplate>
            </ContentControl>

        </StackPanel>

    </Grid>
</Page>
```

請注意，直接繫結到 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 時，就意味著您想要繫結到在集合本身找不到路徑之繫結中的目前項目。 不需要指定 **CurrentItem** 屬性做為繫結的路徑 (但如果情況模稜兩可，您可以這樣做)。 例如，代表小組檢視之 [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365) 的 [**Content**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content) 屬性是繫結到 `Teams`**CollectionViewSource**。 不過，[**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) 中的控制項則是繫結到 `Team` 類別的屬性，因為必要時，**CollectionViewSource** 會自動提供團隊清單中目前選取的團隊。

 

 

