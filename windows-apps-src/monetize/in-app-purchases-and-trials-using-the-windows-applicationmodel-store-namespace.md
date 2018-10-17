---
author: Xansky
ms.assetid: 32572890-26E3-4FBB-985B-47D61FF7F387
description: 了解如何在目標為 Windows10 版本 1607 之前版本的 UWP app 中啟用 App 內購買和試用版。
title: 使用 Windows.ApplicationModel.Store 命名空間的 App 內購買和試用版
ms.author: mhopkins
ms.date: 08/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: UWP, App 內購買, IAP, 附加元件, 試用版, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: bb2c242ea4b7e3881751874c096165279854aa5e
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2018
ms.locfileid: "4692441"
---
# <a name="in-app-purchases-and-trials-using-the-windowsapplicationmodelstore-namespace"></a>使用 Windows.ApplicationModel.Store 命名空間的 App 內購買和試用版

您可以使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間中的成員，將 App 內購買和試用版功能新增到通用 Windows 平台 (UWP) App，以協助您的 App 獲利。 這些 API 也會提供對您 App 授權資訊的存取權。

本節中的文章針對數個常見的案例，提供使用 **Windows.ApplicationModel.Store** 命名空間中成員的深入指引及程式碼範例。 如需 UWP app 中 App 內購買相關基本概念的概觀，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md)。 如需示範如何使用 **Windows.ApplicationModel.Store** 命名空間來實作試用版和 App 內購買的完整範例，請參閱[ Microsoft Store 範例](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)。

> [!IMPORTANT]
> **Windows.ApplicationModel.Store**命名空間不再以新功能更新。 如果您專案的目標為 Visual Studio 中的 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本 (也就是，您以 Windows 10 版本 1607 或更新版本為目標)，建議您改用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間。 如需詳細資訊，請參閱 [App 內購買和試用版](https://msdn.microsoft.com/windows/uwp/monetize/in-app-purchases-and-trials)。 **Windows.ApplicationModel.Store** 命名空間不受支援於使用[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)之 Windows 傳統型應用程式，或應用程式，或使用開發人員中心之開發沙箱遊戲的應用程式或遊戲 (例如與 Xbox Live 整合的任何遊戲)。 這些產品必須使用 **Windows.Services.Store** 命名空間來實作 App 內購買和試用版。

## <a name="get-started-with-the-currentapp-and-currentappsimulator-classes"></a>開始使用 CurrentApp 和 CurrentAppSimulator 類別

**Windows.ApplicationModel.Store** 命名空間的主要進入點是 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx) 類別。 這個類別提供靜態屬性和一些方法，可供您用來取得目前應用程式及其可用附加元件的資訊、取得目前應用程式或其附加元件的授權資訊、為目前的使用者購買應用程式或附加元件，以及執行其他工作。

[CurrentApp](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx) 類別會從 Microsoft Store 取得其資料，因此您必須有開發人員帳戶，而且必須先在 Microsoft Store 中發行您的 App，才能順利在 App 中使用這個類別。 將 App 提交到 Microsoft Store 之前，可以使用這個類別的模擬版本 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx) 測試您的程式碼。 測試 App 後，在將它提交到 Microsoft Store 之前，您必須將所有 **CurrentAppSimulator** 取代為 **CurrentApp**。 如果您的 App 使用 **CurrentAppSimulator**，將無法通過認證。

