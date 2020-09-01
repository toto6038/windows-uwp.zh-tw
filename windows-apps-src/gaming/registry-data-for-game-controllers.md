---
title: 遊戲控制器的登錄資料
description: 深入了解您可以新增至 PC 登錄的資料，讓控制器可用於 UWP 遊戲中。
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.date: 04/08/2019
ms.topic: article
keywords: windows 10, uwp, games, input, registry, custom, 遊戲, 輸入, 登錄, 自訂
ms.localizationpriority: medium
ms.openlocfilehash: ac2ca98a067fb88dfcdc86c4e4ee4047b82206bc
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159272"
---
# <a name="registry-data-for-game-controllers"></a>遊戲控制器的登錄資料

> [!NOTE]
> 本主題適用於與 Windows 10 相容遊戲控制器的製造商，不適用大多數的開發人員。

[Windows.Gaming.Input namespace](/uwp/api/windows.gaming.input) 可讓獨立硬體廠商 (IHV) 將資料新增至 PC 的登錄，讓其裝置顯示成 [Gamepads](/uwp/api/windows.gaming.input.gamepad)、[RacingWheels](/uwp/api/windows.gaming.input.racingwheel)、[ArcadeSticks](/uwp/api/windows.gaming.input.arcadestick)、[FlightSticks](/uwp/api/windows.gaming.input.flightstick) 及 [UINavigationControllers](/uwp/api/windows.gaming.input.uinavigationcontroller) (視情況)。 所有的 IHV 應為其相容的控制器新增此資料。 如此一來，所有的 UWP 遊戲 (及任何使用 WinRT API 的傳統型遊戲) 將能夠支援您的遊戲控制器。

## <a name="mapping-scheme"></a>對應配置

