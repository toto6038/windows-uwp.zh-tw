---
layout: LandingPage
description: 此頁面為您提供開始開發 ARM64 win32 與 UWP 應用程式的資訊。
title: Windows 10 on ARM
author: msatranjr
ms.author: misatran
ms.date: 05/08/2018
ms.localizationpriority: medium
ms.topic: article
keywords: Windows 10 on ARM, ARM, 建置 win32 ARM64 應用程式, 建置 ARM64 驅動程式
ms.openlocfilehash: 83f2a0d03040a682e6965558174294fe27e21bfb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57582328"
---
# <a name="windows-10-on-arm"></a>Windows 10 on ARM
Windows 10 會在由 ARM 處理器提供支援的電腦上執行。 此頁面為您提供深入了解此平台和開始開發應用程式的資訊。 我們也鼓勵您使用頁面底部的連結來提供意見反應。

## <a name="introductory-videos"></a>簡介影片
觀看並了解 Windows 10 如何在 ARM 上執行。

<ul class="cols cols3">
    <li>
        <a href="https://youtu.be/OZtVBDeVqCE"><img alt="Building ARM64 Win32 C++ apps video" src="./images/Arm64Scaled.png" /></a>
        <h3>打造 ARM64 Win32 C++ 應用程式</h3><p>了解如何安裝適用於 Visual Studio 的 ARM64 工具。 然後我們將逐步引導您完成建立及編譯新的 ARM 64 專案。</p>
    </li>
    <li>
        <a href="https://channel9.msdn.com/Events/Build/2018/BRK2438"><img alt="Build 2018 Windows 10 on ARM for developers" src="./images/buildVideoStillScaled.png" /></a>
        <h3>建置開發人員適用的 2018 Windows 10 on ARM</h3><p>了解 Windows 10 on ARM 裝置、x86 模擬如何發揮神奇作用，以及最終如何提交和建置適用於 Windows 10 on ARM 的應用程式。 我們將會示範如何建置適用於傳統型和 UWP 的 ARM64 應用程式。</p>
    </li>
    <li>
        <a href="https://channel9.msdn.com/Events/Ch9Live/Windows-Community-Standup/Kevin-Gallo-January-2018"><img alt="Community standup video featuring Kevin Gallo" src="./images/communityStandupStillScaled.png" /></a>
        <h3>Kevin Gallo 在 Windows Community Standup 例會中的影像</h3><p>深入了解 Windows 10 在 ARM64 上的執行方式，以及熟悉此平台上的應用程式和體驗。</p>
    </li>
</ul>

## <a name="understanding-windows-10-on-arm"></a>了解 Windows 10 on ARM
藉由查看下列資源來了解此平台。

<ul class="cardsF panelContent cols cols2">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm" title="入門" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="Get started icon" src="/media/common/i_get-started.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>開始使用 Windows 10 on ARM</h3>
                    <p class="x-hidden-focus">請查看說明文件以了解基本概念。</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-x86-emulation" title="x86 模擬相關主題" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                             <img class="cardImage" role="presentation" alt="x86 emulation icon" src="/media/common/i_advanced.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>了解 x86 模擬的運作方式</h3>
                    <p class="x-hidden-focus">深入了解 Windows 10 on ARM 的這項重要功能。</p>
                </div>
            </div>
        </div>
    </li>
    <!--<li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="https://blogs.msdn.microsoft.com/harip/" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="" src="/media/common/i_blog.svg?branch=master" data-linktype="external" />
                            </a>
                    </div>
                </a>
                <div class="cardText">
                    <h3>Read the Kernel blog</h3>
                    <p class="x-hidden-focus">Get a deep understanding of the Windows by reading articles that are written by the creators of the kernel.</p>
                </div>
            </div>
        </div>
    </li>-->
</ul>

## <a name="developing-for-windows-10-on-arm"></a>針對 Windows 10 on ARM 進行開發
開始針對 Windows 10 on ARM 量身打造您的應用程式，並善用可用的功能。  

