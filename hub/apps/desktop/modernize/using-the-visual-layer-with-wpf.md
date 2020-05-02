---
title: 使用視覺層搭配 WPF
description: 了解使用視覺層 API 搭配現有 WPF 內容來建立進階動畫及效果的技術。
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: a2f30ba67acc12d622acd09f9fae872ee2058a2f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "66215148"
---
# <a name="using-the-visual-layer-with-wpf"></a>使用視覺層搭配 WPF

您可以在 Windows Presentation Foundation (WPF) 應用程式中使用 Windows 執行階段 Composition API (又稱為[視覺層](/windows/uwp/composition/visual-layer))，建立適用於 Windows 10 使用者的現代化體驗。

您可在 GitHub 取得本教學課程的完整程式碼：[WPF HelloComposition 範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)。

## <a name="prerequisites"></a>必要條件

UWP XAML 裝載 API 具備下列先決條件。

- 本文假設您對使用 WPF 和 UWP 進行應用程式開發已有一定程度的認識。 如需詳細資訊，請參閱：
  - [使用者入門 (WPF)](/dotnet/framework/wpf/getting-started/)
  - [開始使用 Windows 10 應用程式](/windows/uwp/get-started/)
  - [增強您的 Windows 10 傳統型應用程式](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 或更新版本
- Windows 10 版本 1803 或更新版本
- Windows 10 SDK 17134 或更新版本

## <a name="how-to-use-composition-apis-in-wpf"></a>如何在 WPF 中使用 Composition API

在本教學課程中，您會建立簡單的 WPF 應用程式 UI，並在其中加入動畫 Composition 元素。 WPF 和 Composition 元件兩方都會維持簡單形式，但不論元件的複雜度多高，顯示的 interop 程式碼均相同。 完成的應用程式看起來類似如下。

![執行中的應用程式 UI](images/visual-layer-interop/wpf-comp-interop-app-ui.png)

## <a name="create-a-wpf-project"></a>建立 WPF 專案

第一步是建立 WPF 應用程式專案，裡頭包含應用程式定義和 UI 的 XAML 頁面。

如何使用 Visual C# 建立名稱為 _HelloComposition_ 的 WPF 應用程式專案：

1. 開啟 Visual Studio，然後選取 [檔案]   > [新增]   > [專案]  。

    [新增專案]  對話方塊隨即開啟。
1. 在 [已安裝]  類別下，展開 [Visual C#]  節點，然後選取 [Windows Desktop]  。
1. 選取 [WPF 應用程式 (.NET Framework)]  範本。
1. 輸入名稱 HelloComposition  ，選取 [Framework **.NET Framework 4.7.2**]，然後按一下 [確定]  。

    Visual Studio 會建立專案，並開啟預設應用程式視窗 (名為 MainWindow.xaml) 的設計工具。

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>將專案設定為使用 Windows 執行階段 API

若要在您的 WPF 應用程式中使用 Windows 執行階段 (WinRT) API，需要將 Visual Studio 專案設為存取 Windows 執行階段。 此外，Composition API 會廣泛使用向量，因此需要新增使用向量所需的參考。

NuGet 封裝可用於解決這兩項需求。 請安裝最新版本的封裝，將必要的參考新增至您的專案。  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) (需要將預設的封裝管理格式設為 PackageReference。)
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> 雖然建議使用 NuGet 封裝來設定您的專案，但您可以手動新增需要的參考。 如需詳細資訊，請參閱[增強您的 Windows 10 傳統型應用程式](/windows/uwp/porting/desktop-to-uwp-enhance)。 下表所列的是需要新增參考的檔案。

|檔案|位置|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<sdk 版本  >\Windows.Foundation.UniversalApiContract\<版本  >|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<sdk 版本  >\Windows.Foundation.FoundationContract\<版本  >|
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="configure-the-project-to-be-per-monitor-dpi-aware"></a>將專案設為個別監視器 DPI 感知

您新增至應用程式的視覺層內容不會自動調整成符合其顯示畫面的 DPI 設定。 您需要啟用應用程式的個別監視器 DPI 感知，再確定用於建立視覺層內容的程式碼會考慮到應用程式執行當前的 DPI 縮放比例。 在此會將專案設為 DPI 感知。 後續章節會示範如何使用 DPI 資訊來調整視覺層內容。

預設情況下，WPF 應用程式屬於系統 DPI 感知，但必須在 app.manifest 檔案中，將本身宣告為個別監視器 DPI 感知。 如何在應用程式資訊清單檔案中開啟 Windows 層級的個別監視器 DPI 感知：

1. 在 [方案總管]  中，以滑鼠右鍵按一下 [HelloComposition]  專案。
1. 在操作功能表中，選取 [新增]**Add** > [新增項目...]  。
1. 在 [新增項目]  對話方塊中，選取「應用程式資訊清單檔案」，然後按一下 [新增]  。 (您可以保留預設名稱。)
1. 在 app.manifest 檔案中，尋找此 xml，並取消其註解：

    ```xaml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
        </windowsSettings>
      </application>
    ```

1. 在開頭的 `<windowsSettings>` 標籤後，新增此設定：

    ```xaml
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitor</dpiAwareness>
    ```

1. 您也需要在 App.config 檔案中設定 **DoNotScaleForDpiChanges** 設定值。

    開啟 App.Config，並將此 xml 新增至 `<configuration>` 元素：

    ```xml
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Windows.DoNotScaleForDpiChanges=false"/>
    </runtime>
    ```

> [!NOTE]
> **AppContextSwitchOverrides** 只能設定一次。 如果您的應用程式已經設定過一次，則必須以分號在值屬性內分隔此參數。

(如需詳細資訊，請參閱 GitHub 上的[個別監視器 DPI 開發人員指南與範例](https://github.com/Microsoft/WPF-Samples/tree/master/PerMonitorDPI) (英文)。)

## <a name="create-an-hwndhost-derived-class-to-host-composition-elements"></a>建立 HwndHost 衍生類別以裝載複合元素

若要裝載使用視覺層的內容，您需要建立衍生自 [HwndHost](/dotnet/api/system.windows.interop.hwndhost) 的類別。 您會在這裡處理裝載 Composition API 的大部分設定。 在此類別中，要使用[平台引動服務 (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) 和 [COM Interop](/dotnet/api/system.runtime.interopservices.comimportattribute)，將 Composition API 帶入您的 WPF 應用程式。 如需 PInvoke 和 COM Interop 的詳細資訊，請參閱[與非受控程式碼交互操作](/dotnet/framework/interop/index)。

> [!TIP]
> 如有需要，請查看本教學課程結尾的完整程式碼，確定在逐步進行教學課程時，所有程式碼的位置皆正確。

1. 將新類別檔案新增至衍生自 [HwndHost](/dotnet/api/system.windows.interop.hwndhost) 的專案。
    - 在 [方案總管]  中，以滑鼠右鍵按一下 [HelloComposition]  專案。
    - 在內容功能表中，選取 [新增]   > [類別...]  。
    - 在 [新增項目]  對話方塊中，將類別命名為 _CompositionHost.cs_，然後按一下 [新增]  。
1. 在 CompositionHost.cs 中，編輯類別定義，以從 **HwndHost** 衍生。

    ```csharp
    // Add
    // using System.Windows.Interop;

    namespace HelloComposition
    {
        class CompositionHost : HwndHost
        {
        }
    }
    ```

1. 將以下程式碼和建構函式新增至該類別。

    ```csharp
    // Add
    // using Windows.UI.Composition;

    IntPtr hwndHost;
    int hostHeight, hostWidth;
    object dispatcherQueue;
    ICompositionTarget compositionTarget;

    public Compositor Compositor { get; private set; }

    public Visual Child
    {
        set
        {
            if (Compositor == null)
            {
                InitComposition(hwndHost);
            }
            compositionTarget.Root = value;
        }
    }

    internal const int
      WS_CHILD = 0x40000000,
      WS_VISIBLE = 0x10000000,
      LBS_NOTIFY = 0x00000001,
      HOST_ID = 0x00000002,
      LISTBOX_ID = 0x00000001,
      WS_VSCROLL = 0x00200000,
      WS_BORDER = 0x00800000;

    public CompositionHost(double height, double width)
    {
        hostHeight = (int)height;
        hostWidth = (int)width;
    }
    ```

1. 覆寫 **BuildWindowCore** 和 **DestroyWindowCore** 方法。
    > [!NOTE]
    > 在 BuildWindowCore 中，您可以呼叫 _InitializeCoreDispatcher_ 和 _InitComposition_ 方法。 在後續步驟中會建立上述方法。

    ```csharp
    // Add
    // using System.Runtime.InteropServices;

    protected override HandleRef BuildWindowCore(HandleRef hwndParent)
    {
        // Create Window
        hwndHost = IntPtr.Zero;
        hwndHost = CreateWindowEx(0, "static", "",
                                  WS_CHILD | WS_VISIBLE,
                                  0, 0,
                                  hostWidth, hostHeight,
                                  hwndParent.Handle,
                                  (IntPtr)HOST_ID,
                                  IntPtr.Zero,
                                  0);

        // Create Dispatcher Queue
        dispatcherQueue = InitializeCoreDispatcher();

        // Build Composition tree of content
        InitComposition(hwndHost);

        return new HandleRef(this, hwndHost);
    }

    protected override void DestroyWindowCore(HandleRef hwnd)
    {
        if (compositionTarget.Root != null)
        {
            compositionTarget.Root.Dispose();
        }
        DestroyWindow(hwnd.Handle);
    }
    ```

    - [CreateWindowEx](/windows/desktop/api/winuser/nf-winuser-createwindowexa) 和 [DestroyWindow](/windows/desktop/api/winuser/nf-winuser-destroywindow) 需要 PInvoke 宣告， 請將此宣告放置在該類別的程式碼結尾。

    ```csharp
    #region PInvoke declarations

    [DllImport("user32.dll", EntryPoint = "CreateWindowEx", CharSet = CharSet.Unicode)]
    internal static extern IntPtr CreateWindowEx(int dwExStyle,
                                                  string lpszClassName,
                                                  string lpszWindowName,
                                                  int style,
                                                  int x, int y,
                                                  int width, int height,
                                                  IntPtr hwndParent,
                                                  IntPtr hMenu,
                                                  IntPtr hInst,
                                                  [MarshalAs(UnmanagedType.AsAny)] object pvParam);

    [DllImport("user32.dll", EntryPoint = "DestroyWindow", CharSet = CharSet.Unicode)]
    internal static extern bool DestroyWindow(IntPtr hwnd);

    #endregion PInvoke declarations
    ```

1. 使用 [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher) 初始化執行緒。 核心發送器負責處理 WinRT API 的視窗訊息和分派事件。 **CoreDispatcher** 的新執行個體必須建立在具有 CoreDispatcher 的執行緒上。
    - 建立名為 _InitializeCoreDispatcher_ 的方法，並新增程式碼以設定發送器佇列。

    ```csharp
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

    - 發送器佇列也需要 PInvoke 宣告。 將此宣告放在上一個步驟中建立的 PInvoke 宣告  區域中。

    ```csharp
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
    ```

    現在已準備好發送器佇列，可以開始初始化並建立 Composition 內容。

1. 初始化 [Compositor](/uwp/api/windows.ui.composition.compositor)。 Compositor 是一個處理站，可以在 [Windows.UI.Composition](/uwp/api/windows.ui.composition) 命名空間中建立各種類型，範圍包括視覺效果、效果系統和動畫系統。 Compositor 類別也會管理從處理站建立的物件存留期。

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
        ICompositionTarget target = (ICompositionTarget)rawObject;

        if (raw == null) { throw new Exception("QI Failed"); }
    }
    ```

    - **ICompositorDesktopInterop** 和 **ICompositionTarget** 需要 COM 匯入項目。 請將此程式碼放置在 _CompositionHost_ 類別之後，但需位在命名空間宣告中。

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

## <a name="create-a-usercontrol-to-add-your-content-to-the-wpf-visual-tree"></a>建立 UserControl 以將內容新增至 WPF 視覺化樹狀結構

設定裝載 Composition 內容所需的基礎結構時，最後一步是將 HwndHost 新增至 WPF 視覺化樹狀結構。

### <a name="create-a-usercontrol"></a>建立 UserControl

UserControl 是一種方便的程式碼封裝方式，可封裝用來建立和管理 Composition 內容的程式碼，並且輕鬆將內容新增至您的 XAML。

1. 將新的使用者控制檔案新增至您的專案。
    - 在 [方案總管]  中，以滑鼠右鍵按一下 [HelloComposition]  專案。
    - 在操作功能表中，選取 [新增]   > [使用者控制項...]  。
    - 在 [新增項目]  對話方塊中，將使用者控制項命名為 CompositionHostControl.xaml  ，然後按一下 [新增]  。

    會建立 CompositionHostControl.xaml 和 CompositionHostControl.xaml.cs 檔案並新增到您的專案中。
1. 在 CompositionHostControl.xaml 中，將 `<Grid> </Grid>` 標籤更換為 **Border** 元素，也就是您的 HwndHost 將會進入的 XAML 容器。

    ```xaml
    <Border Name="CompositionHostElement"/>
    ```

在使用者控制項的程式碼中，要建立 CompositionHost 類別 (已於上一步建立) 的執行個體，並將其新增為 CompositionHostElement  (您在 XAML 頁面中建立的 Border) 的子元素。

1. 在 CompositionHostControl.xaml.cs 中，新增會在 Composition 程式碼中使用的物件的私用變數。 請將這些變數新增在類別定義後方。

    ```csharp
    CompositionHost compositionHost;
    Compositor compositor;
    Windows.UI.Composition.ContainerVisual containerVisual;
    DpiScale currentDpi;
    ```

1. 新增使用者控制項 **Loaded** 事件的處理常式。 您要在這裡設定 CompositionHost 執行個體。

    - 在建構函式中，連結事件處理常式，如這裡所示 (`Loaded += CompositionHostControl_Loaded;`)。

    ```csharp
    public CompositionHostControl()
    {
        InitializeComponent();
        Loaded += CompositionHostControl_Loaded;
    }
    ```

    - 新增名稱為 CompositionHostControl_Loaded  的事件處理常式方法。
    ```csharp
    private void CompositionHostControl_Loaded(object sender, RoutedEventArgs e)
    {
        // If the user changes the DPI scale setting for the screen the app is on,
        // the CompositionHostControl is reloaded. Don't redo this set up if it's
        // already been done.
        if (compositionHost is null)
        {
            currentDpi = VisualTreeHelper.GetDpi(this);

            compositionHost =
                new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
            ControlHostElement.Child = compositionHost;
            compositor = compositionHost.Compositor;
            containerVisual = compositor.CreateContainerVisual();
            compositionHost.Child = containerVisual;
        }
    }
    ```

    在這個方法中，要設定會在 Composition 程式碼中使用的物件。 以下會快速介紹一下過程。

    - 首先檢查 CompositionHost 執行個體是否已存在，確定僅進行一次設定。

    ```csharp
    // If the user changes the DPI scale setting for the screen the app is on,
    // the CompositionHostControl is reloaded. Don't redo this set up if it's
    // already been done.
    if (compositionHost is null)
    {

    }
    ```

    - 取得目前的 DPI， 這是用來妥善調整您的 Composition 元素。

    ```csharp
    currentDpi = VisualTreeHelper.GetDpi(this);
    ```

    - 建立 CompositionHost 的執行個體，並將其指派為 Border (CompositionHostElement  ) 的子系。

    ```csharp
    compositionHost =
        new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
    ControlHostElement.Child = compositionHost;
    ```

    - 從 CompositionHost 取得 Compositor。

    ```csharp
    compositor = compositionHost.Compositor;
    ```

    - 使用 Compositor 建立容器視覺效果。 這是您要將 Composition 元素新增至其中的 Composition 容器。

    ```csharp
    containerVisual = compositor.CreateContainerVisual();
    compositionHost.Child = containerVisual;
    ```

### <a name="add-composition-elements"></a>新增 Composition 元素

基礎結構就緒後，即可產生要顯示的 Composition 內容。

在這個範例中，您要新增程式碼 [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) 來建立簡單的方塊並加上動畫。

1. 新增 Composition 元素。 在 CompositionHostControl.xaml.cs 中，將這些方法新增至 CompositionHostControl 類別。

    ```csharp
    // Add
    // using System.Numerics;

    public void AddElement(float size, float offsetX, float offsetY)
    {
        var visual = compositor.CreateSpriteVisual();
        visual.Size = new Vector2(size, size);
        visual.Scale = new Vector3((float)currentDpi.DpiScaleX, (float)currentDpi.DpiScaleY, 1);
        visual.Brush = compositor.CreateColorBrush(GetRandomColor());
        visual.Offset = new Vector3(offsetX * (float)currentDpi.DpiScaleX, offsetY * (float)currentDpi.DpiScaleY, 0);

        containerVisual.Children.InsertAtTop(visual);

        AnimateSquare(visual, 3);
    }

    private void AnimateSquare(SpriteVisual visual, int delay)
    {
        float offsetX = (float)(visual.Offset.X); // Already adjusted for DPI.

        // Adjust values for DPI scale, then find the Y offset that aligns the bottom of the square
        // with the bottom of the host container. This is the value to animate to.
        var hostHeightAdj = CompositionHostElement.ActualHeight * currentDpi.DpiScaleY;
        var squareSizeAdj = visual.Size.Y * currentDpi.DpiScaleY;
        float bottom = (float)(hostHeightAdj - squareSizeAdj);

        // Create the animation only if it's needed.
        if (visual.Offset.Y != bottom)
        {
            Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
            animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
            animation.Duration = TimeSpan.FromSeconds(2);
            animation.DelayTime = TimeSpan.FromSeconds(delay);
            visual.StartAnimation("Offset", animation);
        }
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

### <a name="handle-dpi-changes"></a>處理 DPI 變更

用於新增元素並製作元素動畫的程式碼，會在建立元素時將目前的 DPI 縮放比例考量在內，但也需要考慮到應用程式執行時的 DPI 變更。 您可以控制 [HwndHost.DpiChanged](/dotnet/api/system.windows.interop.hwndhost.dpichanged) 事件，以便接收變更通知，並根據新的 DPI 調整計算。

1. 請在 CompositionHostControl_Loaded 方法中，在最後一行後方新增此事件，以連結 DpiChanged 事件處理常式。

    ```csharp
    compositionHost.DpiChanged += CompositionHost_DpiChanged;
    ```

1. 新增名稱為 CompositionHostDpiChanged  的事件處理常式方法。 這段程式碼會調整每個元素的縮放比例和位移，然後重新計算任何未完成的動畫。

    ```csharp
    private void CompositionHost_DpiChanged(object sender, DpiChangedEventArgs e)
    {
        currentDpi = e.NewDpi;
        Vector3 newScale = new Vector3((float)e.NewDpi.DpiScaleX, (float)e.NewDpi.DpiScaleY, 1);

        foreach (SpriteVisual child in containerVisual.Children)
        {
            child.Scale = newScale;
            var newOffsetX = child.Offset.X * ((float)e.NewDpi.DpiScaleX / (float)e.OldDpi.DpiScaleX);
            var newOffsetY = child.Offset.Y * ((float)e.NewDpi.DpiScaleY / (float)e.OldDpi.DpiScaleY);
            child.Offset = new Vector3(newOffsetX, newOffsetY, 1);

            // Adjust animations for DPI change.
            AnimateSquare(child, 0);
        }
    }
    ```

## <a name="add-the-user-control-to-your-xaml-page"></a>將使用者控制項新增至 XAML 頁面

現在可以將使用者控制項新增至 XAML UI。

1. 在 MainWindow.xaml 中，將 [視窗高度] 設為 600，並將 [寬度] 設為 840。
1. 新增 UI 的 XAML。 在 MainWindow.xaml 中，在根 `<Grid> </Grid>` 標籤之間新增此 XAML。

    ```xaml
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="210"/>
        <ColumnDefinition Width="600"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="46"/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <Button Content="Add composition element" Click="Button_Click"
            Grid.Row="1" Margin="12,0"
            VerticalAlignment="Top" Height="40"/>
    <TextBlock Text="Composition content" FontSize="20"
               Grid.Column="1" Margin="0,12,0,4"
               HorizontalAlignment="Center"/>
    <local:CompositionHostControl x:Name="CompositionHostControl1"
                                  Grid.Row="1" Grid.Column="1"
                                  VerticalAlignment="Top"
                                  Width="600" Height="500"
                                  BorderBrush="LightGray"
                                  BorderThickness="3"/>
    ```

1. 操作按鈕點選動作，以建立新元素。 (XAML 中已連結 Click 事件。)

    在 MainWindow.xaml.cs 中，新增此 Button_Click  事件處理常式方法。 此程式碼會呼叫 CompositionHost.AddElement  ，建立隨機產生大小和位移的新元素。

    ```csharp
    // Add
    // using System;

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Random random = new Random();
        float size = random.Next(50, 150);
        float offsetX = random.Next(0, (int)(CompositionHostControl1.ActualWidth - size));
        float offsetY = random.Next(0, (int)(CompositionHostControl1.ActualHeight/2 - size));
        CompositionHostControl1.AddElement(size, offsetX, offsetY);
    }
    ```

現在可以建置並執行 WPF 應用程式。 如有需要，請查看本教學課程結尾的完整程式碼，確定所有程式碼的位置皆正確。

執行應用程式並按一下按鈕時，應該會看見新增到 UI 的方塊動畫。

## <a name="next-steps"></a>接下來的步驟

如需在相同基礎結構上組建且更完整的範例，請參閱 GitHub 上的 [WPF 視覺層整合範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration) (英文)。

## <a name="additional-resources"></a>其他資源

- [使用者入門 (WPF)](/dotnet/framework/wpf/getting-started/) (.NET)
- [與非受控程式碼交互操作](/dotnet/framework/interop/) (.NET)
- [開始使用 Windows 10 應用程式](/windows/uwp/get-started/) (UWP)
- [增強您的 Windows 10 傳統型應用程式](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition 命名空間](/uwp/api/windows.ui.composition) (UWP) (英文)

## <a name="complete-code"></a>完整程式碼

以下為本教學課程的完整程式碼。

### <a name="mainwindowxaml"></a>MainWindow.xaml

```xaml
<Window x:Class="HelloComposition.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloComposition"
        mc:Ignorable="d"
        Title="MainWindow" Height="600" Width="840">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="210"/>
            <ColumnDefinition Width="600"/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="46"/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Button Content="Add composition element" Click="Button_Click"
                Grid.Row="1" Margin="12,0"
                VerticalAlignment="Top" Height="40"/>
        <TextBlock Text="Composition content" FontSize="20"
                   Grid.Column="1" Margin="0,12,0,4"
                   HorizontalAlignment="Center"/>
        <local:CompositionHostControl x:Name="CompositionHostControl1"
                                      Grid.Row="1" Grid.Column="1"
                                      VerticalAlignment="Top"
                                      Width="600" Height="500"
                                      BorderBrush="LightGray" BorderThickness="3"/>
    </Grid>
</Window>
```

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

```csharp
using System;
using System.Windows;

namespace HelloComposition
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            Random random = new Random();
            float size = random.Next(50, 150);
            float offsetX = random.Next(0, (int)(CompositionHostControl1.ActualWidth - size));
            float offsetY = random.Next(0, (int)(CompositionHostControl1.ActualHeight/2 - size));
            CompositionHostControl1.AddElement(size, offsetX, offsetY);
        }
    }
}
```

### <a name="compositionhostcontrolxaml"></a>CompositionHostControl.xaml

```xaml
<UserControl x:Class="HelloComposition.CompositionHostControl"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
             xmlns:local="clr-namespace:HelloComposition"
             mc:Ignorable="d"
             d:DesignHeight="450" d:DesignWidth="800">
    <Border Name="CompositionHostElement"/>
