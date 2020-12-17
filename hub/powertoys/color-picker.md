---
title: Windows 10 的 Powertoy 色彩選擇器公用程式
description: 適用于 Windows 10 的全系統色彩挑選公用程式，可讓您從任何目前正在執行的應用程式中挑選色彩，並自動將十六進位或 RGB 值複製到剪貼簿。
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3c76753d455d28dd44045edf9482b3de9ccd97b8
ms.sourcegitcommit: 46a7e9db64e17a645ee6e888f62a9b04632c56af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618567"
---
# <a name="color-picker-utility"></a>色彩選擇器公用程式

適用于 Windows 10 的全系統色彩挑選公用程式，可讓您從任何目前執行的應用程式中挑選色彩，並自動將其以可設定的格式複製到剪貼簿。

![ColorPicker](../images/pt-colorpicker-hex-editor.png)

## <a name="getting-started"></a>開始使用

### <a name="enable"></a>啟用

若要開始使用色彩選擇器，您必須先確定已在 [Powertoy 設定] (色彩選擇器] 區段中啟用它) 。

### <a name="activate"></a>啟動

啟用之後，您可以選擇下列三種行為的其中一種，在啟動色彩選擇器時，使用啟用快速鍵<kbd>Win</kbd> + <kbd>Shift</kbd> + <kbd>C</kbd> (請注意，您可以在 [設定] 對話方塊中變更此快速鍵) ：

:::image type="content" source="../images/pt-colorpicker-behaviors.png" alt-text="ColorPicker 行為。":::

- **色彩選擇器已啟用編輯器模式** -開啟色彩選擇器。選取色彩之後，會開啟編輯器，並將選取的色彩複製到剪貼簿 (在 [設定] 對話方塊中可設定的預設格式) 。
- **編輯器** -直接開啟編輯器，您可以從這裡選擇歷程記錄中的色彩、微調選取的色彩，或開啟色彩選擇器來捕捉新的色彩。
- **色彩選擇器只** 會開啟色彩選擇器，並將選取的色彩複製到剪貼簿。

### <a name="select-color"></a>選取色彩

色彩選擇器啟動之後，將滑鼠游標停留在您想要複製的色彩上方，然後以滑鼠左鍵按一下滑鼠按鍵以選取色彩。 如果您想要更詳細地查看資料指標周圍的區域，請向上移至 [放大]。

複製的色彩會儲存在剪貼簿中，其格式為 [預設 (HEX]) 中設定的格式。

![選取色彩](../images/pt-colorpicker.gif)

## <a name="editor-usage"></a>編輯器使用方式

編輯器可讓您查看所選色彩的歷程記錄 (最多20個) ，並以任何預先定義的字串格式複製其標記法。 您可以設定編輯器中可見的色彩格式，以及它們顯示的順序。 您可以在 [Powertoy 設定] 中找到此設定。

編輯器也可讓您微調任何挑選的色彩，或取得新的類似色彩。 編輯器會預覽目前選取的色彩2和2個較深色的不同陰影。

按一下任何這些替代色彩陰影會將選取專案新增至挑選色彩的歷程記錄， (顯示在 [色彩歷程記錄] 清單) 的上方。 中間的色彩代表您目前在色彩歷程記錄中選取的色彩。 藉由按一下它，就會顯示微調設定控制項，讓您變更目前色彩的色調或 RGB 值。 按下 [確定] 會將新設定的色彩新增至 [色彩歷程記錄]。

![ColorPicker 編輯器](../images/pt-colorpicker-editor.gif)

若要從色彩歷程記錄移除任何色彩，請以滑鼠右鍵按一下所需的色彩，然後選取 [ *移除*]。

## <a name="settings"></a>設定

色彩選擇器可讓您變更下列設定：

- 啟用快速鍵
- 啟用快捷方式的行為
- 複製的色彩格式 (HEX、RGB 等 ) 
- 編輯器中的色彩格式順序和外觀

![ColorPicker 設定螢幕擷取畫面](../images/pt-colorpicker-settings.png)

## <a name="limitations"></a>限制

- 色彩選擇器無法顯示在 [開始] 功能表或控制中心的上方 (您仍可挑選) 的色彩。
- 如果目前焦點的應用程式是以系統管理員許可權的方式啟動 (以系統管理員身分執行) ，色彩選擇器啟用快捷方式將無法運作，除非 Powertoy 也是以系統管理員許可權啟動。
