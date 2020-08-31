---
title: 設定條碼掃描器
description: 瞭解如何為預定的應用程式設定條碼掃描器。
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 96deeb82f0aa04929ac33001b1aee0603e6474c7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168502"
---
# <a name="configure-a-barcode-scanner"></a>設定條碼掃描器

條碼掃描器可以幾種不同模式設定。  請務必針對其用途妥善設定條碼掃描器。

許多條碼掃描器可在**鍵盤 Wedge** 模式中設定，其可使條碼掃描器顯示為 Windows 鍵盤。  這可讓您掃描條碼到不是條碼掃描器感知的應用程式，例如記事本。  當您在此模式中掃描條碼時，從條碼掃描器解碼的資料會插入到您的插入點，宛如您使用鍵盤輸入資料一般。  若要進一步從您的 UWP 應用程式控制條碼掃描器，您必須在非鍵盤 Wedge 模式中設定。

## <a name="usb-barcode-scanner"></a>USB 條碼掃描器
USB 連接的條碼掃描器必須在 **HID POS 掃描器**模式中設定，才能搭配 Windows 內含的條碼掃描器驅動程式使用。 此驅動程式是發佈至[USB-hid](https://www.usb.org/hid)的**隱藏型銷售點使用方式資料表**規格的實作為起點。  如需啟用 **HID POS 掃描器**模式的指示操作，請參閱條碼掃描器的文件，或連絡條碼掃描器的製造商。  一旦設定為 **HID POS 掃描器**，您的條碼掃描器會在 **POS 條碼掃描器**節點下的 \[裝置管理員\] 中顯示為 **POS HID 條碼掃描器**。

您的條碼掃描器製造商也可能會有廠商特定驅動程式，其使用 **HID POS 掃描器**以外的模式支援 UWP 條碼掃描器 API。  如果您已安裝與 UWP 條碼掃描器 Api 相容的製造商提供的驅動程式，您可能會在裝置管理員中看到 [ **POS 條碼掃描器** ] 底下列出廠商特定的裝置。

## <a name="bluetooth-barcode-scanner"></a>藍牙條碼掃描器
藍牙連接的掃描器必須在**序列埠通訊協定 - 簡單序列介面 (SPP SSI)** 模式中設定，以搭配 UWP 條碼掃描器 API 使用。  如需啟用 **SPP-SSI 模式**的指示操作，請參閱條碼掃描器的文件，或連絡條碼掃描器的製造商。

使用您的藍牙條碼掃描器之前，您必須使用 **設定 > 裝置 > 藍牙 & 其他裝置 > 新增藍牙或其他**裝置。

您可以使用 [Windows. 列舉](/uwp/api/windows.devices.enumeration) 命名空間來起始及控制配對進程。  如需詳細資訊，請參閱[配對裝置](./pair-devices.md)。

[!INCLUDE [feedback](./includes/pos-feedback.md)]