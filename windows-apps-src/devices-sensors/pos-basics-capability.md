---
author: TerryWarwick
title: PointOfService device 功能
description: 要使用 Windows.Devices.PointOfService 命名空間需要具備 PointOfService 功能。
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, point of service, pos, 服務點
ms.localizationpriority: medium
ms.openlocfilehash: dbed55af1a9fa3df14f0a7e3e7c6d1f599b40fd3
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5821467"
---
# <a name="pointofservice-device-capability"></a>PointOfService device 功能
您透過在您的應用程式套件資訊清單中宣告功能，來要求存取 PointOfService API] 您可以使用 Microsoft Visual Studio 中的資訊清單設計工具宣告大部分的功能，或是手動新增功能。  

> [!Important]
> 如果您未在您的應用程式資訊清單中宣告 **pointOfService** 功能，當您嘗試在 Winodws.Devices.PointOfService 命名空間中使用 API 時，您會收到錯誤 **System.UnauthorizedAccessException**。 

## <a name="declare-capability-using-manifest-designer"></a>使用資訊清單設計工具宣告功能

1. 在 **\[方案總管\]** 中，展開您的 UWP 應用程式的專案節點。
2. 按兩下 **\[Package.appxmanifest\]** 檔案。  
*如果資訊清單檔案已在 XML 程式碼檢視中開啟，Visual Studio 會提示您關閉檔案。*
3. 按一下 **\[功能\]** 索引標籤。
4. 按一下功能清單中核取方塊旁的 **\[Point of Service\]**，以啟用服務點裝置功能


## <a name="declare-capability-manually"></a>手動宣告功能

1. 在 **\[方案總管\]** 中，展開您的 UWP 應用程式的專案節點。
2. 以滑鼠右鍵按一下 **\[Package.appxmanifest\]** 檔案，然後選擇 **\[檢視程式碼\]**。
3. 將 PointOfService DeviceCapability 項目新增到您的應用程式資訊清單的功能區段。  

```xml
  <Capabilities>
    <DeviceCapability Name="pointOfService" />
  </Capabilities>
   ```
