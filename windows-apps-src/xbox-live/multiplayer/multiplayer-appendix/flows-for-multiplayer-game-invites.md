---
title: 更新的流程多人連線遊戲的邀請
description: 提供 更新的流程，以實作 Xbox Live 邀請多人連線遊戲。
ms.assetid: 1569588e-3bbc-47d3-8b7d-cc9774071adc
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，多人遊戲 2015
ms.localizationpriority: medium
ms.openlocfilehash: 1092c84521271e996db0b89630d22f7e51727bfa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596883"
---
# <a name="updated-flows-for-multiplayer-game-invites"></a>更新的流程多人連線遊戲的邀請

Xbox One beta 版的意見反應，因為使用者體驗流程的多人連線遊戲邀請已變更自 Xbox 一個復原更新 24，已於 2013 年 11 月 6 日發行。 這是變更**使用者體驗 (UX) 只**並不會影響任何行為或功能，從遊戲標題的角度。 標題的開發人員將不需要變更任何程式碼。

## <a name="summary-of-changes"></a>變更摘要

-   初始的邀請快顯已從 「 加入我的合作對象 」 改成 「&lt;遊戲標題&gt;我們開始 「 現在有一個按鈕可讓使用者啟動遊戲，並直接跳到遊戲。

-   當使用者選擇新，依預設不進行合作對象應用程式 」&lt;遊戲標題&gt;我們開始 」 選項。 讓使用者可以直接跳到遊戲，也會進行此變更。

-   已新增新的快顯通知，表示寄件者端 」 新增\[*數字*\]遊戲的朋友 」。 這使得清除，邀請已送出遊戲的工作階段與使用者的合作對象相關聯時。

下列範例會說明詳細的使用者體驗的流程。 每個資料表會顯示兩位使用者，David 和 Laura 範例流程。 這些流程所示的每個資料行，並以平行方式發生。 <b style="background-color: #FFFF00">反白顯示文字</b>顯示從先前的 UX 流程所做的調整。

## <a name="inviting-users-from-within-a-game"></a>邀請從遊戲內的使用者

<table>
  <tr>
    <td style="border-bottom:solid 1px #fff">
David 是多人遊戲，他正在播放大廳中。<p><br>選擇 David<b>邀請朋友</b>。<p><br>David 選取 Laura。<p><br>快顯出現表示已傳送邀請。 |Laura 扮演不同的遊戲。
    </td>
    <td style="border-bottom:solid 1px #fff">
Laura 扮演不同的遊戲。
    </td>
  </tr>
  <tr>
    <td></td>
    <td>
快顯彈出，指出從 David，邀請並<b style="background-color: #FFFF00">會顯示遊戲的名稱和圖示</b>。 （合作對象應用程式不會不自動-嵌入式管理單元。） <p><br>
在通知中心<b style="background-color: #FFFF00">Laura 可以選擇啟動，然後在接受邀請</b>，<b>接受邀請</b>，或<b style="background-color: #FFFF00">拒絕邀請</b>。
    </td>
  </tr>
  <tr>
    <td colspan="2" style="text-align:center"><b style="background-color: #FFFF00">案例 1:Laura 選取啟動，並接受邀請</b>（這是新的選項）</td>
  </tr>
  <tr>
    <td>
Laura 已加入 David 的合作對象，指出彈出快顯通知。             
    <p><br>
David 是從多人遊戲大廳開始遊戲。                              
    <p><br>
    <b style="background-color: #FFFF00">表示遊戲邀請已傳送至 Laura 彈出快顯通知。</b>
    </td>
    <td>
遊戲啟動，並不會不會貼齊的合作對象應用程式。
    </td>
  </tr>
  <tr>
    <td colspan="2" style="text-align:center"><b>案例 2:Laura 選取接受邀請</b></td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"></td>
    <td style="border-bottom:solid 1px #fff">Laura 聯結合作對象。</td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"><b style="background-color: #FFFF00">表示遊戲邀請已傳送至 Laura 彈出快顯通知。</b></td>
    <td style="border-bottom:solid 1px #fff"></td>
  </tr>
  <tr>
    <td></td>
    <td>快顯通知快顯指出遊戲已找到的合作對象。
    <p><br>
在通知中心，Laura 將可以選擇： <ul>
    <li>   <b>接受邀請的遊戲：</b>遊戲的推出。
    <li>   <b>拒絕遊戲邀請：</b>沒有遊戲的推出。 Laura 仍在合作對象，而且會收到後續遊戲的邀請。         
    <li>   <b style="background-color: #FFFF00">將合作對象：沒有遊戲的推出。 Laura 會從合作對象移除。</b>
    </ul>
    </td>
  </tr>
