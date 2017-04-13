---
author: QuinnRadich
Description: "設計您應用程式的詳細指示和建議的外部說明頁面。"
title: "設計外部說明頁面的指導方針"
label: External help
template: detail.hbs
ms.author: quradic
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 56afd553-c520-4a28-b63d-2e1b3c1d3606
ms.openlocfilehash: 27b3fa1d1765d548e0d290d989eac542e63bc853
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="external-help-pages"></a>外部說明頁面



如果您的應用程式需要複雜內容的詳細說明，請考慮將這些指示裝載在網頁。

## <a name="when-to-use-external-help-pages"></a>何時使用外部說明頁面

對於一般用途或快速參考，外部說明頁面較不方便。 它們適用於太大而無法納入應用程式本身的說明內容，以及其一般對象不會使用的應用程式進階函式的教學課程和指示。

如果您的說明內容簡短或夠明確可顯示在應用程式內，則應該這樣做。 除非必要，否則請不要指示使用者取得應用程式外部的說明。

## <a name="navigating-external-help-pages"></a>瀏覽外部說明頁面

將使用者導向外部說明頁面時，請遵循兩個案例中的其中一個︰
-   它們直接連結到對應已知問題的頁面。 這是內容說明，而且應該盡可能使用。
-   它們會連結到一般說明頁面，可清楚顯示可從中選擇的類別和子類別。

為使用者提供搜尋您說明的方法可能十分有用，但不要讓這個搜尋成為瀏覽您說明的唯一方式。 使用者有時很難描述其問題，這樣會讓搜尋變得困難。 使用者應該能夠快速找到與其問題相關的頁面，而不需要進行搜尋。

## <a name="tutorials-and-detailed-walkthroughs"></a>教學課程和詳細逐步解說

外部說明頁面是為使用者提供教學課程和逐步解說的理想位置 (不論是視訊還是文字)。
-   教學課程應著重在更複雜的概念和進階函式。 使用者應該不需要教學課程，就能使用您的應用程式。
-   請確定這些教學課程的顯示方式與標準說明指示不同。 與想要其問題的直接解決方法的使用者比較起來，尋找進階指示的使用者會更想要進行搜尋。
-   請考慮從目錄和對應到每個教學課程的個別說明頁面連結到教學課程。

## <a name="related-articles"></a>相關文章

* [應用程式說明的指導方針](guidelines-for-app-help.md)
