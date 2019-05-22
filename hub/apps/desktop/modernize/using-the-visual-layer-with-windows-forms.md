---
title: 使用 Windows Form 中的視覺分層
description: 了解技術視覺化的圖層 Api 搭配使用與現有的 Windows Form 內容來建立進階的動畫和效果。
ms.date: 03/18/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0a3aff0bee68b971accd96f895601343726024d0
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985028"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>使用 Windows Form 中的視覺分層

您可以使用 Windows 執行階段撰寫 Api (也稱為[視覺分層](/windows/uwp/composition/visual-layer)) 在您的 Windows Forms 應用程式，以建立新式體驗也淺註冊 Windows 10 使用者。

本教學課程的完整程式碼是可在 GitHub 上：[Windows Form HelloComposition 範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)。

## <a name="prerequisites"></a>先決條件

裝載 API UWP 有這些必要條件。

- 我們假設您已熟悉使用 Windows Form 和 UWP 應用程式開發。 如需詳細資訊，請參閱：
  - [Windows Form 使用者入門](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [開始使用 Windows 10 應用程式](/windows/uwp/get-started/)
  - [加強適用於 Windows 10 桌面應用程式](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET framework 4.7.2 或更新版本
- Windows 10 1803 （含） 以後版本
- 17134 或更新版本的 Windows 10 SDK

## <a name="how-to-use-composition-apis-in-windows-forms"></a>如何在 Windows Form 中使用組合 Api

在本教學課程中，您將建立簡單的 Windows Forms UI 和動畫的撰寫項目加入。 在 Windows Form 和組合元件會保持簡單，但顯示的 interop 程式碼是相同不論元件的複雜度。 完成的應用程式看起來像這樣。

![執行的應用程式 UI](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>建立 Windows Forms 專案

第一個步驟是建立 Windows Forms 應用程式專案，其中包含應用程式定義和主表單 ui。

若要建立視覺效果中的新 Windows Forms 應用程式專案C#名為_HelloComposition_:

1. 開啟 Visual Studio，然後選取**檔案** > **新增** > **專案**。<br/>**新的專案** 對話方塊隨即開啟。
1. 底下**已安裝**分類中，展開**Visual C#** 節點，然後再選取**Windows Desktop**。
1. 選取  **Windows Forms 應用程式 (.NET Framework)** 範本。
1. 輸入名稱_HelloComposition，_ 選取 Framework **.NET Framework 4.7.2**，然後按一下**確定**。

Visual Studio 會建立專案，並開啟名為 Form1.cs 的預設應用程式視窗的設計工具。

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>將專案設定為使用 Windows 執行階段 Api

若要使用 Windows 執行階段 (WinRT) Api 在 Windows Forms 應用程式中，您需要設定您的 Visual Studio 專案以存取 Windows 執行階段。 此外，向量廣泛所撰寫的 Api，因此您需要新增參考，才能使用向量。

NuGet 套件是可用來解決這兩個這些需求。 安裝這些封裝來新增必要參考加入專案的最新版本。  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) （需要預設套件管理格式設為 PackageReference）。
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> 雖然我們建議您設定專案使用的 NuGet 套件，您可以手動新增所需的參考。 如需詳細資訊，請參閱 <<c0> [ 加強您的桌面應用程式適用於 Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)。 下表顯示您需要將參考新增至檔案。

|檔案|Location|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.UniversalApiContract\<*version*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.FoundationContract\<*version*>|
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="create-a-custom-control-to-manage-interop"></a>建立自訂控制項，以管理 interop

主機內容建立視覺化的圖層，您可以建立自訂控制項衍生自[控制](/dotnet/api/system.windows.forms.control)。 這個控制項可讓您存取視窗[處理](/dotnet/api/system.windows.forms.control.handle)，您需要以建立您的視覺分層內容的容器。

這是組態的您用來大部分的裝載 Api 組合。 在此控制項中，您可以使用[平台引動服務 (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code)並[COM Interop](/dotnet/api/system.runtime.interopservices.comimportattribute)帶入您的 Windows Forms 應用程式撰寫的 Api。 如需 PInvoke 和 COM Interop 的詳細資訊，請參閱 <<c0> [ 與 unmanaged 程式碼交互操作](/dotnet/framework/interop/index)。

> [!TIP]
> 如果您要檢查的完整程式碼的教學課程，以確定所有的程式碼是在正確的位置，當您完成本教學課程結尾處。

1. 衍生自專案中加入新的自訂控制項檔案[控制](/dotnet/api/system.windows.forms.control)。
    - 在 **方案總管**，以滑鼠右鍵按一下_HelloComposition_專案。
    - 在操作功能表中，選取**新增** > **新項目...**.
    - 在 **加入新項目**對話方塊中，選取**自訂控制項**。
    - 將控制項_CompositionHost.cs_，然後按一下**新增**。 在 [設計] 檢視中，開啟 CompositionHost.cs。

1. 切換至 CompositionHost.cs 的程式碼檢視，並將下列程式碼新增至類別。

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

1. 您可以將程式碼加入建構函式。

    建構函式，在您呼叫_InitializeCoreDispatcher_並_InitComposition_方法。 您在後續步驟中建立這些方法。

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

1. Initialize a thread with a [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). The core dispatcher is responsible for processing window messages and dispatching events for WinRT APIs. New instances of **Compositor** must be created on a thread that has a CoreDispatcher.
    - Create a method named _InitializeCoreDispatcher_ and add code to set up the dispatcher queue.

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

    - 發送器佇列需要 PInvoke 宣告。 將這個宣告類別的程式碼的結尾。 （我們放入這個程式碼將順利類別程式碼區域。）

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

    您現在已準備好在發送器佇列，而可以開始初始化，並建立組合內容。

1. 初始化[複合](/uwp/api/windows.ui.composition.compositor)。 複合項是建立各種類型中的處理站[Windows.UI.Composition](/uwp/api/windows.ui.composition)視覺分層、 effects 系統，及動畫系統的命名空間。 複合類別也會管理從處理站建立物件的存留期。

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

    - **ICompositorDesktopInterop**並**ICompositionTarget**需要 COM 匯入。 將此程式碼後的_CompositionHost_類別，但在命名空間宣告。

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

## <a name="create-a-custom-control-to-host-composition-elements"></a>建立自訂控制項以主控件組合項目

它是個不錯的主意，將會產生和管理您的撰寫項目，衍生自 CompositionHost 個別控制項中的程式碼。 如此可讓您建立 CompositionHost 類別中可重複使用的 interop 程式碼。

在這裡，您可以建立自訂控制項衍生自 CompositionHost。 這個控制項被新增至 Visual Studio 工具箱中，因此您可以將它新增至您的表單。

1. 衍生自 CompositionHost 專案中加入新的自訂控制項檔案。
    - 在 **方案總管**，以滑鼠右鍵按一下_HelloComposition_專案。
    - 在操作功能表中，選取**新增** > **新項目...**.
    - 在 **加入新項目**對話方塊中，選取**自訂控制項**。
    - 將控制項_CompositionHostControl.cs_，然後按一下**新增**。 在 [設計] 檢視中，開啟 CompositionHostControl.cs。

1. 在 CompositionHostControl.cs [設計] 檢視 [屬性] 窗格中，設定**BackColor**屬性設**ControlLight**。

    設定背景色彩是選擇性的。 此處我們要它讓您可以看到您的自訂控制項與表單背景。

1. 切換至 CompositionHostControl.cs 的程式碼檢視，並更新 CompositionHost 從衍生類別宣告。

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. 更新的建構函式呼叫基底建構函式。

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>將複合項目

就地基礎結構，您現在可以組合內容新增至應用程式 UI。

您可以針對此範例中，加入程式碼來建立，並以動畫顯示簡單的 CompositionHostControl 類別[SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)。

1. 新增撰寫項目。

    在 CompositionHostControl.cs，加入 CompositionHostControl 類別中的這些方法。

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

已自訂的控制項，以主控組合內容之後，您可以將它新增至應用程式 UI。 在這裡，您會新增 CompositionHostControl 您在上一個步驟中建立的執行個體。 Visual Studio 工具箱底下會自動加入 CompositionHostControl **_專案名稱_元件**。

1. 在 Form1.CS [設計] 檢視中加入一個按鈕的 ui。

    - 將按鈕從 [工具箱] 拖曳至 Form1。 請將它放在表單的左上角。 （請參閱教學課程中，若要檢查的控制項位置開頭的映像）。
    - 在 [屬性] 窗格中，變更**文字**屬性從_button1_來_新增撰寫項目_。
    - 調整按鈕的大小，讓所有文字會都顯示。

    (如需詳細資訊，請參閱[How to:將控制項新增至 Windows Forms](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms)。)

1. 新增 CompositionHostControl ui。

    - 拖曳 CompositionHostControl 從 [工具箱] 拖曳至 Form1。 將它放在右邊的按鈕。
    - 調整 CompositionHost，使它填滿表單的其餘部分。

1. 控制代碼的按鈕 click 事件。

   - 在 [屬性] 窗格中，按一下閃電切換至 [事件] 檢視。
   - 在 事件 清單中，選取**按一下 **事件中，輸入*Button_Click*，然後按 Enter。
   - 在 Form1.cs 中會加入此程式碼：

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. 將程式碼加入至按鈕 click 處理常式來建立新的項目。

    - 在 form1.cs 檔，加入程式碼*Button_Click*您先前建立的事件處理常式。 此程式碼會呼叫_CompositionHostControl1.AddElement_來建立新的項目，使用隨機產生的大小和位移。 (CompositionHostControl 的執行個體已自動命名_compositionHostControl1_當您將它拖曳至表單。)

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

您現在可以建置並執行您的 Windows Forms 應用程式。 當您按一下按鈕時，您應該會看到動畫加入至 UI 的平方。

## <a name="next-steps"></a>後續步驟

相同的基礎結構為基礎的更完整範例，請參閱 < [Windows Forms 視覺化的圖層整合範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)GitHub 上。

## <a name="additional-resources"></a>其他資源

- [開始使用 Windows Form](/dotnet/framework/winforms/getting-started-with-windows-forms) (.NET)
- [以相互操作 unmanaged 程式碼](/dotnet/framework/interop/)(.NET)
- [開始使用 Windows 10 應用程式](/windows/uwp/get-started/)(UWP)
- [增強您的桌面應用程式適用於 Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition 命名空間](/uwp/api/windows.ui.composition)(UWP)

## <a name="complete-code"></a>完整程式碼

本教學課程中，以下是完整的程式碼。

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