</UserControl>
```

### <a name="compositionhostcontrolxamlcs"></a>CompositionHostControl.xaml.cs

```csharp
using System;
using System.Numerics;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Media;
using Windows.UI.Composition;

namespace HelloComposition
{
    /// <summary>
    /// Interaction logic for CompositionHostControl.xaml
    /// </summary>
    public partial class CompositionHostControl : UserControl
    {
        CompositionHost compositionHost;
        Compositor compositor;
        Windows.UI.Composition.ContainerVisual containerVisual;
        DpiScale currentDpi;

        public CompositionHostControl()
        {
            InitializeComponent();
            Loaded += CompositionHostControl_Loaded;
        }

        private void CompositionHostControl_Loaded(object sender, RoutedEventArgs e)
        {
            // If the user changes the DPI scale setting for the screen the app is on,
            // the CompositionHostControl is reloaded. Don't redo this set up if it's
            // already been done.
            if (compositionHost is null)
            {
                currentDpi = VisualTreeHelper.GetDpi(this);

                compositionHost = new CompositionHost(CompositionHostElement.ActualHeight, CompositionHostElement.ActualWidth);
                CompositionHostElement.Child = compositionHost;
                compositor = compositionHost.Compositor;
                containerVisual = compositor.CreateContainerVisual();
                compositionHost.Child = containerVisual;
            }
        }

