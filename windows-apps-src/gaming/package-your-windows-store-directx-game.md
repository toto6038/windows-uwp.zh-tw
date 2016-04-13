---
封裝您的通用 Windows 平台 (UWP) DirectX 遊戲
大型通用 Windows 平台 (UWP) 遊戲很容易會膨脹成更大型的遊戲，尤其是支援多國語言，並具有特定區域資產或功能選用高解析度資產的遊戲。
ms.assetid: 68254203-c43c-684f-010a-9cfa13a32a77
---

#  封裝您的通用 Windows 平台 (UWP) DirectX 遊戲


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

大型通用 Windows 平台 (UWP) 遊戲很容易會膨脹成更大型的遊戲，尤其是支援多國語言，並具有特定區域資產或功能選用高解析度資產的遊戲。 在這個主題中，您將了解如何使用應用程式套件與應用程式套件組合來自訂 app，讓客戶只收到他們實際所需的資源。

除了 app 套件模型之外，Windows 10 還支援組合了兩種套件類型的 app 套件組合：

-   app 套件包含平台專屬的可執行檔與程式庫。 一般來說，UWP 遊戲最多可擁有三個 app 套件，分別為適用於 x86、x64 與 ARM CPU 架構的套件。 該硬體平台專用的所有程式碼與資料都必須包含在其 app 套件中。 app 套件也應該包含遊戲的所有核心資產，才能擁有基本的逼真度與效能。
-   資源套件包含選用或擴充的平台中立性資料，例如遊戲資產 (紋理、網格、聲音、文字)。 UWP 遊戲可擁有一或多個資源套件，包含的資源套件適用於高解析度資產或紋理、DirectX 功能層級 11 以上的資源，或特定語言的資產與資源。

如需 app 組合套件與 app 套件的詳細資訊，請閱讀[定義 app 資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321)。

雖然您可將所有內容放置於 app 套件中，但這麼做既沒有效率又是多餘的。 為何要針對每個平台將同樣大小的紋理檔案複製三次呢？特別是 ARM 平台可能用不到。 最好是嘗試將客戶需要下載的項目減到最少，這樣他們才能越快開始遊戲、節省裝置空間並避免可能的計量付費頻寬費用。

若要使用 UWP app 安裝程式的這個功能，請務必在遊戲開發早期先行考量 app 與資源封裝的目錄配置與檔案命名慣例，讓您的工具跟來源能正確輸出目錄配置與檔案命名慣例，讓封裝程序更為簡單。 請在開發或設定資產建立和管理工具與指令碼時，以及在編寫載入或參考資源的程式碼時，遵照此文件概述的規則。

## 為何要建立資源套件？


當您建立 app 時，特別是可在多個地區設定或各種不同的 UWP 硬體平台販售的遊戲 app，您通常需要包含許多檔案的多個版本以支援這些地區設定或平台。 例如，如果您將在美國與日本兩地發行遊戲，您可能需要針對 en-us 地區設定提供一組英文語音檔案，並針對 jp-jp 地區設定提供另一組日文語音檔案。 或者，如果您想要在 ARM 裝置以及 x86 和 x64 平台的遊戲中使用影像，您必須上傳相同的影像資產 3 次，每個 CPU 架構各上傳一次。

此外，如果您的遊戲擁有大量高解析度資源無法適用於較低 DirectX 功能層級的平台，為何要將它們納入基本應用程式套件，並要求使用者下載這些裝置無法使用的大量元件呢？ 將這些高解析度資源分割成選用的資源套件代表，擁有支援這些高解析度資源裝置的客戶可耗費 (通常要計量付費) 頻寬來取得資源，而那些沒有高階裝置的客戶則可以較低的網路使用成本快速取得遊戲。

遊戲資源套件的內容理想選擇包含：

-   國際地區設定特定資產 (當地語系化文字、音訊或影像)
-   適用於不同裝置縮放尺寸 (1.0x、1.4x 與 1.8x) 的高解析度資產
-   適用於較高 DirectX 功能層級 (9、10 與 11) 的高解析度資產

