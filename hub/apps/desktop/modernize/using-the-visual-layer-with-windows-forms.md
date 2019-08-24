---
title: 使用具有 Windows Forms 的視覺效果層
description: 瞭解搭配現有的 Windows Forms 內容使用視覺分層 Api 的技術, 以建立先進的動畫和效果。
ms.date: 03/18/2019
ms.topic: article
keywords: Windows 10, UWP
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 9da9dee48beef6e3c1cd38ffbe9761ed89fd940d
ms.sourcegitcommit: 93d0b2996b4742b33cd6d641e036f42672cf5238
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/23/2019
ms.locfileid: "69999643"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>使用具有 Windows Forms 的視覺效果層

您可以在 Windows Forms 應用程式中使用 Windows 執行階段組合 Api (也稱為「[視覺圖層](/windows/uwp/composition/visual-layer)」) 來建立適用于 Windows 10 使用者的新式體驗。

本教學課程的完整程式碼可在 GitHub 上取得:[Windows Forms HelloComposition 範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)。

## <a name="prerequisites"></a>必要條件

UWP 裝載 API 具有這些必要條件。

- 我們假設您已熟悉如何使用 Windows Forms 和 UWP 來開發應用程式。 如需詳細資訊，請參閱：
  - [使用 Windows Forms 的消費者入門](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [開始使用 Windows 10 應用程式](/windows/uwp/get-started/)
  - [增強您的 Windows 10 桌面應用程式](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 或更新版本
- Windows 10 1803 版或更新版本
- Windows 10 SDK 17134 或更新版本

## <a name="how-to-use-composition-apis-in-windows-forms"></a>如何在 Windows Forms 中使用組合 Api

在本教學課程中, 您會建立簡單的 Windows Forms UI, 並在其中新增動畫組合元素。 Windows Forms 和組合元件都保持簡單, 但不論元件的複雜度為何, 顯示的 interop 程式碼都相同。 完成的應用程式看起來像這樣。

![執行中的應用程式 UI](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>建立 Windows Forms 專案

第一個步驟是建立 Windows Forms 應用程式專案, 其中包括應用程式定義和 UI 的主要表單。

若要在名為_HelloComposition_的視覺效果C#中建立新的 Windows Forms 應用程式專案:

1. 開啟 Visual Studio, 然後  > 選取 [檔案] [**新增** > ] [**專案**]。<br/>[**新增專案**] 對話方塊隨即開啟。
1. 在 [**已安裝**] 類別底下, 展開 [**視覺效果C#**  ] 節點, 然後選取 [ **Windows 桌面**]。
1. 選取 [ **Windows Forms 應用程式 (.NET Framework)** ] 範本。
1. 輸入名稱_HelloComposition,_ 選取 [Framework **.NET Framework 4.7.2**], 然後按一下 **[確定]** 。

Visual Studio 會建立專案, 並開啟名為 Form1.cs 之預設應用程式視窗的設計工具。

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>設定專案以使用 Windows 執行階段 Api

若要在您的 Windows Forms 應用程式中使用 Windows 執行階段 (WinRT) Api, 您需要設定您的 Visual Studio 專案以存取 Windows 執行階段。 此外, 組合 Api 會廣泛使用向量, 因此您需要加入使用向量所需的參考。

NuGet 套件可用來解決這兩種需求。 安裝這些套件的最新版本, 以將必要的參考新增至您的專案。  

- [Microsoft. WINDOWS SDK 合約](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts)(需要預設的套件管理格式設為 PackageReference)。
- [System.object](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> 雖然我們建議使用 NuGet 套件來設定您的專案, 但您可以手動新增必要的參考。 如需詳細資訊, 請參閱[加強 Windows 10 的桌面應用程式](/windows/uwp/porting/desktop-to-uwp-enhance)。 下表顯示您需要加入參考的檔案。

|檔案|Location|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86) \windows Kits\10\References\< *sdk 版本*> \Windows.Foundation.UniversalApiContract\<*版本*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86) \windows Kits\10\References\< *sdk 版本*> \Windows.Foundation.FoundationContract\<*版本*>|
|System.string. 向量 .dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.object .dll|C:\Program Files (x86) \Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7。2|

## <a name="create-a-custom-control-to-manage-interop"></a>建立自訂控制項以管理 interop

若要裝載使用視覺效果層所建立的內容, 您可以建立衍生自[control](/dotnet/api/system.windows.forms.control)的自訂控制項。 此控制項可讓您存取視窗[控制碼](/dotnet/api/system.windows.forms.control.handle), 而您必須在其中建立視覺效果層內容的容器。

這是您執行大部分裝載組合 Api 設定的地方。 在此控制項中, 您可以使用[平台叫用服務 (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code)和[COM Interop](/dotnet/api/system.runtime.interopservices.comimportattribute) , 將組合 api 帶入您的 Windows Forms 應用程式中。 如需 PInvoke 和 COM Interop 的詳細資訊, 請參閱[與非受控程式碼互](/dotnet/framework/interop/index)操作。

> [!TIP]
> 如有需要, 請在教學課程結束時查看完整的程式碼, 以確定所有程式碼都位於您進行本教學課程時的正確位置。

1. 將新的自訂控制項檔案加入至衍生自[control](/dotnet/api/system.windows.forms.control)的專案。
    - 在**方案總管**中, 以滑鼠右鍵按一下 [ _HelloComposition_ ] 專案。
    - 在內容功能表中, 選取 [**加入** > **新專案**]。
    - 在 [**加入新專案**] 對話方塊中, 選取 [**自訂控制項**]。
    - 將控制項命名為_CompositionHost.cs_, 然後按一下 [**新增**]。 CompositionHost.cs 會在設計檢視中開啟。

1. 切換至 CompositionHost.cs 的程式碼, 並將下列程式碼加入至類別。

    ```csharp
    // Add
    // using Windows.UI.Composition;

    IntPtr hwndHost;
    object dispatcherQueue;
    protected ContainerVisual containerVisual;
    protected Compositor compositor;

    private ICompositionTarget compositionTarget;

    public Visual Child
    {
        set
        {
            if (compositor == null)
            {
                InitComposition(hwndHost);
            }
            compositionTarget.Root = value;
        }
    }
    ```

1. 將程式碼加入至函式。

    在此函數中, 您會呼叫_InitializeCoreDispatcher_和_InitComposition_方法。 您會在後續步驟中建立這些方法。

    ```csharp
    public CompositionHost()
    {
        InitializeComponent();

        // Get the window handle.
        hwndHost = Handle;

        // Create dispatcher queue.
        dispatcherQueue = InitializeCoreDispatcher();

        // Build Composition tree of content.
        InitComposition(hwndHost);
    }
    ```

1. 使用[CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher)初始化執行緒。 核心發送器負責處理 WinRT Api 的視窗訊息和分派事件。 您必須在具有 CoreDispatcher 的執行緒上建立**複合**器的新實例。
    - 建立名為_InitializeCoreDispatcher_的方法, 並新增程式碼以設定發送器佇列。

    ```csharp
    // Add
    // using System.Runtime.InteropServices;

    private object InitializeCoreDispatcher()
    {
        DispatcherQueueOptions options = new DispatcherQueueOptions();
        options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
        options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
        options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

        object queue = null;
        CreateDispatcherQueueController(options, out queue);
        return queue;
    }
    ```

    - 發送器佇列需要 PInvoke 宣告。 將此宣告放在類別的程式碼結尾。 (我們會將此程式碼放在區域內, 以保持類別程式碼整齊)。

    ```csharp
    #region PInvoke declarations

    //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    //{
    //    DQTAT_COM_NONE,
    //    DQTAT_COM_ASTA,
    //    DQTAT_COM_STA
    //};
    internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    {
        DQTAT_COM_NONE = 0,
        DQTAT_COM_ASTA = 1,
        DQTAT_COM_STA = 2
    };

    //typedef enum DISPATCHERQUEUE_THREAD_TYPE
    //{
    //    DQTYPE_THREAD_DEDICATED,
    //    DQTYPE_THREAD_CURRENT
    //};
    internal enum DISPATCHERQUEUE_THREAD_TYPE
    {
        DQTYPE_THREAD_DEDICATED = 1,
        DQTYPE_THREAD_CURRENT = 2,
    };

    //struct DispatcherQueueOptions
    //{
    //    DWORD dwSize;
    //    DISPATCHERQUEUE_THREAD_TYPE threadType;
    //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    //};
    [StructLayout(LayoutKind.Sequential)]
    internal struct DispatcherQueueOptions
    {
        public int dwSize;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_TYPE threadType;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    };

    //HRESULT CreateDispatcherQueueController(
    //  DispatcherQueueOptions options,
    //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
    //);
    [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
    internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                            [MarshalAs(UnmanagedType.IUnknown)]
                                            out object dispatcherQueueController);

    #endregion PInvoke declarations
    ```

    您現在已準備好發送器佇列, 並且可以開始初始化並建立組合內容。

1. 初始化[複合](/uwp/api/windows.ui.composition.compositor)器。 複合器是一個 factory, 它會在[Windows. UI 中](/uwp/api/windows.ui.composition)建立各種類型, 橫跨視覺分層、效果系統和動畫系統。 組合器類別也會管理從 factory 建立之物件的存留期。

    ```csharp
    private void InitComposition(IntPtr hwndHost)
    {
        ICompositorDesktopInterop interop;

        compositor = new Compositor();
        object iunknown = compositor as object;
        interop = (ICompositorDesktopInterop)iunknown;
        IntPtr raw;
        interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

        object rawObject = Marshal.GetObjectForIUnknown(raw);
        compositionTarget = (ICompositionTarget)rawObject;

        if (raw == null) { throw new Exception("QI Failed"); }

        containerVisual = compositor.CreateContainerVisual();
        Child = containerVisual;
    }
    ```

    - **ICompositorDesktopInterop**和**ICompositionTarget**需要 COM 匯入。 將此程式碼放在_CompositionHost_類別後面, 但在命名空間宣告中。

    ```csharp
    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }

    #endregion COM Interop
    ```

## <a name="create-a-custom-control-to-host-composition-elements"></a>建立自訂控制項以裝載組合元素

建議您將產生和管理複合專案的程式碼放在衍生自 CompositionHost 的個別控制項中。 這會讓您在 CompositionHost 類別中建立的 interop 程式碼得以重複使用。

在這裡, 您會建立衍生自 CompositionHost 的自訂控制項。 此控制項會加入至 [Visual Studio] 工具箱, 讓您可以將它新增至表單。

1. 將新的自訂控制項檔案加入至衍生自 CompositionHost 的專案。
    - 在**方案總管**中, 以滑鼠右鍵按一下 [ _HelloComposition_ ] 專案。
    - 在內容功能表中, 選取 [**加入** > **新專案**]。
    - 在 [**加入新專案**] 對話方塊中, 選取 [**自訂控制項**]。
    - 將控制項命名為_CompositionHostControl.cs_, 然後按一下 [**新增**]。 CompositionHostControl.cs 會在設計檢視中開啟。

1. 在 [CompositionHostControl.cs 設計檢視] 的 [屬性] 窗格中, 將 [**背景**色彩] 屬性設定為 [ **ControlLight**]。

    設定背景色彩是選擇性的。 我們在這裡執行此動作, 讓您可以看到自訂控制項的表單背景。

1. 切換至程式碼視圖以進行 CompositionHostControl.cs, 並將類別宣告更新為衍生自 CompositionHost。

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. 更新此函式以呼叫基底函數。

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>新增組合元素

基礎結構就緒之後, 您現在可以將組合內容新增至應用程式 UI。

在此範例中, 您會將程式碼新增至 CompositionHostControl 類別, 以建立簡單[SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)的動畫。

1. 加入組合元素。

    在 CompositionHostControl.cs 中, 將這些方法新增至 CompositionHostControl 類別。

    ```csharp
    // Add
    // using Windows.UI.Composition;

    public void AddElement(float size, float offsetX, float offsetY)
    {
        var visual = compositor.CreateSpriteVisual();
        visual.Size = new Vector2(size, size); // Requires references
        visual.Brush = compositor.CreateColorBrush(GetRandomColor());
        visual.Offset = new Vector3(offsetX, offsetY, 0);
        containerVisual.Children.InsertAtTop(visual);

        AnimateSquare(visual, 3);
    }

    private void AnimateSquare(SpriteVisual visual, int delay)
    {
        float offsetX = (float)(visual.Offset.X);
        Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
        float bottom = Height - visual.Size.Y;
        animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
        animation.Duration = TimeSpan.FromSeconds(2);
        animation.DelayTime = TimeSpan.FromSeconds(delay);
        visual.StartAnimation("Offset", animation);
    }

    private Windows.UI.Color GetRandomColor()
    {
        Random random = new Random();
        byte r = (byte)random.Next(0, 255);
        byte g = (byte)random.Next(0, 255);
        byte b = (byte)random.Next(0, 255);
        return Windows.UI.Color.FromArgb(255, r, g, b);
    }
    ```

## <a name="add-the-control-to-your-form"></a>將控制項新增至您的表單

現在您已有可裝載組合內容的自訂控制項, 您可以將它新增至應用程式 UI。 在這裡, 您會新增在上一個步驟中建立之 CompositionHostControl 的實例。 CompositionHostControl 會自動新增至 [ **_專案名稱_元件**] 底下的 [Visual Studio 工具箱] 中。

1. 在 Form1.CS 設計檢視中, 將按鈕新增至 UI。

    - 將按鈕從 [工具箱] 拖曳至 [Form1]。 將它放在表單的左上角。 (請參閱教學課程開頭的影像, 以檢查控制項的位置)。
    - 在 [屬性] 窗格中, 將 [ **Text** ] 屬性從 [ _button1_ ] 變更為 [_加入複合_專案]。
    - 調整按鈕的大小, 以顯示所有文字。

    (如需詳細資訊, [請參閱如何:將控制項新增至](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms)Windows Forms)。

1. 將 CompositionHostControl 新增至 UI。

    - 將 [CompositionHostControl] 從 [工具箱] 拖曳至 [Form1]。 將它放在按鈕的右邊。
    - 調整 CompositionHost 的大小, 使其填滿表單的其餘部分。

1. 處理按鈕的 click 事件。

   - 在 [屬性] 窗格中, 按一下閃電以切換至 [事件] 視圖。
   - 在 [事件] 清單中, 選取 [ **Click** ] 事件, 輸入*Button_Click*, 然後按 enter。
   - 這段程式碼會在 Form1.cs 中新增:

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. 將程式碼新增至按鈕 click 處理常式, 以建立新的專案。

    - 在 Form1.cs 中, 將程式碼新增至您先前建立的*Button_Click*事件處理常式。 這段程式碼會呼叫_CompositionHostControl1_ , 以建立具有隨機產生大小和位移的新元素。 (當您將 CompositionHostControl 的實例拖曳至表單上時, 它會自動命名為_compositionHostControl1_ )。

    ```csharp
    // Add
    // using System;

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Random random = new Random();
        float size = random.Next(50, 150);
        float offsetX = random.Next(0, (int)(compositionHostControl1.Width - size));
        float offsetY = random.Next(0, (int)(compositionHostControl1.Height/2 - size));
        compositionHostControl1.AddElement(size, offsetX, offsetY);
    }
    ```

您現在可以建立並執行您的 Windows Forms 應用程式。 當您按一下按鈕時, 您應該會看到新增至 UI 的動畫正方形。

## <a name="next-steps"></a>後續步驟

如需在相同基礎結構上建立的更完整範例, 請參閱 GitHub 上的[Windows Forms 視覺效果層整合範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)(英文)。

## <a name="additional-resources"></a>其他資源

- [使用 Windows Forms 的消費者入門](/dotnet/framework/winforms/getting-started-with-windows-forms).
- [與非受控程式碼互](/dotnet/framework/interop/)操作.
- [開始使用 Windows 10 應用程式](/windows/uwp/get-started/)UWP
- [增強您的 Windows 10 桌面應用程式](/windows/uwp/porting/desktop-to-uwp-enhance)UWP
- [Windows. UI. 撰寫命名空間](/uwp/api/windows.ui.composition)UWP

## <a name="complete-code"></a>完整程式碼

以下是本教學課程的完整程式碼。

### <a name="form1cs"></a>Form1.cs

```csharp
using System;
using System.Windows.Forms;

namespace HelloComposition
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, EventArgs e)
        {
            Random random = new Random();
            float size = random.Next(50, 150);
            float offsetX = random.Next(0, (int)(compositionHostControl1.Width - size));
            float offsetY = random.Next(0, (int)(compositionHostControl1.Height/2 - size));
            compositionHostControl1.AddElement(size, offsetX, offsetY);
        }
    }
}
```

### <a name="compositionhostcontrolcs"></a>CompositionHostControl.cs

```csharp
using System;
using System.Numerics;
using Windows.UI.Composition;

namespace HelloComposition
{
    class CompositionHostControl : CompositionHost
    {
        public CompositionHostControl() : base()
        {

        }

        public void AddElement(float size, float offsetX, float offsetY)
        {
            var visual = compositor.CreateSpriteVisual();
            visual.Size = new Vector2(size, size); // Requires references
            visual.Brush = compositor.CreateColorBrush(GetRandomColor());
            visual.Offset = new Vector3(offsetX, offsetY, 0);
            containerVisual.Children.InsertAtTop(visual);

            AnimateSquare(visual, 3);
        }

        private void AnimateSquare(SpriteVisual visual, int delay)
        {
            float offsetX = (float)(visual.Offset.X);
            Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
            float bottom = Height - visual.Size.Y;
            animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
            animation.Duration = TimeSpan.FromSeconds(2);
            animation.DelayTime = TimeSpan.FromSeconds(delay);
            visual.StartAnimation("Offset", animation);
        }

        private Windows.UI.Color GetRandomColor()
        {
            Random random = new Random();
            byte r = (byte)random.Next(0, 255);
            byte g = (byte)random.Next(0, 255);
            byte b = (byte)random.Next(0, 255);
            return Windows.UI.Color.FromArgb(255, r, g, b);
        }
    }
}
```

### <a name="compositionhostcs"></a>CompositionHost.cs

```csharp
using System;
using System.Runtime.InteropServices;
using System.Windows.Forms;
using Windows.UI.Composition;

namespace HelloComposition
{
    public partial class CompositionHost : Control
    {
        IntPtr hwndHost;
        object dispatcherQueue;
        protected ContainerVisual containerVisual;
        protected Compositor compositor;
        private ICompositionTarget compositionTarget;

        public Visual Child
        {
            set
            {
                if (compositor == null)
                {
                    InitComposition(hwndHost);
                }
                compositionTarget.Root = value;
            }
        }

        public CompositionHost()
        {
            // Get the window handle.
            hwndHost = Handle;

            // Create dispatcher queue.
            dispatcherQueue = InitializeCoreDispatcher();

            // Build Composition tree of content.
            InitComposition(hwndHost);
        }

        private object InitializeCoreDispatcher()
        {
            DispatcherQueueOptions options = new DispatcherQueueOptions();
            options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
            options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
            options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

            object queue = null;
            CreateDispatcherQueueController(options, out queue);
            return queue;
        }

        private void InitComposition(IntPtr hwndHost)
        {
            ICompositorDesktopInterop interop;

            compositor = new Compositor();
            object iunknown = compositor as object;
            interop = (ICompositorDesktopInterop)iunknown;
            IntPtr raw;
            interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

            object rawObject = Marshal.GetObjectForIUnknown(raw);
            compositionTarget = (ICompositionTarget)rawObject;

            if (raw == null) { throw new Exception("QI Failed"); }

            containerVisual = compositor.CreateContainerVisual();
            Child = containerVisual;
        }

        protected override void OnPaint(PaintEventArgs pe)
        {
            base.OnPaint(pe);
        }

        #region PInvoke declarations

        //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        //{
        //    DQTAT_COM_NONE,
        //    DQTAT_COM_ASTA,
        //    DQTAT_COM_STA
        //};
        internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        {
            DQTAT_COM_NONE = 0,
            DQTAT_COM_ASTA = 1,
            DQTAT_COM_STA = 2
        };

        //typedef enum DISPATCHERQUEUE_THREAD_TYPE
        //{
        //    DQTYPE_THREAD_DEDICATED,
        //    DQTYPE_THREAD_CURRENT
        //};
        internal enum DISPATCHERQUEUE_THREAD_TYPE
        {
            DQTYPE_THREAD_DEDICATED = 1,
            DQTYPE_THREAD_CURRENT = 2,
        };

        //struct DispatcherQueueOptions
        //{
        //    DWORD dwSize;
        //    DISPATCHERQUEUE_THREAD_TYPE threadType;
        //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        //};
        [StructLayout(LayoutKind.Sequential)]
        internal struct DispatcherQueueOptions
        {
            public int dwSize;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_TYPE threadType;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        };

        //HRESULT CreateDispatcherQueueController(
        //  DispatcherQueueOptions options,
        //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
        //);
        [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
        internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                                 [MarshalAs(UnmanagedType.IUnknown)]
                                        out object dispatcherQueueController);

        #endregion PInvoke declarations
    }

    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }
    #endregion COM Interop
}
```