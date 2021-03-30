---
title: IWindowNative. WindowHandle 屬性
description: WinUI COM 屬性，以取得要求的視窗 HWND。
ms.topic: reference
ms.date: 03/09/2021
keywords: winui，Windows UI 程式庫
ms.openlocfilehash: 9c8278b5b19f4e7eeee447ed4a4261064f36194d
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730858"
---
# <a name="iwindownativewindowhandle-property-microsoftuixamlwindowh"></a>IWindowNative. WindowHandle 屬性 (的。 .h) 

取得要求的視窗控制碼。

## <a name="syntax"></a>語法

<!--
[
    object,
    uuid( EECDBF0E-BAE9-4CB6-A68E-9598E1CB57BB ),
    local,
    pointer_default(unique)
]
interface IWindowNative: IUnknown
{
    [propget] HRESULT WindowHandle([out, retval] HWND* hWnd);
};
-->

```cpp
HRESULT WindowHandle(
  HWND* hWnd
);
```

## <a name="parameters"></a>參數

*hWnd* [out]

類型： **HWND***

視窗的控制碼。

## <a name="return-value"></a>傳回值

類型： HRESULT

如果這個方法成功，它會傳回 S_OK。 否則，它會傳回 HRESULT 錯誤碼。

## <a name="remarks"></a>備註

## <a name="examples"></a>範例

在嘗試下列範例之前，請先參閱下列主題：

- 若要使用 WinUI 3 for desktop 專案範本，請設定您的開發電腦並 [設定您的開發環境](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment)。
- 建立並執行初始範本應用程式，以確認您的開發環境如預期般運作，如 [開始使用 WinUI 3 for desktop apps](../winui3/get-started-winui3-for-desktop.md)中所述。

### <a name="customized-window-icon"></a>自訂視窗圖示

在下列範例中，我們會從 **Desktop c #/.NET 5** 範本程式碼的初始 WinUI 開始，並示範如何使用 **WindowHandle** 自訂用於應用程式視窗的圖示。

#### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

1. 首先，我們會新增下列 using 指示詞：

    - [System.runtime.interopservices.outattribute](/dotnet/api/system.runtime.interopservices)：提供對 COM interop 和平台叫用服務的支援。 在此範例中，這是 PInvoke 功能的必要項。
    - [WinRT](/uwp/cpp-ref-for-winrt/winrt)：提供屬於 c + +/winrt 的自訂資料類型 在此範例中，IWindowNative COM 介面的必要項。

    ```csharp
    using System.Runtime.InteropServices;
    using WinRT;
    ```

1. 然後，將 .ico 檔案新增至專案 ( "Images/windowIcon" ) 並設定 [組建動作] (以滑鼠右鍵按一下檔案，然後選取此檔案的 [內容]) 。

1. 在 MainWindow 方法中，我們新增了對函式的呼叫 `LoadIcon("Images/windowIcon.ico");` (下一個步驟中所述的) 參考我們在上一個步驟中新增的 .ico 檔案。

    ```csharp
    public MainWindow()
    {
        LoadIcon("Images/windowIcon.ico");
    
        this.InitializeComponent();
    }
    ```

1. 接下來，我們會新增函式來 `LoadIcon(string iconName)` 取得應用程式的控制碼，並使用各種 [PInvoke](/dotnet/standard/native-interop/pinvoke) 功能（包括 [LoadImage](/windows/win32/api/winuser/nf-winuser-loadimagew) 和 [SendMessage](/windows/win32/api/winuser/nf-winuser-sendmessage)）來設定應用程式圖示。

    ```csharp
    private void LoadIcon(string iconName)
    {
        //Get the Window's HWND
        var hwnd = this.As<IWindowNative>().WindowHandle;
    
        IntPtr hIcon = PInvoke.User32.LoadImage(
            IntPtr.Zero, 
            iconName,
            PInvoke.User32.ImageType.IMAGE_ICON, 
            16, 16, 
            PInvoke.User32.LoadImageFlags.LR_LOADFROMFILE);
    
        PInvoke.User32.SendMessage(
            hwnd, 
            PInvoke.User32.WindowMessage.WM_SETICON, 
            (IntPtr)0, 
            hIcon);
    }    
    ```

1. 最後，我們會從公用 IWindowNative 介面內嵌類型資訊，並從執行時間元件建立類別的實例。 如需詳細資訊，請參閱 [從 managed 元件內嵌類型](/dotnet/standard/assembly/embed-types-visual-studio)。

    ```csharp
    [ComImport]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    [Guid("EECDBF0E-BAE9-4CB6-A68E-9598E1CB57BB")]
    internal interface IWindowNative
    {
        IntPtr WindowHandle { get; }
    }
    ```

1. 如果您已在自己的應用程式中遵循這些步驟，請建立並執行應用程式。 您應該會看到類似下列 (與自訂應用程式圖示) 的應用程式視窗：

    :::image type="content" source="../winui3/images/build-basic/template-app-windowhandle.png" alt-text="範本應用程式與自訂應用程式圖示。":::<br/>*範本應用程式與自訂應用程式圖示。*

## <a name="applies-to"></a>適用於

| 產品 | 版本 |
| --- | --- |
| WinUI | 3.0.0-專案-留尼旺島-0.5、3.0.0-專案-留尼旺島-預覽-0。5 |

## <a name="see-also"></a>另請參閱

[IWindowNative 介面](iwindownative.md)
