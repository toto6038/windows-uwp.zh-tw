---
author: JordanRh1
title: 啟用使用者模式存取 GPIO、I2C 和 SPI
description: 本教學課程描述如何在 Windows 10 上以使用者模式存取 GPIO、I2C、SPI 及 UART。
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, acpi, gpio, i2c, spi, uefi
ms.assetid: 2fbdfc78-3a43-4828-ae55-fd3789da7b34
ms.localizationpriority: medium
ms.openlocfilehash: 09957c19414f586a49a1a2cb9186aa027dc1de07
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5825863"
---
# <a name="enable-usermode-access-to-gpio-i2c-and-spi"></a>啟用使用者模式存取 GPIO、I2C 和 SPI



Windows 10 包含可透過使用者模式直接存取 GPIO、I2C、SPI 和 UART 的新 API。 諸如 Raspberry Pi 2 等開發板會公開這些連線的子集，讓使用者能夠使用自訂電路系統延伸基本運算模組，以處理特定應用程式。 這些低階匯流排通常會與其他重要的內建功能共用，包含一部分在排針上公開的 GPIO 針腳與匯流排。 若要保留系統穩定性，必須指定哪些針腳與匯流排可由使用者模式應用程式安全地修改。 

本文件說明如何在 ACPI 中指定這項設定，並提供工具以驗證是否正確指定設定。 

> [!IMPORTANT]
> 本文件適用對象為 UEFI 和 ACPI 開發人員。 假設這些人員對於 ACPI、ASL 編撰和 SpbCx/GpioClx 已具部分熟悉度。

透過現有的 `GpioClx` 與 `SpbCx` 架構，連接 Windows 上的低階匯流排使用者模式存取。 新驅動程式稱為 *RhProxy*，適用於 Windows IoT 核心版和 Windows 企業版，且會針對使用者模式公開 `GpioClx` 與 `SpbCx` 資源。 若要啟用 API，您必須在 ACPI 表格中宣告 rhproxy 的裝置節點，且應將所有 GPIO 與 SPB 資源對使用者模式公開。 本文件會逐步解說關於編寫與驗證 ASL 的資訊。 


## <a name="asl-by-example"></a>以 ASL 為例

讓我們逐步解說 Raspberry Pi 2 上的 rhproxy 裝置節點宣告。 首先，在 \\_SB scope 中建立 ACPI 裝置宣告。  

```cpp
Device(RHPX) 
{ 
    Name(_HID, "MSFT8000") 
    Name(_CID, "MSFT8000") 
    Name(_UID, 1) 
    
```

* _HID – Hardware Id。將此項設定為廠商特定硬體識別碼。 
* _CID – Compatible Id。必須是「MSFT8000」。  
* _UID – Unique Id。設為 1。  