</table>

## <a name="in-game-invite-flow-in-a-party-and-switching-titles"></a>在遊戲邀請流程：在 合作對象，以及切換項目

<table>
  <tr>
    <td>
一起玩遊戲。
    <p><br>
David 會切換成不同的遊戲，並移至多人遊戲的功能表。
     <p><br>
Xbox 系統 UI 會要求 David 他想要切換至新的遊戲，他可以回答的他的合作對象<b>[是]</b>或是<b>No</b>。
    </td>
    <td style="text-align:top">
一起玩遊戲。
    </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align:center">
      <b>案例 1:[是]</b>
    </td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff">
合作對象談到新的標題。
    <p><br>
在多人遊戲大廳中，David 啟動遊戲。
    <p><br>
    <b style="background-color: #FFFF00">表示該遊戲的邀請已傳送至 Laura 彈出快顯通知。
    </td>
    <td style="border-bottom:solid 1px #fff">
    </td>
  </tr>
  <tr>
    <td></td>
    <td>快顯通知快顯指出遊戲已找到的合作對象。
    <p><br>
Laura 可以從 [通知中心] 中，選擇： <ul>
     <li><b>接受邀請遊戲</b>:新遊戲推出 <li><b>拒絕遊戲邀請：</b>沒有遊戲會啟動，但是 Laura 仍在合作對象，而且將會收到後續遊戲的邀請。
     <li><b style="background-color: #FFFF00"><b>將合作對象：</b>不會有遊戲推出和 Laura 會移除從合作對象。</b>
     </ul>
     </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align:center">
      <b>案例 2:否</b>
    </td>
  </tr>
  <tr>
    <td>
合作對象不會以新的標題。
David 在沒有他的合作對象成員就會播放多人遊戲模式。
David 仍會在合作對象。
    </td>
    <td>
    </td>
  </tr>
</table>

## <a name="invite-flow-from-home"></a>邀請從家裡的流程

<table>
  <tr>
    <td>
瀏覽 David<b>首頁</b>，在他<b>朋友</b>他 清單中，看到 Laura 已上線。
    <p><br>
David 選擇邀請 Laura 到他的合作對象。 快顯通知指出邀請會傳送快顯。 合作對象應用程式是 David 的貼齊。
    </td>
    <td>
Laura 玩遊戲。
    </td>
  </tr>
  <tr>
    <td></td>
    <td>
David 有 Laura 受邀到他的合作對象，指出彈出快顯通知。
    <p><br>
在通知中心，Laura 有選項可<b>接受邀請</b>。
    <p><br>
Laura 接受之後，將貼齊的合作對象應用程式。                                                                         </td>
  </tr>
  <tr>
    <td>
Laura 已加入合作對象，指出彈出快顯通知。
    <p><br>
David 和 Laura 將討論他們想要玩哪些的遊戲。 David 啟動遊戲，並輸入多人遊戲的遊戲模式。
    <p><br>
遊戲可能會提供選項來邀請朋友或自動提取的合作對象的成員。
    <p><br>
    <b style="background-color: #FFFF00">快顯通知指出遊戲邀請已傳送快顯。</b>
    </td>
    <td>
快顯通知快顯指出，遊戲找不到合作對象。
    <p><br>
在通知中心，Laura 可以： <ul>
    <li>   <b>接受邀請的遊戲：</b>遊戲推出 <li>   <b>拒絕遊戲邀請：</b>沒有遊戲的推出，Laura 是仍是在合作對象，而且將會收到後續的邀請。
    <li>   <b style="background-color: #FFFF00">將合作對象：啟動時沒有遊戲，Laura 移除從合作對象。</b>
    </ul>  
    </td>
  </tr>
</table>


## <a name="more-about-the-game-invite-sent-toast"></a>深入了解遊戲邀請傳送快顯通知

**遊戲的邀請傳送**快顯通知只會出現在遠端合作對象成員建立遊戲的工作階段的第一次。 後續要求傳送至遠端合作對象成員不會產生此快顯通知。 這可防止使用者在不用具有重複**邀請傳送遊戲**快顯通知，如果標題進行多次呼叫**PullReservedPlayersAsync**。

**附註**最佳做法是新增所有需要的朋友為保留，並接著呼叫**PullReservedPlayersAsync**一次。