系統將從登錄中的這個位置讀取裝置與廠商識別碼 (VID) **VVVV**、產品識別碼 (PID) **PPPP**、使用量頁面 **UUUU** 及使用量識別碼 **XXXX** 的對應：

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\VVVVPPPPUUUUXXXX`

下表解釋裝置根位置下方的預期值：

<table>
    <tr>
        <th>名稱</th>
        <th>類型</th>
        <th>必要項？</th>
        <th>資訊</th>
    </tr>
    <tr>
        <td>Disabled</td>
        <td>DWORD</td>
        <td>否</td>
        <td>
            <p>指出此特定裝置應該停用。</p>
            <ul>
                <li><b>0</b>：未停用裝置。</li>
                <li><b>1</b>：裝置已停用。</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>說明</td>
        <td>REG_SZ <td>否</td>
        <td>裝置的簡短描述。</td>
    </tr>
</table>

您的裝置安裝程式應該將此資料新增至登錄 (透過安裝或 [INF 檔案](https://docs.microsoft.com/windows-hardware/drivers/install/inf-files))。

以下幾節將詳述裝置根位置下方的子機碼。

### <a name="gamepad"></a>遊戲台

下表列出 \[Gamepad\]**** 子機碼下方的必要與選擇性子機碼：

<table>
    <tr>
        <th>子機碼</th>
        <th>必要？</th>
        <th>資訊</th>
    </tr>
    <tr>
        <td>功能表</td>
        <td>是</td>
        <td rowspan="18" style="vertical-align: middle;">請參閱<a href="#button-mapping">按鈕對應</a></td>
    </tr>
    <tr>
        <td>檢視</td>
        <td>是</td>
    </tr>
    <tr>
        <td>A</td>
        <td>是</td>
    </tr>
    <tr>
        <td>B</td>
        <td>是</td>
    </tr>
    <tr>
        <td>X</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Y</td>
        <td>是</td>
    </tr>
    <tr>
        <td>LeftShoulder</td>
        <td>是</td>
    </tr>
    <tr>
        <td>RightShoulder</td>
        <td>是</td>
    </tr>
    <tr>
        <td>LeftThumbstickButton</td>
        <td>是</td>
    </tr>
    <tr>
        <td>RightThumbstickButton</td>
        <td>是</td>
    </tr>
    <tr>
        <td>DPadUp</td>
        <td>是</td>
    </tr>
    <tr>
        <td>DPadDown</td>
        <td>是</td>
    </tr>
    <tr>
        <td>DPadLeft</td>
        <td>是</td>
    </tr>
    <tr>
        <td>DPadRight</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Paddle1</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Paddle2</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Paddle3</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Paddle4</td>
        <td>否</td>
    </tr>
    <tr>
        <td>LeftTrigger</td>
        <td>是</td>
        <td rowspan="6" style="vertical-align: middle;">請參閱<a href="#axis-mapping">軸對應</a></td>
    </tr>
    <tr>
        <td>RightTrigger</td>
        <td>是</td>
    </tr>
    <tr>
        <td>LeftThumbstickX</td>
        <td>是</td>
    </tr>
    <tr>
        <td>LeftThumbstickY</td>
        <td>是</td>
    </tr>
    <tr>
        <td>RightThumbstickX</td>
        <td>是</td>
    </tr>
    <tr>
        <td>RightThumbstickY</td>
        <td>是</td>
    </tr>
</table>

> [!NOTE]
> 如果您將遊戲控制器新增成支援的 **Gamepad**，強烈建議您也將它新增成支援的 **UINavigationController**。

### <a name="racingwheel"></a>RacingWheel

下表列出 \[RacingWheel\]**** 子機碼下方的必要與選擇性子機碼：

<table>
    <tr>
        <th>子機碼</th>
        <th>必要？</th>
        <th>資訊</th>
    </tr>
    <tr>
        <td>PreviousGear</td>
        <td>是</td>
        <td rowspan="30" style="vertical-align: middle;">請參閱<a href="#button-mapping">按鈕對應</a></td>
    </tr>
    <tr>
        <td>NextGear</td>
        <td>是</td>
    </tr>
    <tr>
        <td>DPadUp</td>
        <td>否</td>
    </tr>
    <tr>
        <td>DPadDown</td>
        <td>否</td>
    </tr>
    <tr>
        <td>DPadLeft</td>
        <td>否</td>
    </tr>
    <tr>
        <td>DPadRight</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button1</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button2</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button3</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button4</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button5</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button6</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button7</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button8</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button10</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button11</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button12</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button13</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button14</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button15</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Button16</td>
        <td>否</td>
    </tr>
    <tr>
        <td>FirstGear</td>
        <td>否</td>
    </tr>
    <tr>
        <td>SecondGear</td>
        <td>否</td>
    </tr>
    <tr>
        <td>ThirdGear</td>
        <td>否</td>
    </tr>
    <tr>
        <td>FourthGear</td>
        <td>否</td>
    </tr>
    <tr>
        <td>FifthGear</td>
        <td>否</td>
    </tr>
    <tr>
        <td>SixthGear</td>
        <td>否</td>
    </tr>
    <tr>
        <td>SeventhGear</td>
        <td>否</td>
    </tr>
    <tr>
        <td>ReverseGear</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Wheel</td>
        <td>是</td>
        <td rowspan="5" style="vertical-align: middle;">請參閱<a href="#axis-mapping">軸對應</a></td>
    </tr>
    <tr>
        <td>節流</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Brake</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Clutch</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Handbrake</td>
        <td>否</td>
    </tr>
    <tr>
        <td>MaxWheelAngle</td>
        <td>是</td>
        <td>請參閱<a href="#properties-mapping">屬性對應</a></td>
    </tr>
</table>

### <a name="arcadestick"></a>ArcadeStick

下表列出 \[ArcadeStick\]**** 子機碼下方的必要與選擇性子機碼：

<table>
    <tr>
        <th>子機碼</th>
        <th>必要？</th>
        <th>資訊</th>
    </tr>
    <tr>
        <td>Action 1</td>
        <td>是</td>
        <td rowspan="12" style="vertical-align: middle;">請參閱<a href="#button-mapping">按鈕對應</a></td>
    </tr>
    <tr>
        <td>Action2</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Action3</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Action4</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Action5</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Action6</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Special1</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Special2</td>
        <td>是</td>
    </tr>
    <tr>
        <td>StickUp</td>
        <td>是</td>
    </tr>
    <tr>
        <td>StickDown</td>
        <td>是</td>
    </tr>
    <tr>
        <td>StickLeft</td>
        <td>是</td>
    </tr>
    <tr>
        <td>StickRight</td>
        <td>是</td>
    </tr>
</table>

### <a name="flightstick"></a>FlightStick

下表列出 \[FlightStick\]**** 子機碼下方的必要與選擇性子機碼：

<table>
    <tr>
        <th>子機碼</th>
        <th>必要？</th>
        <th>資訊</th>
    </tr>
    <tr>
        <td>FirePrimary</td>
        <td>是</td>
        <td rowspan="2" style="vertical-align: middle;">請參閱<a href="#button-mapping">按鈕對應</a></td>
    </tr>
    <tr>
        <td>FireSecondary</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Roll</td>
        <td>是</td>
        <td rowspan="4" style="vertical-align: middle;">請參閱<a href="#axis-mapping">軸對應</a></td>
    </tr>
    <tr>
        <td>音調</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Yaw</td>
        <td>是</td>
    </tr>
    <tr>
        <td>節流</td>
        <td>是</td>
    </tr>
    <tr>
        <td>HatSwitch</td>
        <td>是</td>
        <td>請參閱<a href="#switch-mapping">切換對應</a></td>
    </tr>
</table>

### <a name="uinavigation"></a>UINavigation

下表列出 \[UINavigation\]**** 子機碼下方的必要與選擇性子機碼：

<table>
    <tr>
        <th>子機碼</th>
        <th>必要？</th>
        <th>資訊</th>
    </tr>
    <tr>
        <td>功能表</td>
        <td>是</td>
        <td rowspan="24" style="vertical-align: middle;">請參閱<a href="#button-mapping">按鈕對應</a></td>
    </tr>
    <tr>
        <td>檢視</td>
        <td>是</td>
    </tr>
    <tr>
        <td>接受</td>
        <td>是</td>
    </tr>
    <tr>
        <td>取消</td>
        <td>是</td>
    </tr>
    <tr>
        <td>PrimaryUp</td>
        <td>是</td>
    </tr>
    <tr>
        <td>PrimaryDown</td>
        <td>是</td>
    </tr>
    <tr>
        <td>PrimaryLeft</td>
        <td>是</td>
    </tr>
    <tr>
        <td>PrimaryRight</td>
        <td>是</td>
    </tr>
    <tr>
        <td>Context1</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Context2</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Context3</td>
        <td>否</td>
    </tr>
    <tr>
        <td>Context4</td>
        <td>否</td>
    </tr>
    <tr>
        <td>PageUp</td>
        <td>否</td>
    </tr>
    <tr>
        <td>PageDown</td>
        <td>否</td>
    </tr>
    <tr>
        <td>PageLeft</td>
        <td>否</td>
    </tr>
    <tr>
        <td>PageRight</td>
        <td>否</td>
    </tr>
    <tr>
        <td>ScrollUp</td>
        <td>否</td>
    </tr>
    <tr>
        <td>ScrollDown</td>
        <td>否</td>
    </tr>
    <tr>
        <td>ScrollLeft</td>
        <td>否</td>
    </tr>
    <tr>
        <td>ScrollRight</td>
        <td>否</td>
    </tr>
    <tr>
        <td>SecondaryUp</td>
        <td>否</td>
    </tr>
    <tr>
        <td>SecondaryDown</td>
        <td>否</td>
    </tr>
    <tr>
        <td>SecondaryLeft</td>
        <td>否</td>
    </tr>
    <tr>
        <td>SecondaryRight</td>
        <td>否</td>
    </tr>
</table>

如需有關 UI 瀏覽控制器及上述命令的詳細資訊，請參閱 [UI 瀏覽控制器](https://docs.microsoft.com/windows/uwp/gaming/ui-navigation-controller)。

## <a name="keys"></a>按鍵

以下幾節說明 \[Gamepad\]****、\[RacingWheel\]****、\[ArcadeStick\]****、\[FlightStick\]**** 及 \[UINavigation\]**** 機碼下方每個子機碼的內容。

### <a name="button-mapping"></a>按鈕對應

下表列出必須對應按鈕的值。 例如，如果按下遊戲控制器上的 \[DPadUp\]****，\[DPadUp\]**** 的對應則應包含 \[ButtonIndex\]**** 值 (\[Source\]**** 為 \[Button\]****)。 如果 \[DPadUp\]**** 必須從切換位置對應，\[DPadUp\]**** 對應則應包含 \[SwitchIndex\]**** 與 \[SwitchPosition\]**** 值 (\[Source\]**** 為 \[Switch\]****)。

<table>
    <tr>
        <th>來源</th>
        <th>值名稱</th>
        <th>值類型</th>
        <th>必要？</th>
        <th>值資訊</th>
    </tr>
    <tr>
        <td>按鈕</td>
        <td>ButtonIndex</td>
        <td>DWORD</td>
        <td>是</td>
        <td><b>RawGameController</b> 按鈕陣列中的索引。</td>
    </tr>
    <tr>
        <td rowspan="4" style="vertical-align: middle;">軸</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>是</td>
        <td><b>RawGameController</b> 軸陣列中的索引。</td>
    </tr>
    <tr>
        <td>Invert</td>
        <td>DWORD</td>
        <td>否</td>
        <td>表示在套用 <b>Threshold Percent</b> 與 <b>DebouncePercent</b> 因素前應反轉軸值。</td>
    </tr>
    <tr>
        <td>ThresholdPercent</td>
        <td>DWORD</td>
        <td>是</td>
        <td>表示對應的按鈕值在按下與放開狀態之間轉換的軸位置。 有效的值範圍為 0 到 100。 如果軸值大於或等於此值，則按鈕視為按下。</td>
    </tr>
    <tr>
        <td>DebouncePercent</td>
        <td>DWORD</td>
        <td>是</td>
        <td>
            <p>定義約為 <b>ThresholdPercent</b> 值的視窗大小，用於防反彈回報的按鈕狀態。 有效的值範圍為 0 到 100。 只有在軸值超過防反彈視窗的右界限或下界限時，才會發生按鈕值轉換。 例如，若 <b>ThresholdPercent</b> 為 50，且 <b>DebouncePercent</b> 為 10，便會造成防反彈界限為完整範圍軸值的 45% 與 55%。 在軸值到達 55% 或以上之前，按鈕無法轉換為按下的狀態，在軸值到達 45% 或以下之前，按鈕無法轉換回放開的狀態。</p>
            <p>計算出的反防彈視窗界限限制在 0% 到 100% 之間。 例如，5% 的閾值與 20% 的反防彈視窗，會造成防反彈視窗界限為 0% 與 15%。 系統一律將軸值為 0% 與 100% 的按鈕狀態各自回報為放開或按下，無論閾值與防反彈值為何。</p>
        </td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">參數</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>是</td>
        <td><b>RawGameController</b> 切換陣列中的索引。</td>
    </tr>
    <tr>
        <td>SwitchPosition</td>
        <td>REG_SZ</td>
        <td>是</td>
        <td>
            <p>指出將造成對應的按鈕回報其為已按下中的切換位置。 位置值可以是以下其中一個字串：</p>
            <ul>
                <li>Up</li>
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Down</li>
                <li>DownLeft</li>
                <li>Left</li>
                <li>UpLeft</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>否</td>
        <td>表示相鄰的切換位置也將造成對應的按鈕回報其為已按下中。</td>
    </tr>
</table>

### <a name="axis-mapping"></a>軸對應

下表列出必須對應軸的值：

<table>
    <tr>
        <th>來源</th>
        <th>值名稱</th>
        <th>值類型</th>
        <th>必要？</th>
        <th>值資訊</th>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">按鈕</td>
        <td>MaxValueButtonIndex</td>
        <td>DWORD</td>
        <td>是</td>
        <td>
            <p><b>RawGameController</b> 按鈕中的索引，會轉譯成對應的單向軸值。</p>
            <table>
                <tr>
                    <th>MaxButton</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>false</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>1.0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>MinValueButtonIndex</td>
        <td>DWORD</td>
        <td>否</td>
        <td>
            <p>指示對應的值為雙向。 <b>MaxButton</b> 與 <b>MinButton</b> 的值會結合成單一雙向值，如下所示。</p>
            <table>
                <tr>
                    <th>MinButton</th>
                    <th>MaxButton</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>false</td>
                    <td>false</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>false</td>
                    <td>TRUE</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>true</td>
                    <td>false</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>TRUE</td>
                    <td>0.5</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">軸</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>是</td>
        <td><b>RawGameController</b> 軸陣列中的索引。</td>
    </tr>
    <tr>
        <td>Invert</td>
        <td>DWORD</td>
        <td>否</td>
        <td>表示對應的軸值在傳回之前應先反轉。</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">參數</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>是</td>
        <td><b>RawGameController</b> 切換陣列中的索引。
    </tr>
    <tr>
        <td>MaxValueSwitchPosition</td>
        <td>REG_SZ</td>
        <td>是</td>
        <td>
            <p>下列其中一個字串：</p>
            <ul>
                <li>Up</li>
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Down</li>
                <li>DownLeft</li>
                <li>Left</li>
                <li>UpLeft</li>
            </ul>
            <p>這表示會造成將對應的軸值回報為 1.0 的切換位置。 <b>MaxValueSwitchPosition</b> 的相反方向視為 0.0。 例如，如果 <b>MaxValueSwitchPosition</b> 為 <b>Up</b>，則軸值轉譯如下所示：</p>
            <table>
                <tr>
                    <th>切換位置</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>Center</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>0.0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>否</td>
        <td>
            <p>表示相鄰的切換位置也將造成對應的軸回報 1.0。 在上述範例中，如果有設定 <b>IncludeAdjacent</b>，則會如下所示進行軸轉譯：</p>
            <table>
                <tr>
                    <th>切換位置</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>UpRight</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>UpLeft</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>Center</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>0.0</td>
                </tr>
            </table>
        </td>
    </tr>
</table>

### <a name="switch-mapping"></a>切換對應

可從 \[RawGameController\]**** 其按鈕陣列中的一組按鈕或從切換陣列中的索引來對應切換位置。 無法從軸對應切換位置。

<table>
    <tr>
        <th>來源</th>
        <th>值名稱</th>
        <th>值類型</th>
        <th>值資訊</th>
    </tr>
    <tr>
        <td rowspan="10" style="vertical-align: middle;">按鈕</td>
        <td>ButtonCount</td>
        <td>DWORD</td>
        <td>2、4 或 8</td>
    </tr>
    <tr>
        <td>SwitchKind</td>
        <td>REG_SZ</td>
        <td><b>TwoWay</b>、 <b>FourWay</b>或 <b>EightWay</b>
    </tr>
    <tr>
        <td>UpButtonIndex</td>
        <td>DWORD</td>
        <td rowspan="8" style="vertical-align: middle;">請參閱 <a href="#buttonindex-values">*ButtonIndex 值</a></td>
    </tr>
    <tr>
        <td>DownButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>LeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>RightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>UpRightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>DownRightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>DownLeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>UpLeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="9" style="vertical-align: middle;">軸</td>
        <td>SwitchKind</td>
        <td>REG_SZ</td>
        <td><b>TwoWay</b>、<b>FourWay</b> 或 <b>EightWay</b></td>
    </tr>
    <tr>
        <td>XAxisIndex</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;"><b>YAxisIndex</b> 一律會出現。 <b>XAxisIndex</b> 只會在 <b>SwitchKind</b> 為 <b>FourWay</b> 或 <b>EightWay</b> 時出現。</td>
    </tr>
    <tr>
        <td>YAxisIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDeadZonePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">表示軸中心位置周圍的靜止區域大小。</td>
    </tr>
    <tr>
        <td>YDeadZonePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDebouncePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">定義約為靜止區域上限與下限的視窗大小，這些限制用於反防彈回報的切換狀態。</td>
    </tr>
    <tr>
        <td>YDebouncePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XInvert</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">表示在套用靜止區域與防反彈視窗計算之前應先反轉對應的軸值。</td>
    </tr>
    <tr>
        <td>YInvert</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">參數</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td><b>RawGameController</b> 切換陣列中的索引。
    </tr>
    <tr>
        <td>Invert</td>
        <td>DWORD</td>
        <td>表示切換會以逆時針順序回報其位置，而非以預設的順時針順序回報。</td>
    </tr>
    <tr>
        <td>PositionBias</td>
        <td>DWORD</td>
        <td>
            <p>轉移如何以指定的數量回報位置的起點。 <b>PositionBias</b> 一律從原始起點以順時針方向計算，並會在反轉值順序之前套用。</p>
            <p>例如，可透過設定 <b>Invert</b> 旗標並指定 <b>PositionBias</b> 為 5，將從 <b>DownRight</b> 開始以逆時針順序回報位置的切換標準化：</p>
            <table>
                <tr>
                    <th>位置</th>
                    <th>回報的值</th>
                    <th>在 PositionBias 與 Invert 旗標之後</th>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0</td>
                    <td>3</td>
                </tr>
                <tr>
                    <td>Right</td>
                    <td>1</td>
                    <td>2</td>
                </tr>
                <tr>
                    <td>UpRight</td>
                    <td>2</td>
                    <td>1</td>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>3</td>
                    <td>0</td>
                </tr>
                <tr>
                    <td>UpLeft</td>
                    <td>4</td>
                    <td>7</td>
                </tr>
                <tr>
                    <td>Left</td>
                    <td>5</td>
                    <td>6</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>6</td>
                    <td>5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>7</td>
                    <td>4</td>
                </tr>
            </table>
    </tr>
</table>

#### <a name="buttonindex-values"></a>*ButtonIndex 值

\*ButtonIndex **RawGameController**的按鈕陣列中的值索引：

<table>
    <tr>
        <th>ButtonCount</th>
        <th>SwitchKind</th>
        <th>RequiredMappings</th>
    </tr>
    <tr>
        <td>2</td>
        <td>TwoWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>FourWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>EightWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>8</td>
        <td>EightWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
                <li>UpRightButtonIndex</li>
                <li>DownRightButtonIndex</li>
                <li>DownLeftButtonIndex</li>
                <li>UpLeftButtonIndex</li>
            </ul>
        </td>
    </tr>
</table>

### <a name="properties-mapping"></a>屬性對應

這些是不同對應類型的靜態對應值。

<table>
    <tr>
        <th>對應</th>
        <th>值名稱</th>
        <th>值類型</th>
        <th>值資訊</th>
    </tr>
    <tr>
        <td>RacingWheel</td>
        <td>MaxWheelAngle</td>
        <td>DWORD</td>
        <td>表示方向盤以單向支援的實體方向盤角度上限。 例如，可旋轉 -90 度到 90 度的方向盤會指定 90。</td>
    </tr>
</table>

## <a name="labels"></a>標籤

標籤應出現在裝置根位置下方 \[Labels\]**** 鍵值之下。 \[Labels\]**** 會有 3 個子機碼：\[Buttons\]****、\[Axes\]**** 及 \[Switches\]****。

### <a name="button-labels"></a>按鈕標籤

\[Buttons\]**** 機碼會將 \[RawGameController\]**** 其按鈕陣列中的每個按鈕位置對應至字串。 系統會在內部將每個字串對應至對應的 [GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) 列舉值。 例如，如果遊戲台上有十個按鈕，且 **RawGameController** 剖析出按鈕並在按鈕報告中呈現按鈕的順序為：

```cpp
Menu,               // Index 0
View,               // Index 1
LeftStickButton,    // Index 2
RightStickButton,   // Index 3
LetterA,            // Index 4
LetterB,            // Index 5
LetterX,            // Index 6
LetterY,            // Index 7
LeftBumper,         // Index 8
RightBumper         // Index 9
```

標籤應在 \[Buttons\]**** 機碼下方以此順序顯示：

<table>
    <tr>
        <th>Name</th>
        <th>值 (類型：REG_SZ)</th>
    </tr>
    <tr>
        <td>Button0</td>
        <td>功能表</td>
    </tr>
    <tr>
        <td>Button1</td>
        <td>檢視</td>
    </tr>
    <tr>
        <td>Button2</td>
        <td>LeftStickButton</td>
    </tr>
    <tr>
        <td>Button3</td>
        <td>RightStickButton</td>
    </tr>
    <tr>
        <td>Button4</td>
        <td>LetterA</td>
    </tr>
    <tr>
        <td>Button5</td>
        <td>LetterB</td>
    </tr>
    <tr>
        <td>Button6</td>
        <td>LetterX</td>
    </tr>
    <tr>
        <td>Button7</td>
        <td>LetterY</td>
    </tr>
    <tr>
        <td>Button8</td>
        <td>LeftBumper</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>RightBumper</td>
    </tr>
</table>

### <a name="axis-labels"></a>軸標籤

\[Axes\]**** 機碼會將 \[RawGameController\]**** 其軸陣列中的每個軸位置對應至在 [GameControllerButtonLabel 列舉](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) (英文) 中所列的其中一個標籤，就如同按鈕標籤。 請參閱[按鈕標籤](#button-labels)中的範例。

### <a name="switch-labels"></a>切換標籤

\[Switches\]**** 機碼會將切換位置對應至標籤。 這些值遵循此命令慣例：若要將其索引在 \[RawGameController\]**** 的切換陣列中為 \[x\]** 的切換位置設定標籤，則在 \[Switches\]**** 子機碼下方新增這些值：

* SwitchxUp
* SwitchxUpRight
* SwitchxRight
* SwitchxDownRight
* SwitchxDown
* SwitchxDownLeft
* SwitchxUpLeft
* SwitchxLeft

下表顯示 4 向切換其切換位置的一組範例標籤，該切換會在 \[RawGameController\]**** 的索引 0 處顯示：

<table>
    <tr>
        <th>Name</th>
        <th>值 (類型：REG_SZ)</th>
    </tr>
    <tr>
        <td>Switch0Up</td>
        <td>XboxUp</td>
    </tr>
    <tr>
        <td>Switch0Right</td>
        <td>XboxRight</td>
    </tr>
    <tr>
        <td>Switch0Down</td>
        <td>XboxDown</td>
    </tr>
    <tr>
        <td>Switch0Left</td>
        <td>XboxLeft</td>
    </tr>
</table>

<!--### Label strings

* XboxBack
* XboxStart
* XboxMenu
* XboxView
* XboxUp
* XboxDown
* XboxLeft
* XboxRight
* XboxA
* XboxB
* XboxX
* XboxY
* XboxLeftBumper
* XboxLeftTrigger
* XboxLeftStickButton
* XboxRightBumper
* XboxRightTrigger
* XboxRightStickButton
* XboxPaddle1
* XboxPaddle2
* XboxPaddle3
* XboxPaddle4
* Mode
* Select
* Menu
* View
* Back
* Start
* Options
* Share
* Up
* Down
* Left
* Right
* LetterA
* LetterB
* LetterC
* LetterL
* LetterR
* LetterX
* LetterY
* LetterZ
* Cross
* Circle
* Square
* Triangle
* LeftBumper
* LeftTrigger
* LeftStickButton
* Left1
* Left2
* Left3
* RightBumper
* RightTrigger
* RightStickButton
* Right1
* Right2
* Right3
* Paddle1
* Paddle2
* Paddle3
* Paddle4
* Plus
* Minus
* DownLeftArrow
* DialLeft
* DialRight
* Suspension-->

## <a name="example-registry-file"></a>範例登錄檔案

為了顯示這些對應與值如何全部集結在一起，以下是一般 \[RacingWheel\]**** 的範例登錄檔案：

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004]
"Description" = "Example Wheel Device"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\Labels\Buttons]
"Button0" = "LetterA"
"Button1" = "LetterB"
"Button2" = "LetterX"
"Button3" = "LetterY"
"Button6" = "Menu"
"Button7" = "View"
"Button8" = "RightStickButton"
"Button9" = "LeftStickButton"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\Labels\Switches]
"Switch0Down" = "Down"
"Switch0Left" = "Left"
"Switch0Right" = "Right"
"Switch0Up" = "Up"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel]
"MaxWheelAngle" = dword:000001c2

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Brake]
"AxisIndex" = dword:00000002
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button1]
"ButtonIndex" = dword:00000000

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button2]
"ButtonIndex" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button3]
"ButtonIndex" = dword:00000002

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button4]
"ButtonIndex" = dword:00000003

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button5]
"ButtonIndex" = dword:00000009

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button6]
"ButtonIndex" = dword:00000008

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button7]
"ButtonIndex" = dword:00000007

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button8]
"ButtonIndex" = dword:00000006

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Clutch]
"AxisIndex" = dword:00000003
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadDown]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Down"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadLeft]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Left"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadRight]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Right"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadUp]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Up"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FifthGear]
"ButtonIndex" = dword:00000010

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FirstGear]
"ButtonIndex" = dword:0000000c

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FourthGear]
"ButtonIndex" = dword:0000000f

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\NextGear]
"ButtonIndex" = dword:00000004

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\PreviousGear]
"ButtonIndex" = dword:00000005

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\ReverseGear]
"ButtonIndex" = dword:0000000b

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\SecondGear]
"ButtonIndex" = dword:0000000d

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\SixthGear]
"ButtonIndex" = dword:00000011

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\ThirdGear]
"ButtonIndex" = dword:0000000e

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Throttle]
"AxisIndex" = dword:00000001
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Wheel]
"AxisIndex" = dword:00000000
"Invert" = dword:00000000
```

## <a name="see-also"></a>另請參閱

* [Windows. 輸入命名空間](/uwp/api/windows.gaming.input)
* [Windows. 輸入. 自訂命名空間](/uwp/api/windows.gaming.input.custom)
* [INF 檔案](/windows-hardware/drivers/install/inf-files)