使用 **CurrentAppSimulator** 時，是在開發電腦上名為 WindowsStoreProxy.xml 的本機檔案中描述 App 授權以及應用程式內產品的初始狀態。 如需有關此檔案的詳細資訊，請參閱[使用 WindowsStoreProxy.xml 檔案搭配 CurrentAppSimulator](#proxy)。

如需有關您可以使用 **CurrentApp** 和 **CurrentAppSimulator** 來執行之常見工作的詳細資訊，請參閱下列文章。

| 主題       | 描述                 |
|----------------------------|-----------------------------|
| [在試用版本中排除或限制某些功能](exclude-or-limit-features-in-a-trial-version-of-your-app.md) | 如果您讓客戶在試用期間免費使用 App，您可以在試用期間排除或限制某些功能，吸引客戶升級成完整版的 App。 |
| [啟用應用程式內產品購買](enable-in-app-product-purchases.md)      |  無論您的 app 是否免費，都可以直接從 app 內銷售內容、其他 app 或新的 app 功能 (例如解除鎖定遊戲的下一個關卡)。 以下示範如何在 App 中啟用這些產品。  |
| [啟用消費性應用程式內產品購買](enable-consumable-in-app-product-purchases.md)      | 您可以透過 Microsoft Store 商業平台提供消費性的應用程式內產品 (亦即可購買、使用，然後再次購買的項目)，為客戶提供既健全又可靠的購買體驗。 這對於像遊戲內貨幣 (金幣、錢幣等) 這種可在買來後用來購買特定火力升級配備的東西，特別有用。 |
| [管理大型的應用程式內產品型錄](manage-a-large-catalog-of-in-app-products.md)      |   如果您的 App 提供大型的應用程式內產品型錄，您可以選擇性地依照本主題中描述的程序來協助管理型錄。    |
| [使用收據來驗證產品購買](use-receipts-to-verify-product-purchases.md)      |   成功購買產品時所產生的每個 Microsoft Store 交易可以選擇性地傳回交易收據，以便提供所列出產品的相關資訊與客戶的貨幣成本。 如果您的 App 需要確認使用者已購買 App，或是已從 Microsoft Store 進行應用程式內產品購買，這項資訊的存取將可支援這些情況。 |

<span id="proxy" />

## <a name="using-the-windowsstoreproxyxml-file-with-currentappsimulator"></a>使用 WindowsStoreProxy.xml 檔案搭配 CurrentAppSimulator

使用 **CurrentAppSimulator** 時，是在開發電腦上名為 WindowsStoreProxy.xml 的本機檔案中描述 App 授權以及應用程式內產品的初始狀態。 更改 App 狀態的 **CurrentAppSimulator** 方法 (例如購買授權或處理 App 內購買) 只會更新記憶體中 **CurrentAppSimulator** 物件的狀態。 不會變更 WindowsStoreProxy.xml 的內容。 當 App 重新啟動時，授權狀態會還原到 WindowsStoreProxy.xml 中所描述的狀態。

WindowsStoreProxy.xml 檔案預設會建立在下列位置︰%UserProfile%\AppData\Local\Packages\\&lt;應用程式套件資料夾&gt;\LocalState\Microsoft\Windows Store\ApiData。 您可以編輯此檔案，在 **CurrentAppSimulator** 屬性中定義您想要模擬的案例。

雖然您可以修改此檔案中的值，但我們還是建議您建立自己的 WindowsStoreProxy.xml 檔案 (在 Visual Studio 專案的資料資料夾) 來改用 **CurrentAppSimulator**。 模擬交易時，請呼叫 [ReloadSimulatorAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.reloadsimulatorasync) 載入您的檔案。 如果您沒有呼叫 **ReloadSimulatorAsync** 載入自己的 WindowsStoreProxy.xml 檔案，**CurrentAppSimulator** 會建立/載入 (但不覆寫) 預設的 WindowsStoreProxy.xml 檔案。

> [!NOTE]
> 請注意 **CurrentAppSimulator** 必須要到 **ReloadSimulatorAsync** 完成之後才會完全初始化。 而且由於 **ReloadSimulatorAsync** 是非同步方法，所以必須小心避免發生在一個執行緒上查詢 **CurrentAppSimulator** 時，同時在另一個執行緒上進行初始化的競爭情形。 有一個技巧是使用旗標，指出初始化已完成。 從 Microsoft Store 安裝的 App 必須使用 **CurrentApp** 而不是 **CurrentAppSimulator**，此時不會呼叫 **ReloadSimulatorAsync**，因此不會發生剛才所提到的競爭情形。 基於這個原因，請設計您的程式碼能在這兩種情況下非同步和同步運作。


<span id="proxy-examples" />

### <a name="examples"></a>範例

此範例是一個 WindowsStoreProxy.xml 檔案 (UTF-16 編碼)，說明試用模式的 App 在 2015 年 1 月 19 日的 05:00 (UTC) 到期。

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="UTF-16"?>
<CurrentApp>
  <ListingInformation>
    <App>
      <AppId>2B14D306-D8F8-4066-A45B-0FB3464C67F2</AppId>
      <LinkUri>http://apps.windows.microsoft.com/app/2B14D306-D8F8-4066-A45B-0FB3464C67F2</LinkUri>
      <CurrentMarket>en-US</CurrentMarket>
      <AgeRating>3</AgeRating>
      <MarketData xml:lang="en-us">
        <Name>App with a trial license</Name>
        <Description>Sample app for demonstrating trial license management</Description>
        <Price>4.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </App>
  </ListingInformation>
  <LicenseInformation>
    <App>
      <IsActive>true</IsActive>
      <IsTrial>true</IsTrial>
      <ExpirationDate>2015-01-19T05:00:00.00Z</ExpirationDate>
    </App>
  </LicenseInformation>
  <Simulation SimulationMode="Automatic">
    <DefaultResponse MethodName="LoadListingInformationAsync_GetResult" HResult="E_FAIL"/>
  </Simulation>
</CurrentApp>
```

下一個範例也是一個 WindowsStoreProxy.xml 檔案 (UTF-16 編碼)，說明已購買的 App 有一個功能在 2015 年 1 月 19 日的 05:00 (UTC) 到期，而且有一個消費性 App 內購買。

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="utf-16" ?>
<CurrentApp>
  <ListingInformation>
    <App>
      <AppId>988b90e4-5d4d-4dea-99d0-e423e414ffbc</AppId>
      <LinkUri>http://apps.windows.microsoft.com/app/988b90e4-5d4d-4dea-99d0-e423e414ffbc</LinkUri>
      <CurrentMarket>en-us</CurrentMarket>
      <AgeRating>3</AgeRating>
      <MarketData xml:lang="en-us">
        <Name>App with several in-app products</Name>
        <Description>Sample app for demonstrating an expiring in-app product and a consumable in-app product</Description>
        <Price>5.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </App>
    <Product ProductId="feature1" LicenseDuration="10" ProductType="Durable">
      <MarketData xml:lang="en-us">
        <Name>Expiring Item</Name>
        <Price>1.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </Product>
    <Product ProductId="consumable1" LicenseDuration="0" ProductType="Consumable">
      <MarketData xml:lang="en-us">
        <Name>Consumable Item</Name>
        <Price>2.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </Product>
  </ListingInformation>
  <LicenseInformation>
    <App>
      <IsActive>true</IsActive>
      <IsTrial>false</IsTrial>
    </App>
    <Product ProductId="feature1">
      <IsActive>true</IsActive>
      <ExpirationDate>2015-01-19T00:00:00.00Z</ExpirationDate>
    </Product>
  </LicenseInformation>
  <ConsumableInformation>
    <Product ProductId="consumable1" TransactionId="00000001-0000-0000-0000-000000000000" Status="Active"/>
  </ConsumableInformation>
</CurrentApp>
```


<span id="proxy-schema" />

### <a name="schema"></a>結構描述

本節列出定義 WindowsStoreProxy.xml 檔案結構的 XSD 檔案。 若要在使用 WindowsStoreProxy.xml 檔案時將這個結構描述套用到 Visual Studio 中的 XML 編輯器，請執行下列動作︰

1. 在 Visual Studio 中開啟 WindowsStoreProxy.xml 檔案。
2. 在 **\[XML\]** 功能表中，按一下 **\[建立結構描述\]**。 這會根據 XML 檔案的內容建立一個暫時的 WindowsStoreProxy.xsd 檔案。
3. 使用下面的結構描述取代該 .xsd 檔案的內容。
4. 將檔案儲存到您可以將它套用到多個應用程式專案的位置。
5. 在 Visual Studio 中切換到您的 WindowsStoreProxy.xml 檔案。
6. 在 **\[XML\]** 功能表中，按一下 **\[結構描述\]**，然後在清單中找出 WindowsStoreProxy.xsd 檔案的那一列。 如果檔案的位置不是您想要的位置 (例如，如果仍然顯示暫存檔案)，請按一下 **\[加入\]**。 導覽到正確的檔案，然後按一下 **\[確定\]**。 現在，您應該會在清單中看到該檔案。 確認該結構描述的 **\[使用\]** 欄中出現核取記號。

完成這個動作之後，您對 WindowsStoreProxy.xml 進行的編輯就會依照結構描述。 如需詳細資訊，請參閱[做法︰選取要使用的 XML 結構描述](http://go.microsoft.com/fwlink/p/?LinkId=403014)。

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:import namespace="http://www.w3.org/XML/1998/namespace"/>
  <xs:element name="CurrentApp" type="CurrentAppDefinition"></xs:element>
  <xs:complexType name="CurrentAppDefinition">
    <xs:sequence>
      <xs:element name="ListingInformation" type="ListingDefinition" minOccurs="1" maxOccurs="1"/>
      <xs:element name="LicenseInformation" type="LicenseDefinition" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ConsumableInformation" type="ConsumableDefinition" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Simulation" type="SimulationDefinition" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
  </xs:complexType>
  <xs:simpleType name="ResponseCodes">
    <xs:restriction base="xs:string">
      <xs:enumeration value="S_OK">
        <xs:annotation>
          <xs:documentation>0x00000000</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_INVALIDARG">
        <xs:annotation>
          <xs:documentation>0x80070057</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_CANCELLED">
        <xs:annotation>
          <xs:documentation>0x800704C7</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_FAIL">
        <xs:annotation>
          <xs:documentation>0x80004005</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_OUTOFMEMORY">
        <xs:annotation>
          <xs:documentation>0x8007000E</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="ERROR_ALREADY_EXISTS">
        <xs:annotation>
          <xs:documentation>0x800700B7</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="ConsumableStatus">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Active"/>
      <xs:enumeration value="PurchaseReverted"/>
      <xs:enumeration value="PurchasePending"/>
      <xs:enumeration value="ServerError"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="StoreMethodName">
    <xs:restriction base="xs:string">
      <xs:enumeration value="RequestAppPurchaseAsync_GetResult" id="RPPA"/>
      <xs:enumeration value="RequestProductPurchaseAsync_GetResult" id="RFPA"/>
      <xs:enumeration value="LoadListingInformationAsync_GetResult" id="LLIA"/>
      <xs:enumeration value="ReportConsumableFulfillmentAsync_GetResult" id="RPFA"/>
      <xs:enumeration value="LoadListingInformationByKeywordsAsync_GetResult" id="LLIKA"/>
      <xs:enumeration value="LoadListingInformationByProductIdAsync_GetResult" id="LLIPA"/>
      <xs:enumeration value="GetUnfulfilledConsumablesAsync_GetResult" id="GUC"/>
      <xs:enumeration value="GetAppReceiptAsync_GetResult" id="GARA"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="SimulationMode">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Interactive"/>
      <xs:enumeration value="Automatic"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="ListingDefinition">
    <xs:sequence>
      <xs:element name="App" type="AppListingDefinition"/>
      <xs:element name="Product" type="ProductListingDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="ConsumableDefinition">
    <xs:sequence>
      <xs:element name="Product" type="ConsumableProductDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="AppListingDefinition">
    <xs:sequence>
      <xs:element name="AppId" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="LinkUri" type="xs:anyURI" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrentMarket" type="xs:language" minOccurs="1" maxOccurs="1"/>
      <xs:element name="AgeRating" type="xs:unsignedInt" minOccurs="1" maxOccurs="1"/>
      <xs:element name="MarketData" type="MarketSpecificAppData" minOccurs="1" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="MarketSpecificAppData">
    <xs:sequence>
      <xs:element name="Name" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Description" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Price" type="xs:float" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencySymbol" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencyCode" type="xs:string" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute ref="xml:lang" use="required"/>
  </xs:complexType>
  <xs:complexType name="MarketSpecificProductData">
    <xs:sequence>
      <xs:element name="Name" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Price" type="xs:float" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencySymbol" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencyCode" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Description" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Tag" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Keywords" type="KeywordDefinition" minOccurs="0" maxOccurs="1"/>
      <xs:element name="ImageUri" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute ref="xml:lang" use="required"/>
  </xs:complexType>
  <xs:complexType name="ProductListingDefinition">
    <xs:sequence>
      <xs:element name="MarketData" type="MarketSpecificProductData" minOccurs="1" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="ProductId" use="required">
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:maxLength value="100"/>
          <xs:pattern value="[^,]*"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="LicenseDuration" type="xs:integer" use="optional"/>
    <xs:attribute name="ProductType" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:simpleType name="guid">
    <xs:restriction base="xs:string">
      <xs:pattern value="[\da-fA-F]{8}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{12}"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="ConsumableProductDefinition">
    <xs:attribute name="ProductId" use="required">
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:maxLength value="100"/>
          <xs:pattern value="[^,]*"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="TransactionId" type="guid" use="required"/>
    <xs:attribute name="Status" type="ConsumableStatus" use="required"/>
    <xs:attribute name="OfferId" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:complexType name="LicenseDefinition">
    <xs:sequence>
      <xs:element name="App" type="AppLicenseDefinition"/>
      <xs:element name="Product" type="ProductLicenseDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="AppLicenseDefinition">
    <xs:sequence>
      <xs:element name="IsActive" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="IsTrial" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ExpirationDate" type="xs:dateTime" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="ProductLicenseDefinition">
    <xs:sequence>
      <xs:element name="IsActive" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ExpirationDate" type="xs:dateTime" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute name="ProductId" type="xs:string" use="required"/>
    <xs:attribute name="OfferId" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:complexType name="SimulationDefinition" >
    <xs:sequence>
      <xs:element name="DefaultResponse" type="DefaultResponseDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="SimulationMode" type="SimulationMode" use="optional"/>
  </xs:complexType>
  <xs:complexType name="DefaultResponseDefinition">
    <xs:attribute name="MethodName" type="StoreMethodName" use="required"/>
    <xs:attribute name="HResult" type="ResponseCodes" use="required"/>
  </xs:complexType>
  <xs:complexType name="KeywordDefinition">
    <xs:sequence>
      <xs:element name="Keyword" type="xs:string" minOccurs="0" maxOccurs="10"/>
    </xs:sequence>
  </xs:complexType>
</xs:schema>
```


<span id="proxy-descriptions" />

### <a name="element-and-attribute-descriptions"></a>元素和屬性描述

本節說明 WindowsStoreProxy.xml 檔案中的元素和屬性。

此檔案的根元素是 **CurrentApp** 元素，代表目前的 App。 此元素包含下列子項元素。

|  元素  |  必要  |  數量  |  描述   |
|-------------|------------|--------|--------|
|  [ListingInformation](#listinginformation)  |    是        |  1  |  包含 App 清單中的資料。            |
|  [LicenseInformation](#licenseinformation)  |     是       |   1    |   描述此 App 和其耐久性附加元件可使用的授權。     |
|  [ConsumableInformation](#consumableinformation)  |      否      |   0 或 1   |   描述此 App 可使用的消費性附加元件。      |
|  [Simulation](#simulation)  |     否       |      0 或 1      |   描述在測試期間，各種 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx) 方法的呼叫在 App 中如何運作。    |

<span id="listinginformation" />

#### <a name="listinginformation-element"></a>ListingInformation 元素

此元素包含 App 清單中的資料。 **ListingInformation** 是 **CurrentApp** 元素必要的子項。

**ListingInformation** 包含下列子項元素。

|  元素  |  必要  |  數量  |  描述   |
|-------------|------------|--------|--------|
|  [App](#app-child-of-listinginformation)  |    是   |  1   |    提供有關 App 的資料。         |
|  [Product](#product-child-of-listinginformation)  |    否  |  0 或以上   |      描述 App 的附加元件。     |     |

<span id="app-child-of-listinginformation"/>

#### <a name="app-element-child-of-listinginformation"></a>App 元素 (ListingInformation 的子項)

此元素描述 App 的授權。 **App** 是 [ListingInformation](#listinginformation) 元素必要的子項。

**App** 包含下列子項元素。

|  元素  |  必要  |  數量  | 描述   |
|-------------|------------|--------|--------|
|  **AppId**  |    是   |  1   |   識別 Microsoft Store 中 App 的 GUID。 測試時可以是任何 GUID。        |
|  **LinkUri**  |    是  |  1   |    Microsoft Store 清單頁面的 URI。 測試時可以是任何有效的 URI。         |
|  **CurrentMarket**  |    是  |  1   |    客戶的國家/地區。         |
|  **AgeRating**  |    是  |  1   |     表示 App 最小年齡分級的整數。 此值與您在提交 App 時在開發人員中心儀表板中指定的值相同。 Microsoft Store 所使用的值為︰3、7、12 和 16。 如需這些分級的詳細資訊，請參閱[年齡分級](../publish/age-ratings.md)。        |
|  [MarketData](#marketdata-child-of-app)  |    是  |  1 或以上      |    包含特定國家/地區的 App 相關資訊。 對於列出 App 的每個國家/地區，您必須各包含一個 **MarketData** 元素。       |    |

<span id="marketdata-child-of-app"/>

#### <a name="marketdata-element-child-of-app"></a>MarketData 元素 (App 的子項)

此元素提供特定國家/地區的 App 相關資訊。 對於列出 App 的每個國家/地區，您必須各包含一個 **MarketData** 元素。 **MarketData** 是 [App](#app-child-of-listinginformation) 元素必要的子項。

**MarketData** 包含下列子項元素。

|  元素  |  必要  |  數量  | 描述   |
|-------------|------------|--------|--------|
|  **Name**  |    是   |  1   |   在此國家/地區的 App 名稱。        |
|  **Description**  |    是  |  1   |      用於此國家/地區的 App 描述。       |
|  **Price**  |    是  |  1   |     在此國家/地區的 App 價格。        |
|  **CurrencySymbol**  |    是  |  1   |     在此國家/地區中使用的貨幣符號。        |
|  **CurrencyCode**  |    否  |  0 或 1      |      在此國家/地區中使用的貨幣代碼。         |  |

**MarketData** 具有下列屬性。

|  屬性  |  必要  |  描述   |
|-------------|------------|----------------|
|  **xml: lang**  |    是        |     指定市場資料資訊適用的國家/地區。          |  |

<span id="product-child-of-listinginformation"/>

#### <a name="product-element-child-of-listinginformation"></a>Product 元素 (ListingInformation 的子項)

此元素描述 App 的附加元件。 **Product** 是 [ListingInformation](#listinginformation) 元素選用的子項，包含一或多個 [MarketData](#marketdata-child-of-product) 元素。

**Product** 具有下列屬性。

|  屬性  |  必要  |  描述   |
|-------------|------------|----------------|
|  **ProductId**  |    是        |    包含 App 用來識別附加元件的字串。           |
|  **LicenseDuration**  |    否        |    指示授權在項目購買之後將會一直有效的天數。 由產品購買建立之新授權的到期日期是購買日期加上授權持續時間。 只有當 **ProductType** 屬性是 **Durable** 時，才會使用這個屬性；消費性附加元件會忽略這個屬性。           |
|  **ProductType**  |    否        |    包含可識別應用程式內產品持續性的值。 支援的值為 **Durable** (預設值) 和 **Consumable**。 如果是耐久性類型，[LicenseInformation](#licenseinformation) 下的 [Product](#product-child-of-licenseinformation) 元素會描述額外資訊，如果是消費性類型，則是在 [ConsumableInformation](#consumableinformation) 下的 [Product](#product-child-of-consumableinformation) 元素描述額外資訊。           |  |

<span id="marketdata-child-of-product"/>

#### <a name="marketdata-element-child-of-product"></a>MarketData 元素 (Product 的子項)

此元素提供特定國家/地區的附加元件相關資訊。 對於列出附加元件的每個國家/地區，您必須各包含一個 **MarketData** 元素。 **MarketData** 是 [Product](#product-child-of-listinginformation) 元素必要的子項。

**MarketData** 包含下列子項元素。

|  元素  |  必要  |  數量  | 描述   |
|-------------|------------|--------|--------|
|  **Name**  |    是   |  1   |   在此國家/地區的附加元件名稱。        |
|  **Price**  |    是  |  1   |     在此國家/地區的附加元件價格。        |
|  **CurrencySymbol**  |    是  |  1   |     在此國家/地區中使用的貨幣符號。        |
|  **CurrencyCode**  |    否  |  0 或 1      |      在此國家/地區中使用的貨幣代碼。         |  
|  **Description**  |    否  |   0 或 1   |      用於此國家/地區的附加元件描述。       |
|  **Tag**  |    否  |   0 或 1   |      附加元件的[自訂開發人員資料](../publish/enter-add-on-properties.md#custom-developer-data) (也稱為標記)。       |
|  **Keywords**  |    否  |   0 或 1   |      最多可以有 10 個 **Keyword** 元素，包含附加元件的[關鍵字](../publish/enter-add-on-properties.md#keywords)。       |
|  **ImageUri**  |    否  |   0 或 1   |      附加元件清單中[影像的 URI](../publish/create-add-on-store-listings.md#icon)。           |  |

**MarketData** 具有下列屬性。

|  屬性  |  必要  |  描述   |
|-------------|------------|----------------|
|  **xml: lang**  |    是        |     指定市場資料資訊適用的國家/地區。          |  |

<span id="licenseinformation"/>

#### <a name="licenseinformation-element"></a>LicenseInformation 元素

此元素描述此 App 和其耐久性應用程式內產品可使用的授權。 **LicenseInformation** 是 **CurrentApp** 元素必要的子項。

**LicenseInformation** 包含下列子項元素。

|  元素  |  必要  |  數量  | 描述   |
|-------------|------------|--------|--------|
|  [App](#app-child-of-licenseinformation)  |    是   |  1   |    描述 App 的授權。         |
|  [Product](#product-child-of-licenseinformation)  |    否  |  0 或以上   |      描述 App 中耐久性附加元件的授權狀態。         |   |

下表顯示如何結合 **App** 和 **Product** 元素下的值來模擬一些常見的情形。

|  模擬的情形  |  IsActive  |  IsTrial  | ExpirationDate   |
|-------------|------------|--------|--------|
|  完整授權  |    true   |  false  |    不存在。 它實際上可以存在並指定未來的日期，但建議您省略 XML 檔案的元素。 如果它存在，而且指定過去的日期，則 **IsActive** 會被忽略並視為 false。          |
|  在試用期間  |    true  |  true   |      &lt;未來的日期時間&gt; 此元素必須存在，因為 **IsTrial** 為 true。 您可以造訪顯示目前國際標準時間 (UTC) 的網站，來得知要設定的未來時間，以取得您想要的剩餘試用期間。         |
|  試用版已到期  |    false  |  true   |      &lt;過去的日期時間&gt; 此元素必須存在，因為 **IsTrial** 為 true。 您可以造訪顯示目前國際標準時間 (UTC) 的網站，得知「過去」的時間 (以 UTC 表示)。         |
|  無效  |    false  | false       |     &lt;任何值或省略&gt;          |  |

<span id="app-child-of-licenseinformation"/>

#### <a name="app-element-child-of-licenseinformation"></a>App 元素 (LicenseInformation 的子項)

此元素描述 App 的授權。 **App** 是 [LicenseInformation](#licenseinformation) 元素必要的子項。

**App** 包含下列子項元素。

|  元素  |  必要  |  數量  | 描述   |
|-------------|------------|--------|--------|
|  **IsActive**  |    是   |  1   |    描述此 App 目前的授權狀態。 值 **true** 表示授權有效；**false** 表示無效的授權。 無論 App 是否有試用模式，此值通常為 **true**。  將此值設定為 **false** 可測試您的 App 在授權無效時是如何運作。           |
|  **IsTrial**  |    是  |  1   |      描述此 App 目前的試用狀態。 值 **true** 表示 App 正在試用期間；**false** 表示 App 不在試用，可能因為已購買 App，或試用期間已到期。         |
|  **ExpirationDate**  |    否  |  0 或 1       |     此 App 到期的試用日期，以國際標準時間 (UTC) 表示。 日期的格式必須為︰yyyy-mm-ddThh:mm:ss.ssZ。 例如，2015 年 1 月 19 日 05:00 要指定為 2015-01-19T05:00:00.00Z。 當 **IsTrial** 為 **true** 時，這是必要的元素。 否則就不需要。          |  |

<span id="product-child-of-licenseinformation"/>

#### <a name="product-element-child-of-licenseinformation"></a>Product 元素 (LicenseInformation 的子項)

此元素描述 App 中耐久性附加元件的授權狀態。 **Product** 是 [LicenseInformation](#licenseinformation) 元素選用的子項。

**Product** 包含下列子項元素。

|  元素  |  必要  |  數量  | 描述   |
|-------------|------------|--------|--------|
|  **IsActive**  |    是   |  1     |    描述此附加元件目前的授權狀態。 值 **true** 表示附加元件可以使用；**false** 表示附加元件無法使用或尚未購買           |
|  **ExpirationDate**  |    否   |  0 或 1     |     附加元件到期的日期，以國際標準時間 (UTC) 表示。 日期的格式必須為︰yyyy-mm-ddThh:mm:ss.ssZ。 例如，2015 年 1 月 19 日 05:00 要指定為 2015-01-19T05:00:00.00Z。 如果此元素存在，附加元件就會有到期日期。 如果不存在，附加元件就不會過期。  |  

**Product** 具有下列屬性。

|  屬性  |  必要  |  描述   |
|-------------|------------|----------------|
|  **ProductId**  |    是        |   包含 App 用來識別附加元件的字串。            |
|  **OfferId**  |     否       |   包含 App 用來識別附加元件所屬類別的字串。 這可以對大型的項目型錄提供支援，如[管理大型的應用程式內產品型錄](manage-a-large-catalog-of-in-app-products.md)中所述。           |

<span id="simulation"/>

#### <a name="simulation-element"></a>Simulation 元素

此元素描述在測試期間，各種 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx) 方法的呼叫在 App 中如何運作。 **Simulation** 是 **CurrentApp** 元素選用的子項，包含零或多個 [DefaultResponse](#defaultresponse) 元素。

**Simulation** 具有下列屬性。

|  屬性  |  必要  |  描述   |
|-------------|------------|----------------|
|  **SimulationMode**  |    否        |      值可以是 **Interactive** 或 **Automatic**。 當此屬性設為 **Automatic** 時，方法會自動傳回指定的 HRESULT 錯誤碼。 這可以在執行自動的測試案例時使用。       |

<span id="defaultresponse"/>

#### <a name="defaultresponse-element"></a>DefaultResponse 元素

此元素描述 **CurrentAppSimulator** 方法傳回的預設錯誤碼。 **DefaultResponse** 是 [Simulation](#simulation) 元素選用的子項。

**DefaultResponse** 具有下列屬性。

|  屬性  |  必要  |  描述   |
|-------------|------------|----------------|
|  **MethodName**  |    是        |   對此屬性指派[結構描述](#schema)中 **StoreMethodName** 類型所顯示的其中一個列舉值。 這些列舉值代表在測試期間，您想要模擬 App 中錯誤碼傳回值的 **CurrentAppSimulator** 方法。 例如，值 **RequestAppPurchaseAsync_GetResult** 表示您想要模擬 [RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.requestapppurchaseasync) 方法的錯誤碼傳回值。            |
|  **HResult**  |     是       |   對此屬性指派[結構描述](#schema)中 **ResponseCodes** 類型所顯示的其中一個列舉值。 這些列舉值代表您想要為已指派給此 **DefaultResponse** 元素之 **MethodName** 屬性的方法傳回的錯誤碼。           |

<span id="consumableinformation"/>

#### <a name="consumableinformation-element"></a>ConsumableInformation 元素

此元素描述此 App 可使用的消費性附加元件。 **ConsumableInformation** 是 **CurrentApp** 元素選用的子項，可以包含零或多個 [Product](#product-child-of-consumableinformation) 元素。

<span id="product-child-of-consumableinformation"/>

#### <a name="product-element-child-of-consumableinformation"></a>Product 元素 (ConsumableInformation 的子項)

此元素描述消費性附加元件。 **Product** 是 [ConsumableInformation](#consumableinformation) 元素選用的子項。

**Product** 具有下列屬性。

|  屬性  |  必要  |  描述   |
|-------------|------------|----------------|
|  **ProductId**  |    是        |   包含 App 用來識別消費性附加元件的字串。            |
|  **TransactionId**  |     是       |   包含 App 用來追蹤整個履行程序之消費性產品的購買交易的 GUID (做為字串)。 請參閱[啟用消費性應用程式內產品購買](enable-consumable-in-app-product-purchases.md)。            |
|  **Status**  |      是      |  包含 App 用來表示消費性產品之履行狀態的字串。 值可以是 **Active**、**PurchaseReverted**、**PurchasePending** 或 **ServerError**。             |
|  **OfferId**  |     否       |    包含 App 用來識別消費性產品所屬類別的字串。 這可以對大型的項目型錄提供支援，如[管理大型的應用程式內產品型錄](manage-a-large-catalog-of-in-app-products.md)中所述。           |
