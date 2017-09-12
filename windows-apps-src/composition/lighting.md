---
author: jebishop
ms.assetid: fb8ae71d-5c88-4c85-9257-a9607d5179b1
title: "光源"
description: "光源物件是與 SceneLightingEffect 搭配使用，以模擬動態光源和反射。"
ms.author: jimwalk
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 3f922cae8aa0787f8be6496997df1021dda8e142
ms.sourcegitcommit: b42d14c775efbf449a544ddb881abd1c65c1ee86
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2017
---
# <a name="lighting"></a>光源

[**CompositionLight**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionLight) 物件是與 [**SceneLightingEffect**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) 搭配使用，以模擬動態光源與反射。

您可以將光源套用至[**視覺效果**](https://msdn.microsoft.com/library/windows/apps/Dn706858)和 XAML [**UIElements**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)。

## <a name="applying-lights-to-xaml-uielements"></a>將光源套用至 XAML UIElement

[**XamlLight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamllight)物件是用來套用[**CompositionLights**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionLight)，以動態設定 XAML UIElement 的光源。 XamlLight 提供方法，根據目前是否為使用中，將目標設為 UIElement 或其他 XAML Brush、將光源套用至 UIElement 的樹狀結構，以及協助管理 CompositionLight 資源的存留期。

* 如果您將目標設為具有 XamlLight 的**Brush**，則使用該 Brush 之任何 UIElement 的各部分都會透過光源亮起。
* 如果您將目標設為具有 XamlLight 的**UIElement**，則整個 UIElement 和其子 UIElement 都會透過光源亮起。

## <a name="creating-and-using-a-xamllight"></a>建立和使用 XamlLight

[**XamlLight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamllight)是基底類別，可用來建立自訂光源。

以下自訂 XamlLight 的範例使用附加的屬性將目標設為特定元素︰

```csharp
public sealed class FancyOrangeSpotLight : XamlLight
{
    // Register an attached proeprty that enables apps to set a UIElement or Brush as a target for this light type in markup.
    public static readonly DependencyProperty IsTargetProperty =
        DependencyProperty.RegisterAttached(
        "IsTarget",
        typeof(Boolean),
        typeof(FancyOrangeSpotLight),
        new PropertyMetadata(null, OnIsTargetChanged)
    );
    public static void SetIsTarget(DependencyObject target, Boolean value)
    {
        target.SetValue(IsTargetProperty, value);
    }
    public static Boolean GetIsTarget(DependencyObject target)
    {
        return (Boolean) target.GetValue(IsTargetProperty);
    }

    // Handle attached property changed to automatically target and untarget UIElements and Brushes.
    private static void OnIsTargetChanged(DependencyObject obj,
                                            DependencyPropertyChangedEventArgs e)
    {
        var isAdding = (Boolean)e.NewValue;

        if (isAdding)
        {
            if (obj is UIElement)
            {
                XamlLight.AddTargetElement(GetIdStatic(), obj as UIElement);
            }
            else if (obj is Brush)
            {
                XamlLight.AddTargetBrush(GetIdStatic(), obj as Brush);
            }
        }
        else
        {
            if (obj is UIElement)
            {
                XamlLight.RemoveTargetElement(GetIdStatic(), obj as UIElement);
            }
            else if (obj is Brush)
            {
                XamlLight.RemoveTargetBrush(GetIdStatic(), obj as Brush);
            }
        }
    }

    protected override void OnConnected(UIElement newElement)
    {
        // OnConnected is called when the first target UIElement is shown on the screen. This enables delaying composition object creation until it's actually necessary.
        CompositionLight = Window.Current.Compositor.CreateSpotLight();
        CompositionLight.InnerConeColor = Colors.Orange;
        CompositionLight.OuterConeColor = Colors.Yellow;
        CompositionLight.InnerConeAngleInDegrees = 30;
        CompositionLight.OuterConeAngleInDegrees = 45;
    }

    protected override void OnDisconnected(UIElement oldElement)
    {
        // OnDisconnected is called when there are no more target UIElements on the screen. The CompositionLight should be disposed when no longer required.
        CompositionLight.Dispose();
        CompositionLight = null;
    }

    protected override string GetId()
    {
        return GetIdStatic();
    }

    private static string GetIdStatic()
    {
        // This specifies the unique name of the light. In most cases you should use the type's FullName.
        return typeof(FancyPointerTrackerLight).FullName;
    }
}
```

以下範例顯示上方所定義之自訂光源的不同可能用法︰

```xml
<Page>
    <Page.Lights>
        <local:SimpleOrangeSpotLight />
    </Page.Lights>

    <StackPanel>
        <!-- this border will be lit by a FancyOrangeSpotLight, but not its children -->
        <Border BorderThickness="1">
            <Border.BorderBrush>
                <SolidColorBrush Color="Orange" local:FancyOrangeSpotLight.IsTarget="true" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>

        <!-- this border and its children will be lit by FancyOrangeSpotLight -->
        <Border BorderThickness="2" local:FancyOrangeSpotLight.IsTarget="true">
            <Border.BorderBrush>
                <SolidColorBrush Color="Purple" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>

        <!-- this border will not be lit -->
        <Border BorderThickness="2">
            <Border.BorderBrush>
                <SolidColorBrush Color="Green" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>
    </StackPanel>
<Page>
```

> [!Important]
> 如上述範例所示，只有最小版本等於 Windows 10 Creators Update 或更新版本的應用程式，才支援使用標記來設定 UIElement.Lights。 針對將目標設為較舊版本的應用程式，則必須透過程式碼後置來建立光源。

## <a name="additional-resources"></a>其他資源

* [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs) 中的進階 UI 和組合範例。