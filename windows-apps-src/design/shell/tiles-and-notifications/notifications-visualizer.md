---
author: andrewleader
Description: Notifications Visualizer is a new Universal Windows Platform (UWP) app in the Store that helps developers design adaptive live tiles for Windows 10.
title: 通知視覺化工具
ms.assetid: FCBB7BB1-2C79-484B-8FFC-26FE1934EC1C
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: af8b2489346e1ef81c5cae304802814b79b8b950
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2018
ms.locfileid: "3988237"
---
# <a name="notifications-visualizer"></a>通知視覺化工具

 


通知視覺化檢視是 [Microsoft Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) 中新的通用 Windows 平台 (UWP) app，可協助開發人員設計適用於 Windows 10 的彈性動態磚和互動式快顯通知。


## <a name="overview"></a>概觀

和 Visual Studio 的 XAML 編輯器/設計檢視一樣，當您編輯 XML 承載時，通知視覺化檢視應用程式會提供即時的磚和快顯通知視覺預覽。 應用程式也會檢查錯誤，這可確保您建立的是有效的磚或快顯通知承載。

應用程式中的這個螢幕擷取畫面顯示 XML 承載，以及磚大小在選取的裝置上如何顯示：

![含有程式碼與磚的通知視覺化檢視應用程式編輯器的螢幕擷取畫面](images/notif-visualizer-001.png)

 

使用通知視覺化檢視，您可以建立及測試彈性磚和快顯通知承載，不需要編輯和部署您自己的應用程式。 建立含有理想的視覺效果的承載後，您便可將它整合到您的應用程式。 若要深入了解，請參閱[傳送本機磚通知](sending-a-local-tile-notification.md)和[傳送本機快顯通知](send-local-toast.md)。

**注意**：Windows [開始] 功能表和快顯通知的通知視覺化檢視模擬不一定完全精確，而且它不支援部分進階承載屬性。 當您有想要的磚或快顯通知時，請釘選磚或彈出快顯通知來測試它，確認它顯示為您想要的樣子。

 

## <a name="features"></a>功能

通知視覺化檢視隨附數個範例承載，以展示彈性動態磚和互動式快顯通知可能的型態並協助您開始。 您可以實驗所有不同的文字選項、群組/子群組、背景影像，而且您可以看到磚如何適應不同的裝置和螢幕。 在您變更後，您可以將更新的承載儲存為檔案以供日後使用。

編輯器提供即時的錯誤和警告。 例如，如果您的承載大於 5 KB (平台限制)，當您的承載超過該限制時，通知視覺化檢視會警告您。 它會針對不正確的屬性名稱或值提供警告，以協助您偵錯視覺的問題。

您可以控制磚屬性，例如顯示名稱、色彩、標誌、ShowName 及徽章值。 這些選項可協助您立即了解您的磚內容和磚通知承載的互動方式，以及它們產生的結果。

應用程式的這個螢幕擷取畫面顯示磚編輯器：

![含有磚的通知視覺化工具編輯器的螢幕擷取畫面](images/notif-visualizer-004.png)

 

## <a name="related-topics"></a>相關主題

* [在市集中取得通知視覺化工具](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)
* [建立彈性磚](create-adaptive-tiles.md)
* [互動式快顯通知](adaptive-interactive-toasts.md)