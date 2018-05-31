---
description: 了解如何設計在各種裝置與螢幕尺寸上都很容易瀏覽且看起來很棒的 UWP 應用程式並撰寫應用程式程式碼。
title: UWP app 版面配置設計
author: mijacobs
layout: LandingPage
keywords: uwp 應用程式配置、通用 Windows 平台、應用程式設計、介面
ms.author: mijacobs
ms.date: 3/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.assetid: 1aa12606-8a99-4db3-8311-90e02fde9cf1
ms.localizationpriority: medium
ms.openlocfilehash: 2bad76a2e8d6a817be8aae98da6fb8fa0386b147
ms.sourcegitcommit: 346b5c9298a6e9e78acf05944bfe13624ea7062e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/05/2018
ms.locfileid: "1707369"
---
# <a name="layout-for-uwp-apps"></a>UWP app 的版面配置

這些文章可協助您建立在不同螢幕尺寸、視窗大小、解析度及方向上看起來都會很棒的彈性化 UI。 


## <a name="responsive-layouts"></a>回應式版面配置

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                        <a href="screen-sizes-and-breakpoints-for-responsive-design.md">
                            <img src="images/landing/breakpoints.png" alt=" " style="display: block; width: 100%; height: auto;" />
                            </a>
                        </div>
                    </div> 
                    <div class="cardText">
                        <h3><a href="screen-sizes-and-breakpoints-for-responsive-design.md">螢幕大小與中斷點</a></h3>
                        <p>整個 Windows 10 生態系統中，裝置目標和螢幕大小有太多種，以致於無法針對每一個最佳化您的 UI。 我們建議設計用於幾個稱做中斷點的重要寬度，做為替代方案。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                        <a href="screen-sizes-and-breakpoints-for-responsive-design.md">
                            <img src="images/landing/reposition.png" alt=" " style="display: block; width: 100%; height: auto;"/>
                            </a>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="responsive-design.md">回應式設計技術</a></h3>
                        <p>當您針對特定螢幕寬度自訂應用程式的 UI 時，我們假設您要建立回應式設計。 以下是六種您可以用來自訂應用程式 UI 的回應式設計技術。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

## <a name="pages-and-panels"></a>頁面及面板

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <!-- <div class="cardImageOuter">
                        <div class="cardImage">
                        <a href="layouts-with-xaml.md">
                            <img src="images/landing/reposition.png" alt=" " style="display: block; width: 100%; height: auto;"/>
                            </a>
                        </div>
                    </div> -->
                    <div class="cardText">
                        <h3><a href="layouts-with-xaml.md">使用 XAML 建立回應式版面配置</a></h3>
                        <p>了解如何使用 XAML 版面配置面板，讓您的應用程式具有回應性及調適性。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <!-- <div class="cardImageOuter">
                        <div class="cardImage">
                        <a href="layout-panels.md">
                            <img src="images/landing/reposition.png" alt=" " style="display: block; width: 100%; height: auto;"/>
                            </a>
                        </div>
                    </div> -->
                    <div class="cardText">
                        <h3><a href="layout-panels.md">版面配置面板</a></h3>
                        <p>深入了解每個面板的每種配置類型，以及示範如何使用它們來配置 XAML UI 元素。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>    
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <!-- <div class="cardImageOuter">
                        <div class="cardImage">
                        <a href="grid-tutorial.md">
                            <img src="images/landing/reposition.png" alt=" " style="display: block; width: 100%; height: auto;"/>
                            </a>
                        </div>
                    </div> -->
                    <div class="cardText">
                        <h3><a href="grid-tutorial.md">使用 Grid 和 StackPanel 建立配置</a></h3>
                        <p>使用 Grid 和 StackPanel 元素，使用 XAML 來建立天氣應用程式的簡單版面配置。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>  
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <!-- <div class="cardImageOuter">
                        <div class="cardImage">
                        <a href="alignment-margin-padding.md">
                            <img src="images/landing/breakpoints.png" alt=" " style="display: block; width: 100%; height: auto;" />
                            </a>
                        </div>
                    </div>  -->
                    <div class="cardText">
                        <h3><a href="alignment-margin-padding.md">對齊、邊界及邊框間距</a></h3>
                        <p>除了維度屬性 (寬度、高度及限制) 之外，元素還可以有對齊、邊界及邊框間距屬性，這些屬性會在元素進行版面配置階段並在 UI 中轉譯時，影響配置行為。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>

</ul>


## <a name="windows-and-views"></a>視窗與檢視

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <!-- <div class="cardImageOuter">
                        <div class="cardImage">
                        <a href="show-multiple-views.md">
                            <img src="images/landing/breakpoints.png" alt=" " style="display: block; width: 100%; height: auto;" />
                            </a>
                        </div>
                    </div>  -->
                    <div class="cardText">
                        <h3><a href="show-multiple-views.md">顯示多重檢視</a></h3>
                        <p>讓使用者檢視不同視窗中應用程式的獨立部分。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>

</ul>

## <a name="transformations"></a>轉換

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <!-- <div class="cardImageOuter">
                        <div class="cardImage">
                        <a href="show-multiple-views.md">
                            <img src="images/landing/breakpoints.png" alt=" " style="display: block; width: 100%; height: auto;" />
                            </a>
                        </div>
                    </div>  -->
                    <div class="cardText">
                        <h3><a href="transforms.md">旋轉、傾斜、縮放及其他轉換</a></h3>
                        <p>使用轉換旋轉、傾斜及縮放元素。 您甚至可以使用轉換，讓它 2-D 內容的外觀看起來像是 3-D。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>

</ul>



