---
ms.assetid: 4A4C2802-E674-4C04-8A6D-D7C1BBF1BD20
title: 裝置資訊屬性
description: 每個裝置都具有相關聯的 DeviceInformation 屬性，您可以在需要特定資訊或正在建置裝置選取器時使用。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5834a798509d3a0910572b3d7f50ad8eda815998
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684816"
---
# <a name="device-information-properties"></a>裝置資訊屬性



**重要 API**

- [**Windows. 列舉**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)

每個裝置都具有相關聯的 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 屬性，您可以在需要特定資訊或正在建置裝置選取器時使用。 這些屬性可以指定為 AQS 篩選器來限制您正在列舉的裝置，以便尋找具有指定特性的裝置。 您也可以使用這些屬性，來指定要針對每個裝置傳回的資訊。 這可讓您指定要傳回給應用程式的裝置資訊。

如需在裝置選取器中使用 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 屬性的詳細資訊，請參閱[建置裝置選取器](build-a-device-selector.md)。 本主題說明如何要求資訊屬性，同時也會列出一些常見的屬性及其用途。

[  **DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 物件是由一個身分識別 ([**DeviceInformation.Id**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id))、一個類型 ([**DeviceInformation.Kind**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.kind)) 及一個屬性包 ([**DeviceInformation.Properties**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.properties)) 所組成。 **DeviceInformation** 物件的所有其他屬性均衍生自 **Properties** 屬性包。 例如，[**Name**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) 是衍生自 **System.ItemNameDisplay**。 這表示屬性包一律包含判斷其他屬性所需的資訊。

## <a name="requesting-properties"></a>正在要求屬性

[  **DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 物件擁有一些基本屬性，例如 [**Id**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 和 [**Kind**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.kind)，但是大部分的屬性會儲存在 [**Properties**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.properties) 底下的屬性包中。 因此，屬性包會包含用來從屬性包找出屬性的屬性。 例如，使用 [System.ItemNameDisplay](https://docs.microsoft.com/windows/desktop/properties/props-system-itemnamedisplay) 來找出 [**Name**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) 屬性。 這是具有使用者易記名稱的通用和已知屬性的案例。 Windows 提供數個使用者易記的名稱，讓查詢屬性更加容易。

當您正在要求屬性時，並不會受限於具有使用者易記名稱的通用屬性。 您可以指定基本的 GUID 和屬性識別碼 (PID) 來要求任何可用的屬性，甚至是個別裝置或驅動程式所提供的自訂屬性。 指定自訂屬性的格式是 "`{GUID} PID`"。 例如：「`{744e3bed-3684-4e16-9f8a-07953a8bf2ab} 7`」。 

> [!Note]
> 您可以在設備磁碟機的裝置屬性金鑰標頭檔中，找到屬性 Guid 的清單。

某些屬性在所有 [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationkind) 物件中是通用的，但大部分都是特定類型獨有的。 下列區段會列出一些按個別 **DeviceInformationKind** 排序的通用屬性。 如需不同類型如何彼此關聯的詳細資訊，請參閱 **DeviceInformationKind**。

## <a name="deviceinterface-properties"></a>DeviceInterface 屬性

**DeviceInterface** 是預設值，並且是 app 案例中最常使用的 [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationkind) 物件。 除非裝置 API 指出不同的特定 **DeviceInformationKind**，否則這是您應該使用的物件類型。

| 名稱                                  | 在工作列搜尋方塊中輸入    | 說明                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **System.object**        | GUID    | **DeviceInformationKind.DeviceContainer** 的身分識別，其中包含涵蓋這個 **DeviceInterface** 的 **Device**。 您可以將此值連同 **DeviceInformationKind.DeviceContainer** 傳遞至 [**CreateFromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createfromidasync)，以尋找適當的容器。                                                                                    |
| **InterfaceClassGuid** | GUID    | 此介面代表的介面類別 GUID。                                                                                                                                                                                                                                                                                                                                                       |
| **DeviceInstanceId**   | 字串  | 父項 **DeviceInformationKind.Device** 的身分識別。 您可以將此值連同 **DeviceInformationKind.Device** 傳遞至 [**CreateFromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createfromidasync)，以尋找適當的裝置。                                                                                                                                                                   |
| **InterfaceEnabled**   | 布林值 | 表示介面是否已啟用。 [**DeviceInformation. IsEnabled**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.isenabled)衍生自這個屬性。                                                                                                                                                                                                                                                           |
| **Glyphicon 新增**          | 字串  | 字符的圖示路徑。                                                                                                                                                                                                                                                                                                                                                                                  |
| **System.object**          | 布林值 | 表示這是否是 **System.Devices.InterfaceClassGuid** 的預設裝置。 這通常用於印表機。 由於有多個音訊預設值，因此這不適用於音訊。 使用 [**GetDefaultAudioRenderId**](https://docs.microsoft.com/uwp/api/windows.media.devices.mediadevice.getdefaultaudiorenderid) 或 [**GetDefaultAudioCaptureId**](https://docs.microsoft.com/uwp/api/windows.media.devices.mediadevice.getdefaultaudiocaptureid) 來取得音訊預設值。 |
| **[系統] 圖示**               | 字串  | 圖示路徑。                                                                                                                                                                                                                                                                                                                                                                                                |
| **ItemNameDisplay**            | 字串  | 裝置物件的最佳顯示名稱。                                                                                                                                                                                                                                                                                                                                                              |

 

## <a name="device-properties"></a>裝置屬性

| 名稱                                  | 在工作列搜尋方塊中輸入       | 說明                                                                                                                                                                                                                                                                              |
|---------------------------------------|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ClassGuid**          | GUID       | 裝置安裝期間使用的裝置類別。 如需詳細資訊，請參閱[裝置安裝類別](https://docs.microsoft.com/windows-hardware/drivers/install/device-setup-classes)。                                                                                                                                                            |
| **CompatibleIds**      | String\[\] | 裝置的相容識別碼。 Windows 判斷要安裝在裝置上的最佳驅動程式時會用到這些資訊。 如需詳細資訊，請參閱[相容識別碼](https://docs.microsoft.com/windows-hardware/drivers/install/compatible-ids)。                                                                                                |
| **System.object**        | GUID       | 包含此裝置之 **DeviceInformationKind.DeviceContainer** 的身分識別。 您可以將此值連同 **DeviceInformationKind.DeviceContainer** 傳遞至 [**CreateFromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createfromidasync)，以尋找適當的容器。          |
| **DeviceCapabilities** | UInt32     | CM\_DEVCAP\_X 功能旗標的位 OR，其定義于**CfgMgr32**中。 如需詳細資訊，請參閱[**DEVPKEY\_裝置\_功能**](https://docs.microsoft.com/windows-hardware/drivers/install/devpkey-device-capabilities)。                                                                                             |
| **DeviceHasProblem**   | 布林值    | 裝置目前出現問題且很可能無法正常運作。 這可能是因為過期、遺漏或不正確的驅動程式所導致。                                                                                                                                                |
| **DeviceInstanceId**   | 字串     | 裝置的身分識別。 這也會是 [**DeviceInformation.Id**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 的值。                                                                                                                                                                       |
| **Device.devicemanufacturer** | 字串     | 裝置的製造商。                                                                                                                                                                                                                                                          |
| **HardwareIds**        | String\[\] | 裝置的硬體識別碼。 Windows 在判斷要安裝的最佳驅動程式時會用到這些識別碼。 裝置廠商可以使用這個屬性，從他們的 app 中識別他們的裝置。 如需詳細資訊，請參閱[硬體識別碼](https://docs.microsoft.com/windows-hardware/drivers/install/hardware-ids)。                                         |
| **[System.object]**             | 字串     | 父項裝置的 [**DeviceInformation.Id**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id)。 這是指連線父項，不是 **DeviceContainer** 父項。                                                                                                                                 |
| **System. Devices**            | 布林值    | 表示裝置目前是否存在並可以使用。                                                                                                                                                                                                                         |
| **ItemNameDisplay**            | 字串     | 此裝置物件的最佳顯示名稱。 在此案例中，這不一定是最佳的使用者名稱。 藉由參考相關聯的 **DeviceContainer** 或 **DeviceInterface** 的 **System.ItemNameDisplay**，就能找到更有可能的使用者易記名稱候選項目。 |

 

## <a name="devicecontainer-properties"></a>DeviceContainer 屬性

| 名稱                              | 在工作列搜尋方塊中輸入       | 說明                                                                                                                                                        |
|-----------------------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **[系統]. [類別]**       | String\[\] | 裝置所屬類別的說明清單。 這份清單會以單數類別的形式提供。 例如，「顯示器」、「電話」或「音訊裝置」。  |
| **CategoryIds**    | String\[\] | 包含此裝置所屬類別的清單。 例如，**Audio.Headphone**、**Display.Monitor** 或 **Input.Gaming**。                                  |
| **CategoryPlural** | String\[\] | 裝置所屬類別的說明清單。 這份清單會以複數類別的形式提供。 例如，「顯示器」、「電話」或「音訊裝置」。 |
| **CompatibleIds**  | String\[\] | 所有子項 **DeviceInformationKind.Device** 物件的相容識別碼集合。                                                                       |
| **已連接的裝置**      | 布林值    | 表示裝置目前是否與系統連線。                                                                                          |
| **Glyphicon 新增**      | 字串     | 字符的圖示路徑。                                                                                                                                           |
| **HardwareIds**    | String\[\] | 所有子項 **DeviceInformationKind.Device** 物件的硬體識別碼集合。                                                                         |
| **[系統] 圖示**           | 字串     | 圖示路徑。                                                                                                                                                         |
| **System.webserver**   | 布林值    | 如果這個 **DeviceContainer** 代表系統本身則為 **true**；如果裝置不在系統內則為 **false**。                                              |
| **[系統]。製造商**   | 字串     | 裝置的製造商。                                                                                                                                    |
| **ModelName**      | 字串     | 裝置容器的型號名稱。                                                                                                                                |
| **系統。配對的**         | 布林值    | 表示目前已與系統配對的任何子項 **DeviceInformationKind.Device** 物件是無線還是網路裝置。             |
| **ItemNameDisplay**        | 字串     | 此裝置的最佳顯示名稱。                                                                                                                             |

 

## <a name="deviceinterfaceclass-properties"></a>DeviceInterfaceClass 屬性

| 名稱                       | 在工作列搜尋方塊中輸入   | 說明                            |
|----------------------------|--------|----------------------------------------|
| **ItemNameDisplay** | 字串 | 此裝置的最佳顯示名稱。 |

 

## <a name="devicepanel-properties"></a>DevicePanel 屬性

| 名稱                                            | 在工作列搜尋方塊中輸入    | 說明                                                                                                      |
|-------------------------------------------------|---------|------------------------------------------------------------------------------------------------------------------|
| **[PanelId]**                | 字串  | **DevicePanel**物件的識別碼。                                                                    |
| **[PanelGroup]**             | 字串  | 父**PanelGroup**的識別碼。                                                                      |
 
 
 
## <a name="associationendpoint-properties"></a>AssociationEndpoint 屬性

| 名稱                                  | 在工作列搜尋方塊中輸入       | 說明                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|---------------------------------------|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Aep. AepId**          | 字串     | 此裝置的身分識別。 這也會是 [**DeviceInformation.Id**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 的值。                                                                                                                                                                                                                                                                                                                                                                        |
| **Aep. CanPair**        | 布林值    | 表示裝置是否可與系統配對。 [**DeviceInformationPairing. CanPair**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationpairing.canpair)衍生自這個屬性。                                                                                                                                                                                                                                                                                                       |
| **Aep. Category**       | String\[\] | 裝置的所屬類別。 例如，印表機或相機。                                                                                                                                                                                                                                                                                                                                                                                                             |
| **Aep. ContainerId**    | GUID       | 父項 **AssociationEndpointContainer** 物件的識別碼。                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **Aep. DeviceAddress**  | 字串     | 裝置的位址。 如果裝置為網路裝置，則這會是 IP 位址。                                                                                                                                                                                                                                                                                                                                                                                                  |
| **Aep. Connectionmultiplexer.isconnected**    | 布林值    | 表示裝置目前是否與系統連線。                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **Aep. IsPaired**       | 布林值    | 表示裝置目前是否已配對。 [**DeviceInformationPairing. IsPaired**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationpairing.ispaired)衍生自這個屬性。                                                                                                                                                                                                                                                                                                                      |
| **Aep. IsPresent**      | 布林值    | 指出裝置目前是否存在，這表示該裝置存在且可透過網路或無線通訊協定來探索。 一旦裝置與系統配對後，系統便會快取裝置。 之後，查詢 **AssociationEndpoint** 物件時，系統便會自動探索裝置。 因此，您不能依賴只使用查詢來探索裝置，以指出裝置目前是否可供使用。 這就是為什麼這個屬性很重要的原因。 |
| **Aep。製造商**   | 字串     | 裝置的製造商。                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| **Aep. ModelId**        | GUID       | 裝置的型號識別碼。                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| **Aep. ModelName**      | 字串     | 裝置的型號名稱。                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **Aep. ProtocolId**     | GUID       | 表示用來探索此 **AssocationEndpoint** 裝置的通訊協定。                                                                                                                                                                                                                                                                                                                                                                                                            |
| **Aep. SignalStrength** | Int32      | 裝置的訊號強度。 這個屬性僅適用於某些通訊協定。                                                                                                                                                                                                                                                                                                                                                                                                |
| **ItemNameDisplay**            | 字串     | 裝置的最佳顯示名稱。                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

 

## <a name="associationendpointcontainer-properties"></a>AssociationEndpointContainer 屬性

| 名稱                                                | 在工作列搜尋方塊中輸入       | 說明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|-----------------------------------------------------|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **AepContainer 類別**          | String\[\] | 裝置的所屬類別。 例如，印表機或相機。                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| **AepContainer 的子系**            | String\[\] | 屬於這個容器之 **AssocationEndpoint** 物件的識別碼集合。                                                                                                                                                                                                                                                                                                                                                                                                                                |
| **AepContainer. CanPair**             | 布林值    | 表示其中一個子項 **AssociationEndpoint** 裝置是否可與系統配對。 [**DeviceInformationPairing. CanPair**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationpairing.canpair)衍生自這個屬性。                                                                                                                                                                                                                                                                                                       |
| **AepContainer. ContainerId**         | GUID       | 此裝置的身分識別。 這也會是 [**DeviceInformation.Id**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 的值 (但採用 GUID 形式)。                                                                                                                                                                                                                                                                                                                                                                                            |
| **AepContainer. IsPaired**            | 布林值    | 表示目前是否已與其中一個子項 **AssociationEndpoint** 裝置配對。 [**DeviceInformationPairing. IsPaired**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationpairing.ispaired)衍生自這個屬性。                                                                                                                                                                                                                                                                                                                      |
| **AepContainer. IsPresent**           | 布林值    | 指出其中一個子項 **AssociationEndpoint** 裝置目前是否存在，這表示該裝置存在且可透過網路或無線通訊協定來探索。 一旦裝置與系統配對後，系統便會快取裝置。 之後，查詢 **AssociationEndpoint** 物件時，系統便會自動探索裝置。 因此，您不能依賴只使用查詢來探索裝置，以指出裝置目前是否可供使用。 這就是為什麼這個屬性很重要的原因。 |
| **AepContainer。製造商**        | 字串     | 裝置的製造商。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| **AepContainer. ModelIds**            | String\[\] | 裝置的型號識別碼清單。 每個型號都是採用字串形式的 GUID。                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| **AepContainer. ModelName**           | 字串     | 裝置的型號名稱。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| **AepContainer. ProtocolIds**         | GUID\[\]   | 協助建置 **AssociationEndpointContainer** 物件的通訊協定識別碼清單。 請記住，**AssociationEndpointContainer** 裝置的建立方式是，針對同一個實體裝置收集所有透過不同通訊協定探索到的 **AssociationEndpoint** 裝置來建立。                                                                                                                                                                                                                           |
| **AepContainer. SupportedUriSchemes** | String\[\] | 此裝置支援的傳播 URI 配置清單。                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| **AepContainer. SupportsAudio**       | 布林值    | 表示此裝置是否支援音效傳播。                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| **AepContainer. SupportsImages**      | 布林值    | 表示此裝置是否支援影像傳播。                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| **AepContainer. SupportsVideo**       | 布林值    | 表示此裝置是否支援視訊傳播。                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| **ItemNameDisplay**                          | 字串     | 裝置的最佳顯示名稱。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |

 

## <a name="associationendpointservice-properties"></a>AssociationEndpointService 屬性

| 名稱                                            | 在工作列搜尋方塊中輸入    | 說明                                                                                                      |
|-------------------------------------------------|---------|------------------------------------------------------------------------------------------------------------------|
| **AepService. AepId**             | 字串  | 父項 **AssociationEndpoint** 物件的識別碼。                                                     |
| **AepService. ContainerId**       | GUID    | 父項 **AssociationEndpointContainer** 物件的識別碼。                                            |
| **AepService. ParentAepIsPaired** | 布林值 | 表示父項 **AssociationEndpoint** 物件是否已與系統配對。                           |
| **AepService. ProtocolId**        | GUID    | 用來探索此裝置的通訊協定身分識別。                                                           |
| **AepService. ServiceClassId**    | GUID    | 此裝置所代表的服務身分識別。                                                             |
| **AepService. ServiceId**         | 字串  | 此服務的身分識別。 這也會是 [**DeviceInformation.Id**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 的值。 |
| **ItemNameDisplay**                      | 字串  | 服務的最佳顯示名稱。                                                                           |
