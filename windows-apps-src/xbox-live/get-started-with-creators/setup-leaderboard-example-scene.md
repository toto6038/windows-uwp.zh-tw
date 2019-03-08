---
title: 在 Unity 中使用排行榜場景
description: 顯示正確設定 Unity 排行榜場景的步驟
ms.date: 04/24/2018
ms.topic: get-started-article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 unity、 排行榜
ms.openlocfilehash: d931a0fdcbdec5dd9deb53876a19e1b475afcb93
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589743"
---
# <a name="the-leaderboard-example-scene-in-unity"></a>在 Unity 在排行榜範例場景

[Unity 外掛程式](https://github.com/Microsoft/xbox-live-unity-plugin)裝載數目來展示支援 Unity 開發人員可使用的 Xbox Live 服務的範例場景。 一個這類場景是排行榜範例場景。 在以下螢幕擷取畫面中，您會看到，排行榜範例場景只會顯示登入面板 Xbox Live 使用者和面板包含排行榜。 如果您達到 play 此時沒有加入此場景，您會發現 [登入] 面板會填入假的使用者資料，但排行榜載入任何資訊。 若要取得此範例場景，載入實際的排行榜，您必須先新增一些項目。

![排行榜場景螢幕擷取畫面](../images/unity/leaderboard-scene-1804.JPG)

## <a name="prerequisites"></a>必要條件

在 Xbox Live 的排行榜為基礎的 Xbox Live 的統計資料服務中的統計資料。 您可以填入資料的排行榜之前您必須有一些與您的測試帳戶相關聯的統計資料。 如果您未以您的標題已新增統計資料可以了解如何執行這項操作，請閱讀[新增至您的 Unity 專案的播放程式統計資料和排行榜](add-stats-and-leaderboards-in-unity.md)。 該文章的統計資料區段中執行的動作之後返回此處顯示範例排行榜場景中的狀態。

## <a name="the-leaderboard-inspector"></a>排行榜偵測器

排行榜 prefab 有一些可以在排行榜的指令碼元件，例如 UI 的 [偵測器] 區段中變更的設定*佈景主題*，播放程式相關聯的排行榜 xbox 控制器設定，以及其他排行榜的設定。 您可以看到排行榜設定分成下列偵測器中的幾個不同區段。

![排行榜偵測器螢幕擷取畫面](../images/unity/leaderboard_script_inspector.JPG)

### <a name="theme-and-display-settings"></a>佈景主題] 和 [顯示設定

本節提供一個設定稱為*佈景主題*。 這是簡單的下拉式清單可讓您用於您的排行榜 prefab 暗色或亮色佈景主題。 這會變更背景、 字型和色彩 prefab 映像。 在 unity 播放器播放場景時，經常會看到效果。

![淺色佈景主題](../images/unity/leaderboard_light_theme.JPG) ![深色佈景主題](../images/unity/leaderboard_dark_theme.JPG)

### <a name="stat-configuration"></a>統計資料組態

此區段可讓您判斷排行榜填入資料列時，將會擷取哪些種類的資料。

- **播放程式數目**-這會指定哪一個播放程式是與排行榜相關聯。
- **Stat** -用來填入排行榜資料統計資料。 這是必要的排行榜 prefab，載入資料。
- **排行榜類型**-此下拉式功能表將篩選套用至排行榜資料載入。 如果*Global*選取排行榜將會是未篩選，而且會顯示每個播放程式，使用值為選取狀態。如果*朋友*選取排行榜會篩選為只顯示玩家在排行榜也是在您的好友名單。 如果*我的最愛*選取排行榜會篩選為只顯示玩家在排行榜也會在您的 [我的最愛] 清單中。
- **項目計數**-1 到 100 之間的指定一次，將會傳回多少資料列的排行榜範圍滑桿。 數字組會決定每頁顯示的排行榜資料列數目。

### <a name="controller-configuration"></a>控制器組態

排行榜 prefab 可讓開發人員輕鬆地設定 Xbox 控制器使用。 Prefab 的控制站組態區段可讓您啟用，並選擇控制排行榜 prefab 的按鈕。

- **啟用控制器輸入**-簡單的選項按鈕切換。 若選取此選項則您可以使用 Xbox 控制器 prefab 進行互動。 支援所需的控制器。
- **搖桿數目**-指定哪一個控制站可以與這個排行榜 prefab 互動。
- **下一個頁面控制器按鈕**-下拉式清單功能表的按鈕載入下一頁的排行榜資料列的控制項。
- **上一頁控制器按鈕**-下拉式清單功能表的按鈕載入排行榜資料列的前一頁的控制項。
- **檢視控制器 下一步 按鈕**-在切換檢視類型之間**所有**並**最近我**。
- **上一個檢視控制器 按鈕**-在切換檢視類型之間**所有**並**最近我**。
- **垂直捲軸輸入軸**-指定哪些控制器軸是與捲動相關聯的字串。
- **捲動速度乘數**-決定控制器捲軸的速度。

> [!NOTE]
> 變更用於變更的頁面或檢視排行榜的按鈕也會變更用於每個函式的排行榜按鈕相關聯的圖片。 您不需要改變的 UI 參考一節，以手動方式來比對 按鈕的圖片。

### <a name="ui-references"></a>UI 參考

此區段會控制映像和排行榜 prefab 的一般結構。 此區段不需要成功使用排行榜 prefab 的變更。 不過，您可能需要調整此區段可供您自己自訂的 prefab 的外觀。

## <a name="populating-the-unity-player-leaderboard-with-fake-data"></a>填入 Unity 播放器排行榜 （使用假的資料）

您必須將排行榜 prefab 的統計資料以填入資料的 Unity 播放器排行榜。 檢視排行榜 prefab 偵測器中的會顯示，可能需要的物件型別的`Stat Base`做為公用參數，在其指令碼。 您可以將任一`State Base`輸入 prefabs `IntegerStat`， `DoubleStat`，或`StringStat`之 prefabs 資料夾中的 Xbox Live Unity 外掛程式，並將它放在此位置在排行榜中以 prefab。

![將拖放 Stat Prefab](../images/unity/stat-to-leaderbaord-drag.gif)

現在在 Unity 播放場景，您會發現排行榜填入假資料，例如下面。

![假的資料填入排行榜螢幕擷取畫面](../images/unity/leaderboard-fake-data-1804.JPG)

## <a name="populating-a-visual-studio-built-project-with-real-data"></a>填入 Visual Studio 建置專案以真實資料

若要填入的排行榜使用實際的資料，您必須建置您的遊戲在本機執行您的電腦上的標題。 您將需要本機組建，因為 Unity 編輯器中並沒有存取 Xbox Live。 除了建置在本機執行專案時，您必須在您排行榜 」 狀態的已初始化，且有標題的值來設定統計資料。 若要關聯至您的排行榜 」 狀態，您必須修改的識別碼和排行榜 prefab 的統計資料物件的顯示名稱。 識別碼必須符合的設定中 」 狀態[合作夥伴中心](https://partner.microsoft.com/dashboard)。 您已經這樣做之後，建置您的專案中所述[建置的 Xbox Live 設定 Unity 文件中的區段](configure-xbox-live-in-unity.md#build-and-test-the-project)。 執行為 x64 這個專案建置目標為本機電腦應該可讓您使用實際的玩家代號登入，並填入以真實資料排行榜。