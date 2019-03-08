---
title: 設定您 AppXManifest 多人遊戲
description: 了解如何設定您的 UWP AppXManifest 以便 Xbox Live 多人遊戲的邀請。
ms.assetid: 72f179e7-4705-4161-9b8a-4d6a1a05b8f7
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 通訊協定啟用、 多人遊戲
ms.localizationpriority: medium
ms.openlocfilehash: 13b04a86fdc4e4f661dd1c181dda7d9c9e4c1c8a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646023"
---
# <a name="configure-your-appxmanifest-for-multiplayer"></a>設定您 AppXManifest 多人遊戲

您需要進行一些更新.appxmanifest 檔案，Visual Studio 專案中，如果下列條件成立：
- 您正在開發的 UWP
- 您想要實作以邀請其他使用者加入您的標題的播放程式的能力

如果您沒有進行此步驟中，您的標題會取得收件者的播放程式會接受邀請，以播放時啟用的通訊協定。

## <a name="open-your-packageappxmanifest"></a>開啟您 Package.appxmanifest

Package.appxmanifest 檔案通常位於與您的 Visual Studio 專案的方案檔相同的目錄。  或者，您可以在 [方案總管] 中找到它。

![](../../images/multiplayer/multiplayer_open_appxmanifest.png)

## <a name="add-new-entry"></a>加入新項目

您必須將下列內容加入```<Extensions>```項目底下```<Applications>```Package.appxmanifest 檔案中

```
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-xbl-multiplayer" />
  </uap:Extension>
</Extensions>
```

例如：

![](../../images/multiplayer/multiplayer_appxmanifest_changes.png)

儲存並重建您的標題。  若要了解如何使用多人遊戲管理員來實作邀請您標題的播放程式的功能，請參閱[與朋友玩多人遊戲](../multiplayer-manager/play-multiplayer-with-friends.md)
