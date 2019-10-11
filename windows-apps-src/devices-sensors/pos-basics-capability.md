---
title: PointOfService device 功能
description: 要使用 Windows.Devices.PointOfService 命名空間需要具備 PointOfService 功能。
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: f120e093ab65224ca4c32b64640100b6abd1a36a
ms.sourcegitcommit: 0301f794f994e604ffc131de7e40ffcede3530c9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/08/2019
ms.locfileid: "72036258"
---
# <a name="pointofservice-device-capability"></a>PointOfService device 功能
您透過在您的應用程式套件資訊清單中宣告功能，來要求存取 PointOfService API] 您可以使用 Microsoft Visual Studio 中的資訊清單設計工具宣告大部分的功能，或是手動新增功能。  

> [!Important]
> 如果您未在應用程式資訊清單中宣告**PointOfService**功能，則當您嘗試在 PointOfService 命名空間中使用 API 時，將會收到**system.unauthorizedaccessexception**錯誤。 

## <a name="declare-capability-using-manifest-designer"></a>使用資訊清單設計工具宣告功能

1. 在 **\[方案總管\]** 中，展開您的 UWP 應用程式的專案節點。
2. 按兩下 [Package.appxmanifest] 檔案。  
*如果資訊清單檔案已在 XML 程式碼視圖中開啟，Visual Studio 會提示您關閉該檔案。*
3. 按一下 **\[功能\]** 索引標籤。
4. 按一下功能清單中核取方塊旁的 **\[Point of Service\]** ，以啟用服務點裝置功能


## <a name="declare-capability-manually"></a>手動宣告功能

1. 在 **\[方案總管\]** 中，展開您的 UWP 應用程式的專案節點。
2. 以滑鼠右鍵按一下 **\[Package.appxmanifest\]** 檔案，然後選擇 **\[檢視程式碼\]** 。
3. 將 PointOfService DeviceCapability 項目新增到您的應用程式資訊清單的功能區段。  

```xml
  <Capabilities>
    <DeviceCapability Name="pointOfService" />
  </Capabilities>
   ```
