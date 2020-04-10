---
title: 使用視覺層搭配 Windows Forms
description: 了解使用視覺層 API 搭配現有 Windows Forms 內容來建立進階動畫及效果的技術。
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 9da9dee48beef6e3c1cd38ffbe9761ed89fd940d
ms.sourcegitcommit: 93d0b2996b4742b33cd6d641e036f42672cf5238
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/23/2019
ms.locfileid: "69999643"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>使用視覺層搭配 Windows Forms

您可在 Windows Forms 應用程式中使用 Windows 執行階段 Composition API (又稱為[視覺層](/windows/uwp/composition/visual-layer))，以建立適用於 Windows 10 使用者的現代化體驗。

您可在 GitHub 上取得本教學課程的完整程式碼：[Windows Forms HelloComposition 範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition).

## <a name="prerequisites"></a>必要條件

UWP 主控 API 具備下列先決條件。

- 假設您對使用 Windows Forms 和 UWP 進行應用程式開發已有一些熟悉。 如需詳細資訊，請參閱：
  - [開始使用 Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [開始使用 Windows 10 應用程式](/windows/uwp/get-started/)
  - [增強您的 Windows 10 傳統型應用程式](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 或更新版本
- Windows 10 版本 1803 或更新版本
- Windows 10 SDK 17134 或更新版本

## <a name="how-to-use-composition-apis-in-windows-forms"></a>如何在 Windows Forms 中使用 Composition API

在本教學課程中，您會建立簡單的 Windows Forms UI，並在其中加入動畫 Composition 元素。 Windows Forms 和 Composition 元件都保持簡單，但不論元件的複雜度為何，顯示的 Interop 程式碼都相同。 完成的應用程式看起來像這樣。

![執行中的應用程式 UI](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>建立 Windows Forms 專案

第一步是建立 Windows Forms 應用程式專案，其包含應用程式定義和 UI 的主要表單。

若要使用 Visual C# 建立名稱為 _HelloComposition_ 的 Windows Forms 應用程式專案，請執行下列動作：

1. 開啟 Visual Studio，然後選取 [檔案]   > [新增]   > [專案]  。<br/>[新增專案]  對話方塊隨即開啟。
1. 在 [已安裝]  類別下，展開 [Visual C#]  節點，然後選取 [Windows Desktop]  。
1. 選取 [Windows Forms 應用程式 (.NET Framework)]  範本。
1. 輸入名稱 _HelloComposition_，選取 Framework **.NET Framework 4.7.2**，然後按一下 [確定]  。

Visual Studio 會建立專案，並針對名為 Form1.cs 的預設應用程式視窗，開啟設計工具。

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>設定專案以使用 Windows 執行階段 API

若要在您的 Windows Forms 應用程式中使用 Windows 執行階段 (WinRT) API，您需要設定 Visual Studio 專案，以存取 Windows 執行階段。 此外，Composition API 會廣泛使用向量，因此，您需要新增使用向量所需的參考。

NuGet 套件可用於解決這兩項需求。 安裝最新版的套件，將必要的參考新增至您的專案。  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) (需要設為 PackageReference 的預設套件管理格式。)
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> 雖然我們建議使用 NuGet 套件來設定您的專案，但您可以手動新增需要的參考。 如需詳細資訊，請參閱[增強您的 Windows 10 傳統型應用程式](/windows/uwp/porting/desktop-to-uwp-enhance)。 下表顯示您需要加入參考的檔案。

|檔案|位置|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk 版本*>\Windows.Foundation.UniversalApiContract\<*版本*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk 版本*>\Windows.Foundation.FoundationContract\<*版本*>|
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="create-a-custom-control-to-manage-interop"></a>建立自訂控制項來管理 Interop

若要裝載使用視覺層所建立的內容，可以建立衍生自 [Control](/dotnet/api/system.windows.forms.control) 的自訂控制項。 此控制項可讓您存取視窗 [Handle](/dotnet/api/system.windows.forms.control.handle)，以便建立視覺層內容的容器。

您可以在這裡進行用於託管 Composition API 的大部分組態。 在此控制項中，您會使用 [平台叫用服務 (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) 和 [COM Interop](/dotnet/api/system.runtime.interopservices.comimportattribute)，將 Composition API 帶入您的 Windows Forms 應用程式。 如需 PInvoke 和 COM Interop 的詳細資訊，請參閱[與非受控程式碼互通](/dotnet/framework/interop/index)。

> [!TIP]
> 如有需要，請在本教學課程最後檢查完整的程式碼，以確保當您逐步進行教學課程時，所有程式碼皆位於正確的位置。

1. 將新的自訂控制項檔案新增到衍生自 [Control](/dotnet/api/system.windows.forms.control) 的專案。
    - 在 [方案總管]  中，以滑鼠右鍵按一下 _HelloComposition_ 專案。
    - 在操作功能表中，選取 [新增]**Add** > [新項目...]  。
    - 在 [新增項目]  對話方塊中，選取 [報表控制項]  。
    - 將控制項命名為 _CompositionHost.cs_，然後按一下 [新增]  。 CompositionHost.cs 會在 [設計] 檢視中開啟。

1. 切換至 CompositionHost.cs 的程式碼檢視，並將下列程式碼新增至此類別。

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

1. 將程式碼新增至建構函式。

    在建構函式中，您可以呼叫 _InitializeCoreDispatcher_ 和 _InitComposition_ 方法。 您會在後續步驟中建立這些方法。

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

1. 使用 [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher) 初始化執行緒。 核心發送器負責處理 WinRT API 的視窗訊息和分派事件。 **Compositor** 的新執行個體必須建立在具有 CoreDispatcher 的執行緒上。
    - 建立名為 _InitializeCoreDispatcher_ 的方法，並新增程式碼以設定發送器佇列。

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

    - 發送器佇列需要 PInvoke 宣告。 將此宣告放置在該類別的程式碼結尾。 (我們會將此程式碼放在區域內，讓類別程式碼保持整齊。)

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

    您現在已準備好發送器佇列，並可開始初始化並建立組合內容。

1. 初始化 [Compositor](/uwp/api/windows.ui.composition.compositor)。 Compositor 是一個處理站，可以在跨越視覺層、效果系統和動畫系統的 [Windows.UI.Composition](/uwp/api/windows.ui.composition) 命名空間中建立各種類型。 Compositor 類別也會管理從處理站建立的物件存留期。

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

    - **ICompositorDesktopInterop** 和 **ICompositionTarget** 需要 COM 匯入。 將此程式碼放置在 _CompositionHost_ 類別後面，但在命名空間宣告中。

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

## <a name="create-a-custom-control-to-host-composition-elements"></a>建立自訂控制項來裝載組合元素

建議您將可產生和管理組合元素的程式碼放在衍生自 CompositionHost 的個別控制項中。 這會讓您在 CompositionHost 類別中建立的 Interop 程式碼得以重複使用。

在此，您會建立衍生自 CompositionHost 的自訂控制項。 此控制項會新增至 Visual Studio 工具箱，讓您可將其新增至表單。

1. 將新的自訂控制項檔案新增到衍生自 CompositionHost 的專案。
    - 在 [方案總管]  中，以滑鼠右鍵按一下 _HelloComposition_ 專案。
    - 在操作功能表中，選取 [新增]**Add** > [新項目...]  。
    - 在 [新增項目]  對話方塊中，選取 [報表控制項]  。
    - 將控制項命名為 _CompositionHostControl.cs_，然後按一下 [新增]  。 CompositionHostControl.cs 會在 [設計] 檢視中開啟。

1. 在 CompositionHostControl.cs 設計檢視的 [屬性] 窗格中，將 **BackColor** 屬性設定為 **ControlLight**。

    設定背景色彩是選擇性作業。 我們在此執行此作業，讓您可以看到表單背景的自訂控制項。

1. 切換至 CompositionHostControl.cs 的程式碼檢視，並將類別宣告更新為衍生自 CompositionHost。

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. 更新建構函式以呼叫基底建構函式。

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>新增組合元素

基礎結構已就緒之後，即可將組合內容新增至應用程式 UI。

在此範例中，您將程式碼新增至 CompositionHostControl 類別，以建立簡單的 [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)並產生其動畫。

1. 新增組合元素。

    在 CompositionHostControl.cs 中，將這些方法新增至 CompositionHostControl 類別。

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

您現在已有可裝載組合內容的自訂控制項，請將其新增至應用程式 UI。 在此，您會新增在上一個步驟中建立的 CompositionHostControl 執行個體。 CompositionHostControl 會自動新增至 Visual Studio 工具箱的 [_專案名稱_ 元件]  之下。

1. 在 Form1.CS 設計檢視中，將 Button 新增至 UI。

    - 從工具箱將 Button 拖曳至 Form1。 將其放在表單的左上角。 (請參閱教學課程開頭的影像，以檢查控制項的位置。)
    - 在 [屬性] 窗格中，將 [Text]  屬性從 [button1]  變更為 [新增組合元素]  。
    - 調整 Button 的大小，以便顯示所有文字。

    (如需詳細資訊，請參閱 [如何：將控制項新增至 Windows Forms](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms)。)

1. 將 CompositionHostControl 新增至 UI。

    - 將 CompositionHostControl 從工具箱拖曳至 Form1。 將其放在 Button 的右邊。
    - 調整 CompositionHost 的大小，使其填滿表單的其餘部分。

1. 處理 button click 事件。

   - 在 [屬性] 窗格中，按一下閃電以切換至 [事件] 檢視。
   - 在事件清單中，選取 [Click]  事件，鍵入 Button_Click  ，然後按 Enter。
   - 此程式碼會在 Form1.cs 中新增：

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. 將程式碼新增至 button click 處理常式，以建立新的元素。

    - 在 Form1.cs 中，將程式碼新增至您先前建立的 *Button_Click* 事件處理常式。 此程式碼會呼叫 _CompositionHostControl1.AddElement_，以建立具有隨機產生大小和位移的新元素。 (CompositionHostControl 的執行個體會在您將其拖曳到表單時，自動命名為 _compositionHostControl1_。)

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

您現在可以建置及執行 Windows Forms 應用程式。 當您按一下按鈕時，應該會看見新增到 UI 的動畫方塊。

## <a name="next-steps"></a>接下來的步驟

如需在相同基礎結構上建置的更完整範例，請參閱 GitHub 上的 [Windows Forms 視覺層整合範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)。

## <a name="additional-resources"></a>其他資源

- [Windows Forms 使用者入門](/dotnet/framework/winforms/getting-started-with-windows-forms) (.NET)
- [與非受控程式碼互通](/dotnet/framework/interop/) (.NET)
- [開始使用 Windows 10 應用程式](/windows/uwp/get-started/) (UWP)
- [增強您的 Windows 10 傳統型應用程式](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition 命名空間](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>完整程式碼

這裡提供本教學課程的完整程式碼。

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