<ul class="cardsF panelContent cols cols3">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="https://blogs.windows.com/buildingapps/?p=52087" title="建置 ARM64 應用程式" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="Build ARM64 Win32 apps blog icon" src="/media/common/i_build.svg" data-linktype="external" />
                    </div>
                    </a>
                <div class="cardText">
                    <h3>使用 SDK 建置 ARM64 應用程式</h3>
                    <p class="x-hidden-focus">請查看此部落格文章，我們會在其中逐步引導您將應用程式編譯為 ARM64，進而以原生方式在 Windows 10 on ARM 上執行。</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-troubleshooting-arm32" title="對 arm32 應用程式進行疑難排解" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="UWP apps on ARM icon" src="/media/common/i_code-edit.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>ARM 上的 UWP 應用程式</h3>
                    <p class="x-hidden-focus">遵循本指引來設定您的通用 Windows 平台 (UWP) 應用程式，準備邁向成功。</p>                    
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows-hardware/drivers/debugger/debugging-arm64" title="對 ARM64 應用程式進行偵錯" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                             <img class="cardImage" role="presentation" alt="Debugging on ARM icon" src="/media/common/i_debug.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>在 ARM 上偵錯</h3>
                    <p class="x-hidden-focus">讓您的程式碼在 Windows 10 on ARM 上順利執行。</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows-hardware/drivers/develop/building-arm64-drivers" title="建置 ARM64 驅動程式" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="Building ARM64 drivers icon" src="/media/common/i_drivers.svg" data-linktype="external" />
                            </a>
                    </div>
                </a>
                <div class="cardText">
                    <h3>使用 WDK 建置 ARM64 驅動程式</h3>
                    <p class="x-hidden-focus">針對 ARM64 重新編譯您的驅動程式。</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-troubleshooting-x86" title="對 x86 應用程式進行疑難排解" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="x86 apps on ARM icon" src="/media/common/i_code-blocks.svg" data-linktype="external" />
                            </a>
                    </div>
                </a>
                <div class="cardText">
                    <h3>ARM 上的 x86 應用程式</h3>
                    <p class="x-hidden-focus">開發您的 x86 應用程式，使其在 Windows 10 on ARM 上展現最佳效能。</p>
                </div>
            </div>
        </div>
    </li>
</ul>

<!--## Other videos
<ul class="cols cols4">
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
</ul>-->

## <a name="let-us-know-if-you-have-feedback"></a>讓我們知道您的意見反應
我們會運用來自您與現有客戶的意見反應，持續改進我們的產品。 如果您有任何想法、深陷問題中，或者只是想要分享您的絕佳體驗，以下這些連結可為您提供協助。

<ul class="cardsM cols cols3">
<li>
        <a class="card" href="feedback-hub://?tabid=2&contextid=803" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Feedback hub icon" src="/media/common/i_feedback.svg" data-linktype="external" />
            <div class="cardText">
                <h3>使用意見反應中樞</h3>
                <p>我們是否錯過任何事？ 您有絕佳的想法嗎？ 讓我們在意見反應中樞中獲知。</p>
            </div>
        </a>
    </li>
    <li>
        <a class="card" href="mailto:woafeedback@microsoft.com" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Report a bug icon" src="/media/common/i_mail.svg" data-linktype="external" />
            <div class="cardText">
                <h3>回報錯誤 (bug)</h3>
                <p>在我們的平台中找到錯誤 (bug)？ 透過電子郵件將詳細資料傳送給我們。</p>
            </div>
        </a>
    </li>
    <li>
        <a class="card" href="https://github.com/MicrosoftDocs/windows-uwp/tree/docs/landing/arm-docs" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Give doc feedback icon" src="/media/common/i_form.svg" data-linktype="external" />
            <div class="cardText">
                <h3>提供文件意見反應</h3>
                <p>您發現我們的文件有問題？ 您希望我們澄清一下嗎？ 在我們文件 GitHub 存放庫上提出問題。</p>
            </div>
        </a>
    </li>
</ul>