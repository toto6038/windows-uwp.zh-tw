---
title: 裝置入口網站遠端輸入 API 參考
description: 了解如何在 Xbox 上從遠端傳送控制器、鍵盤和滑鼠輸入。
ms.localizationpriority: medium
ms.openlocfilehash: e0db86ad50bfb1cb27f516243542a554e710c3ea
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "8200513"
---
# <a name="remote-input-api-reference"></a>遠端輸入 API 參考   
您可以透過此 API 即時從遠端傳送控制器、鍵盤和滑鼠輸入。

**要求**

方法      | 要求 URI
:------     | :-----
Websocket | /ext/remoteinput
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求**

Websocket 一系列位元組陣列訊息。 每個訊息格式如下所示。

第一個位元組表示輸入類型。 支援下列輸入類型：

| 輸入類型        | 位元組值 |
|------------|-------------|
鍵盤 KeyCodes | 0x01
鍵盤 ScanCodes | 0x02
滑鼠 | 0x03
全部清除 | 0x04

對於 KeyboardKeyCodes 和 KeyboardScanCodes，第二個位元組是 keycode 或 scancode 的值，第三個位元組是且向下按的 0x01 和放開的 0x00。

對於滑鼠訊息，下一個值是網路順序的 UINT16 (2 位元組)，指出滑鼠事件的類型：

| 動作類型        | UINT16 值 |
|------------|-------------|
移動 | 0x0001
LeftDown | 0x0002
LeftUp | 0x0004
RightDown | 0x0008
RightUp | 0x0010
MiddleDown | 0x0020
MiddleUp | 0x0040
X1Down | 0x0080
X1Up | 0x0100
X2Down | 0x0200
X2Up | 0x0400
VerticalWheelMoved | 0x0800
HorizontalWheelMoved | 0x1000

依照網路順序此位元組後面是兩個 UINT32 值，以及滾輪動作適用的選用第三個 UINT32。 前兩個值是 X 和 Y 座標，第三個是滾輪差異。 X 和 Y 座標應該是介於 0 和 65535 之間的值，指出在水平和垂直平面中滑鼠的相對位置。

全部清除表示目前任何按住的按鍵應該會釋放。 不預期任何其他位元組。

對於傳送遊樂台輸入，表示遊樂台按鈕按下的 keycode 值可以搭配 KeyboardKeyCodes 使用。 這些值如下：

| 遊樂台類型        | 位元組值 |
|------------|-------------|
VK_GAMEPAD_A                       |  0xC3
VK_GAMEPAD_B                       |  0xC4
VK_GAMEPAD_X                       |  0xC5
VK_GAMEPAD_Y                       |  0xC6
VK_GAMEPAD_RIGHT_SHOULDER          |  0xC7
VK_GAMEPAD_LEFT_SHOULDER           |  0xC8
VK_GAMEPAD_LEFT_TRIGGER            |  0xC9
VK_GAMEPAD_RIGHT_TRIGGER           |  0xCA
VK_GAMEPAD_DPAD_UP                 |  0xCB
VK_GAMEPAD_DPAD_DOWN               |  0xCC
VK_GAMEPAD_DPAD_LEFT               |  0xCD
VK_GAMEPAD_DPAD_RIGHT              |  0xCE
VK_GAMEPAD_MENU                    |  0xCF
VK_GAMEPAD_VIEW                    |  0xD0
VK_GAMEPAD_LEFT_THUMBSTICK_BUTTON  |  0xD1
VK_GAMEPAD_RIGHT_THUMBSTICK_BUTTON |  0xD2
VK_GAMEPAD_LEFT_THUMBSTICK_UP      |  0xD3
VK_GAMEPAD_LEFT_THUMBSTICK_DOWN    |  0xD4
VK_GAMEPAD_LEFT_THUMBSTICK_RIGHT   |  0xD5
VK_GAMEPAD_LEFT_THUMBSTICK_LEFT    |  0xD6
VK_GAMEPAD_RIGHT_THUMBSTICK_UP     |  0xD7
VK_GAMEPAD_RIGHT_THUMBSTICK_DOWN   |  0xD8
VK_GAMEPAD_RIGHT_THUMBSTICK_RIGHT  |  0xD9
VK_GAMEPAD_RIGHT_THUMBSTICK_LEFT   |  0xDA


**回應**   

- 無

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 要求成功
4XX | 錯誤碼
5XX | 錯誤碼

<br />
**可用裝置系列**

* Windows Xbox