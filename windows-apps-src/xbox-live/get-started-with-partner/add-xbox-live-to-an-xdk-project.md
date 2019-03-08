---
title: 加入 Xbox Live 的 XDK 專案
description: 了解如何將加入新的或現有 Xbox Developer Kit (XDK) 專案的 Xbox Live。
ms.assetid: fc6f987c-1a87-4ff5-b063-891591aa6653
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 xdk
ms.localizationpriority: medium
ms.openlocfilehash: f17765b09dcb0b6f5c89d168f2d3d9611a60ffa6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649043"
---
# <a name="add-xbox-live-to-a-new-or-existing-xdk-project"></a>將 加入新的或現有的 XDK 專案的 Xbox Live

本主題概要說明如何新增至新的或現有的 XDK 專案的 Xbox Live。

程序如下：

- 設定您的 Xbox One 開發環境
- 取得您的識別碼
- 設定您的開發主機
- 將 TitleID 和 SCID 新增至您的二進位檔


## <a name="setup-up-your-xbox-one-development-environment"></a>設定您的 Xbox One 開發環境
首先，依照 XDK 文件中的 「 設定 Xbox 一個開發環境 」 一節安裝主控台

## <a name="get-your-ids"></a>取得您的識別碼

若要啟用 Xbox Live 服務，您必須取得數個識別碼，以設定您的開發套件和程式的標題。 這些可以透過相同的程序。

您會取得您的識別碼執行動作的程序[Xbox Live 服務組態](../xbox-live-service-configuration.md)

## <a name="configure-your-development-console"></a>設定您的開發主機

當您已經有您的識別碼時，請遵循[設定您的開發主控台](configure-your-development-console.md)指南來設定您的開發主控台。

## <a name="add-the-titleid-and-scid-to-your-binary"></a>將 TitleID 和 SCID 新增至您的二進位檔
沙箱平台層級上設定的每個開發套件時，TitleID SCID 會繫結至特定的二進位檔。 TitleID 和 SCID 加入您的二進位檔，來修改該二進位檔的 Package.appxmanifest 中新增節點<Extensions>節點，如下所示：

```
<Applications>
    ...
    <Application ...>
      ...
      <Extensions>
        <mx:Extension Category="xbox.live">
           <mx:XboxLive TitleId="<your titleID>" PrimaryServiceConfigId="<your SCID>" RequireXboxLive="<boolean indicating Live requirement>" />
        </mx:Extension>
      </Extensions>
   </Application>
</Applications>
```

如需有關 AppxManifest.xml 檔案的詳細資訊，請參閱 Visual Studio 中的專案範本適用於 Xbox 一個開發。

請參閱 < 應用程式資訊清單架構應用程式的說明資訊清單結構描述。

**RequireXboxLive 旗標**除非 Windows.Networking.Connectivity 連接層級傳回為 XboxLiveAccess 和標題清除使用 Xbox Live 的驗證，如果將不啟動旗標設定為 true，標題 RequireXboxLive。 這樣可確保所需的最新的內容更新標題。 如果執行標題時，會失去連線能力，標題會遭擱置。

只有 「 網際網路要求 」 的標題應該將 RequireXboxLive 標示為 true，並請注意，標記您的標題，以這種方式並不保證標題所需的服務都已啟動並執行。