接下來我們會宣告應對使用者模式公開的所有 GPIO 與 SPB 資源。 資源宣告的順序非常重要，因為系統會使用資源索引將屬性與資源產生關聯。 若公開多個 I2C 或 SPI 匯流排，則系統會將第一個宣告的匯流排視為該類型的「預設」匯流排，且其將為 [Windows.Devices.I2c.I2cController](https://msdn.microsoft.com/library/windows/apps/windows.devices.i2c.i2ccontroller.aspx) 與 [Windows.Devices.Spi.SpiController](https://msdn.microsoft.com/library/windows/apps/windows.devices.spi.spicontroller.aspx) 之 `GetDefaultAsync()` 方法傳回的執行個體。 

### <a name="spi"></a>SPI 

Raspberry Pi 具有兩個公開的 SPI 匯流排。 SPI0 具有兩個硬體晶片選擇線路，而 SPI1 具有一個硬體晶片選擇線路。 每個匯流排的每個晶片選擇線路，皆需要一個 SPISerialBus() 資源宣告。 下列兩個 SPISerialBus 資源宣告，適用於 SPI0 的兩個晶片選擇線路。 DeviceSelection 欄位包含唯一值，而驅動程式會將其解譯為硬體晶片選擇線路識別碼。 您放置於 DeviceSelection 欄位的精確值，取決於驅動程式如何解譯 ACPI 連線描述元的此欄位。  

```cpp
// Index 0 
SPISerialBus(              // SCKL - GPIO 11 - Pin 23 
                           // MOSI - GPIO 10 - Pin 19 
                           // MISO - GPIO 9  - Pin 21 
                           // CE0  - GPIO 8  - Pin 24 
    0,                     // Device selection (CE0) 
    PolarityLow,           // Device selection polarity 
    FourWireMode,          // wiremode 
    0,                     // databit len: placeholder 
    ControllerInitiated,   // slave mode 
    0,                     // connection speed: placeholder 
    ClockPolarityLow,      // clock polarity: placeholder 
    ClockPhaseFirst,       // clock phase: placeholder 
    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name 
    0,                     // ResourceSourceIndex 
                           // Resource usage 
    )                      // Vendor Data 

// Index 1 
SPISerialBus(              // SCKL - GPIO 11 - Pin 23 
                           // MOSI - GPIO 10 - Pin 19 
                           // MISO - GPIO 9  - Pin 21 
                           // CE1  - GPIO 7  - Pin 26 
    1,                     // Device selection (CE1) 
    PolarityLow,           // Device selection polarity 
    FourWireMode,          // wiremode 
    0,                     // databit len: placeholder 
    ControllerInitiated,   // slave mode 
    0,                     // connection speed: placeholder 
    ClockPolarityLow,      // clock polarity: placeholder 
    ClockPhaseFirst,       // clock phase: placeholder 
    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name 
    0,                     // ResourceSourceIndex 
                           // Resource usage 
    )                      // Vendor Data 

```

軟體如何知道這兩個資源應與相同匯流排建立關聯？ 在 DSD 中指定匯流排易記名稱與資源索引之間的對應：  

```cpp
Package(2) { "bus-SPI-SPI0", Package() { 0, 1 }}, 
```

這會建立名為 “SPI0” 的匯流排，包含兩個晶片選擇線路 – 資源索引 0 和 1。 必須另外使用數個屬性，以宣告 SPI 匯流排的各種功能。  

```cpp
Package(2) { "SPI0-MinClockInHz", 7629 }, 
Package(2) { "SPI0-MaxClockInHz", 125000000 },
```

**MinClockInHz** 和 **MaxClockInHz** 屬性會指定控制器支援的最小與最大時脈速度。 API 會防止使用者指定超出此範圍以外的值。 時脈速度會傳遞至連線描述元 (ACPI 區段 6.4.3.8.2.2) 之 _SPE 欄位中的 SPB 驅動程式。  

```cpp
Package(2) { "SPI0-SupportedDataBitLengths", Package() { 8 }}, 
```

**SupportedDataBitLengths** 屬性會列出控制器支援的資料位元長度。 可在以逗號分隔的清單中指定多個值。 API 會防止使用者指定此清單以外的值。 資料位元長度會傳遞至連線描述元 (ACPI 區段 6.4.3.8.2.2) 之 _LEN 欄位中的 SPB 驅動程式。  

您可以將這些資源宣告為「範本」。 某些欄位在系統開機時是固定的，而其他欄位則會於執行階段動態指定。 SPISerialBus 描述元的下列欄位會固定︰ 

* DeviceSelection 
* DeviceSelectionPolarity 
* WireMode 
* SlaveMode 
* ResourceSource 

下列欄位為預留位置，適用於使用者在執行階段指定的值： 

* DataBitLength 
* ConnectionSpeed 
* ClockPolarity 
* ClockPhase 

由於 SPI1 僅包含單晶片選擇線路，因此會宣告單一 `SPISerialBus()` 資源： 

```cpp
// Index 2 

SPISerialBus(              // SCKL - GPIO 21 - Pin 40 
                           // MOSI - GPIO 20 - Pin 38 
                           // MISO - GPIO 19 - Pin 35 
                           // CE1  - GPIO 17 - Pin 11 
    1,                     // Device selection (CE1) 
    PolarityLow,           // Device selection polarity 
    FourWireMode,          // wiremode 
    0,                     // databit len: placeholder 
    ControllerInitiated,   // slave mode 
    0,                     // connection speed: placeholder 
    ClockPolarityLow,      // clock polarity: placeholder 
    ClockPhaseFirst,       // clock phase: placeholder 
    "\\_SB.SPI1",          // ResourceSource: SPI bus controller name 
    0,                     // ResourceSourceIndex 
                           // Resource usage 
    )                      // Vendor Data 

```

在 DSD 中指定隨附的易記名稱宣告 (必要項目)，且會參照此資源宣告的索引。 

```cpp
Package(2) { "bus-SPI-SPI1", Package() { 2 }}, 
```

這會建立名為 “SPI1” 的匯流排，並將它與資源索引 2 建立關聯。  

#### <a name="spi-driver-requirements"></a>SPI 驅動程式需求 

* 必須使用 `SpbCx` 或具有 SpbCx 相容性 
* 必須通過 [MITT SPI 測試](https://msdn.microsoft.com/library/windows/hardware/dn919873.aspx)
* 必須支援 4Mhz 時脈速度 
* 必須支援 8 位元資料長度 
* 必須支援所有 SPI 模式︰0、1、2、3 

### <a name="i2c"></a>I2C 

接下來我們會宣告 I2C 資源。 Raspberry Pi 會在針腳 3 和 5 上公開單一 I2C 匯流排。 

```cpp
// Index 3 
I2CSerialBus(              // Pin 3 (GPIO2, SDA1), 5 (GPIO3, SCL1) 
    0xFFFF,                // SlaveAddress: placeholder 
    ,                      // SlaveMode: default to ControllerInitiated 
    0,                     // ConnectionSpeed: placeholder 
    ,                      // Addressing Mode: placeholder 
    "\\_SB.I2C1",          // ResourceSource: I2C bus controller name 
    , 
    , 
    )                      // VendorData 

```

在 DSD 中指定隨附的易記名稱宣告 (必要項目)： 

```cpp
Package(2) { "bus-I2C-I2C1", Package() { 3 }}, 
```

這會宣告具有易記名稱 “I2C1” 的 I2C 匯流排，它會參照資源索引 3，這是我們先前所宣告的 I2CSerialBus() 資源索引。 

I2CSerialBus() 描述元的下列欄位會固定： 

* SlaveMode 
* ResourceSource 

下列欄位為預留位置，適用於使用者在執行階段指定的值。 

* SlaveAddress 
* ConnectionSpeed 
* AddressingMode 

#### <a name="i2c-driver-requirements"></a>I2C 驅動程式需求 

* 必須使用 SpbCx 或具有 SpbCx 相容性 
* 必須通過 [MITT I2C 測試](https://msdn.microsoft.com/library/windows/hardware/dn919852.aspx) 
* 必須支援 7 位元定址 
* 必須支援 100kHz 時脈速度 
* 必須支援 400kHz 時脈速度 

### <a name="gpio"></a>GPIO 

接下來，我們會宣告對使用者模式公開的所有 GPIO 針腳。 我們提供下列指導方針，以協助您決定要公開的針腳： 

* 在已公開的排針上宣告所有的針腳。 
* 宣告連線至按鈕和 LED 等實用內建功能的針腳。 
* 不要宣告已保留供系統功能用途，或是未連線至任何功能的針腳。 

下列 ASL 區塊會宣告兩個針腳 – GPIO4 與 GPIO5。 為求簡明起見，此處不會顯示其他針腳。 「附錄 C」包含可用於產生 GPIO 資源的範例 PowerShell 指令碼。 

```cpp
// Index 4 – GPIO 4 
GpioIO(Shared, PullUp, , , , “\\_SB.GPI0”, , , , ) { 4 } 
GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, “\\_SB.GPI0”,) { 4 } 

// Index 6 – GPIO 5 
GpioIO(Shared, PullUp, , , , “\\_SB.GPI0”, , , , ) { 5 } 
GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, “\\_SB.GPI0”,) { 5 } 
```

宣告 GPIO 針腳時，必須遵守下列需求︰ 

* 僅支援記憶體對應的 GPIO 控制器。 不支援透過 I2C/SPI 連接介面的 GPIO 控制器。 控制器驅動程式若在 [CLIENT_CONTROLLER_BASIC_INFORMATION](https://msdn.microsoft.com/library/windows/hardware/hh439358.aspx) 架構中設定 [MemoryMappedController](https://msdn.microsoft.com/library/windows/hardware/hh439449.aspx) 旗標來回應 [CLIENT_QueryControllerBasicInformation](https://msdn.microsoft.com/library/windows/hardware/hh439399.aspx) 回呼，則它是記憶體對應控制器。 
* 每個針腳皆需要 GpioIO 和 GpioInt 資源。 GpioInt 資源必須緊接著 GpioIO 資源，且必須參考相同的針腳編號。 
* GPIO 資源必須依照遞增針腳編號排序。 
* 每個 GpioIO 和 GpioInt 資源在針腳清單中，皆必須僅包含一個完整針腳編號。 
* 所有描述元的 ShareType 欄位皆必須為 Shared 
* GpioInt 描述元的 EdgeLevel 欄位必須為 Edge 
* GpioInt 描述元的 ActiveLevel 欄位必須為 ActiveBoth 
* PinConfig 欄位 
  * 在 GpioIO 與 GpioInt 描述元中必須相同 
  * 必須為 PullUp、PullDown 或 PullNone 的其中之一。 不能是 PullDefault。
  * 提取設定必須符合針腳的電源開啟狀態。 自電源開啟狀態在指定提取模式中放置針腳時，不得變更針腳的狀態。 例如，若資料工作表指定針腳提供上拉，請將 PinConfig 指定為 PullUp。  

韌體、UEFI 和驅動程式初始化程式碼不應於開機期間變更針腳的電源開啟狀態。 只有使用者才知道連結至針腳的項目，以及哪些狀態轉換安全無虞。 必須列出每個針腳的電源開啟狀態，以讓使用者能夠設計透過針腳正確連接介面的硬體。 針腳不得在開機期間意外變更狀態。  

#### <a name="supported-drive-modes"></a>支援的驅動模式 

若 GPIO 控制器除了高阻抗輸入與 CMOS 輸出外還支援內建上拉與下拉電阻，則您必須透過選用的 SupportedDriveModes 屬性指定此項目。 

```cpp
Package (2) { “GPIO-SupportedDriveModes”, 0xf }, 
```

SupportedDriveModes 屬性會指出 GPIO 控制器支援的驅動模式。 在上述範例中，支援下列所有的驅動模式。 此屬性為下列值的位元遮罩： 

| 旗標值 | 驅動模式 | 說明 |
|------------|------------|-------------|
| 0x1        | InputHighImpedance | 針腳支援高阻抗輸入，其會對應 ACPI 中的 “PullNone” 值。 |
| 0x2        | InputPullUp | 針腳支援內建上拉電阻，其會對應 ACPI 中的 “PullUp” 值。 |
| 0x4        | InputPullDown | 針腳支援內建下拉電阻，其會對應 ACPI 中的 “PullDown” 值。 |
| 0x8        | OutputCmos | 針腳支援產生極 High 與極 Low (與漏極開路相反)。 |

絕大部分的 GPIO 控制器皆支援 InputHighImpedance 與 OutputCmos。 若未指定 SupportedDriveModes 屬性，則此為預設值。 

若 GPIO 訊號在傳到已公開的排針前會通過電壓位準移位器，則即使無法在外部排針上觀察驅動模式，亦會宣告 SOC 支援驅動模式。 例如，若針腳通過雙向電壓位準移位器而將針腳顯示為具有上拉電阻的漏極開路，則即使針腳已設為高阻抗輸入，您也絕不會在已公開的排針上看到高阻抗狀態。 您仍應宣告針腳支援高阻抗輸入。 

#### <a name="pin-numbering"></a>針腳編號 

Windows 支援兩種針腳編號配置︰ 

* 連續針腳編號 – 使用者可見的數字 0、1、2...至多不超過已公開針腳的數目。 0 為 ASL 中宣告的第一個 GpioIo 資源，1 為 ASL 中宣告的第二個 GpioIo 資源，以下類推。 
* 原生針腳編號 – 使用者會看見在 GpioIo 描述元中指定的針腳編號，例如 4、5、12、13...。  

```cpp
Package (2) { “GPIO-UseDescriptorPinNumbers”, 1 }, 
```

**UseDescriptorPinNumbers** 屬性會告知 Windows 使用原生針腳編號而非序列式針腳編號。 若未指定 UseDescriptorPinNumbers 屬性或其值為零，Windows 會預設使用序列式針腳編號。 

若使用原生針腳編號，則您亦必須指定 **PinCount** 屬性。 

```cpp
Package (2) { “GPIO-PinCount”, 54 }, 
```

**PinCount** 屬性應符合透過 `GpioClx` 驅動程式 [CLIENT_QueryControllerBasicInformation](https://msdn.microsoft.com/library/windows/hardware/hh439399.aspx) 回呼中之 **TotalPins** 屬性所傳回的值。 

選擇與您電路板現有發佈文件最相容的編號配置。 例如 Raspberry Pi 會使用原生的針腳編號，這是因為許多現有的針腳輸出電路圖使用 BCM2835 針腳編號。 MinnowBoardMax 會使用序列式針腳編號，這是因為其中使用的現有針腳輸出電路圖極少，且由於在超過 200 個以上的針腳中僅會公開 10 個針腳，因此使用序列式針腳編號可精簡開發人員的使用體驗。 應以減輕開發人員工作負擔，做為決定使用序列式或原生針腳編號的關鍵。 

#### <a name="gpio-driver-requirements"></a>GPIO 驅動程式需求 

* 必須使用 `GpioClx`
* 必須為 SOC 記憶體對應 
* 必須使用模擬的 ActiveBoth 中斷處理 

### <a name="uart"></a>UART 

如果您的 UART 驅動程式使用 `SerCx` 或 `SerCx2`，您可以使用 rhproxy 向使用者模式公開驅動程式。 建立類型 `GUID_DEVINTERFACE_COMPORT` 之裝置介面的 UART 驅動程式不需要使用 rhproxy。 收件匣 `Serial.sys` 驅動程式是這些案例之一。

若要向使用者模式公開 `SerCx` 樣式 UART，請宣告 `UARTSerialBus` 資源，如下所示。

```cpp
// Index 2 
UARTSerialBus(           // Pin 17, 19 of JP1, for SIO_UART2 
    115200,                // InitialBaudRate: in bits ber second 
    ,                      // BitsPerByte: default to 8 bits 
    ,                      // StopBits: Defaults to one bit 
    0xfc,                  // LinesInUse: 8 1-bit flags to declare line enabled 
    ,                      // IsBigEndian: default to LittleEndian 
    ,                      // Parity: Defaults to no parity 
    ,                      // FlowControl: Defaults to no flow control 
    32,                    // ReceiveBufferSize 
    32,                    // TransmitBufferSize 
    "\\_SB.URT2",          // ResourceSource: UART bus controller name 
    , 
    , 
    , 
    )
```

僅在其他所有欄位皆為使用者於執行階段指定值的預留位置時，才會固定 ResourceSource 欄位。 

隨附的易記名稱宣告為︰ 

```cpp
Package(2) { "bus-UART-UART2", Package() { 2 }}, 
```

這會為控制器指派易記名稱 “UART2”，這是使用者將從使用者模式用來存取匯流排的識別碼。  

## <a name="runtime-pin-muxing"></a>執行階段針腳多工處理 

針腳多工處理可針對不同功能使用相同的實體針腳。 數個不同的晶片上周邊裝置 (例如 I2C 控制器、SPI 控制器 和 GPIO 控制器) 可能會路由至 SOC 上相同的實體針腳。 多工區塊會控制任何指定時間位於針腳上的使用中功能。 就以往而言，韌體負責建立開機時的功能指派，而此指派作業在整個開機工作階段維持靜態。 執行階段針腳多工處理新增了可於執行階段重新設定針腳功能指派的功能。 這讓使用者可透過快速重新設定電路板針腳，在執行階段選擇針腳功能以加速開發工作，同時可讓硬體支援較靜態設定更為廣泛的應用程式。 

使用者可運用針對 GPIO、I2C、SPI 和 UART 的支援，而無須額外撰寫任何程式碼。 使用者使用 [OpenPin()](https://msdn.microsoft.com/library/dn960157.aspx) 或 [FromIdAsync()](https://msdn.microsoft.com/windows.devices.i2c.i2cdevice.fromidasync) 開啟 GPIO 或匯流排時，基礎實體針腳會自動針對要求的功能執行多工處理。 若已有其他功能使用針腳，則 OpenPin() 或 FromIdAsync() 呼叫會失敗。 若使用者藉由處置 [GpioPin](https://msdn.microsoft.com/library/windows/apps/windows.devices.gpio.gpiopin.aspx)、[I2cDevice](https://msdn.microsoft.com/library/windows/apps/windows.devices.i2c.i2cdevice.aspx)、[SpiDevice](https://msdn.microsoft.com/library/windows/apps/windows.devices.spi.spidevice.aspx) 或 [SerialDevice](https://msdn.microsoft.com/library/windows/apps/windows.devices.serialcommunication.serialdevice.aspx) 物件來關閉裝置，則系統會釋放針腳使其之後可供其他功能開啟。 

Windows 包含適用於 [GpioClx](https://msdn.microsoft.com/library/windows/hardware/hh439515.aspx)、[SpbCx](https://msdn.microsoft.com/library/windows/hardware/hh406203.aspx) 和 [SerCx](https://msdn.microsoft.com/library/windows/hardware/dn265349.aspx) 架構針腳多工處理的內建支援。 這些架構可彼此搭配運作，在存取 GPIO 針腳或匯流排時將針腳自動切換至正確的功能。 針腳的存取採用仲裁式處理，以避免在多個用戶端之間發生衝突。 除了此內建支援外，適用於針腳多工處理的介面與通訊協定為一般用途，且可延伸用來支援其他裝置和案例。 

本文件會先說明關於針腳多工處理的基礎介面與通訊協定，接著再說明如何將針腳多工處理支援新增至 GpioClx、SpbCx 以及 SerCx 控制器驅動程式。 

### <a name="pin-muxing-architecture"></a>針腳多工處理架構 

本節說明關於針腳多工處理的基礎介面與通訊協定。 支援 GpioClx/SpbCx/SerCx 驅動程式針腳多工處理不一定必須具備基礎通訊協定知識。 如需如何支援 GpioCls/SpbCx/SerCx 驅動程式針腳多工處理的詳細資訊，請參閱[在 GpioClx 用戶端驅動程式中實作針腳多工處理支援](#supporting-muxing-support-in-gpioclx-client-drivers)與[在 SpbCx 與 SerCx 控制器驅動程式中使用多工處理支援](#supporting-muxing-in-spbcx-and-sercx-controller-drivers)。 

針腳多工處理是透過數個元件彼此合作而實現。 

* 針腳多工處理伺服器 – 這些是控制針腳多工處理控制區塊的驅動程式。 針腳多工處理伺服器會透過保留多工處理資源的要求 (透過 *IRP_MJ_CREATE*)，以及切換針腳功能的要求 (透過 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 要求)，接收來自用戶端的針腳多工處理要求。 由於多工處理區塊有時為 GPIO 區塊的一部分，因此針腳多工處理伺服器通常為 GPIO 驅動程式。 即使多工處理區塊為個別周邊裝置，GPIO 驅動程式亦是放置多工處理功能的邏輯位置。 
* 針腳多工處理用戶端 – 這些是使用針腳多工處理的驅動程式。 針腳多工處理用戶端會接收來自 ACPI 韌體的針腳多工處理資源。 針腳多工處理資源是一種連線資源，並由資源中樞所管理。 針腳多工處理用戶端會開啟資源控制代碼，以保留針腳多工處理資源。 為使硬體變更生效，用戶端必須傳送 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 要求以認可設定。 用戶端會關閉控制代碼以釋放針腳多工處理資源，此時多工處理設定會回復為預設狀態。 
* ACPI 韌體 – 指定具 `MsftFunctionConfig()` 資源的多工處理設定。 MsftFunctionConfig 資源會表明用戶端需要哪些針腳、使用何種多工處理設定。 MsftFunctionConfig 資源包含功能編號、提取設定以及針腳編號清單。 MsftFunctionConfig 資源會提供給針腳多工處理用戶端做為硬體資源，而驅動程式在執行 PrepareHardware 回呼時會接收這些資源，這與 GPIO 和 SPB 連線資源近似。 用戶端接收資源中樞識別碼，可用來開啟資源的控制代碼。 

> 您必須將 `/MsftInternal` 命令列參數傳遞至 `asl.exe` 以編譯包含 `MsftFunctionConfig()` 描述元的 ASL 檔案，因為這些描述元目前正由 ACPI 工作委員會審核。 例如： `asl.exe /MsftInternal dsdt.asl`

針腳多工處理相關作業順序如下所示。 

![針腳多工處理用戶端伺服器互動](images/usermode-access-diagram-1.png)

1.  用戶端在其 [EvtDevicePrepareHardware()](https://msdn.microsoft.com/library/windows/hardware/ff540880.aspx) 回呼中，接收來自 ACPI 韌體的 MsftFunctionConfig 資源。
2.  用戶端使用資源中樞協助程式函式 `RESOURCE_HUB_CREATE_PATH_FROM_ID()` 自資源識別碼建立路徑，然後開啟路徑控制代碼 (使用 [ZwCreateFile()](https://msdn.microsoft.com/library/windows/hardware/ff566424.aspx)、[IoGetDeviceObjectPointer()](https://msdn.microsoft.com/library/windows/hardware/ff549198.aspx) 或 [WdfIoTargetOpen()](https://msdn.microsoft.com/library/windows/hardware/ff548634.aspx))。
3.  伺服器使用資源中樞協助程式函式 `RESOURCE_HUB_ID_FROM_FILE_NAME()` 從檔案路徑擷取資源中樞識別碼，然後查詢資源中樞以取得資源描述元。
4.  伺服器針對描述元中的每個針腳執行共用仲裁，並完成 IRP_MJ_CREATE 要求。
5.  用戶端針對接收的控制代碼發出 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 要求。
6.  為回應 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS*，伺服器會將每個針腳上的指定功能設為使用中，以執行硬體多工處理作業。
7.  用戶端會繼續執行根據已要求針腳多工處理設定的作業。
8.  用戶端不再需要多工處理針腳時，即會關閉控制代碼。
9.  為回應關閉的控制代碼，伺服器會將針腳回復為其初始狀態。

### <a name="protocol-description-for-pin-muxing-clients"></a>針腳多工處理用戶端的通訊協定描述

本節說明用戶端如何使用針腳多工處理功能。 這不適用於 `SerCx` 和 `SpbCx` 控制器驅動程式，因為架構會代表控制器驅動程式實作此通訊協定。

####    <a name="parsing-resources"></a>剖析資源

WDF 驅動程式在其 [EvtDevicePrepareHardware()](https://msdn.microsoft.com/library/windows/hardware/ff540880.aspx) 常式中接收 `MsftFunctionConfig()` 資源。 可透過下列欄位識別 MsftFunctionConfig 資源：

```cpp
CM_PARTIAL_RESOURCE_DESCRIPTOR::Type = CmResourceTypeConnection
CM_PARTIAL_RESOURCE_DESCRIPTOR::u.Connection.Class = CM_RESOURCE_CONNECTION_CLASS_FUNCTION_CONFIG
CM_PARTIAL_RESOURCE_DESCRIPTOR::u.Connection.Type = CM_RESOURCE_CONNECTION_TYPE_FUNCTION_CONFIG
```

`EvtDevicePrepareHardware()` 常式可能會擷取 MsftFunctionConfig 資源，如下所示：

```cpp
EVT_WDF_DEVICE_PREPARE_HARDWARE evtDevicePrepareHardware;

_Use_decl_annotations_
NTSTATUS
evtDevicePrepareHardware (
    WDFDEVICE WdfDevice,
    WDFCMRESLIST ResourcesTranslated
    )
{
    PAGED_CODE();

    LARGE_INTEGER connectionId;
    ULONG functionConfigCount = 0;

    const ULONG resourceCount = WdfCmResourceListGetCount(ResourcesTranslated);
    for (ULONG index = 0; index < resourceCount; ++index) {
        const CM_PARTIAL_RESOURCE_DESCRIPTOR* resDescPtr =
            WdfCmResourceListGetDescriptor(ResourcesTranslated, index);

        switch (resDescPtr->Type) {
        case CmResourceTypeConnection:
            switch (resDescPtr->u.Connection.Class) {
            case CM_RESOURCE_CONNECTION_CLASS_FUNCTION_CONFIG:
                switch (resDescPtr->u.Connection.Type) {
                case CM_RESOURCE_CONNECTION_TYPE_FUNCTION_CONFIG:                    
                    switch (functionConfigCount) {
                    case 0:
                        // save the connection ID
                        connectionId.LowPart = resDescPtr->u.Connection.IdLowPart;
                        connectionId.HighPart = resDescPtr->u.Connection.IdHighPart;
                        break;
                    } // switch (functionConfigCount)
                    ++functionConfigCount;
                    break; // CM_RESOURCE_CONNECTION_TYPE_FUNCTION_CONFIG

                } // switch (resDescPtr->u.Connection.Type)
                break; // CM_RESOURCE_CONNECTION_CLASS_FUNCTION_CONFIG
            } // switch (resDescPtr->u.Connection.Class)
            break;
        } // switch
    } // for (resource list)

    if (functionConfigCount < 1) {
        return STATUS_INVALID_DEVICE_CONFIGURATION;
    }
    // TODO: save connectionId in the device context for later use

    return STATUS_SUCCESS;
}
```

####    <a name="reserving-and-committing-resources"></a>保留和認可資源

若用戶端想要多工處理針腳，則會保留和認可 MsftFunctionConfig 資源。 下列範例說明用戶端如何保留並認可 MsftFunctionConfig 資源。

```cpp
_IRQL_requires_max_(PASSIVE_LEVEL)
NTSTATUS AcquireFunctionConfigResource (
    WDFDEVICE WdfDevice,
    LARGE_INTEGER ConnectionId,
    _Out_ WDFIOTARGET* ResourceHandlePtr
    )
{
    PAGED_CODE();

    //
    // Form the resource path from the connection ID
    //
    DECLARE_UNICODE_STRING_SIZE(resourcePath, RESOURCE_HUB_PATH_CHARS);
    NTSTATUS status = RESOURCE_HUB_CREATE_PATH_FROM_ID(
            &resourcePath,
            ConnectionId.LowPart,
            ConnectionId.HighPart);
    if (!NT_SUCCESS(status)) {
        return status;
    }
    
    //
    // Create a WDFIOTARGET
    //
    WDFIOTARGET resourceHandle;
    status = WdfIoTargetCreate(WdfDevice, WDF_NO_ATTRIBUTES, &resourceHandle);
    if (!NT_SUCCESS(status)) {
        return status;
    }

    //
    // Reserve the resource by opening a WDFIOTARGET to the resource
    //
    WDF_IO_TARGET_OPEN_PARAMS openParams;
    WDF_IO_TARGET_OPEN_PARAMS_INIT_OPEN_BY_NAME(
        &openParams,
        &resourcePath,
        FILE_GENERIC_READ | FILE_GENERIC_WRITE);

    status = WdfIoTargetOpen(resourceHandle, &openParams);
    if (!NT_SUCCESS(status)) {
        return status;
    }
    //
    // Commit the resource
    //
    status = WdfIoTargetSendIoctlSynchronously(
            resourceHandle,
            WDF_NO_HANDLE,      // WdfRequest
            IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS,
            nullptr,            // InputBuffer
            nullptr,            // OutputBuffer
            nullptr,            // RequestOptions
            nullptr);           // BytesReturned
    
    if (!NT_SUCCESS(status)) {
        WdfIoTargetClose(resourceHandle);
        return status;
    }

    //
    // Pins were successfully muxed, return the handle to the caller
    //
    *ResourceHandlePtr = resourceHandle;
    return STATUS_SUCCESS;
}
```

驅動程式應將 WDFIOTARGET 存放於其中一個內容區域，以便稍後關閉。 驅動程式準備好發行多工處理設定時，應該呼叫 [WdfObjectDelete()](https://msdn.microsoft.com/library/windows/hardware/ff548734.aspx) 以關閉資源控制代碼，或如果您要重複使用 WDFIOTARGET，則呼叫 [WdfIoTargetClose()](https://msdn.microsoft.com/library/windows/hardware/ff548586.aspx)。

```cpp
    WdfObjectDelete(resourceHandle);
```

用戶端關閉其資源控制代碼時，多工處理的針腳會回復初始狀態，以供其他用戶端立即取得。

### <a name="protocol-description-for-pin-muxing-servers"></a>適用於針腳多工處理伺服器的通訊協定描述

本節說明針腳多工處理伺服器如何將其功能對用戶端公開。 這不適用於 `GpioClx` 迷你連接埠驅動程式，因為架構會代表用戶端驅動程式實作此通訊協定。 如需如何在 `GpioClx` 用戶端驅動程式中支援針腳多工處理的詳細資訊，請參閱[在 GpioClx 用戶端驅動程式中實作多工處理支援](#supporting-muxing-support-in-gpioclx-client-drivers)。

####    <a name="handling-irpmjcreate-requests"></a>處理 IRP_MJ_CREATE 要求

若用戶端想要保留針腳多工處理資源，則會開啟資源控制代碼。 針腳多工處理伺服器會透過來自資源中樞的重新剖析作業，接收 *IRP_MJ_CREATE* 要求。 *IRP_MJ_CREATE* 要求的結尾路徑元件會包含資源中樞識別碼，其為使用十六進位格式的 64 位元整數。 伺服器應會使用來自 reshub.h 的 `RESOURCE_HUB_ID_FROM_FILE_NAME()`，從檔名擷取資源中樞識別碼，並將 *IOCTL_RH_QUERY_CONNECTION_PROPERTIES* 傳送至資源中樞以取得 `MsftFunctionConfig()` 描述元。

伺服器應會驗證描述元，並從描述元擷取共用的模式與針腳清單。 接著它應會執行針腳的共用仲裁，若執行成功，則會將針腳標示為保留然後再完成要求。

若針腳清單上的每個針腳皆順利執行共用仲裁，則整體共用仲裁作業亦會成功。 應仲裁處理每個針腳，如下所示 ︰

*   若針腳已不保留，則會成功執行共用仲裁。
*   若針腳已保留供專屬用途，則共用仲裁會失敗。
*   若針腳已保留供共用，
  * 且傳入的要求為共用，則會成功執行共用仲裁。
  * 而傳入的要求為專屬用途，則共用仲裁作業會失敗。

若共用仲裁作業失敗，則應使用 *STATUS_GPIO_INCOMPATIBLE_CONNECT_MODE* 完成要求。 若共用仲裁作業成功，則應使用 *STATUS_SUCCESS* 完成要求。

請注意，傳入要求的共用模式應取自 MsftFunctionConfig 描述元，而非 [IrpSp-&gt;Parameters.Create.ShareAccess](https://msdn.microsoft.com/library/windows/hardware/ff548630.aspx)。

####    <a name="handling-ioctlgpiocommitfunctionconfigpins-requests"></a>處理 IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS 要求

用戶端藉由開啟控制代碼成功保留 MsftFunctionConfig 資源後，可傳送 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 要求伺服器執行實際硬體多工處理作業。 當伺服器收到 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 時，對於針腳清單中的每個針腳，它應該會 

*   將 PNP_FUNCTION_CONFIG_DESCRIPTOR 架構之 PinConfiguration 成員中指定的提取模式設定到硬體。
*   將針腳多工處理至由 PNP_FUNCTION_CONFIG_DESCRIPTOR 架構之 FunctionNumber 成員指定的功能。

隨後伺服器應會以 *STATUS_SUCCESS* 完成要求。

FunctionNumber 的意義是由伺服器所定義，且 MsftFunctionConfig 描述元是在知悉伺服器如何解譯此欄位的情況下撰寫而成。

請謹記，伺服器在關閉控制代碼時必須將針腳回復為接收 IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS 時的原本設定，因此伺服器可能需要儲存針腳的狀態後再修改針腳。

####    <a name="handling-irpmjclose-requests"></a>處理 IRP_MJ_CLOSE 要求

若用戶端不再需要多工處理資源，則會關閉其控制代碼。 伺服器收到 *IRP_MJ_CLOSE* 要求時，應會將針腳回復為接收 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* 時的狀態。 若用戶端從未傳送 *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS*，則無須執行任何動作。 隨後伺服器應會將針腳標示為可供共用仲裁使用，並以 *STATUS_SUCCESS* 完成要求。 請務必正確同步 *IRP_MJ_CLOSE* 處理與 *IRP_MJ_CREATE* 處理。

### <a name="authoring-guidelines-for-acpi-tables"></a>撰寫 ACPI 表格的指導方針

本節說明如何提供多工處理資源至用戶端驅動程式。 請注意，您必須具有 Microsoft ASL 編譯器建置 14327 或更新版本，才能編譯包含 `MsftFunctionConfig()` 資源的表格。 `MsftFunctionConfig()` 資源會提供至針腳多工處理用戶端做為硬體資源。 `MsftFunctionConfig()` 資源應提供至需要執行針腳多工處理變更的驅動程式 (通常為 SPB 與序列控制器驅動程式)，但不應提供至 SPB 與序列周邊裝置驅動程式，這是因為控制器驅動程式會處理多工設定。
`MsftFunctionConfig()` ACPI 巨集定義如下：

```cpp
  MsftFunctionConfig(Shared/Exclusive
                PinPullConfig,
                FunctionNumber,
                ResourceSource,
                ResourceSourceIndex,
                ResourceConsumer/ResourceProducer,
                VendorData) { Pin List }

```

* Shared/Exclusive (共用/專屬) – 若為專屬 ，則會由單一用戶端個別取得此針腳； 若為共用，則可讓多個共用用戶端取得資源。 請一律將此項目設為專屬，因為允許多個未協調的用戶端存取可多工資源，可能導致資料競爭而造成非預期的結果。 
* PinPullConfig – 下列其中一個 
  * PullDefault – 使用 SOC 定義的電源開啟預設提取設定 
  * PullUp – 啟用上拉電阻 
  * PullDown – 啟用下拉電阻 
  * PullNone – 停用所有提取電阻 
* FunctionNumber – 編寫程式到多工處理的功能編號。 
* ResourceSource – 針腳多工處理伺服器的 ACPI 命名空間路徑 
* ResourceSourceIndex – 將此項目設為 0 
* ResourceConsumer/ResourceProducer – 將此項目設為 ResourceConsumer 
* VendorData – 由針腳多工處理伺服器定義意義的選用二進位資料。 此項目通常應為空白
* Pin List (針腳清單) – 套用設定之針腳編號的清單 (以逗號分隔)。 若針腳多工處理伺服器為 GpioClx 驅動程式，則這些是 GPIO 針腳編號，並與 GpioIo 描述元中的針腳編號具有相同意義。 

下列範例說明如何將 MsftFunctionConfig() 資源提供給 I2C 控制器驅動程式。 

```cpp
Device(I2C1) 
{ 
    Name(_HID, "BCM2841") 
    Name(_CID, "BCMI2C") 
    Name(_UID, 0x1) 
    Method(_STA) 
    { 
        Return(0xf) 
    } 
    Method(_CRS, 0x0, NotSerialized) 
    { 
        Name(RBUF, ResourceTemplate() 
        { 
            Memory32Fixed(ReadWrite, 0x3F804000, 0x20) 
            Interrupt(ResourceConsumer, Level, ActiveHigh, Shared) { 0x55 } 
            MsftFunctionConfig(Exclusive, PullUp, 4, "\\_SB.GPI0", 0, ResourceConsumer, ) { 2, 3 } 
        }) 
        Return(RBUF) 
    } 
} 
```

除了一般由控制器驅動程式取得的記憶體與插斷資源外，亦會指定 `MsftFunctionConfig()` 資源。 此資源可讓 I2C 控制器驅動程式將針腳 2 和 3 (由位於 \\_SB.GPIO0 的裝置節點管理) 放置於功能 4，且啟用上拉電阻。 

## <a name="supporting-muxing-support-in-gpioclx-client-drivers"></a>在 GpioClx 用戶端驅動程式中提供多工處理支援 

`GpioClx` 具備適用於針腳多工處理的內建支援。 GpioClx 迷你連接埠驅動程式 (亦稱為「GpioClx 用戶端驅動程式」) 驅動 GPIO 控制器硬體。 自 Windows 10 組建 14327 開始，GpioClx 迷你連接埠驅動程式可透過實作兩個新 DDI，新增針腳多工處理支援： 

* CLIENT_ConnectFunctionConfigPins – 由 `GpioClx` 呼叫，命令迷你連接埠驅動程式套用指定的多工處理設定。 
* CLIENT_DisconnectFunctionConfigPins – 由 `GpioClx` 呼叫，命令迷你連接埠驅動程式回復多工處理設定。 

如需這些常式的說明，請參閱 [GpioClx 事件的回呼函式](https://msdn.microsoft.com/library/windows/hardware/hh439464.aspx)。

除了這兩個新 DDI 外，應稽核現有 DDI 的針腳多工處理相容性： 

* CLIENT_ConnectIoPins/CLIENT_ConnectInterrupt – GpioClx 呼叫 CLIENT_ConnectIoPins，以命令迷你連接埠驅動程式設定 GPIO 輸入或輸出的設定針腳。 GPIO 與 MsftFunctionConfig 彼此互斥，這表示針腳絕對不會同時連線 GPIO 與 MsftFunctionConfig。 由於針腳的預設功能並不必然是 GPIO，因此在呼叫 ConnectIoPins 時不一定要將針腳多工處理至 GPIO。 必須要有 ConnectIoPins 才能執行所有必要作業，讓針腳隨時可供 GPIO IO 使用，包括多工處理作業。 *CLIENT_ConnectInterrupt* 的行為方式應會近似，這是因為您可將插斷視為是 GPIO 輸入的特殊情況。 
* CLIENT_DisconnectIoPins/CLIENT_DisconnectInterrupt – 這些常式應讓針腳回到呼叫 CLIENT_ConnectIoPins/CLIENT_ConnectInterrupt 時的狀態，除非另有指定 PreserveConfiguration 旗標。 除了將針腳方向回復為預設狀態外，迷你連接埠亦應會將每個針腳的多工處理狀態回復為呼叫 _Connect 常式時的狀態。 

例如，假設針腳的預設多工處理設定為 UART，且針腳亦可用作 GPIO。 當呼叫 CLIENT_ConnectIoPins 以連線 GPIO 針腳時，它應會將針腳多工處理至 GPIO，且在 CLIENT_DisconnectIoPins 中應會將針腳多工回復為 UART。 一般而言，_Disconnect 常式應會復原 _Connect 常式完成的作業。 

## <a name="supporting-muxing-in-spbcx-and-sercx-controller-drivers"></a>在 SpbCx 和 SerCx 控制器驅動程式中支援多工處理 

自 Windows 10 組建 14327 開始，`SpbCx` 與 `SerCx` 架構包含適用於針腳多工處理的內建支援，可將 `SpbCx` 與 `SerCx` 控制器驅動程式做為針腳多工處理用戶端，而無須針對控制器驅動程式進行任何程式碼變更。 透過擴充方式，可讓連線至啟用多工處理功能之 SpbCx/SerCx 控制器驅動程式的任何 SpbCx/SerCx 周邊裝置驅動程式，觸發針腳多工處理活動。 

下圖說明這些每個元件之間的相依性。 如您所見，針腳多工處理會將來自 SerCx 和 SpbCx 控制器驅動程式的相依性，導入至通常負責執行多工處理的 GPIO 驅動程式。 

![針腳多工處理相依性](images/usermode-access-diagram-2.png)

執行裝置初始化時，`SpbCx` 與 `SerCx` 架構會剖析所有提供作為裝置之硬體資源的 `MsftFunctionConfig()` 資源。 隨後 SpbCx/SerCx 會依需求取得和釋放針腳多工處理資源。

`SpbCx` 在呼叫用戶端驅動程式的 [EvtSpbTargetConnect()](https://msdn.microsoft.com/library/windows/hardware/hh450818.aspx) 回呼之前，會將針腳多工處理設定套用於其 *IRP_MJ_CREATE* 處理常式。 若無法套用多工處理設定，則不會呼叫控制器驅動程式的 `EvtSpbTargetConnect()` 回呼。 因此，SPB 控制器驅動程式可能會依據呼叫 `EvtSpbTargetConnect()` 的時間，假設針腳已多工處理至 SPB 功能。

`SpbCx` 在完成叫用控制器驅動程式的 [EvtSpbTargetDisconnect()](https://msdn.microsoft.com/library/windows/hardware/hh450820.aspx) 回呼後，將針腳多工處理設定回復於其 *IRP_MJ_CLOSE* 處理常式。 最後，針腳會在周邊裝置驅動程式開啟 SPB 控制器驅動程式控制代碼時多工處理至 SPB 功能，並在周邊裝置驅動程式關閉控制代碼時結束多工處理。

`SerCx` 行為方式近似。 `SerCx` 會在叫用控制器驅動程式的 [EvtSerCx2FileOpen()](https://msdn.microsoft.com/library/windows/hardware/dn265209.aspx) 回呼前，取得所有位於其 *IRP_MJ_CREATE* 處理常式的 `MsftFunctionConfig()` 資源，並在叫用控制器驅動程式的 [EvtSerCx2FileClose](https://msdn.microsoft.com/library/windows/hardware/dn265208.aspx) 回呼後，釋放所有位於其 IRP_MJ_CLOSE 處理常式的資源。

針對 `SerCx` 與 `SpbCx` 控制器驅動程式的動態針腳多工處理產生的影響，在於它們必須能夠容忍針腳於特定時間自 SPB/UART 功能結束多工處理。 控制器驅動程式必須假設在呼叫 `EvtSpbTargetConnect()` 或 `EvtSerCx2FileOpen()` 前，不會多工處理針腳。 執行下列回呼時，並不必然要將針腳多工處理至 SPB/UART。 下列並非完整的清單，但代表控制器驅動程式最常實作的 PNP 常式。

* DriverEntry 
* EvtDriverDeviceAdd 
* EvtDevicePrepareHardware/EvtDeviceReleaseHardware 
* EvtDeviceD0Entry/EvtDeviceD0Exit 

## <a name="verification"></a>驗證 

當您準備好測試 rhproxy 時，使用下列逐步程序會有幫助。

1. 確認每個 `SpbCx`、`GpioClx` 及 `SerCx` 控制器驅動程式正確載入和操作
1. 確認 `rhproxy` 出現在系統上。 有些 Windows 版本和組建沒有它。
1. 編譯和載入您 rhproxy 節點，使用 `ACPITABL.dat`
1. 確認 `rhproxy` 裝置節點存在
1. 確認 `rhproxy` 載入並啟動
1. 確認預期的裝置公開到使用者模式
1. 確認您可以從命令列與每部裝置互動
1. 確認您可以從 UWP app 與每部裝置互動
1. 執行 HLK 測試

### <a name="verify-controller-drivers"></a>確認控制器驅動程式

因為 Rhproxy 公開系統上的其他裝置到使用者模式，只有那些裝置已在運作它才有作用。 第一個步驟是確認那些裝置 (您想要公開的 I2C、SPI、GPIO 控制器) 已在運作。

在命令提示字元中，執行
```
devcon status *
```

查看輸出，並確認相關的所有裝置已啟動。 如果裝置出現問題代碼，您需要時進行疑難排解以得知該裝置為何不載入。 初始顯示平台期間，所有的裝置應該已經啟用。 疑難排解 `SpbCx`、`GpioClx` 或 `SerCx` 控制器驅動程式是否超出本文件範圍。

### <a name="verify-that-rhproxy-is-present-on-the-system"></a>確認 rhproxy 出現在系統上

確認 `rhproxy` 服務出現在系統上。

```
reg query HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\rhproxy
```

如果登錄機碼不存在，rhproxy 也不存在您的系統上。 Rhproxy 會出現在所有組建的 IoT 核心版和 Windows 企業版組建 15063 及更新版本。

### <a name="compile-and-load-asl-with-acpitabldat"></a>使用 ACPITABL.dat 編譯和載入 ASL 

現在您已經製作 rhproxy ASL 節點，是時候進行編譯並將其載入。 您可以將 rhproxy 節點編譯到獨立的 AML 檔案，該檔案可以附加到系統 ACPI 資料表。 或者，如果您可以存取您系統的 ACPI 來源，您可以直接將 rhproxy 節點插入至您平台的 ACPI 資料表。 但是，在初始顯示期間，可能更容易使用 `ACPITABL.dat`。

1. 建立一個名為 yourboard.asl 檔案，並將 RHPX 裝置節點置於 DefinitionBlock 當中： 
```
DefinitionBlock ("ACPITABL.dat", "SSDT", 1, "MSFT", "RHPROXY", 1)
{
    Scope (\_SB)
    {
        Device(RHPX)
        {
        ...
        }
    }
}
```
2.  下載 [WDK](https://docs.microsoft.com/windows-hardware/drivers/download-the-wdk)，並尋找 `asl.exe`，於 `C:\Program Files (x86)\Windows Kits\10\Tools\x64\ACPIVerify`
3.  執行下列命令以產生 ACPITABL.dat：
```
asl.exe yourboard.asl
```
4.  在測試過程中，將產生的 ACPITABL.dat 檔案複製至系統上的 c:\windows\system32。
5.  在測試過程中，開啟系統上的 testsigning：
```
bcdedit /set testsigning on
```
6.  在測試過程中將系統重新開機。 系統會將 ACPITABL.dat 中定義的 ACPI 表格，附加至系統韌體表格。

### <a name="verify-that-the-rhproxy-device-node-exists"></a>確認 rhproxy 裝置節點存在

執行下列命令以列舉 rhproxy 裝置節點。
```
devcon status *msft8000
```
Devcon 的輸出應該表示裝置已經存在。 如果裝置節點不存在，ACPI 資料表格並未成功新增至系統。

### <a name="verify-that-rhproxy-is-loading-and-starting"></a>確認 rhproxy 載入並啟動

檢查 rhproxy 的狀態：
```
devcon status *msft8000
```
如果輸出表示該 rhproxy 已啟動，rhproxy 已成功載入並啟動。 如果您看到問題代碼，您需要進行調查。 一些常見的問題代碼有：
* 問題 51- `CM_PROB_WAITING_ON_DEPENDENCY` - 系統未啟動 rhproxy，因為它的其中一個相依性無法載入。 這表示傳遞至 rhproxy 的資源指向無效的 ACPI 節點，或者目標裝置未啟動。 首先，請再次檢查所有的裝置成功執行 (參閱上述「確認控制器驅動程式」)。 然後，再次檢查您 ASL，並確保您所有的資源路徑 (例如`\_SB.I2C1`) 正確，並指向您 DSDT 中的有效節點。
* 問題 10 - `CM_PROB_FAILED_START` - Rhproxy 無法啟動，很可能是因為資源剖析問題。 移到您的 ASL 資源並再次檢查 DSD 中的資源索引，確認 GPIO 資源依照增加的 PIN 碼順序指定。

### <a name="verify-that-the-expected-devices-are-exposed-to-usermode"></a>確認預期的裝置公開到使用者模式

現在 rhproxy 正在執行中，它應該已建立使用者模式可以存取的裝置介面。 我們會使用數個命令列工具來列舉裝置，並查看他們是否出現。

複製[https://github.com/ms-iot/samples](https://github.com/ms-iot/samples)存放庫和組建`GpioTestTool`， `I2cTestTool`， `SpiTestTool`，以及`Mincomm`範例。 複製工具到進行測試的裝置，並使用下列命令列舉裝置。
```
I2cTestTool.exe -list
SpiTestTool.exe -list
GpioTestTool.exe -list
MinComm.exe -list
```
您應該會看到您的裝置和列出的易記名稱。 如果您看不到正確的裝置和易記名稱，請再次檢查您的 ASL。

### <a name="verify-each-device-on-the-command-line"></a>確認命令列上的每一部裝置

下一個步驟是使用的命令列工具開啟並與裝置互動。

I2CTestTool 範例：
```
I2cTestTool.exe 0x55 I2C1
> write {1 2 3}
> read 3
> writeread {1 2 3} 3
```

SpiTestTool 範例：
```
SpiTestTool.exe -n SPI1
> write {1 2 3}
> read 3
```

GpioTestTool 範例：
```
GpioTestTool.exe 12
> setdrivemode output
> write 0
> write 1
> setdrivemode input
> read
> interrupt on
> interrupt off
```

MinComm (序列) 範例。 執行前請先連接 Rx 到 Tx：
```
MinComm "\\?\ACPI#FSCL0007#3#{86e0d1e0-8089-11d0-9ce4-08003e301f73}\0000000000000006"
(type characters and see them echoed back)
```

### <a name="verify-each-device-from-a-uwp-app"></a>確認 UWP app 的每個裝置

使用下列範例驗證 UWP 的裝置可運作。

| 範例 | 連結 |
|------|------|
| IoT-GPIO | https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/IoT-GPIO |
| IoT-I2C | https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/IoT-I2C | 
| IoT-SPI | https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/IoT-SPI |
| CustomSerialDeviceAccess | https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomSerialDeviceAccess |

### <a name="run-the-hlk-tests"></a>執行 HLK 測試

下載 [Hardware Lab Kit (HLK)](https://docs.microsoft.com/windows-hardware/test/hlk/windows-hardware-lab-kit)。 提供下列測試：
 * [GPIO WinRT 功能和壓力測試](https://docs.microsoft.com/windows-hardware/test/hlk/testref/f1fc0922-1186-48bd-bfcd-c7385a2f6f96)
 * [I2C WinRT 寫入測試 (需要 EEPROM)](https://docs.microsoft.com/windows-hardware/test/hlk/testref/2ab0df1b-3369-4aaf-a4d5-d157cb7bf578)
 * [I2C WinRT 讀取測試 (需要 EEPROM)](https://docs.microsoft.com/windows-hardware/test/hlk/testref/ca91c2d2-4615-4a1b-928e-587ab2b69b04)
 * [I2C WinRT 不存在從屬位址測試](https://docs.microsoft.com/windows-hardware/test/hlk/testref/2746ad72-fe5c-4412-8231-f7ed53d95e71)
 * [I2C WinRT 進階功能測試 (需要 mbed LPC1768)](https://docs.microsoft.com/windows-hardware/test/hlk/testref/a60f5a94-12b2-4905-8416-e9774f539f1d)
 * [SPI WinRT 時脈頻率驗證測試 (需要 mbed LPC1768)](https://docs.microsoft.com/windows-hardware/test/hlk/testref/50cf9ccc-bbd3-4514-979f-b0499cb18ed8)
 * [SPI WinRT IO 傳輸測試 (需要 mbed LPC1768)](https://docs.microsoft.com/windows-hardware/test/hlk/testref/00c892e8-c226-4c71-9c2a-68349fed7113)
 * [SPI WinRT 分散驗證測試](https://docs.microsoft.com/windows-hardware/test/hlk/testref/20c6b079-62f7-4067-953f-e252bd271938)
 * [SPI WinRT 傳輸間隔偵測測試 (需要 mbed LPC1768)](https://docs.microsoft.com/windows-hardware/test/hlk/testref/6da79d04-940b-4c49-8f00-333bf0cfbb19)

當您在 HLK 管理員中選取 rhproxy 裝置節點時，會自動選取適用的測試。

在 HLK 管理員中，選取 [資源中樞 Proxy 裝置]：

![HLK 管理員螢幕擷取畫面](images/usermode-hlk-1.png)

然後按一下 [測試] 索引標籤，選取 [I2C WinRT、Gpio WinRT 和 Spi WinRT 測試]。

![HLK 管理員螢幕擷取畫面](images/usermode-hlk-2.png)

按一下 [執行選取的項目]。 如需每項測試的詳細說明文件，請以滑鼠右鍵按一下測試，然後再按一下 [測試描述]。

## <a name="resources"></a>資源

| 目的地 | 連結 |
|-------------|------|
| ACPI 5.0 規格 | http://acpi.info/spec.htm |
| Asl.exe (Microsoft ASL 編譯器) | https://msdn.microsoft.com/library/windows/hardware/dn551195.aspx |
| Windows.Devices.Gpio  | https://msdn.microsoft.com/library/windows/apps/windows.devices.gpio.aspx | 
| Windows.Devices.I2c | https://msdn.microsoft.com/library/windows/apps/windows.devices.i2c.aspx |
| Windows.Devices.Spi | https://msdn.microsoft.com/library/windows/apps/windows.devices.spi.aspx |
| Windows.Devices.SerialCommunication | https://msdn.microsoft.com/library/windows/apps/windows.devices.serialcommunication.aspx |
| 測試撰寫與執行架構 (TAEF) | https://msdn.microsoft.com/library/windows/hardware/hh439725.aspx |
| SpbCx | https://msdn.microsoft.com/library/windows/hardware/hh450906.aspx |
| GpioClx   | https://msdn.microsoft.com/library/windows/hardware/hh439508.aspx |
| SerCx | https://msdn.microsoft.com/library/windows/hardware/ff546939.aspx |
| MITT I2C 測試 | https://msdn.microsoft.com/library/windows/hardware/dn919852.aspx |
| GpioTestTool | https://developer.microsoft.com/windows/iot/samples/GPIOTestTool |
| I2cTestTool   | https://developer.microsoft.com/windows/iot/samples/I2cTestTool | 
| SpiTestTool | https://developer.microsoft.com/windows/iot/samples/spitesttool |
| MinComm (序列) |    https://github.com/ms-iot/samples/tree/develop/MinComm |
| Hardware Lab Kit (HLK) | https://msdn.microsoft.com/library/windows/hardware/dn930814.aspx |

## <a name="apendix"></a>附錄

### <a name="appendix-a---raspberry-pi-asl-listing"></a>附錄 A - Raspberry Pi ASL 清單

標頭針腳輸出：https://developer.microsoft.com/windows/iot/samples/PinMappingsRPi2

```
DefinitionBlock ("ACPITABL.dat", "SSDT", 1, "MSFT", "RHPROXY", 1)
{

    Scope (\_SB)
    {
        //
        // RHProxy Device Node to enable WinRT API
        //
        Device(RHPX)
        {
            Name(_HID, "MSFT8000")
            Name(_CID, "MSFT8000")
            Name(_UID, 1)

            Name(_CRS, ResourceTemplate()
            {
                // Index 0
                SPISerialBus(              // SCKL - GPIO 11 - Pin 23
                                           // MOSI - GPIO 10 - Pin 19
                                           // MISO - GPIO 9  - Pin 21
                                           // CE0  - GPIO 8  - Pin 24
                    0,                     // Device selection (CE0)
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    0,                     // databit len: placeholder
                    ControllerInitiated,   // slave mode
                    0,                     // connection speed: placeholder
                    ClockPolarityLow,      // clock polarity: placeholder
                    ClockPhaseFirst,       // clock phase: placeholder
                    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                                           // Resource usage
                    )                      // Vendor Data

                // Index 1
                SPISerialBus(              // SCKL - GPIO 11 - Pin 23
                                           // MOSI - GPIO 10 - Pin 19
                                           // MISO - GPIO 9  - Pin 21
                                           // CE1  - GPIO 7  - Pin 26
                    1,                     // Device selection (CE1)
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    0,                     // databit len: placeholder
                    ControllerInitiated,   // slave mode
                    0,                     // connection speed: placeholder
                    ClockPolarityLow,      // clock polarity: placeholder
                    ClockPhaseFirst,       // clock phase: placeholder
                    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                                           // Resource usage
                    )                      // Vendor Data

                // Index 2
                SPISerialBus(              // SCKL - GPIO 21 - Pin 40
                                           // MOSI - GPIO 20 - Pin 38
                                           // MISO - GPIO 19 - Pin 35
                                           // CE1  - GPIO 17 - Pin 11
                    1,                     // Device selection (CE1)
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    0,                     // databit len: placeholder
                    ControllerInitiated,   // slave mode
                    0,                     // connection speed: placeholder
                    ClockPolarityLow,      // clock polarity: placeholder
                    ClockPhaseFirst,       // clock phase: placeholder
                    "\\_SB.SPI1",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                                           // Resource usage
                    )                      // Vendor Data
                // Index 3
                I2CSerialBus(              // Pin 3 (GPIO2, SDA1), 5 (GPIO3, SCL1)
                    0xFFFF,                // SlaveAddress: placeholder
                    ,                      // SlaveMode: default to ControllerInitiated
                    0,                     // ConnectionSpeed: placeholder
                    ,                      // Addressing Mode: placeholder
                    "\\_SB.I2C1",          // ResourceSource: I2C bus controller name
                    ,
                    ,
                    )                      // VendorData

                // Index 4 - GPIO 4 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 4 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 4 }
                // Index 6 - GPIO 5 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 5 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 5 }
                // Index 8 - GPIO 6 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 6 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 6 }
                // Index 10 - GPIO 12 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 12 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 12 }
                // Index 12 - GPIO 13 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 13 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 13 }
                // Index 14 - GPIO 16 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 16 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 16 }
                // Index 16 - GPIO 18 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 18 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 18 }
                // Index 18 - GPIO 22 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 22 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 22 }
                // Index 20 - GPIO 23 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 23 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 23 }
                // Index 22 - GPIO 24 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 24 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 24 }
                // Index 24 - GPIO 25 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 25 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 25 }
                // Index 26 - GPIO 26 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 26 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 26 }
                // Index 28 - GPIO 27 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 27 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 27 }
                // Index 30 - GPIO 35 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 35 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 35 }
                // Index 32 - GPIO 47 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 47 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 47 }
            })

            Name(_DSD, Package()
            {
                ToUUID("daffd814-6eba-4d8c-8a91-bc9bbf4aa301"),
                Package()
                {
                    // Reference http://www.raspberrypi.org/documentation/hardware/raspberrypi/spi/README.md
                    // SPI 0
                    Package(2) { "bus-SPI-SPI0", Package() { 0, 1 }},                       // Index 0 & 1
                    Package(2) { "SPI0-MinClockInHz", 7629 },                               // 7629 Hz
                    Package(2) { "SPI0-MaxClockInHz", 125000000 },                          // 125 MHz
                    Package(2) { "SPI0-SupportedDataBitLengths", Package() { 8 }},          // Data Bit Length
                    // SPI 1
                    Package(2) { "bus-SPI-SPI1", Package() { 2 }},                          // Index 2
                    Package(2) { "SPI1-MinClockInHz", 30518 },                              // 30518 Hz
                    Package(2) { "SPI1-MaxClockInHz", 125000000 },                          // 125 MHz
                    Package(2) { "SPI1-SupportedDataBitLengths", Package() { 8 }},          // Data Bit Length
                    // I2C1
                    Package(2) { "bus-I2C-I2C1", Package() { 3 }},
                    // GPIO Pin Count and supported drive modes
                    Package (2) { "GPIO-PinCount", 54 },
                    Package (2) { "GPIO-UseDescriptorPinNumbers", 1 },
                    Package (2) { "GPIO-SupportedDriveModes", 0xf },                        // InputHighImpedance, InputPullUp, InputPullDown, OutputCmos
                }
            })
        }
    }
}

```

### <a name="appendix-b---minnowboardmax-asl-listing"></a>附錄 B - MinnowBoardMax ASL 清單

標頭針腳輸出：https://developer.microsoft.com/windows/iot/samples/PinMappingsMBM

```
DefinitionBlock ("ACPITABL.dat", "SSDT", 1, "MSFT", "RHPROXY", 1)
{
    Scope (\_SB)
    {
        Device(RHPX)
        {
            Name(_HID, "MSFT8000")
            Name(_CID, "MSFT8000")
            Name(_UID, 1)

            Name(_CRS, ResourceTemplate() 
            {  
                // Index 0 
                SPISerialBus(            // Pin 5, 7, 9 , 11 of JP1 for SIO_SPI
                    1,                     // Device selection
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    8,                     // databit len
                    ControllerInitiated,   // slave mode
                    8000000,               // Connection speed
                    ClockPolarityLow,      // Clock polarity
                    ClockPhaseSecond,      // clock phase
                    "\\_SB.SPI1",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                    ResourceConsumer,      // Resource usage
                    JSPI,                  // DescriptorName: creates name for offset of resource descriptor
                    )                      // Vendor Data  
    
                // Index 1     
                I2CSerialBus(            // Pin 13, 15 of JP1, for SIO_I2C5 (signal)
                    0xFF,                  // SlaveAddress: bus address
                    ,                      // SlaveMode: default to ControllerInitiated
                    400000,                // ConnectionSpeed: in Hz
                    ,                      // Addressing Mode: default to 7 bit
                    "\\_SB.I2C6",          // ResourceSource: I2C bus controller name (For MinnowBoard Max, hardware I2C5(0-based) is reported as ACPI I2C6(1-based))
                    ,
                    ,
                    JI2C,                  // Descriptor Name: creates name for offset of resource descriptor
                    )                      // VendorData
    
                // Index 2
                UARTSerialBus(           // Pin 17, 19 of JP1, for SIO_UART2
                    115200,                // InitialBaudRate: in bits ber second
                    ,                      // BitsPerByte: default to 8 bits
                    ,                      // StopBits: Defaults to one bit
                    0xfc,                  // LinesInUse: 8 1-bit flags to declare line enabled
                    ,                      // IsBigEndian: default to LittleEndian
                    ,                      // Parity: Defaults to no parity
                    ,                      // FlowControl: Defaults to no flow control
                    32,                    // ReceiveBufferSize
                    32,                    // TransmitBufferSize
                    "\\_SB.URT2",          // ResourceSource: UART bus controller name
                    ,
                    ,
                    UAR2,                  // DescriptorName: creates name for offset of resource descriptor
                    )                      
    
                // Index 3
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO2",) {0}  // Pin 21 of JP1 (GPIO_S5[00])
                // Index 4
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO2",) {0} 
    
                // Index 5
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO2",) {1}  // Pin 23 of JP1 (GPIO_S5[01])
                // Index 6
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO2",) {1}
    
                // Index 7
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO2",) {2}  // Pin 25 of JP1 (GPIO_S5[02])
                // Index 8
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO2",) {2} 
    
                // Index 9
                UARTSerialBus(           // Pin 6, 8, 10, 12 of JP1, for SIO_UART1
                    115200,                // InitialBaudRate: in bits ber second
                    ,                      // BitsPerByte: default to 8 bits
                    ,                      // StopBits: Defaults to one bit
                    0xfc,                  // LinesInUse: 8 1-bit flags to declare line enabled
                    ,                      // IsBigEndian: default to LittleEndian
                    ,                      // Parity: Defaults to no parity
                    FlowControlHardware,   // FlowControl: Defaults to no flow control
                    32,                    // ReceiveBufferSize
                    32,                    // TransmitBufferSize
                    "\\_SB.URT1",          // ResourceSource: UART bus controller name
                    ,
                    ,
                    UAR1,              // DescriptorName: creates name for offset of resource descriptor
                    )  
    
                // Index 10
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {62}  // Pin 14 of JP1 (GPIO_SC[62])
                // Index 11
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {62} 

                // Index 12
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {63}  // Pin 16 of JP1 (GPIO_SC[63])
                // Index 13
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {63} 
    
                // Index 14
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {65}  // Pin 18 of JP1 (GPIO_SC[65])
                // Index 15
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {65} 
    
                // Index 16
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {64}  // Pin 20 of JP1 (GPIO_SC[64])
                // Index 17
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {64} 
    
                // Index 18
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {94}  // Pin 22 of JP1 (GPIO_SC[94])
                // Index 19
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {94} 
    
                // Index 20
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {95}  // Pin 24 of JP1 (GPIO_SC[95])
                // Index 21
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {95} 
    
                // Index 22
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {54}  // Pin 26 of JP1 (GPIO_SC[54])
                // Index 23
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {54}
            })
    
            Name(_DSD, Package() 
            {
                ToUUID("daffd814-6eba-4d8c-8a91-bc9bbf4aa301"),
                Package() 
                {
                    // SPI Mapping
                    Package(2) { "bus-SPI-SPI0", Package() { 0 }},

                    Package(2) { "SPI0-MinClockInHz", 100000 },
                    Package(2) { "SPI0-MaxClockInHz", 15000000 },
                    // SupportedDataBitLengths takes a list of support data bit length
                    // Example : Package(2) { "SPI0-SupportedDataBitLengths", Package() { 8, 7, 16 }},
                    Package(2) { "SPI0-SupportedDataBitLengths", Package() { 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32 }},
                     // I2C Mapping
                    Package(2) { "bus-I2C-I2C5", Package() { 1 }},
                    // UART Mapping
                    Package(2) { "bus-UART-UART2", Package() { 2 }},
                    Package(2) { "bus-UART-UART1", Package() { 9 }},
                }
            })
        }
    }
}
```

### <a name="appendix-c---sample-powershell-script-to-generate-gpio-resources"></a>附錄 C - 產生 GPIO 資源的範例 Powershell 指令碼

下列指令碼可用來產生 Raspberry Pi 的 GPIO 資源宣告︰

```
$pins = @(
    @{PinNumber=4;PullConfig='PullUp'},
    @{PinNumber=5;PullConfig='PullUp'},
    @{PinNumber=6;PullConfig='PullUp'},
    @{PinNumber=12;PullConfig='PullDown'},
    @{PinNumber=13;PullConfig='PullDown'},
    @{PinNumber=16;PullConfig='PullDown'},
    @{PinNumber=18;PullConfig='PullDown'},
    @{PinNumber=22;PullConfig='PullDown'},
    @{PinNumber=23;PullConfig='PullDown'},
    @{PinNumber=24;PullConfig='PullDown'},
    @{PinNumber=25;PullConfig='PullDown'},
    @{PinNumber=26;PullConfig='PullDown'},
    @{PinNumber=27;PullConfig='PullDown'},
    @{PinNumber=35;PullConfig='PullUp'},
    @{PinNumber=47;PullConfig='PullUp'})
    
# generate the resources
$FIRST_RESOURCE_INDEX = 4
$resourceIndex = $FIRST_RESOURCE_INDEX
$pins | % {
    $a = @"
// Index $resourceIndex - GPIO $($_.PinNumber) - $($_.Name)
GpioIO(Shared, $($_.PullConfig), , , , "\\_SB.GPI0", , , , ) { $($_.PinNumber) }
GpioInt(Edge, ActiveBoth, Shared, $($_.PullConfig), 0, "\\_SB.GPI0",) { $($_.PinNumber) }
"@    
    Write-Host $a
    $resourceIndex += 2;
}
```