以上所有項目皆定義於屬於 UWP 專案的 package.appxmanifest 中，以及最終套件的目錄結構中。 因為有了新的 Visual Studio UI，如果您按照此文件中的程序操作，應不需要手動進行編輯。

> **重要** 這些資源的載入與管理會透過 **Windows.ApplicationModel.Resources**\* API 加以處理。 如果您使用這些應用程式模型資源 API 載入地區設定、縮放係數或 DirectX 功能層級的正確檔案，則不需使用明確檔案路徑載入資產；只要將您要的資產一般化檔案名稱提供給資源 API，讓資源管理系統為使用者目前的平台與地區設定 (您也可使用這些相同的 API 來直接指定) 取得正確的資源變數。

 

有兩個基本方法可指定資源封裝的資源：

-   資產檔案擁有相同的檔案名稱、且資源套件特定版本放置於特定的已命名目錄中。 這些目錄名稱由系統保留。 例如，\\en-us, \\scale-140, \\dxfl-dx11。
-   資產檔案儲存在任意名稱的資料夾中，但是檔案以一般標籤命名，該標籤會附加系統為表示語言或其他限定詞而保留的字串。 特別是，限定詞字串會附加到一般化檔案名稱的底線 (“\_”) 後面。 例如，\\assets\\menu\_option1\_lang-en-us.png, \\assets\\menu\_option1\_scale-140.png, \\assets\\coolsign\_dxfl-dx11.dds。 您也可以組合這些字串。 例如，\\assets\\menu\_option1\_scale-140\_lang-en-us.png。
    > **注意** 當用於檔案名稱而非單獨使用在目錄名稱時，語言限定詞必須採取格式 "lang-<tag>"，例如 "lang-en-us"，如[如何使用限定詞命名資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)中所述。

     

資源封裝期間可以結合多個目錄名稱使其更具特異性。 不過，名稱不得重複。 例如，\\en-us\\menu\_option1\_lang-en-us.png 就是重複。

您可以在資源目錄下指定任何您需要的未保留子目錄名稱，只要每個資源目錄中的目錄結構都相同即可。 例如，\\dxfl-dx10\\assets\\textures\\coolsign.dds。 當您載入或參考資產時，必須將路徑名稱一般化，針對語言、縮放或 DirectX 功能層級移除資料夾節點或檔案名稱中的所有限定詞。 例如，若要參考程式碼中其中一個變數為 \\dxfl-dx10\\assets\\textures\\coolsign.dds 的資產，請使用 \\assets\\textures\\coolsign.dds。 同樣地，若要參考變數為 \\images\\background\_scale-140.png 的資產，請使用 \\images\\background.png。

以下是保留的目錄名稱與檔案名稱底線首碼：

| 資產類型                   | 資源套件目錄名稱                                                                                                                  | 資源套件檔案名稱尾碼                                                                                                    |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| 當地語系化的資產             | Windows 10 提供的所有可能語言，或語言與地區設定組合。 (在資料夾名稱中，不需要使用限定詞前置詞 "lang-")。 | "\_" 後面緊接語言、地區設定或語言地區設定指定名稱。 例如，分別是 "\_en"、"\_us" 或 "\_en-us"。 |
| 縮放尺寸資產        | scale-100、scale-140、scale-180。 以上分別是針對 1.0x、1.4x 與 1.8x UI 縮放尺寸。                                     | "\_" 後面緊接 "scale-100"、"scale-140" 或 "scale-180"。                                                                    |
| DirectX 功能層級資產 | dxfl-dx9、dxfl-dx10 以及 dxfl-dx11。 以上分別是針對 DirectX 9、10 與 11 功能層級。                                     | "\_" 後面緊接 "dxfl-dx9"、"dxfl-dx10" 或 "dxfl-dx11"。                                                                     |

 

## 定義當地語系化的語言資源套件


地區設定特定檔案位於該語言名稱的專案目錄中 (例如，"en")。

若要將應用程式設定為支援多國語言的當地語系化資產，您應該：