        protected override void OnDpiChanged(DpiScale oldDpi, DpiScale newDpi)
        {
            base.OnDpiChanged(oldDpi, newDpi);
            currentDpi = newDpi;
            Vector3 newScale = new Vector3((float)newDpi.DpiScaleX, (float)newDpi.DpiScaleY, 1);

            foreach (SpriteVisual child in containerVisual.Children)
            {
                child.Scale = newScale;
                var newOffsetX = child.Offset.X * ((float)newDpi.DpiScaleX / (float)oldDpi.DpiScaleX);
                var newOffsetY = child.Offset.Y * ((float)newDpi.DpiScaleY / (float)oldDpi.DpiScaleY);
                child.Offset = new Vector3(newOffsetX, newOffsetY, 1);

                // Adjust animations for DPI change.
                AnimateSquare(child, 0);
            }
        }

        public void AddElement(float size, float offsetX, float offsetY)
        {
            var visual = compositor.CreateSpriteVisual();
            visual.Size = new Vector2(size, size);
            visual.Scale = new Vector3((float)currentDpi.DpiScaleX, (float)currentDpi.DpiScaleY, 1);
            visual.Brush = compositor.CreateColorBrush(GetRandomColor());
            visual.Offset = new Vector3(offsetX * (float)currentDpi.DpiScaleX, offsetY * (float)currentDpi.DpiScaleY, 0);

            containerVisual.Children.InsertAtTop(visual);

            AnimateSquare(visual, 3);
        }

