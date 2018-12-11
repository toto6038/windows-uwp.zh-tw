---
Description: Identify the input devices connected to a Universal Windows Platform (UWP) device and identify their capabilities and attributes.
title: 識別輸入裝置
ms.assetid: B2E93FBF-C508-44D9-BA46-ECFDAA8746F4
label: Identify input devices
template: detail.hbs
keywords: 裝置, 數位板, 輸入, 互動
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c45ad71643b0d75efcb130c1175952822197a161
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8897702"
---
# <a name="identify-input-devices"></a>識別輸入裝置


識別連接至通用 Windows 平台 (UWP) 裝置的輸入裝置，以及識別它們的功能和屬性。

> **重要 API**：[**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)、[**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br208383)、[**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)

## <a name="retrieve-mouse-properties"></a>擷取滑鼠屬性


[**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648) 命名空間包含 [**MouseCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225626) 類別，這個類別可以用來擷取由一或多個已連接滑鼠所公開的屬性。 做法是建立一個新的 **MouseCapabilities** 物件並取得您感興趣的屬性。

**注意：** 這裡所討論屬性傳回的值以所有偵測到滑鼠為根據： 布林值屬性會傳回非零至少一個滑鼠支援特定的功能，而數值屬性會傳回任何一個所公開的最大值滑鼠。

 

下列程式碼會使用一系列的 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素來顯示個別的滑鼠屬性和值。

```CSharp
private void GetMouseProperties()
{
    MouseCapabilities mouseCapabilities = new Windows.Devices.Input.MouseCapabilities();
    MousePresent.Text = mouseCapabilities.MousePresent != 0 ? "Yes" : "No";
    VertWheel.Text = mouseCapabilities.VerticalWheelPresent != 0 ? "Yes" : "No";
    HorzWheel.Text = mouseCapabilities.HorizontalWheelPresent != 0 ? "Yes" : "No";
    SwappedButtons.Text = mouseCapabilities.SwapButtons != 0 ? "Yes" : "No";
    NumButtons.Text = mouseCapabilities.NumberOfButtons.ToString();
}
```

## <a name="retrieve-keyboard-properties"></a>擷取鍵盤屬性


