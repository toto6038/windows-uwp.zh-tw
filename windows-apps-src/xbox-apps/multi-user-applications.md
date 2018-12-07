---
title: 多使用者應用程式的簡介
description: Xbox 多使用者模型的簡單高階簡介。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 2dde6ed3-7f53-48a6-aebe-2605230decb8
ms.localizationpriority: medium
ms.openlocfilehash: b56140f9a71c8233d2832c2b0da6ed927b5a19ac
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2018
ms.locfileid: "8794293"
---
# <a name="introduction-to-multi-user-applications"></a>多使用者應用程式的簡介

本主題是 Xbox 多使用者模型的簡單高階簡介。

Xbox One 使用者模型已針對支援多個使用者在單一裝置上合作進行遊戲的遊戲主機需求進行調整。 它可讓多個使用者 (每個使用者都有自己的控制器) 在單一互動式工作階段中同時登入並且使用主機。 這與其他 Windows 裝置不同。 例如：
* **Windows 桌上型電腦**允許多個使用者使用相同的裝置，但是每個使用者有自己的互動工作階段，而且每個工作階段與裝置上的其他工作階段完全獨立。
* **Windows 手機**只允許單一使用者使用裝置。 該單一使用者是在 OOBE (全新體驗) 期間決定，而且登入之後便無法登出。 實際上，如果其他使用者想要使用此裝置，則必須重設裝置。 
* **Xbox One** 允許多個使用者在單一互動式工作階段中同時登入並使用此裝置。

Xbox One 使用者模型中的每個使用者都是由本機使用者帳戶支援。 此本機使用者帳戶會與 Xbox Live 帳戶 (以及 Microsoft 帳戶) 相關聯。 這表示 Xbox 使用者帳戶與 Xbox Live 帳戶及 Microsoft 帳戶有嚴格的一對一對應。

## <a name="single-user-applications"></a>單一使用者應用程式
根據預設，通用 Windows 平台 (UWP) App 會在啟動應用程式之使用者的內容中執行。 這些「單一使用者應用程式」**(SUA) 只會注意該單一使用者，並會以與其他 Windows 裝置上的使用者模型相容的模式執行。 Xbox 使用者模型會管理哪個使用者與 App 相關聯，並保證特定使用者會在特定 App 啟動時登入。 在這個模型中，UWP App 和遊戲作者不需要特別執行任何動作以在 Xbox 上執行。 

## <a name="multi-user-applications"></a>多使用者應用程式
UWP 遊戲可以選擇加入 Xbox One 多使用者模型。 這些「多使用者應用程式」**(MUA) 會在系統帳戶 (稱為預設帳戶) 的內容中執行，而且可以充分利用 Xbox One 使用者模型的彈性和功能。 針對這些遊戲，Xbox 使用者模型不會管理哪個使用者與遊戲相關聯，遊戲甚至不需要使用者登入以執行。 這表示這些遊戲必須撰寫成明確知道並管理其使用者需求︰是否需要登入的使用者、是否實作目前使用者的概念、是否允許來自多個使用者的同時輸入等等。
   
若要選擇加入多使用者模型：   
1. 在 Visual Studio 中，開啟您的專案。   
2. 選取 package.appxmanifest.xml 檔案。   
3. 以滑鼠右鍵按一下，並選取 **\[檢視程式碼\]**。   
4. 在 `<Properties></Properties>` 區段中加入以下這一行：

```
<uap:SupportedUsers>multiple</uap:SupportedUsers>
```

### <a name="identifying-users-and-inputs"></a>識別使用者和輸入
開發人員可以使用 KeyRoutedEventArgs.DeviceId (由 KeyUp 和 KeyDown 路由事件使用)，來區分從不同輸入所產生的事件。
使用 Windows.System.UserDeviceAssociation.FindUserFromDeviceId 方法，將能協助識別與特定輸入相關聯的使用者。

請參閱 [KeyRoutedEventArgs.DeviceId](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.keyroutedeventargs.deviceid) 主題以取得詳細資訊。


## <a name="guidance-on-which-model-to-choose"></a>要選擇哪一個模型的指導方針
所有 UWP App 和大部分的單一使用者遊戲都可以撰寫成 SUA。 我們建議只考慮將多人合作遊戲加入 Xbox One 多使用者模型。

## <a name="see-also"></a>另請參閱
- [Xbox One 上的 UWP](index.md)