        private void AnimateSquare(SpriteVisual visual, int delay)
        {
            float offsetX = (float)(visual.Offset.X); // Already adjusted for DPI.

            // Adjust values for DPI scale, then find the Y offset that aligns the bottom of the square
            // with the bottom of the host container. This is the value to animate to.
            var hostHeightAdj = CompositionHostElement.ActualHeight * currentDpi.DpiScaleY;
            var squareSizeAdj = visual.Size.Y * currentDpi.DpiScaleY;
            float bottom = (float)(hostHeightAdj - squareSizeAdj);

            // Create the animation only if it's needed.
            if (visual.Offset.Y != bottom)
            {
                Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
                animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
                animation.Duration = TimeSpan.FromSeconds(2);
                animation.DelayTime = TimeSpan.FromSeconds(delay);
                visual.StartAnimation("Offset", animation);
            }
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
using System.Windows.Interop;
using Windows.UI.Composition;

namespace HelloComposition
{
    class CompositionHost : HwndHost
    {
        IntPtr hwndHost;
        int hostHeight, hostWidth;
        object dispatcherQueue;
        ICompositionTarget compositionTarget;

        public Compositor Compositor { get; private set; }

        public Visual Child
        {
            set
            {
                if (Compositor == null)
                {
                    InitComposition(hwndHost);
                }
                compositionTarget.Root = value;
            }
        }

        internal const int
          WS_CHILD = 0x40000000,
          WS_VISIBLE = 0x10000000,
          LBS_NOTIFY = 0x00000001,
          HOST_ID = 0x00000002,
          LISTBOX_ID = 0x00000001,
          WS_VSCROLL = 0x00200000,
          WS_BORDER = 0x00800000;

        public CompositionHost(double height, double width)
        {
            hostHeight = (int)height;
            hostWidth = (int)width;
        }

        protected override HandleRef BuildWindowCore(HandleRef hwndParent)
        {
            // Create Window
            hwndHost = IntPtr.Zero;
            hwndHost = CreateWindowEx(0, "static", "",
                                      WS_CHILD | WS_VISIBLE,
                                      0, 0,
                                      hostWidth, hostHeight,
                                      hwndParent.Handle,
                                      (IntPtr)HOST_ID,
                                      IntPtr.Zero,
                                      0);

            // Create Dispatcher Queue
            dispatcherQueue = InitializeCoreDispatcher();

            // Build Composition Tree of content
            InitComposition(hwndHost);

            return new HandleRef(this, hwndHost);
        }

        protected override void DestroyWindowCore(HandleRef hwnd)
        {
            if (compositionTarget.Root != null)
            {
                compositionTarget.Root.Dispose();
            }
            DestroyWindow(hwnd.Handle);
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

            Compositor = new Compositor();
            object iunknown = Compositor as object;
            interop = (ICompositorDesktopInterop)iunknown;
            IntPtr raw;
            interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

            object rawObject = Marshal.GetObjectForIUnknown(raw);
            compositionTarget = (ICompositionTarget)rawObject;

            if (raw == null) { throw new Exception("QI Failed"); }
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


        [DllImport("user32.dll", EntryPoint = "CreateWindowEx", CharSet = CharSet.Unicode)]
        internal static extern IntPtr CreateWindowEx(int dwExStyle,
                                                      string lpszClassName,
                                                      string lpszWindowName,
                                                      int style,
                                                      int x, int y,
                                                      int width, int height,
                                                      IntPtr hwndParent,
                                                      IntPtr hMenu,
                                                      IntPtr hInst,
                                                      [MarshalAs(UnmanagedType.AsAny)] object pvParam);

        [DllImport("user32.dll", EntryPoint = "DestroyWindow", CharSet = CharSet.Unicode)]
        internal static extern bool DestroyWindow(IntPtr hwnd);


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