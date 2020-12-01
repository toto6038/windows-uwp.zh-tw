---
description: '敏捷式物件是可以從任何執行緒中存取的一個。 如果您需要以安全的方式跨單元封送處理非 agile 物件，c #/WinRT 會提供 agile 參考的支援。'
title: '使用 c #/WinRT 的 Agile 物件'
ms.date: 11/17/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a963f1a9918e6732028ad74903b096a38a14ec8f
ms.sourcegitcommit: 3b10880007fe9e29ea2b9305fe62ced239d974be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2020
ms.locfileid: "96355227"
---
# <a name="agile-objects-in-cwinrt"></a>C #/WinRT 中的 Agile 物件

大部分的 Windows 執行階段類別都是 *敏捷* 式，這表示可以從不同單元的任何執行緒存取它們。 您所撰寫的 c #/WinRT 類型預設為 agile，而且無法選擇不使用這些類型的行為。

不過，投射的 c #/WinRT 型別 (，其中包括 Windows SDK 所提供的 Windows 執行階段類型，而 WinUI 程式庫) 不一定是 agile。 例如，許多代表 UI 物件的類型都不是 agile。 當您使用非 agile 型別時，您必須考慮它們的執行緒模型和封送處理行為。 如果您需要以安全的方式跨單元封送處理非 agile 物件，c #/WinRT 會提供 agile 參考的支援。

> [!NOTE]
> Windows 執行階段是以 COM 為基礎。 在 COM 條款中，會向 `ThreadingModel` = *Both* 註冊敏捷式類別。 如需有關 COM 執行緒模式和 Apartment 的詳細資訊，請參閱[了解與使用 COM 執行緒模式](/previous-versions/ms809971(v=msdn.10))。

## <a name="check-for-agile-support"></a>檢查 agile 支援

若要檢查 Windows 執行階段的物件是否為 agile，請使用下列程式碼來判斷此物件是否支援 [IAgileObject](/windows/desktop/api/objidl/nn-objidl-iagileobject) 介面。

```csharp
var queryAgileObject = testObject.As<IAgileObject>();

if (queryAgileObject != null) {
    // testObject is agile.
}
```

## <a name="create-an-agile-reference"></a>建立 agile 參考

若要建立非 agile 物件的 agile 參考，您可以使用 `AsAgile` 擴充方法。 `AsAgile` 是泛型擴充方法，可套用至任何投射的 c #/WinRT 型別。 如果類型不是投射的型別，則會擲回例外狀況。 以下是使用 [PopupMenu](/uwp/api/Windows.UI.Popups.PopupMenu) 物件的範例，這是 Windows SDK 的非 agile 型別。

```csharp
var nonAgileObj = new Windows.UI.Popups.PopupMenu();
AgileReference<Windows.UI.Popups.PopupMenu> agileReference = nonAgileObj.AsAgile();
```

您現在可以傳遞 `agileReference` 至不同單元中的執行緒，並在該處使用它。

```csharp
await Task.Run(() => {
        Windows.UI.Popups.PopupMenu nonAgileObjAgain = agileReference.Get()
    });
```
