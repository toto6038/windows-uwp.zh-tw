---
title: 開始使用 Visual Studio for UWP 遊戲
description: 了解如何設定 Visual Studio 專案，若要啟用 UWP 遊戲的 Xbox Live
ms.assetid: b53bc91f-79db-4d8f-8919-b9144e2d609b
ms.date: 11/28/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 314d5e937bb8680dc26b7dfdfa15ff90f8f26c81
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651403"
---
# <a name="get-started-using-visual-studio-for-uwp-games"></a>開始使用 Visual Studio for UWP 遊戲

## <a name="requirements"></a>需求

1. 中的註冊**[合作夥伴中心開發人員計劃](https://developer.microsoft.com/store/register)**。
2. **[Windows 10](https://microsoft.com/windows)**。
3. **[Visual Studio](https://www.visualstudio.com/)** 具有**通用 Windows 應用程式開發工具**。 所需要的最低的 UWP 應用程式的版本為 Visual Studio 2015 Update 3。 我們建議您使用最新版的 Visual Studio 開發人員和安全性更新。 
4. **[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) v10.0.10586.0**或更新版本。

> [!IMPORTANT]
> 如果使用 Windows 10 SDK (也稱為 Creators Update) 10.0.15063.0 版，則需要 visual Studio 2017 或更新版本。

## <a name="create-a-new-product-in-partner-center"></a>在合作夥伴中心建立新的產品

每個 Xbox Live 的標題必須有在中建立的產品[合作夥伴中心](https://partner.microsoft.com/dashboard)您將能夠登入，並進行 Xbox Live 服務呼叫之前。 請參閱[UDC 上建立一個標題](create-a-new-title.md)如需詳細資訊。

## <a name="configuring-your-development-device"></a>設定開發裝置

是您在裝置上所需的下列初步安裝步驟，讓您可以成功使用 Xbox Live 和呼叫各種的 Xbox Live 服務的登入。

### <a name="set-your-sandbox"></a>設定您的沙箱

沙箱提供，讓您[Xbox Live 服務組態](../xbox-live-service-configuration.md)分開零售，直到您準備好發行您的標題。 您可能會累積一些資料是特有的沙箱。 例如假設您的標題會定義狀態稱為*致勝*，及您測試您的職稱時累積多個使用者帳戶中的致勝。 這個值會是特定項目的開發沙箱和致勝如果您切換到播放程式標題的零售版本，不會提供。

請參閱[Xbox Live 沙箱](../xbox-live-sandboxes.md)文章，以進一步了解，並了解如何設定您的沙箱。

### <a name="sign-in-with-a-test-account"></a>使用測試帳戶登入

登入您的開發沙箱，您必須建立測試帳戶，或您的沙箱中佈建存取一般 Microsoft 帳戶 (MSA)。 這會提供您的遊戲程式開發，以及一些其他優點的改進的安全性。

若要深入了解測試帳戶，以及如何建立一個，請參閱[Xbox Live 的測試帳戶](../xbox-live-test-accounts.md)

## <a name="visual-studio-project-setup"></a>Visual Studio 專案安裝程式

### <a name="1-open-a-uwp-project"></a>1.開啟 UWP 專案
如果您還沒有現有的 UWP 專案，您可以建立一個執行下列動作：

1. 在 Visual Studio 中，**檔案** > **新** > **專案**。
2. 在 **新的專案**對話方塊中，選取**視覺化C#**   >  **Windows** > **通用**節點在左的窗格中，按一下**空白應用程式 (通用 Windows)** 從右窗格中。
3. 在對話方塊的下半部中，提供專案名稱，並指定專案的位置。
4. 指定的目標版本和最低版本的 Windows 10 SDK。 請參閱[選擇 UWP 版本](https://docs.microsoft.com/windows/uwp/updates-and-versions/choose-a-uwp-version)如需詳細資訊。

![在 VS 中建立專案](../images/getting_started/vs-create-project.gif)

> [!NOTE]
> Xbox Live API (XSAPI) 需要 10.0.10586.0 的最小版本或更高版本。

### <a name="2-add-references-to-the-xbox-live-api-xsapi-in-your-project"></a>2.在您的專案中加入參考至 Xbox Live API (XSAPI)

Xbox 服務 API UWP 和 XDK，以及 c + + 和 WinRT 類別有和其命名空間建構為**Microsoft.Xbox.Live.SDK.*。UWP**和**Microsoft.Xbox.Live.SDK.*。XboxOneXDK**。

1. **UWP**適用於開發人員建置 UWP 遊戲時，它可以在 PC、 Xbox One 或 Windows Phone 上執行。
2. **XboxOneXDK**適合ID@Xbox和管理使用 Xbox One XDK 的開發人員。
3. C + + SDK 可用來針對 c + + 的遊戲引擎，其中當做 WinRT SDK 是使用 c + + 撰寫的遊戲引擎C#，或 JavaScript。
4. 使用 c + + 引擎 WinRT 時，您應該使用 C + + /CX 使用 hats (^)。 C + + 是建議的 API，可用於 c + + 的遊戲引擎。  

> [!TIP]
> 您可以深入了解 Xbox One 在上執行 UWP [Xbox One 上的 UWP 應用程式開發入門](https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started)。

若要使用 Xbox Live API 從您的專案，您可以藉由使用 NuGet 套件，或加入 API 來源加入至二進位檔的參考。 新增 NuGet 套件可以加快編譯過程，而新增來源可以讓偵錯變容易。 本文將逐步引導您使用 NuGet 套件。 如果您想要使用來源，請參閱[編譯的 Xbox Live Api 來源專案中的 UWP](add-xbox-live-apis-source-to-a-uwp-project.md)。 您可以加入 Xbox Live SDK NuGet 封裝：

1. 在 Visual Studio 中，移至**工具** > **NuGet 套件管理員** > **管理方案的 NuGet 套件...**.
2. 在 [NuGet 套件管理員] 中，按一下**瀏覽**，然後輸入**Xbox.Live.SDK**在搜尋方塊中。
3. 選取的 Xbox Live SDK，您想要從左側清單中使用的版本。 在此情況下，我們將使用 Microsoft.Xbox.Live.SDK.WinRT.UWP 封裝。
3. 在視窗的右側，請檢查您的專案旁邊的方塊，然後按一下**安裝**。

![新增透過 NuGet XBL](../images/getting_started/vs-add-nuget-xbl.gif)


> [!IMPORTANT]
> 針對`Microsoft.Xbox.Live.SDK.Cpp.*`專案，請務必包含標頭`#include <xsapi\services.h>`中專案的來源。

### <a name="3-optional-using-connected-storage-andor-secure-sockets"></a>3.（選擇性）使用已連接儲存體和/或安全通訊端
根據您使用的 Windows sdk 版本，您可能需要安裝額外的內容，或以手動方式將參考加入至您的專案才能使用 Xbox Live[連接的儲存體](../storage-platform/connected-storage/connected-storage-technical-overview.md)或安全通訊端。 如果您想要使用連接儲存體功能，您將需要存取`Windows.Gaming.XboxLive.Storage`命名空間。 如果您想要使用安全通訊端，您將需要存取`Windows.Networking.XboxLive`。

#### <a name="windows-10-sdk-version-10016299-or-higher"></a>Windows 10 SDK 版本 10.0.16299 或更新版本
如果您的目標 Windows 10 SDK 10.0.16299 或更新版本中，然後您將能夠存取連接的儲存體的命名空間，而不進行任何額外的工作。 若要存取安全通訊端，您必須將參考加入**適用於 UWP 的 Windows 桌面延伸模組**。 您可以藉由來這樣做：

1. 在 [**方案總管] 中**，以滑鼠右鍵按一下**參考**節點，然後挑選**加入參考...**
2. 在左邊**參考管理員**對話方塊中，選取**通用 Windows** > **延伸**。
3. 在出現的清單，搜尋**適用於 UWP 的 Windows 桌面延伸模組**選取符合您的 Windows 10 SDK 的版本旁邊的核取方塊。
4. 按一下 [確定] 。

![在 VS 中加入新參考](../images/getting_started/get-started-vs-add-ref.png)

#### <a name="windows-10-sdk-version-10015063-or-lower"></a>Windows 10 SDK 版本 10.0.15063 或更舊版本
如果您想要使用連接儲存體 」 或 「 安全通訊端，您必須安裝 Xbox Live 平台擴充功能 SDK，才能新增至您的專案參考。 您可以藉由來這樣做：

1. 下載並解壓縮[Xbox Live 平台擴充功能 SDK](https://aka.ms/xblextsdk)。
2. 解壓縮之後，請執行包含符合您所使用的 Windows 10 SDK 版本的 MSI 檔案。

您已安裝的 Xbox Live 平台擴充功能 SDK 之後，您必須在 Visual Studio 中加入它的參考。 您可以藉由來這樣做：

1. 在 [**方案總管] 中**，以滑鼠右鍵按一下**參考**節點，然後挑選**加入參考...**
2. 在左邊**參考管理員**對話方塊中，選取**通用 Windows** > **延伸**。
3. 在出現的清單，搜尋**適用於 UWP 的 Windows 桌面延伸模組**選取符合您的 Windows 10 SDK 的版本旁邊的核取方塊。
4. 按一下 [確定] 。

### <a name="4-associate-your-visual-studio-project-with-your-uwp-app"></a>4.您的 UWP 應用程式相關聯的 Visual Studio 專案

針對您的遊戲能夠登入必須是您在合作夥伴中心中建立的產品與相關聯。 您可以使用市集關聯精靈將 Visual Studio 中的遊戲。 在 Visual Studio 中，執行下列作業：

1.  以滑鼠右鍵按一下主專案 （啟始專案），請按一下**存放區** > **與市集建立關聯的應用程式...**
2.  使用登入**Windows 開發人員帳戶**用來建立應用程式，如果系統要求，並遵循提示。

> [!TIP]
> 請參閱[封裝應用程式](https://docs.microsoft.com/windows/uwp/packaging/)如需有關準備適用於 Windows 市集的遊戲。

### <a name="5-add-internet-capabilities-to-your-visual-studio-project"></a>5.Visual Studio 專案中加入網際網路功能
您的 UWP 專案必須指定與 Xbox Live 通訊的網際網路功能。 您可以藉由設定這些屬性：

1. 按兩下**package.appxmanifest**以開啟 Visual Studio 中的檔案**資訊清單設計工具**。
2. 按一下 **能力**索引標籤，然後檢查**網際網路 （用戶端）**。

![在 VS 中加入新參考](../images/getting_started/get-started-vs-add-capability.png)

### <a name="6-associate-your-visual-studio-project-with-your-xbox-live-enabled-title"></a>6.您的 Xbox Live 啟用標題相關聯的 Visual Studio 專案

若要對談的 Xbox Live 服務，您必須將服務組態檔新增至您的專案。 這都可以透過輕鬆完成：

1. 在您的啟始專案，以滑鼠右鍵按一下並選取**新增** > **新項目**。
2. 選取 **文字檔**輸入並將它命名**xboxservices.config**。
3. 以滑鼠右鍵按一下檔案，選取**屬性**，並確定：
    1. **建置動作**設定為**內容**，及  
    2. **複製到輸出目錄**設定為**永遠複製**。
5.  編輯組態檔，使用下列範本中，取代**TitleId**並**PrimaryServiceConfigId**適用於您的標題的值。 您可以在合作夥伴中心，從根 Xbox Live 頁面取得正確的值。 **PrimaryServiceConfigId**會出現在合作夥伴中心**SCID**。

```json
    {
       "TitleId" : "your title ID (JSON number in decimal)",
       "PrimaryServiceConfigId" : "your primary service config ID"
    }
```

例如：

```json
    {
        "TitleId" : 1563044810,
        "PrimaryServiceConfigId" : "12200100-88da-4d8b-af88-e38f5d2a2bca"
    }
```

> [!TIP]
> Xboxservices.config 內的所有值都都區分大小寫的。 請參閱[服務組態](../xbox-live-service-configuration.md)如需有關取得 TitleID 和 PrimaryServiceConfigId。

### <a name="7-optional-add-multiplayer-capabilities"></a>7.（選擇性）新增多人遊戲的功能

如果您計劃新增多人遊戲支援新增至您的標題，並想要實作的能力，以邀請其他使用者的多人遊戲，玩家，則您需要將其他欄位新增至 AppXManifest 檔案。 請參閱[設定 AppXManifest 的多人](../multiplayer/service-configuration/configure-your-appxmanifest-for-multiplayer.md)如需詳細資訊。

## <a name="learn-more"></a>深入了解

[Xbox Live SDK 範例](https://github.com/Microsoft/xbox-live-samples)很適合用來查看如何使用 Xbox Live 的 Api，並展示中的開發人員提供的 ApiID@Xbox程式。 若要使用的範例，您必須變更您的沙箱 XDKS.1。