-   為您支援的每個語言與地區設定 (例如，en-us、jp-jp、zh-cn、fr-fr 等等) 建立應用程式子目錄 (或檔案版本)。
-   在開發期間，將所有資產 (例如，當地語系化音訊檔案、紋理與功能表圖形) 的複本放置在對應的語言地區設定子目錄中，即使每種語言或地區設定都一樣。 為取得最佳的使用者體驗，請務必在使用者尚未取得其所屬地區設定的可用語言資源套件 (如果有語言資源套件，或他們在下載與安裝語言資源套件後意外將其刪除) 時，警示使用者。
-   請確定每個目錄中的每個資產或字串資源檔案 (.resw) 都擁有相同的名稱。 例如，即使檔案內容用於不同語言，menu\_option1.png 在 \\en-us 與 \\jp-jp 目錄中應該擁有相同的名稱。 在此案例中，這兩個目錄的檔案名稱為 \\en-us\\menu\_option1.png 與 \\jp-jp\\menu\_option1.png。
    > **注意** 您可選擇性地將地區設定加入檔案名稱中，並將檔案儲存在相同的目錄；例如 \\assets\\menu\_option1\_lang-en-us.png、\\assets\\menu\_option1\_lang-jp-jp.png。

     

-   使用 [**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022) 與 [**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) 中的 API 為您的 app 指定並載入地區設定特定資源。 此外，請使用不包含特定地區設定的資產參考，因為這些 API 會根據使用者的設定判斷正確的地區設定，然後為使用者擷取正確的資源。
-   在 Microsoft Visual Studio 2015 中，選取 [**專案->存放區->建立應用程式套件...**]，然後建立套件。

## 定義縮放尺寸資源套件


Windows 10 提供 3 個使用者介面縮放係數：1.0x、1.4x 與 1.8x。 使用者可變更這些縮放係數，且會根據以下數個組合係數在安裝期間設定預設值：螢幕大小、螢幕解析度，以及使用者與螢幕的假設平均距離。 使用者也可以調整縮放係數以提升可讀性。 您的遊戲應該要同時具備 DPI 感知與縮放尺寸感知，以獲得最佳體驗。 這類感知功能代表您要為這 3 個縮放尺寸個別建立不同的重要視覺資產版本。 這也包含指標互動與點擊測試！

在設定您的 app 以支援不同 UWP app 縮放係數的資源套件時，您應該：

-   為您將支援的每個縮放係數 (scale-100、scale-140 與 scale-180) 建立 app 子目錄 (或檔案版本)。
-   在開發期間，將所有資產的適當縮放尺寸複本放置在每個縮放尺寸資源目錄中，即使它們還沒有縮放尺寸的分別也一樣。
-   請確定每個目錄中的每個資產都擁有相同的名稱。 例如，即使檔案內容不同，menu_option1.png 在 \\scale-100 與 \\scale-180 目錄中應該擁有相同的名稱。 在此案例中，這兩個目錄的檔案名稱為 \\scale-100\\menu\_option1.png 與 \\scale-140\\menu\_option1.png。
    > **注意** 同樣的，您可選擇性地將縮放尺寸尾碼加入檔案名稱，並將檔案儲存在相同的目錄；例如 \\assets\\menu\_option1\_scale-100.png、\\assets\\menu\_option1\_scale-140.png。

     

-   使用 [**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) 中的 API 載入資產。 系統應該將資產參考一般化 (無尾碼)，省去特定的縮放變化。 系統將會針對顯示器和使用者的設定，擷取適當的縮放資產。
-   在 Visual Studio 2015 中，選取 [**專案->存放區->建立應用程式套件...**]，然後建立套件。

## 定義 DirectX 功能層級資源套件


DirectX 功能層級對應到針對前版與目前 DirectX 版本 (特別是，Direct3D) 設定的 GPU 功能。 這包含著色器模型規格與功能、著色器語言支援、紋理壓縮支援，以及整體的圖形管線功能。

您的基礎 App 套件應使用基礎紋理壓縮格式：BC1、BC2 或 BC3。 任何 UWP 裝置，從低階的 ARM 平台到專用的多重 GPU 工作站與媒體電腦，都能使用這些格式。

