---
title: IWindowNative 介面
description: WinUI COM 介面，可提供 XAML 與原生視窗之間的互通性。
ms.topic: reference
ms.date: 03/09/2021
keywords: winui，Windows UI 程式庫
ms.openlocfilehash: e641ab73a87e993fddb3f6aa8b90a0149744b99f
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730855"
---
# <a name="iwindownative-interface-microsoftuixamlwindowh"></a>IWindowNative 介面 (的介面) 

啟用 XAML 與原生視窗之間的交互操作。

此介面是由 [視窗](/windows/winui/api/microsoft.ui.xaml.window)所執行，桌面應用程式可使用此介面取得視窗的基礎 HWND。

## <a name="inheritance"></a>繼承

IWindowNative 介面繼承自 IUnknown 介面。 IWindowNative 也有下列類型的成員：

- [屬性](#properties)

## <a name="properties"></a>屬性

IWindowNative 介面具有這些屬性。

| 屬性 | 描述 |
| --- | --- |
| [IWindowNative：： WindowHandle](iwindownative-windowhandle.md) | 取得要求的視窗 HWND。 |

## <a name="applies-to"></a>適用於

| 產品 | 版本 |
| --- | --- |
| WinUI | 3.0.0-專案-留尼旺島-0.5、3.0.0-專案-留尼旺島-預覽-0。5 |

## <a name="see-also"></a>另請參閱

[Windows UI 程式庫 (WinUI)](../index.md)