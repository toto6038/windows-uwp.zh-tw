---
title: 要求地圖驗證金鑰
description: 您的通用 Windows app 必須經過驗證，才能使用 MapControl 和 Windows.Services.Maps 命名空間中的地圖服務。
ms.assetid: 13B400D7-E13F-4F07-ACC3-9C34087F0F73
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10 , UWP, 地圖驗證金鑰, 地圖控制項
ms.localizationpriority: medium
ms.openlocfilehash: 195c643fe10daf45ae709962a3b675162874c77b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171832"
---
# <a name="request-a-maps-authentication-key"></a>要求地圖驗證金鑰

> [!WARNING]
> 舊版 Windows 10 可能無法使用線上地圖服務。 在下列版本上，隨 mapcontrol 可能不會再顯示 Windows 中的對應和 Api。 Maps 命名空間可能不會傳回結果：
> - Windows 10，1607版和更舊版本：地圖服務將于2020年10月起從全球無法使用
> - Windows 10，1703版和更舊版本：在[中國銷售的某些裝置](/windows-hardware/customize/desktop/unattend/microsoft-windows-mapcontrol-desktop-chinavariantwin10)上無法使用地圖服務

您的 [通用 windows 應用程式](../get-started/universal-application-platform-guide.md) 必須經過驗證，才能使用 [**隨 mapcontrol**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 和 [**Maps**](/uwp/api/Windows.Services.Maps) 命名空間中的對應服務。 若要驗證您的應用程式，則必須指定地圖驗證金鑰。 本主題說明如何從 [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)要求地圖驗證金鑰，然後將它新增到您的 app。

**提示**：若要深入了解如何在 app 中使用地圖，請從 GitHub 的 [Windows-universal-samples 存放庫](https://github.com/Microsoft/Windows-universal-samples)下載下列範例：

-   [通用 Windows 平台 (UWP) 地圖範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)

## <a name="get-a-key"></a>取得金鑰


使用 [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)，即可為您的通用 Windows app 建立和管理地圖驗證金鑰。

建立新的金鑰

1.  在您的瀏覽器中，流覽至 Bing Maps 開發人員中心 ([https://www.bingmapsportal.com](https://www.bingmapsportal.com/)) 。

2.  如果系統要求您登入，請輸入您的 Microsoft 帳戶，然後按一下 **\[登入\]**。

3.  選擇要與您的「Bing 地圖服務」帳戶建立關聯的帳戶。 若要使用您的 Microsoft 帳戶，請按一下 **\[是\]**。 否則，請按一下 **\[使用其他帳戶登入\]**。

4.  如果您還沒有「Bing 地圖服務」帳戶，請建立一個新的「Bing 地圖服務」帳戶。 輸入 **\[帳戶名稱\]**、**\[連絡人名稱\]**、**\[公司名稱\]**、**\[電子郵件地址\]** 及 **\[電話號碼\]**。 接受使用規定之後，按一下 **\[建立\]**。

5.  在 **[我的帳戶]** 功能表下方，按一下 **[我的金鑰]**。

6.  如果您之前已建立金鑰，請按一下連結，以建立新的金鑰。 否則，請前往建立金鑰表單。

7.  完成 **\[建立金鑰\]** 表單，然後按一下 **\[建立\]**。

    -   **應用程式名稱：** 您應用程式的名稱。
    -   **應用程式 URL (選擇性)：** 您應用程式的 URL。
    -   **金鑰類型：** 請選取 **\[基本\]** 或 **\[企業\]**。
    -   **應用程式類型：** 選取 **Windows 應用程式** 以在您的通用 windows 應用程式中使用。

    以下是表單的外觀範例。

    ![[建立金鑰] 表單的範例。](images/createkeydialog.png)

8.  按一下 **\[建立\]** 之後，新金鑰就會顯示在 **\[建立金鑰\]** 表單下方。 請將它複製到安全的地方，或立即將它新增到您的 app，如下一個步驟所述。

## <a name="add-the-key-to-your-app"></a>將金鑰新增到 app


需要有地圖驗證金鑰，才能在通用 Windows app 中使用 [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 和地圖服務 ([**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps))。 視需要將它新增到地圖控制項和地圖服務物件。

### <a name="to-add-the-key-to-a-map-control"></a>將金鑰新增到地圖控制項

若要驗證 [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)，請將 [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken) 屬性設定為驗證金鑰值。 您可以根據喜好設定，在程式碼或 XAML 標記中設定這個屬性。 如需有關使用 **MapControl** 的詳細資訊，請參閱[顯示地圖的 2D、3D 和 Streetside 檢視](display-maps.md)。

-   這個範例會在程式碼中將 **MapServiceToken** 設定為驗證金鑰的值。

    ```cs
    MapControl1.MapServiceToken = "abcdef-abcdefghijklmno";
    ```

-   這個範例會在 XAML 標記中將 **MapServiceToken** 設定為驗證金鑰的值。

    ```xml
    <Maps:MapControl x:Name="MapControl1" MapServiceToken="abcdef-abcdefghijklmno"/>
    ```

### <a name="to-add-the-key-to-map-services"></a>將金鑰新增到地圖服務

若要使用 [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps) 命名空間中的服務，請將 [**ServiceToken**](/uwp/api/windows.services.maps.mapservice.servicetoken) 屬性設定為驗證金鑰值。 如需有關使用地圖服務的詳細資訊，請參閱[顯示路線和路線指引](routes-and-directions.md)和[執行地理編碼與反向地理編碼](geocoding.md)。

-   這個範例會在程式碼中將 **ServiceToken** 設定為驗證金鑰的值。

    ```cs
    MapService.ServiceToken = "abcdef-abcdefghijklmno";
    ```

## <a name="related-topics"></a>相關主題

* [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)
* [UWP 地圖範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [地圖的設計指導方針](./display-maps.md)
* [Build 2015 影片：跨手機、平板電腦和電腦運用 Windows app 中的地圖與位置功能](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 車流量應用程式範例](https://github.com/Microsoft/Windows-appsample-trafficapp)