DirectX 功能層級 10 或更高層級支援的紋理格式應新增到資源套件中，以節省本機磁碟空間與下載頻寬。 這樣就能為 11 使用更進階的壓縮配置，如 BC6H 與 BC7 (如需詳細資訊，請參閱 [Direct3D 11 中的紋理區塊壓縮](https://msdn.microsoft.com/library/windows/desktop/hh308955)。) 這些格式對於現代 GPU 所支援的高解析度紋理資產來說更有效率，使用它們可改善外觀、 效能與您的遊戲在高階平台上的空間需求。

| DirectX 功能層級 | 支援的紋理壓縮 |
|-----------------------|-------------------------------|
| 9                     | BC1、BC2、BC3                 |
| 10                    | BC4、BC5                      |
| 11                    | BC6H、BC7                     |

 

此外，每個 DirectX 功能層級支援不同的著色器模型版本。 編譯的著色器資源可以每個功能層級為基礎加以建立，且可包含在 DirectX 功能層級資源套件中。 此外，某些較新版本的著色器模型可使用舊版著色器模型無法使用的資產 (如一般貼圖)。 這些著色器模型特定資產也應該包含在 DirectX 功能層級資源套件中。

資源機制主要著重在支援的資產紋理格式上，因此它只支援 3 個整體功能層級。 如果您需要在子層級 (副版本) 使用個別著色器，如 DX9\_1 與 DX9\_3，您的資產管理與轉譯程式碼必須明確處理這些著色器。

在設定您的 app 以支援不同 DirectX 功能層級的資源套件時，您應該：

-   為您將支援的每個 DirectX 功能層級 (dxfl-dx9、dxfl-dx10 與 dxfl-dx11) 建立應用程式子目錄 (或檔案版本)。
-   在開發期間，將功能層級特定資產放置於每個功能層級資源目錄。 與地區設定和縮放尺寸不同，您可能在遊戲的每個功能層級中擁有不同的轉譯程式碼分支，如果您的紋理、編譯的著色器或其他資產只用於其中一個支援的功能層級，或功能層級子集中，請將對應的資產只放置在會用到它們的功能層級目錄中。 針對所有功能層級皆會載入的資產，請確定每個功能層級資源目錄都有所屬版本，且名稱相同。 例如，針對名為 "coolsign.dds" 的功能層級獨立紋理，將 BC3 壓縮版放置於 \\dxfl-dx9 目錄，並將 BC7 壓縮版放置於 \\dxfl-dx11 目錄。
-   確定每個資產 (如果可供多個功能層級使用) 在每個目錄中的名稱都相同。 例如，即使檔案內容不同，coolsign.dds 在 \\dxfl-dx9 與 \\dxfl-dx11 目錄中應該擁有相同的名稱。 在此案例中，這兩個目錄的檔案名稱為 \\dxfl-dx9\\coolsign.dds 與 \\dxfl-dx11\\coolsign.dds。
    > **注意** 同樣的，您可選擇性地將功能層級尾碼加入檔案名稱，並將檔案儲存在相同的目錄；例如 \\textures\\coolsign\_dxfl-dx9.dds、\\textures\\coolsign\_dxfl-dx11.dds。

     

-   設定圖形資源時宣告支援的 DirectX 功能層級。
    ```cpp
    D3D_FEATURE_LEVEL featureLevels[] = 
    {
      D3D_FEATURE_LEVEL_11_1,
      D3D_FEATURE_LEVEL_11_0,
      D3D_FEATURE_LEVEL_10_1,
      D3D_FEATURE_LEVEL_10_0,
      D3D_FEATURE_LEVEL_9_3,
      D3D_FEATURE_LEVEL_9_1
    };
    ```

    ```cpp
    ComPtr<ID3D11Device> device;
    ComPtr<ID3D11DeviceContext> context;
    D3D11CreateDevice(
        nullptr,                    // Use the default adapter.
        D3D_DRIVER_TYPE_HARDWARE,
        0,                      // Use 0 unless it is a software device.
        creationFlags,          // defined above
        featureLevels,          // What the app will support.
        ARRAYSIZE(featureLevels),
        D3D11_SDK_VERSION,      // This should always be D3D11_SDK_VERSION.
        &device,                    // created device
        &m_featureLevel,            // The feature level of the device.
        &context                    // The corresponding immediate context.
    );
    ```

-   使用 [**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) 中的 API 載入資源。 系統應該將資產參考一般化 (無尾碼)，省去功能層級。 不過，和語言及縮放不同的是，系統不會自動判斷對指定顯示器最佳的功能層級；也就是會讓您根據程式碼邏輯加以判斷。 一旦您做出判斷之後，使用 API 將慣用的功能層級通知作業系統。 接著，系統就能夠根據該喜好設定，擷取正確的資產。 以下程式碼範例會示範如何將平台目前的 DirectX 功能層級通知您的 app：
    
    ```cpp
    // Set the current UI thread's MRT ResourceContext's DXFeatureLevel with the right DXFL. 

    Platform::String^ dxFeatureLevel;
        switch (m_featureLevel)
        {
        case D3D_FEATURE_LEVEL_9_1:
        case D3D_FEATURE_LEVEL_9_2:
        case D3D_FEATURE_LEVEL_9_3:
            dxFeatureLevel = L"DX9";
            break;
        case D3D_FEATURE_LEVEL_10_0:
        case D3D_FEATURE_LEVEL_10_1:
            dxFeatureLevel = L"DX10";
            break;
        default:
            dxFeatureLevel = L"DX11";
        }

        ResourceContext::SetGlobalQualifierValue(L"DXFeatureLevel", dxFeatureLevel);
    ```

    > **注意** 在您的程式碼中，依名稱 (或功能層級目錄下的路徑) 直接載入紋理。 請勿包含功能層級目錄名稱或尾碼。 例如，載入 "textures\\coolsign.dds"，而非 "dxfl-dx11\\textures\\coolsign.dds" 或 "textures\\coolsign\_dxfl-dx11.dds"。

     

-   現在，使用 [**ResourceManager**](https://msdn.microsoft.com/library/windows/apps/br206078) 來尋找符合目前的 DirectX 功能層級的檔案。 **ResourceManager** 會傳回一個 [**ResourceMap**](https://msdn.microsoft.com/library/windows/apps/br206089) (可以使用 [**ResourceMap::GetValue**](https://msdn.microsoft.com/library/windows/apps/br206098) 或 [**ResourceMap::TryGetValue**](https://msdn.microsoft.com/library/windows/apps/jj655438) 查詢)，還有一個提供的 [**ResourceContext**](https://msdn.microsoft.com/library/windows/apps/br206064)。 這會傳回最符合透過呼叫 [**SetGlobalQualifierValue**](https://msdn.microsoft.com/library/windows/apps/mt622101) 所指定之 DirectX 功能層級的 [**ResourceCandidate**](https://msdn.microsoft.com/library/windows/apps/br206051)。
    
    ```cpp
    // An explicit ResourceContext is needed to match the DirectX feature level for the display on which the current view is presented.

    auto resourceContext = ResourceContext::GetForCurrentView();
    auto mainResourceMap = ResourceManager::Current->MainResourceMap;

    // For this code example, loader is a custom ref class used to load resources.
    // You can use the BasicLoader class from any of the 8.1 DirectX samples similarly.


    auto possibleResource = mainResourceMap->GetValue(
        L"Files/BumpPixelShader.cso",
        resourceContext
    );
    Platform::String^ resourceName = possibleResource->ValueAsString;
    ```

-   在 Visual Studio 2015 中，選取 [**專案->存放區->建立應用程式套件...**]，然後建立套件。
-   確定您在 package.appxmanifest 資訊清單設定中啟用應用程式組合套件。

## 相關主題


* [定義應用程式資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321)
* [封裝 app](https://msdn.microsoft.com/library/windows/apps/mt270969)
* [App 封裝工具 (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767)

 

 






<!--HONumber=Mar16_HO1-->


