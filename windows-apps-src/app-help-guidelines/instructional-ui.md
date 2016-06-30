---
author: QuinnRadich
Description: "設計指示性使用者介面 (UI)，教導使用者如何使用您的 Windows 市集 app。"
title: "設計指示性 UI 的指導方針"
label: Instructional UI
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: 1c8ab97a5625243a50fa3b40ae93f5928dc7d851

---

# 指示性 UI 指導方針



在某些情況下，教導使用者有關 App 中不明顯的功能 (例如特定觸控互動) 可能十分有用。 在這些情況下，您需要透過使用者介面 (UI) 向使用者呈現指示，讓他們可以使用可能沒有注意到的功能。

## <span id="when_to_use_instructional_ui"></span><span id="WHEN_TO_USE_INSTRUCTIONAL_UI"></span>何時使用指示性 UI

指示性 UI 必須謹慎使用。 過度使用時，即可輕鬆地予以略過，也可能會惹惱使用者，使其無效。

指示性 UI 應該用來協助使用者探索重要且不明顯的應用程式功能，例如使用者可能感興趣的觸控手勢或設定。 它也可以用來通知使用者有關應用程式中他們可能已忽略的新功能或變更。

除非您的 App 依賴觸控手勢，否則不應該使用指示性 UI 向使用者教導您 App 的基本功能。

## <span id="writing_instructional_ui"></span><span id="WRITING_INSTRUCTIONAL_UI"></span>撰寫指示性 UI 的原則

良好的指示性 UI 是與使用者有關並且對使用者具有教育性，而且可以提升使用者體驗。 它應該是︰

-   **簡單：**使用者不想因複雜資訊而中斷他們的體驗
-   **可記憶：**使用者不想要每次嘗試工作時都看到相同的指示，因此指示必須是他們記得的事項。
-   **立即相關︰**如果指示性 UI 未教導使用者有關他們立即要執行的作業，則他們沒有任何原因需要注意它。

請避免過度使用指示性 UI，並務必選擇正確的主題。 請不要教導：

-   **基礎功能︰**如果使用者需要您應用程式的使用指示，請考慮讓應用程式的設計更具直覺式。
-   **明顯功能：**如果使用者可以在沒有指示的情況下自行找出功能，則指示性 UI 只是會礙事。
-   **複雜功能︰**指示性 UI 必須簡潔，而且對複雜功能感興趣的使用者通常都願意找出指示，並不需要提供給他們。

請避免讓指示性 UI 造成使用者的不方便。 請勿執行下列動作：

-   **隱藏重要資訊：**指示性 UI 應該永遠不會干擾您應用程式的其他功能。
-   **強制使用者參與︰**使用者應該可以忽略指示性 UI，並且仍然繼續執行應用程式。
-   **顯示重複資訊︰**不要使用指示性 UI 不斷騷擾使用者，即使是第一次忽略它時也一樣。 新增再次顯示指示性 UI 的設定是比較好的做法。

## <span id="examples_of_instructional_ui"></span><span id="EXAMPLES_OF_INSTRUCTIONAL_UI"></span>指示性 UI 範例

以下是指示性 UI 可協助使用者學習的一些實例：

-   **協助使用者探索觸控互動。** 下列螢幕擷取畫面顯示指示性 UI 如何教導玩家在 Cut the Rope 遊戲中使用觸控手勢。

    ![遊戲的螢幕擷取畫面顯示指示性 UI 訊息 "slide acress to cut the rope"](images/in-game-controls-3.png)

-   **塑造良好的第一印象。** 當「影片時刻」第一次啟動時，指示性 UI 會提示使用者開始建立影片，但不會妨礙其體驗。

    ![影片時刻應用程式的啟動畫面](images/instructional-ui-movie.png)

-   **引導使用者執行複雜工作中的下一步。** 在 Windows 郵件 App 中，收件匣底部的提示會引導使用者進入 \[設定\] 以存取較舊的郵件。

    ![顯示指示性 UI 訊息的 Windows 郵件 app 的裁剪螢幕擷取畫面](images/instructional-ui-mail-inbox.png)

    使用者按一下訊息時，App 的 \[設定\] 飛出視窗會顯示在畫面的右側，讓使用者完成工作。 這些螢幕擷取畫面會顯示使用者按一下指示性 UI 訊息之前與之後，郵件應用程式的情況。

    | 之前                                                               | 之後                                                                                                        |
    |----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
    | ![Windows 郵件 app 的螢幕擷取畫面](images/instructional-ui-mail.png) | ![具有延伸式設定飛出視窗的 Windows 郵件 App 螢幕擷取畫面](images/instructional-ui-mail-flyout.png) |

## <span id="related_topics"></span>相關文章

* [App 說明的指導方針](guidelines-for-app-help.md)



<!--HONumber=Jun16_HO4-->