[**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648) 命名空間包含 [**KeyboardCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225623) 類別，這個類別可以用來擷取是否已連接鍵盤。 做法是建立一個新的 **KeyboardCapabilities** 物件並取得 [**KeyboardPresent**](https://msdn.microsoft.com/library/windows/apps/br225625) 屬性。

下列程式碼會使用一個 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素來顯示鍵盤屬性和值。

```CSharp
private void GetKeyboardProperties()
{
    KeyboardCapabilities keyboardCapabilities = new Windows.Devices.Input.KeyboardCapabilities();
    KeyboardPresent.Text = keyboardCapabilities.KeyboardPresent != 0 ? "Yes" : "No";
}
```

## <a name="retrieve-touch-properties"></a>擷取觸控屬性


[**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648) 命名空間包含 [**TouchCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225644) 類別，這個類別可以用來擷取是否已連接任何觸控數位板。 做法是建立一個新的 **TouchCapabilities** 物件並取得您感興趣的屬性。

**注意：** 這裡所討論屬性傳回的值以所有偵測到的觸控數位板為根據： 布林值屬性會傳回非零至少一個數位板支援特定的功能，而數值屬性會傳回最大值任何一個數位板所公開。

 

下列程式碼會使用一系列的 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素來顯示觸控屬性和值。

```CSharp
private void GetTouchProperties()
{
    TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();
    TouchPresent.Text = touchCapabilities.TouchPresent != 0 ? "Yes" : "No";
    Contacts.Text = touchCapabilities.Contacts.ToString();
}
```

## <a name="retrieve-pointer-properties"></a>擷取指標屬性


[**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648) 命名空間包含 [**PointerDevice**](https://msdn.microsoft.com/library/windows/apps/br225633) 類別，這個類別可以用來擷取是否有任何裝置支援指標輸入 (觸控、觸控板、滑鼠或手寫筆)。 做法是建立一個新的 **PointerDevice** 物件並取得您感興趣的屬性。

**注意：** 這裡所討論屬性傳回的值以所有偵測到的指標裝置為根據： 布林值屬性會傳回非零至少一個裝置支援特定的功能，而數值屬性會傳回公開的最大值透過任一指標裝置。

下列程式碼會使用一個表格來顯示每個指標裝置的屬性和值。

```CSharp
private void GetPointerDevices()
{
    IReadOnlyList<PointerDevice> pointerDevices = Windows.Devices.Input.PointerDevice.GetPointerDevices();
    int gridRow = 0;
    int gridColumn = 0;

    for (int i = 0; i < pointerDevices.Count; i++)
    {
        // Pointer device type.
        TextBlock textBlock1 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock1);
        textBlock1.Text = (i + 1).ToString() + " Pointer Device Type:";
        Grid.SetRow(textBlock1, gridRow);
        Grid.SetColumn(textBlock1, gridColumn);

        TextBlock textBlock2 = new TextBlock();
        textBlock2.Text = pointerDevices[i].PointerDeviceType.ToString();
        Grid_PointerProps.Children.Add(textBlock2);
        Grid.SetRow(textBlock2, gridRow++);
        Grid.SetColumn(textBlock2, gridColumn + 1);

        // Is external?
        TextBlock textBlock3 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock3);
        textBlock3.Text = (i + 1).ToString() + " Is External?";
        Grid.SetRow(textBlock3, gridRow);
        Grid.SetColumn(textBlock3, gridColumn);

        TextBlock textBlock4 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock4);
        textBlock4.Text = pointerDevices[i].IsIntegrated.ToString();
        Grid.SetRow(textBlock4, gridRow++);
        Grid.SetColumn(textBlock4, gridColumn + 1);

        // Maximum contacts.
        TextBlock textBlock5 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock5);
        textBlock5.Text = (i + 1).ToString() + " Max Contacts:";
        Grid.SetRow(textBlock5, gridRow);
        Grid.SetColumn(textBlock5, gridColumn);

        TextBlock textBlock6 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock6);
        textBlock6.Text = pointerDevices[i].MaxContacts.ToString();
        Grid.SetRow(textBlock6, gridRow++);
        Grid.SetColumn(textBlock6, gridColumn + 1);

        // Physical device rectangle.
        TextBlock textBlock7 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock7);
        textBlock7.Text = (i + 1).ToString() + " Physical Device Rect:";
        Grid.SetRow(textBlock7, gridRow);
        Grid.SetColumn(textBlock7, gridColumn);

        TextBlock textBlock8 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock8);
        textBlock8.Text = pointerDevices[i].PhysicalDeviceRect.X.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Y.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Width.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Height.ToString();
        Grid.SetRow(textBlock8, gridRow++);
        Grid.SetColumn(textBlock8, gridColumn + 1);

        // Screen rectangle.
        TextBlock textBlock9 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock9);
        textBlock9.Text = (i + 1).ToString() + " Screen Rect:";
        Grid.SetRow(textBlock9, gridRow);
        Grid.SetColumn(textBlock9, gridColumn);

        TextBlock textBlock10 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock10);
        textBlock10.Text = pointerDevices[i].ScreenRect.X.ToString() + "," +
            pointerDevices[i].ScreenRect.Y.ToString() + "," +
            pointerDevices[i].ScreenRect.Width.ToString() + "," +
            pointerDevices[i].ScreenRect.Height.ToString();
        Grid.SetRow(textBlock10, gridRow++);
        Grid.SetColumn(textBlock10, gridColumn + 1);

        gridColumn += 2;
        gridRow = 0;
    }
```

## <a name="related-articles"></a>相關文章


**範例**
* [基本輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延遲輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [使用者互動模式範例](http://go.microsoft.com/fwlink/p/?LinkID=619894)

**封存範例**
* [輸入：裝置功能範例](http://go.microsoft.com/fwlink/p/?linkid=231530)
 

 




