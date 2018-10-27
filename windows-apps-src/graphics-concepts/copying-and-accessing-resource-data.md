---
title: 複製和存取資源資料
description: 使用方式旗幟表示了應用程式將會如何使用資源資料，並在可能的情況下將資源放置於記憶體中效能最佳的區域。 資源資料在所有資源中都會被複製，以便 CPU 或 GPU 可以在不影響效能的情況下進行存取。
ms.assetid: 6A09702D-0FF2-4EA6-A353-0F95A3EE34E2
keywords:
- 複製和存取資源資料
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e7b0f06711b4a908f8990dfb16968400c685c15f
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2018
ms.locfileid: "5695112"
---
# <a name="copying-and-accessing-resource-data"></a>複製和存取資源資料


使用方式旗幟表示了應用程式將會如何使用資源資料，並在可能的情況下將資源放置於記憶體中效能最佳的區域。 資源資料在所有資源中都會被複製，以便 CPU 或 GPU 可以在不影響效能的情況下進行存取。

您不需要考慮資源究竟是建立於視訊記憶體之中，還是系統記憶體之中，或是決定執行階段是否應該管理記憶體。 透過 WDDM (Windows 顯示驅動程式模型) 架構，應用程式藉由使用方式旗幟建立 Direct3D 資源，以表示應用程式將會如何使用資源資料。 此驅動程式模型會把資源所使用的記憶體虛擬化。作業系統/驅動程式/記憶體管理員應考慮到預期使用量，在可能的情況下將資源放置於記憶體中效能最佳的區域。

在預設案例下，資源可供 GPU 使用。 有些時候，資源資料必須供 CPU 使用。 若要將資源予以複製，使適當的處理器可在不影響效能的情況下進行存取，您需要具備 API 方法運作方式的基本知識。

## <a name="span-idcopyingspanspan-idcopyingspanspan-idcopyingspancopying-resource-data"></a><span id="Copying"></span><span id="copying"></span><span id="COPYING"></span>複製資源資料


Direct3D 執行建立呼叫時，會在記憶體中建立資源。 他們可以在視訊記憶體、系統記憶體，或任何其他類型的記憶體之中建立。 由於 WDDM 驅動程式模型會把此記憶體虛擬化，應用程式不再需要追蹤資源究竟是建立於哪一種記憶體之中。

理想中，所有資源最好都位於視訊記憶體之中，以供 GPU 立即存取。 然而，有時候資源資料必須供 CPU 進行讀取，或是讓 CPU 已寫入的資源資料供 GPU 進行存取。 Direct3D 藉由要求應用程式明確指定使用方式，並在需要將資源資料進行複製時提供幾種方法，以應付這些不同的情況。

根據建立資源的方式，並不一定都可以直接存取基礎資料。 這表示資源資料必須從來源資源中複製到另一個可供適當處理器進行存取的資源。 在 Direct3D 之中，GPU 可直接存取預設資源，CPU 則可直接存取動態及暫存資源。

資源建立後，就無法變更其使用方式。 但您可以將一個資源的內容複製到另一個被指定為不同使用方式的資源之中。 您可以選擇將資源資料從一個資源複製到另一個，或是從記憶體中將資料複製到一個資源。

資源可分為兩種︰可對應與不可對應。 以動態或暫存使用方式建立的資源為可對應資源，而以預設或不可變動使用方式建立的資源為不可對應資源。

在不可對應資源之間複製資料非常迅速，因為這是最常見的案例，其執行也已最佳化。 由於這些資源無法由 CPU 直接存取，因此其已進行最佳化，使 GPU 可以快速地對其進行操作。

在可對應資源之間複製資料則可能較容易發生問題，因為其效能將會取決於資源建立時指定的使用方式。 例如：GPU 可以快速地讀取動態資源，但卻無法對其進行寫入，並且 GPU 無法直接地讀取或寫入暫存資源。

