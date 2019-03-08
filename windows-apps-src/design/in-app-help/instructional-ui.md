---
Description: 設計教導使用者如何使用您的 UWP 應用程式說明的使用者介面 (UI)。
title: 設計指示性 UI 的指導方針
label: Instructional UI
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: c87e2f06-339d-4413-b585-172752964f56
ms.localizationpriority: medium
ms.openlocfilehash: b39507fb1333fdb642601c6b4828c3d160c6ceb5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656433"
---
# <a name="instructional-ui-guidelines"></a>指示性 UI 指導方針



在某些情況下，教導使用者有關應用程式中不明顯的功能 (例如特定觸控互動) 可能十分有用。 在這些情況下，您需要透過使用者介面 (UI) 向使用者呈現指示，讓他們可以使用可能沒有注意到的功能。

## <a name="when-to-use-instructional-ui"></a>何時使用指示性 UI

指示性 UI 必須謹慎使用。 過度使用時，即可輕鬆地予以略過，也可能會惹惱使用者，使其無效。

指示性 UI 應該用來協助使用者探索重要且不明顯的應用程式功能，例如使用者可能感興趣的觸控手勢或設定。 它也可以用來通知使用者有關應用程式中他們可能已忽略的新功能或變更。

除非您的應用程式依賴觸控手勢，否則不應該使用指示性 UI 向使用者教導您應用程式的基本功能。

## <a name="principles-of-writing-instructional-ui"></a>撰寫指示性 UI 的原則

良好的指示性 UI 是與使用者有關並且對使用者具有教育性，而且可以提升使用者體驗。 它應該是︰

-   **簡單：** 使用者不想中斷與複雜資訊的使用者體驗
-   **令人印象深刻：** 使用者不想看到的相同指示，每次嘗試一項工作，因此需要為它們就會記住的指示。
-   **立即相關：** 如果說明 UI 不會教導他們立即想要的項目使用者，他們不需要留意它的原因。

請避免過度使用指示性 UI，並務必選擇正確的主題。 請不要教導：

-   **基本功能：** 如果使用者需要使用您的應用程式的指示，請考慮讓應用程式的設計更具直覺性。
-   **明顯的功能：** 如果使用者可以找出自己沒有指示上的功能，然後說明 UI 會只收到的方式。
-   **複雜的功能：** 說明 UI 必須很簡潔，且複雜的功能有興趣的使用者通常是對找出指示，而且不需要提供它們。

請避免讓指示性 UI 造成使用者的不方便。 請勿執行下列動作：

-   **難以理解的重要資訊：** 說明 UI 永遠不應該妨礙您的應用程式的其他功能。
-   **強制使用者都可以參與：** 使用者應該能夠略過指示的 UI，仍然繼續透過應用程式的進度。
-   **顯示重複的資訊：** 不騷擾教學的 UI 中，與使用者，即使它們忽略它第一次。 新增再次顯示指示性 UI 的設定是比較好的做法。

## <a name="examples-of-instructional-ui"></a>指示性 UI 範例

以下是指示性 UI 可協助使用者學習的一些實例：

-   **協助使用者探索觸控互動。** 下列螢幕擷取畫面顯示指示性 UI 如何教導玩家在 Cut the Rope 遊戲中使用觸控手勢。

    ![遊戲的螢幕擷取畫面顯示指示性 UI 訊息 "slide acress to cut the rope"](images/in-game-controls-3.png)

-   **建立絕佳的第一印象。** 當「影片時刻」第一次啟動時，指示性 UI 會提示使用者開始建立影片，但不會妨礙其體驗。

    ![影片時刻應用程式的啟動畫面](images/instructional-ui-movie.png)

-   **引導使用者進行下一個步驟中複雜的工作。** 在 Windows 郵件 App 中，收件匣底部的提示會引導使用者進入 [設定] 以存取較舊的郵件。

    ![顯示指示性 UI 訊息的 Windows 郵件應用程式的裁剪螢幕擷取畫面](images/instructional-ui-mail-inbox.png)

    使用者按一下訊息時，App 的 [設定] 飛出視窗會顯示在畫面的右側，讓使用者完成工作。 這些螢幕擷取畫面會顯示使用者按一下指示性 UI 訊息之前與之後，郵件應用程式的情況。

    | 之前                                                               | 之後                                                                                                        |
    |----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
    | ![Windows 郵件應用程式的螢幕擷取畫面](images/instructional-ui-mail.png) | ![具有延伸式設定飛出視窗的 Windows 郵件應用程式螢幕擷取畫面](images/instructional-ui-mail-flyout.png) |

## <a name="related-articles"></a>相關文章

* [如需應用程式說明的指導方針](guidelines-for-app-help.md)
