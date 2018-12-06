---
description: 針對手寫筆、Surface Dial 和其他輸入類型最佳化您的應用程式。
title: 輸入與互動
keywords: 應用程式輸入, 自訂 UWP 應用程式
label: Input and interactions
layout: LandingPage
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
ms.assetid: b771d452-c3ac-4d97-8482-eaf81bf34306
ms.localizationpriority: medium
ms.openlocfilehash: 4f66d808cafcc6fba89cebde352d191335068925
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8751470"
---
# <a name="input-and-interactions"></a>輸入與互動

<!-- <div>
  <img src="images/keyboard/keyboard-hero.jpg" alt="" />
  <img src="images/input-interactions/icons-inputdevices03.png" />
</div> -->

UWP app 可自動處理各種輸入並在各種不同的裝置上執行 — 例如，您不需要額外做任何動作即可啟用觸控輸入。 但是您有時候可能會想要針對特定類型的輸入或裝置將您的應用程式最佳化。 例如，如果您建立繪圖應用程式，您可能想要自訂手寫筆輸入的處理方式。

本節中的設計與程式碼撰寫指示可協助您針對特定類型輸入自訂您的 UWP app。

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <!-- <div class="cardImageOuter">
                        <div class="cardImage" style="background-color: #f2f2f2" >
                        <a href="input-primer.md">
                            <img src="images/input-interactions/icons-inputdevices03.png" alt=" " style="display: block; width: 100%; height: auto;" />
                            </a>
                        </div>
                    </div>  -->
                    <div class="cardText">
                        <h3><a href="input-primer.md">輸入基本資訊</a></h3>
                        <p>與特定尺寸規格配對使用時，請熟悉各個輸入裝置類型及其行為、功能與限制。</p>
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
                        <div class="cardImage" style="background-color: #f2f2f2">
                        <a href="identify-input-devices.md">
                            <img src="images/landing-page/fluentdesign-app-sm.png" alt=" " style="display: block; width: 100%; height: auto;"/>
                            </a>
                        </div>
                    </div> -->
                    <div class="cardText">
                        <h3><a href="gaze-interactions.md">新增! 注視輸入</a></h3>
                        <p>根據使用者眼睛和頭部的位置和移動追蹤他們的注視。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

<!-- 
## Input primer

See our <b>[Input primer](index.md)</b> to familiarize yourself with each input device type and its behaviors, capabilities, and limitations when paired with certain form factors. -->


<ul class="panelContent cardsL" style="margin-left: 1px">
    <li>              
        <div style="display:block" class="cardSize">
            <div style="display:block" class="cardPadding">
                <div style="display:block" class="card">
                    <div style="display:block" class="cardText">
                        <h3>輸入</h3>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/identify-input-devices">識別輸入裝置</a></p>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/handle-pointer-input">指標</a></p>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/pen-and-stylus-interactions">手寫筆與 Windows Ink</a></p>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/touch-interactions">觸控</a></p>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/mouse-interactions">滑鼠</a></p>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/keyboard-interactions">鍵盤</a></p>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/gamepad-and-remote-interactions">遊戲台與遙控器</a></p>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/touchpad-interactions">觸控板</a></p>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/windows-wheel-interactions">Surface Dial</a></p>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/multiple-input-design-guidelines">多重輸入</a></p>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/input-injection">輸入插入</a></p>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/custom-text-input">自訂文字輸入</a></p>                        
                    </div>
                </div>
            </div>
        </div>        
    </li>  
    <li>              
        <div style="display:block" class="cardSize">
            <div style="display:block" class="cardPadding">
                <div style="display:block" class="card">
                    <div style="display:block" class="cardText">
                        <h3>互動</h3>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/drag-and-drop">拖放</a></p>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/guidelines-for-panning">移動瀏覽</a></p>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/guidelines-for-rotation">旋轉</a></p>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/guidelines-for-textselection">選取文字和影像</a></p>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/guidelines-for-targeting">目標預測</a></p>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/guidelines-for-visualfeedback">視覺化回饋</a></p>
                    </div>
                </div>
            </div>
        </div>        
    </li>
    <li>              
        <div style="display:block" class="cardSize">
            <div style="display:block" class="cardPadding">
                <div style="display:block" class="card">
                    <div style="display:block" class="cardText">
                        <h3>語音與 AI</h3>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/speech-interactions">語音</a></p>
                        <p style="display: block;"><a  href="/windows/uwp/design/input/cortana-interactions">Cortana</a></p>  
                    </div>
                </div>
            </div>
        </div>        
    </li>            
       