應用程式若要將資料從使用預設使用方式的資源，複製到另一個使用暫存使用方式的資源 (使 CPU 可以讀取該資料，即 GPU 讀回問題) ，必須小心進行操作。 請參閱下方的[存取資源資料](#accessing)。

## <a name="span-idaccessingspanspan-idaccessingspanspan-idaccessingspanaccessing-resource-data"></a><span id="Accessing"></span><span id="accessing"></span><span id="ACCESSING"></span>存取資源資料


存取資源需必須先對應該資源。基本上，「對應」即表示應用程式正在嘗試讓 CPU 對記憶體進行存取。 為使 CPU 能存取基本記憶體而對資源進行對應的程序，可能會導致某些效能瓶頸。基於這個原因，應注意如何及何時執行這項工作。

若應用程式嘗試在錯誤的時間點進行對應資源，其執行效能可能會降低到停止的程度。 若應用程式嘗試在作業完成之前對其結果進行存取，將可能導致管線停頓。

若在錯誤的時間點進行對應作業，可能會因為強制 GPU 與 CPU 互相同步而導致效能嚴重下降 。 此同步動作會在應用程式嘗試在 GPU 完成將資源複製到另一個 CPU 可以對應的資源前對其進行存取時發生。

### <a name="span-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanperformance-considerations"></a><span id="Performance_Considerations"></span><span id="performance_considerations"></span><span id="PERFORMANCE_CONSIDERATIONS"></span>效能考量

建議您將您的電腦視為一台以兩種不同類型處理器構成的平行結構運作的機器︰一個或多個 CPU，以及一個或多個 GPU。 如同任何平行結構一樣，當每個處理器都排程了足夠的工作，以避免其進入閒置狀態或等待別的處理器完成工作時，運作才可達到最佳效能。

GPU/CPU 平行處理原則中最糟的情況，就是強制一個處理器等候另一個處理器工作完成的結果。 Direct3D 藉由將複製方法非同步化，以避免發生這種情況，即複製不見得要在方法傳回時才能執行。

其優點在於應用程式不需要在 CPU 存取資料時才付出效能降低的成本，即呼叫 Map 方法時。 若 Map 方法在資料確實複製後才被呼叫，便不會造成任何效能上的損失。 另一方面，若 Map 方法在資料複製之前就被呼叫，便會發生管線停頓。

Direct3D 中的非同步呼叫 (大部分的方法都是非同步的，尤其是轉譯呼叫) 會儲存在所謂的 *「命令緩衝區」* 中。 這個緩衝區位於圖形驅動程式的內部，並用來對基礎硬體進行批次呼叫，使 Microsoft Windows 從使用者模式切換至核心模式這種極為耗費成本狀況的發生能夠盡量減少。

命令緩衝區在下列四種情況下會被排清，並造成使用者/核心模式的切換。

1.  呼叫 Present 時。
2.  呼叫 Flush 時。
3.  命令緩衝區已滿。其大小為動態且由作業系統及圖形驅動程式控制。
4.  CPU 需要存取命令緩衝區中一個尚待執行命令的結果。

在以上四種情況中，第四種情況會對效能造成最嚴重的影響。 若應用程式發出對資源或子資源進行複製的呼叫，該呼叫會排入命令緩衝區的佇列之中。

若應用程式試圖在命令緩衝區排清前，對一項複製呼叫的目標暫存資源進行對應，將會導致管線停頓，因為不只呼叫 Copy 方法的命令需要執行，命令緩衝區佇列中所有其他的命令也都要執行。 這將會導致 GPU 與 CPU 進行同步，因為 CPU 將會等待 GPU 清空命令緩衝區並填滿 CPU 需要的資源後，才能存取暫存資源。 當 GPU 完成複製之後，CPU 才會開始存取暫存資源，然而此期間 GPU 就會進入閒置狀態。

若經常性地在執行階段進行此動作，將會大幅降低效能。 基於這個原因，對應以預設使用方式建立的資源時，應小心謹慎。 應用程式必須等候夠長的時間，讓命令緩衝區被清空並且完成執行佇列中所有的命令，才能嘗試對應相對應的暫存資源。

應用程式應該等候多久才行？ 至少需要等待兩個影格的時間，因為這樣才能使 CPU 和 GPU 之間的平行處理原則得到最大且有效率的調控。 GPU 的運作方式，是當應用程式藉由送出呼叫到命令緩衝區中以處理第 N 個影格時，GPU 正忙碌於執行前一個影格 (N-1) 的呼叫。

因此，若應用程式要對應一個來自於視訊記憶體中的資源，並且在第 N 個影格時複製一個資源，這項呼叫實際上會在第 N+1 個影格，即應用程式正在為下一個影格送出呼叫時，才會被執行。 複製應會在應用程式正在處理第 N+2 個影格時完成。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">影格</th>
<th align="left">GPU/CPU 狀態</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">N</td>
<td align="left"><ul>
<li>CPU 為目前的影格送出轉譯呼叫。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">N+1</td>
<td align="left"><ul>
<li>GPU 執行 CPU 在第 N 個影格時送出的呼叫。</li>
<li>CPU 為目前的影格送出轉譯呼叫。</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">N+2</td>
<td align="left"><ul>
<li>GPU 完成執行 CPU 在第 N 個影格時送出的呼叫。結果準備就緒。</li>
<li>GPU 執行 CPU 在第 N+1 個影格時送出的呼叫。</li>
<li>CPU 為目前的影格送出轉譯呼叫。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">N+3</td>
<td align="left"><ul>
<li>GPU 完成執行 CPU 在第 N+1 個影格時送出的呼叫。 結果準備就緒。</li>
<li>GPU 執行 CPU 在第 N+2 個影格時送出的呼叫。</li>
<li>CPU 為目前的影格送出轉譯呼叫。</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">N+4</td>
<td align="left">...</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[資源](resources.md)

 

 