</ul>

<!-- <div class="side-by-side">
<div class="side-by-side-content">
<p>
<b>[Surface Dial](windows-wheel-interactions.md)</b><br/>
Learn how to integrate this brand new category of input device into your Windows apps.</br>
This device is intended as a secondary, multi-modal input device that complements or modifies input from a primary device.
</p>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Cortana](cortana-interactions.md)</b><br/>
Extend the basic functionality of Cortana with voice commands that launch and execute a single action in an external application.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Speech](speech-interactions.md)</b><br/>
Integrate speech recognition and text-to-speech (also known as TTS, or speech synthesis) directly into the user experience of your app.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Pen](pen-and-stylus-interactions.md)</b><br/>
Optimize your UWP app for pen input to provide both standard pointer device functionality and the best Windows Ink experience for your users.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Keyboard](keyboard-interactions.md)</b><br/>
Keyboard input is an important part of the overall user interaction experience for apps. The keyboard is indispensable to people with certain disabilities or users who just consider it a more efficient way to interact with an app.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Touch](touch-interactions.md)</b><br/>
UWP includes a number of different mechanisms for handling touch input, all of which enable you to create an immersive experience that your users can explore with confidence.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Touchpad](touchpad-interactions.md)</b><br/>
A touchpad combines both indirect multi-touch input with the precision input of a pointing device, such as a mouse. This combination makes the touchpad suited to both a touch-optimized UI and the smaller targets of productivity apps.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Mouse](mouse-interactions.md)</b><br/>
Mouse input is best suited for user interactions that require precision when pointing and clicking. This inherent precision is naturally supported by the UI of Windows, which is optimized for the imprecise nature of touch.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Gamepad and remote control](gamepad-and-remote-interactions.md)</b><br/>
UWP apps now support gamepad and remote control input. Gamepads and remote controls are the primary input devices for Xbox and TV experiences.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<p>
<b>[Multiple inputs](multiple-input-design-guidelines.md)</b><br/>
To accommodate as many users and devices as possible, we recommend that you design your apps to work with as many input types as possible (gesture, speech, touch, touchpad, mouse, and keyboard). Doing so will maximize flexibility, usability, and accessibility.
</p>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Identify input devices](identify-input-devices.md)</b><br/>
Identify the input devices connected to a Universal Windows Platform (UWP) device and identify their capabilities and attributes.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Handle pointer input](handle-pointer-input.md)</b><br/>
Receive, process, and manage input data from pointing devices, such as touch, mouse, pen/stylus, and touchpad, in Universal Windows Platform (UWP) apps.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p><b>[Custom text input](custom-text-input.md)</b><br/>
The core text APIs in the Windows.UI.Text.Core namespace enable a UWP app to receive text input from any text service supported on Windows devices. This enables the app to receive text in any language and from any input type, like keyboard, speech, or pen.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Selecting text and images](guidelines-for-textselection.md)</b><br/>
This article describes selecting and manipulating text, images, and controls and provides user experience guidelines that should be considered when using these mechanisms in your apps.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<p>
<b>[Panning](guidelines-for-panning.md)</b><br/>
Panning or scrolling lets users navigate within a single view, to display the content of the view that does not fit within the viewport.
</p>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Optical zoom and resizing](guidelines-for-optical-zoom.md)</b><br/>
This article describes Windows zooming and resizing elements and provides user experience guidelines for using these interaction mechanisms in your apps.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Rotation](guidelines-for-rotation.md)</b><br/>
This article describes the new Windows UI for rotation and provides user experience guidelines that should be considered when using this new interaction mechanism in your UWP app.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p><b>[Targeting](guidelines-for-targeting.md)</b><br/>
Touch targeting in Windows uses the full contact area of each finger that is detected by a touch digitizer. The larger, more complex set of input data reported by the digitizer is used to increase precision when determining the user's intended (or most likely) target.
</p>
</div>
<div class="side-by-side-content-right">
<p><b>[Visual feedback](guidelines-for-visualfeedback.md)</b><br/>
Use visual feedback to show users when their interactions are detected, interpreted, and handled. Visual feedback can help users by encouraging interaction. It indicates the success of an interaction, which improves the user's sense of control. It also relays system status and reduces errors.
</p>
</div>
</div>
</div> -->


