---
ms.assetid: 25B18BA5-E584-4537-9F19-BB2C8C52DFE1
title: app 功能宣告
description: 功能必須在您的通用 Windows 平台 (UWP) app 的套件資訊清單中進行宣告，才能存取特定 API、資源 (例如圖片或音樂) 或裝置 (例如相機或麥克風)。
---
# app 功能宣告

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

功能必須在您的通用 Windows 平台 (UWP) app 的[套件資訊清單](https://msdn.microsoft.com/library/windows/apps/BR211474)中進行宣告，才能存取特定 API、資源 (例如圖片或音樂) 或裝置 (例如相機或麥克風)。

您可以藉由在 app 的[套件資訊清單](https://msdn.microsoft.com/library/windows/apps/BR211474)中宣告功能，來要求存取特定資源或 API。 您可以使用 Microsoft Visual Studio 中的[資訊清單設計工具](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230259.aspx)來宣告一般的功能，也可以手動新增。 如需詳細資訊，請參閱[如何在套件資訊清單中指定功能](https://msdn.microsoft.com/library/windows/apps/BR211477)。 請務必了解，當客戶從市集取得您的 app 時，會得知該 app 宣告的所有功能。 因此，請避免宣告您的 app 不需要的功能。

部分功能提供 app 存取*敏感資源*的權限。 這些資源會視為敏感資源是因為它們可以存取使用者的個人資料或使用者必須付費才能使用。 受設定 app 管理的隱私權設定可讓使用者動態控制敏感資源的存取權。 因此，您的 app 不會假設敏感資源可隨時使用這點非常重要。 如需存取敏感資源的詳細資訊，請參閱[隱私權感知 app 的指導方針](https://msdn.microsoft.com/library/windows/apps/Hh768223)。 提供應用程式*敏感資源*存取權的功能，其功能案例旁邊會加註星號 (\*)。

本文探討下面所述的四種功能類別。

-   適用於大部分常見 app 案例的一般用途功能。

-   可以讓您的應用程式存取周邊和內部裝置的裝置功能。

-   需要特殊公司帳戶以送出到「市集」來使用它們的特殊用途功能。 如需有關公司帳戶的詳細資訊，請參閱[帳戶類型、位置和費用](https://msdn.microsoft.com/library/windows/apps/JJ863494)。

-   只有 Microsoft 和其合作夥伴可以使用的受限制的功能。

## 一般用途功能

一般用途功能適用於大部分常見的應用程式案例。

<table>
        <thead>
            <tr>
                <th>功能案例</th>
                <th>功能使用方式</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>**音樂***</td>
                <td>
                    The **musicLibrary** capability provides programmatic access to the user's Music, allowing the app to enumerate and access all files in the library without user interaction. This capability is typically used in jukebox apps that make use of the entire Music library.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **musicLibrary** capability only when the scenarios for your app require programmatic access and can't be realized by using the **file picker**.

                    The **musicLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="musicLibrary"/&gt;
&lt;/功能&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**圖片***</td>
                <td>
                    **picturesLibrary** 功能提供以程式設計方式存取使用者 [圖片] 的功能，讓 app 不需要使用者互動，就能列舉和存取媒體櫃中的所有檔案。 這個功能通常用於相片應用程式，這類應用程式需要存取整個 [圖片] 媒體櫃。

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **picturesLibrary** capability only when the scenarios for your app require programmatic access and can't be realized them by using the **file picker**.

                    The **picturesLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="picturesLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**影片***</td>
                <td>
                    The **videosLibrary** capability provides programmatic access to the user's Videos, allowing the app to enumerate and access all files in the library without user interaction. This capability is typically used in movie-playback apps that make use of the entire Videos library.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **videosLibrary** capability only when the scenarios for your app require programmatic access and can't be realized by using the **file picker**.

                    The **videosLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="videosLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**抽取式存放裝置**</td>
                <td>
                    The **removableStorage** capability provides programmatic access to files on removable storage, like USB keys and external hard drives, filtered to the file-type associations declared in the package manifest. For example, if a document-reader app declares a .doc file-type association, it can open .doc files on the removable storage device, but not other types of files. Be careful when you declare this capability, because users may include a variety of info in their removable storage devices, and will expect your app to provide a valid justification for programmatic access to the removable storage for all files of the declared type.

                    Users will expect your app to handle any file associations that you declare. So don't declare file associations that your app cannot handle responsibly. The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app.

                    Declare the **removableStorage** capability only when the scenarios for your app require programmatic access and can't be realized by using the [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847).

                    The **removableStorage** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="removableStorage"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**網際網路和公用網路***</td>
                <td>
                    There are two capabilities that provide different levels of access to the Internet and public networks.

                    The **internetClient** capability indicates that apps can receive incoming data from the Internet. Cannot act as a server. No local network access.

                    The **internetClientServer** capability indicates that apps can receive incoming data from the Internet. Can act as a server. No local network access.

                    Most apps that have a web service component will use **internetClient**. Apps that enable peer-to-peer (P2P) scenarios where the app needs to listen for incoming network connections should use **internetClientServer**. The **internetClientServer** capability includes the access that the **internetClient** capability provides, so you don't need to specify **internetClient** when you specify **internetClientServer**.
                </td>
            </tr>
            <tr>
                <td>**Homes and work networks***</td>
                <td>
                    The **privateNetworkClientServer** capability provides inbound and outbound access to home and work networks through the firewall. This capability is typically used for games that communicate across the local area network (LAN), and for apps that share data across a variety of local devices. If your app specifies **musicLibrary**, **picturesLibrary**, or **videosLibrary**, you don't need to use this capability to access the corresponding library in a Home Group. On Windows, this capability does not provide access to the Internet.
                </td>
            </tr>
            <tr>
                <td>**Appointments**</td>
                <td>
                    The **appointments** capability provides access to the user’s appointment store. This capability allows read access to appointments obtained from the synced network accounts and to other apps that write to the appointment store. With this capability, your app can create new calendars and write appointments to calendars that it creates.

                    The **appointments** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="appointments"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**連絡人***</td>
                <td>
                    The **contacts** capability provides access to the aggregated view of the contacts from various contacts stores. This capability gives the app limited access (network permitting rules apply) to contacts that were synced from various networks and the local contact store.

                    The **contacts** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="contacts"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**程式碼產生**</td>
                <td>
                    The **codeGeneration** capability allows apps to access the following functions which provide JIT capabilities to apps.

                    - [**VirtualProtectFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169846)
                    - [**CreateFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994453)
                    - [**OpenFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169844)
                    - [**MapViewOfFileFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994454)
                </td>
            </tr>
            <tr>
                <td>**AllJoyn**</td>
                <td>
                    The **allJoyn** capability allows AllJoyn-enabled apps and devices on a network to discover and interact with each other.

                    All apps that access APIs in the [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) namespace must use this capability.
                </td>
            </tr>
            <tr>
                <td>**Phone calls**</td>
                <td>
                    The **phoneCall** capability allows apps to access all of the phone lines on the device and perform the following functions.

                    - Place a call on the phone line and show the system dialer without prompting the user.
                    - Access line-related metadata.
                    - Access line-related triggers.
                    - Allows the user-selected spam filter app to set and check block list and call origin information.

                    The **phoneCall** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="phoneCall"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                    The **phoneCallHistoryPublic** capability allows apps to read cellular and some VOIP call history information on the device. This capability also allows the app to write VOIP call history entries. This capability is required to access all members of the [**PhoneCallHistoryStore**](https://msdn.microsoft.com/library/windows/apps/Dn705931) class.
                </td>
            </tr>
            <tr>
                <td>**錄製的通話資料夾***</td>
                <td>
                    <p>**recordedCallsFolder** 裝置功能讓 app 能夠存取錄製的通話資料夾。</p>
                    <p>在您的 app 套件資訊清單中宣告 **recordedCallsFolder** 功能時，它必須包含 **mobile** 命名空間，如下所示。</p>
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;mobile:Capability Name="recordedCallsFolder"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**使用者帳戶資訊***</td>
                <td>
                    <p>**userAccountInformation** 功能讓 app 能夠存取使用者的名稱和圖片。</p>
                    <p>需要具備此功能才能存取 Windows.System.User 命名空間內的某些 API。</p>
                    <p>在您的 app 套件資訊清單中宣告 **userAccountInformation** 功能時，它必須包含 **uap** 命名空間，如下所示。</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="userAccountInformation"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**VOIP 通話**</td>
                <td>
                    <p>**voipCall** 功能讓 app 能夠存取 [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) 命名空間中的 VOIP 通話 API。</p>
                    <p>在您的 app 套件資訊清單中宣告 **voipCall** 功能時，它必須包含 **uap** 命名空間，如下所示。</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="voipCall"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**3D 物件**</td>
                <td>
                    <p>**objects3D** 功能讓應用程式能以設計程式的方式存取 3D 物件檔案。 這項功能通常用於必須存取整個 3D 物件庫的 3D 應用程式和遊戲。</p>
                    <p>需要具備這個功能，才能使用 [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) 命名空間中的 API 存取包含 3D 物件的資料夾。</p>
                    <p>在您的 app 套件資訊清單中宣告 **objects3D** 功能時，它必須包含 **uap** 命名空間，如下所示。</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="objects3d"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**讀取已封鎖訊息***</td>
                <td>
                    <p>**blockedChatMessages** 功能讓 app 能夠讀取已由垃圾郵件篩選 app 封鎖的簡訊和多媒體簡訊訊息。</p>
                    <p>需要具備這個功能，才能使用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 命名空間中的 API 存取已封鎖的訊息。</p>
                    <p>在您的 app 套件資訊清單中宣告 **blockedChatMessages** 功能時，它必須包含 **uap** 命名空間，如下所示。</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="blockedChatMessages"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**聊天訊息存取**</td>
                <td>
                    <p>**chat** 功能讓 app 能夠讀取及刪除簡訊。 這個功能也能讓 app 將聊天訊息儲存於系統資料存放區中。</p>
                    <p>需要具備這個功能，才能使用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 命名空間中的某些 API。</p>
                    <p>在您的 app 套件資訊清單中宣告 **chat** 功能時，它必須包含 **uap** 命名空間，如下所示。</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="chat"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**IoT 低階匯流排硬體**</td>
                <td>
                    <p>**lowLevelDevices** 功能讓在 IoT 裝置上執行的 app 能夠存取低階匯流排硬體，例如 GPIO、I2C、SPI、ADC 和 PWM。</p>
                    <p>需要具備此功能才能存取 [**Windows.Devices.Spi**](https://msdn.microsoft.com/library/windows/apps/Dn708178) 命名空間中的某些 API。</p>
                    <p>在您的 app 套件資訊清單中宣告 **lowLevelDevices** 功能時，它必須包含 **iot** 命名空間，如下所示。</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;iot:Capability Name="lowLevelDevices"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**IoT 系統管理**</td>
                <td>
                    <p>**systemManagement** 功能讓 app 具備基本的系統管理權限，例如關機或重新啟動、地區設定和時區。</p>
                    <p>需要具備這個功能，才能存取 [**Windows.System**](https://msdn.microsoft.com/library/windows/apps/BR241814) 命名空間中的某些 API。</p>
                    <p>在您的 app 套件資訊清單中宣告 **systemManagement** 功能時，它必須包含 **iot** 命名空間，如下所示。</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;iot:Capability Name="systemManagement"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
        </tbody>
</table>

 
## 裝置功能

裝置功能可以讓您的 app 存取周邊和內部裝置。 裝置功能是使用 app 套件資訊清單中的 **DeviceCapability** 元素來指定。 這個元素可能需要其他子元素，而且您必須手動將一些裝置功能新增到套件資訊清單。 如需詳細資訊，請參閱[如何在套件資訊清單中指定裝置功能](https://msdn.microsoft.com/library/windows/apps/Dn263092)和[**DeviceCapability Schema reference**](https://msdn.microsoft.com/library/windows/apps/BR211430)。

| 功能案例 | 功能使用方式 |
|---------------------|------------------|
| **位置**\* | **location** 功能提供存取位置功能，您可以透過專用硬體 (例如電腦中的 GPS 感應器) 來擷取這類功能，或是從可用的網路資訊加以衍生。 當使用者從 [設定]**** 常用鍵停用定位服務時，app 必須能夠處理這種狀況。 |
| **麥克風** | **microphone** 功能提供存取麥克風的音訊饋送，讓 app 能夠從連接的麥克風錄音。 當使用者從 [設定]**** 常用鍵停用麥克風時，app 必須能夠處理這種狀況。 |
| **近接** | **proximity** 功能可以讓多個近接的裝置彼此通訊。 這個功能通常用於輕鬆的多人遊戲，以及交換資訊的 app 中。 裝置會試著使用提供最佳連線的通訊技術，例如藍牙、Wi-Fi 以及網際網路。 這個功能只能用於起始裝置間的通訊。 |
| **網路攝影機** | **webcam** 功能提供存取內建相機或外接式網路攝影機的視訊饋送，讓 app 能夠擷取相片和影片。 在 Windows 上，當使用者從 [設定]**** 常用鍵停用相機時，app 必須能夠處理這種狀況。<br/>**webcam** 功能只授與對視訊串流的存取權。 若要同時授與對音訊串流的存取權，必須新增 **microphone** 功能。 |
| **USB** | **usb** 裝置功能可啟用對[針對 USB 裝置更新應用程式資訊清單](http://go.microsoft.com/fwlink/p/?LinkId=302259)中所述之 API 的存取。 |
| **人性化介面裝置 (HID)** | **humaninterfacedevice** 裝置功能可啟用對[如何指定 HID 的裝置功能](https://msdn.microsoft.com/library/windows/apps/Dn263091)。 |
| **服務點 (POS)** | **pointOfService** 裝置功能可以啟用對 [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071) 命名空間中 API 的存取。 這個命名空間可以讓您的 app 存取服務指標 (POS) 條碼掃描器和磁條讀取器。 這個命名空間提供一個廠商中性介面，可讓您從 Windows 市集應用程式存取來自各個製造商的 POS 裝置。 |
| **藍牙** | **bluetooth** 裝置功能讓 app 能夠與兩個都透過 Generic Attribute (GATT) 或 Classic Basic Rate (RFCOMM) 通訊協定且配對好的藍牙裝置通訊。<br/>需要具備這個功能，才能使用 [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413) 命名空間中的某些 API。 |
| **Wi-Fi 網路功能** | **wiFiControl** 裝置功能讓 app 能夠掃描並連線到 Wi-Fi 網路。<br/>需要具備這個功能，才能使用 [**Windows.Devices.WiFi**](https://msdn.microsoft.com/library/windows/apps/Dn975224) 命名空間中的某些 API。 |
| **無線電狀態** | **radios** 裝置功能讓 app 能夠在 Wi-Fi 和藍牙無線電之間切換。<br/>需要具備這個功能，才能使用 [**Windows.Devices.Radios**](https://msdn.microsoft.com/library/windows/apps/Dn996447) 命名空間中的 API。  |
| **光碟片** | **optical** 裝置功能讓 app 能夠存取光碟機 (例如 CD、DVD 及藍光) 上的功能。<br/>需要具備這個功能，才能使用 [**Windows.Devices.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn263667) 命名空間中的某些 API。 |
| **動作活動** | **activity** 裝置功能讓 app 能夠偵測裝置目前的動作。<br/>需要具備這個功能，才能使用 [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408) 命名空間中的某些 API。 |

## 特殊和受限制的功能

**重要**  
特殊和受限制的功能適用於非常特殊的情況。 這些功能的用法受到高度限制，而且受其他市集上架原則和審查規定的規範。

這類功能在某些情況下是必要且適當的，例如具備雙因素驗證的銀行系統，使用者需提供含數位憑證的智慧卡來確認身分識別。 其他應用程式主要可能是針對企業客戶所設計，而且可能需要存取公司資源，若使用者沒有網域認證，便無法存取這類公司資源。

您必須使用公司帳戶，才能將套用特殊用途功能的應用程式提交到市集。 相反地，受限制的功能不需要市集的特殊公司帳戶，開發人員無法使用它們。 只有 Microsoft 和其合作夥伴開發的 app 才能使用受限制的功能。 如需有關公司帳戶的詳細資訊，請參閱[帳戶類型、位置和費用](https://msdn.microsoft.com/library/windows/apps/JJ863494)。

和其他功能不同的是，在您的 app 套件資訊清單中宣告受限制的功能時，所有受限制的功能必須包含 **rescap** 命名空間。 下列範例顯示如何宣告 **appCaptureSettings** 功能。

```xml
<Capabilities>
    <rescap:Capability Name="appCaptureSettings"/>
</Capabilities>
```

您也必須在 Package.appxmanifest 檔案的頂端新增 **xmlns:rescap** 命名空間宣告，如下所示。

```xml
<Package
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
    xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
    xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp wincap rescap">
```

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">功能案例</th>
<th align="left">功能使用方式</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**企業版**</td>
<td align="left"><p>Windows 網域認證可以讓使用者使用自己的認證登入遠端資源，如同使用者提供自己的使用者名稱和密碼一樣。 **enterpriseAuthentication** 特殊功能通常用於企業中與伺服器連線的企業營運應用程式。</p>
<p>您不需要針對網際網路上的一般通訊使用此功能。</p>

<p>**enterpriseAuthentication** 特殊功能是用來支援常見的企業營運應用程式。 請勿在不需要存取公司資源的 app 中宣告此功能。 [
            **file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) 提供健全的 UI 機制，能夠讓使用者開啟網路共用上要供 app 使用的檔案。 只有當您 app 的案例需要以程式設計方式進行存取，而您無法使用「檔案選擇器」****來實現時，才需宣告 **enterpriseAuthentication** 特殊功能。</p>
<p>在您的 app 套件資訊清單中宣告 **enterpriseAuthentication** 功能時，它必須包含 **uap** 命名空間，如下所示。</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="enterpriseAuthentication"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div>
<p>**enterpriseDataPolicy** 功能可讓 app 定義和使用裝置適用的企業特定原則。 若要使用下列類別的所有成員，必須要有這個功能。</p>
<ul>
<li>[**FileProtectionManager**](https://msdn.microsoft.com/library/windows/apps/Dn705151)</li>
<li>[**DataProtectionManager**](https://msdn.microsoft.com/library/windows/apps/Dn706017)</li>
<li>[**ProtectionPolicyManager**](https://msdn.microsoft.com/library/windows/apps/Dn705170)</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">**共用使用者憑證**</td>
<td align="left"><p>**sharedUserCertificates** 特殊功能可讓 app 在「共用使用者」存放區中新增和存取軟體和硬體型憑證，例如儲存在智慧卡的憑證。 這個功能通常用於需要使用智慧卡進行身分驗證的金融或企業 app。</p>
<p>在您的 app 套件資訊清單中宣告 **sharedUserCertificates** 功能時，它必須包含 **uap** 命名空間，如下所示。</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="sharedUserCertificates"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div></td>
</tr>
<tr class="odd">
<td align="left">**文件***</td>
<td align="left"><p>**documentsLibrary** 功能提供以程式設計方式存取使用者的 [文件] (已篩選出套件資訊清單中宣告的檔案類型關聯) 的功能，以支援對 OneDrive 進行離線存取。 例如，如果 DOC 閱讀程式 app 宣告 .doc 檔案類型關聯，則它可以開啟 [文件] 中的 .doc 檔案，但無法開啟其他類型的檔案。</p>
<p>宣告 **documentsLibrary** 特殊功能的 app 無法存取家用群組電腦上的 [文件]。 [
            file picker](https://msdn.microsoft.com/library/windows/apps/Hh465174) 提供健全的 UI 機制，能夠讓使用者開啟要供 app 使用的檔案。 只有在您無法使用檔案選擇器時，才宣告 **documentsLibrary** 特殊功能。</p>
<p>若要使用 **documentsLibrary** 特殊功能，app 必須：</p>
<ul>
<li>使用有效的 OneDrive URL 或資源識別碼，協助對特定的 OneDrive 內容進行跨平台離線存取。</li>
<li>在離線時自動將開啟的檔案儲存到使用者的 OneDrive</li>
</ul>
<p>針對這兩個目的而使用 **documentsLibrary** 特殊功能的 app，也可選擇使用此功能開啟另一份文件內的內嵌內容。 只接受上述使用 **documentsLibrary** 特殊功能的方式。</p>
<ul>
<li><p>您的 app 無法存取手機內部儲存空間中的文件庫。 不過，如果另一個 app 在選用的 SD 記憶卡上建立 [文件] 資料夾，您的 app 能夠看到該資料夾。</p></li>
</ul>
<p>在您的 app 套件資訊清單中宣告 **documentsLibrary** 功能時，它必須包含 **uap** 命名空間，如下所示。</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="documentsLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div></td>
</tr>
<tr class="even">
<td align="left">**遊戲 DVR 設定**</td>
<td align="left"><p>**appCaptureSettings** 受限制的功能讓 app 能夠控制「遊戲 DVR」的使用者設定。</p>
<p>需要具備這個功能，才能使用 [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/BR226738) 命名空間中的某些 API。</p></td>
</tr>
<tr class="odd">
<td align="left">**行動數據**</td>
<td align="left"><p>**cellularDeviceControl** 受限制的功能讓應用程式能夠控制行動電話通訊裝置。</p>
<p>**cellularDeviceIdentity** 功能讓應用程式能夠存取行動電話通訊識別資料。</p>
<p>**cellularMessaging** 功能讓 app 能夠使用簡訊和 RCS。</p>
<p>需要具備這些功能，才能使用 [**Windows.Devices.Sms**](https://msdn.microsoft.com/library/windows/apps/BR206567) 命名空間中的某些 API。</p>
<p>從 Windows 10 開始，app 會呼叫 [**AppIDList**](https://msdn.microsoft.com/library/windows/apps/Dn393996))。</p></td>
</tr>
<tr class="even">
<td align="left">**裝置解除鎖定**</td>
<td align="left"><p>**deviceUnlock** 受限制的功能讓 app 能夠解除鎖定裝置，以供開發人員和企業側載案例使用。</p></td>
</tr>
<tr class="odd">
<td align="left">**雙 SIM 卡磚**</td>
<td align="left"><p>**dualSimTiles** 受限制的功能讓應用程式能夠在配備多張 SIM 卡的裝置上建立其他應用程式清單項目。</p>
<p>需要具備這個功能，才能使用 [**Windows.UI.StartScreen**](https://msdn.microsoft.com/library/windows/apps/BR242235) 命名空間中的某些 API。</p></td>
</tr>
<tr class="even">
<td align="left">**企業共用存放裝置**</td>
<td align="left"><p>**enterpriseDeviceLockdown** 受限制的功能讓應用程式能夠使用裝置鎖定 API，以及存取企業共用存放裝置資料夾。</p></td>
</tr>
<tr class="odd">
<td align="left">**系統輸入導入**</td>
<td align="left"><p>**inputInjection** 受限制的功能讓應用程式能夠以程式設計方式，將各種不同形式的輸入 (例如，HID、觸控、手寫筆、鍵盤或滑鼠) 導入系統中。 這個功能通常用於可控制系統的共同作業應用程式。</p>
<div class="alert">
**注意：**針對電腦，從具備此功能的 app 導入的輸入源，只能透過位於相同應用程式容器的處理程序來接收。
</div>
</td>
</tr>
<tr class="even">
<td align="left">**觀察輸入***</td>
<td align="left"><p>**inputObservation** 受限制的功能讓 app 能夠觀察和攔截系統所接收到的各種不同形式的原始輸入 (例如，HID、觸控、手寫筆、鍵盤或滑鼠)，而不論其最終目的地為何。</p></td>
</tr>
<tr class="odd">
<td align="left">**抑制輸入**</td>
<td align="left"><p>**inputSuppression** 受限制的功能讓應用程式能夠隱藏系統所接收到的各種不同形式的原始輸入 (例如，HID、觸控、手寫筆、鍵盤或滑鼠)。</p></td>
</tr>
<tr class="even">
<td align="left">**VPN 應用程式**</td>
<td align="left"><p>**networkingVpnProvider** 受限制的功能讓應用程式能夠具備 VPN 功能的完整存取權，包括管理連線的能力，以及提供 VPN 外掛程式功能。</p>
<p>需要具備這個功能，才能使用 [**Windows.Networking.Vpn**](https://msdn.microsoft.com/library/windows/apps/Dn434040) 命名空間中的某些 API。</p></td>
</tr>
<tr class="odd">
<td align="left">**其他應用程式管理**</td>
<td align="left"><p>**packageManagement** 受限制的功能讓應用程式能夠直接管理其他應用程式。</p>
<p>**packageQuery** 裝置功能讓應用程式能夠收集其他應用程式的相關資訊。</p>
<p>需要具備這個功能，才能使用存取 [**PackageManager**](https://msdn.microsoft.com/library/windows/apps/BR240960) 類別中的某些方法和屬性。</p></td>
</tr>
<tr class="even">
<td align="left">**畫面投影**</td>
<td align="left"><p>**screenDuplication** 受限制的功能讓應用程式能將畫面投影到其他裝置。</p>
<p>需要具備這個功能，才能使用 DirectX 命名空間中的 API。</p></td>
</tr>
<tr class="odd">
<td align="left">**使用者主體名稱**</td>
<td align="left"><p>**userPrincipalName** 受限制的功能讓應用程式能夠修改和存取相片的縮圖快取。</p>
<p>需要具備這個功能，才能呼叫 [**GetUserNameEx**](https://msdn.microsoft.com/library/windows/desktop/ms724435) 函式。</p></td>
</tr>
<tr class="even">
<td align="left">**電子錢包**</td>
<td align="left"><p>**walletSystem** 受限制的功能讓應用程式能完整存取儲存式電子錢包卡。</p>
<p>需要具備這個功能，才能使用 [**Windows.ApplicationModel.Wallet.System**](https://msdn.microsoft.com/library/windows/apps/Mt171610) 命名空間中的 API。</p></td>
</tr>
<tr class="odd">
<td align="left">**位置歷程記錄**</td>
<td align="left"><p>**locationHistory** 受限制的功能讓 app 能夠存取裝置的位置歷程記錄。</p>
<p>需要具備這個功能，才能使用 [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/BR225603) 命名空間中的 API。</p></td>
</tr>
<tr class="even">
<td align="left">**應用程式關閉確認**</td>
<td align="left"><p>**confirmAppClose** 受限制的功能讓 app 能夠關閉本身和它們自己的視窗，並延遲關閉它們的 app。</p>
<p>需要具備這個功能，才能使用 [**Windows.UI.ViewManagement**](https://msdn.microsoft.com/library/windows/apps/BR242295) 命名空間中的 API。</p></td>
</tr>
<tr class="odd">
<td align="left">**通訊記錄***</td>
<td align="left"><p>**phoneCallHistory** 受限制的功能讓 app 能夠讀取通訊記錄並刪除記錄中的項目。</p>
<p>需要具備這個功能，才能使用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 命名空間中的 API。</p></td>
</tr>
<tr class="even">
<td align="left">**系統層級的約會存取**</td>
<td align="left"><p>**appointmentsSystem** 受限制的功能讓 app 能夠讀取和修改使用者行事曆上的所有約會。</p>
<p>需要具備這個功能，才能使用 [**Windows.ApplicationModel.Appointment**](https://msdn.microsoft.com/library/windows/apps/Dn263359) 命名空間中的 API。</p></td>
</tr>
<tr class="odd">
<td align="left">**系統層級的聊天訊息存取***</td>
<td align="left"><p>**chatSystem** 受限制的功能讓 app 能夠讀取和寫入所有簡訊和多媒體簡訊訊息。</p>
<p>需要具備這個功能，才能使用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 命名空間中的 API。</p></td>
</tr>
<tr class="even">
<td align="left">**系統層級的連絡人存取**</td>
<td align="left"><p>**contactsSystem** 受限制的功能讓 app 能夠讀取已指定為受限制或敏感的連絡人資訊，並修改現有的連絡人資訊。</p>
<p>需要具備這個功能，才能使用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 命名空間中的 API。</p></td>
</tr>
<tr class="odd">
<td align="left">**電子郵件存取***</td>
<td align="left"><p>**email** 受限制的功能讓 app 能夠讀取、分級，以及傳送電子郵件給使用者。</p>
<p>需要具備這個功能，才能使用 [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285) 命名空間中的 API。</p></td>
</tr>
<tr class="even">
<td align="left">**系統層級的電子郵件存取**</td>
<td align="left"><p>**emailSystem** 受限制的功能讓 app 能夠讀取、分級，以及傳送受限制或敏感的電子郵件給使用者。</p>
<p>需要具備這個功能，才能使用 [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285) 命名空間中的 API。</p></td>
</tr>
<tr class="odd">
<td align="left">**系統層級的通話記錄存取**</td>
<td align="left"><p>**phoneCallHistorySystem** 受限制的功能讓 app 能夠透過變更現有項目及撰寫新項目來完整修改通話記錄。</p>
<p>需要具備這個功能，才能使用 [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) 命名空間中的 API。</p></td>
</tr>
<tr class="even">
<td align="left">**傳送簡訊***</td>
<td align="left"><p>**smsSend** 受限制的功能讓 app 能夠傳送簡訊和多媒體簡訊訊息。</p>
<p>需要具備這個功能，才能使用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 命名空間中的 API。</p></td>
</tr>
<tr class="odd">
<td align="left">**所有使用者資料的系統層級存取**</td>
<td align="left"><p>**userDataSystem** 受限制的功能讓 app 能夠存取使用者資料系統資料存放區。</p></td>
</tr>
<tr class="even">
<td align="left">**市集預覽功能**</td>
<td align="left"><p>**PreviewStore** 受限制的功能讓 app 能夠擷取並購買 app 內產品的 SKU。</p>
<p>需要具備這個功能，才能使用 [**Windows.ApplicationModel.Store.Preview**](https://msdn.microsoft.com/library/windows/apps/Mt185546) 命名空間中的特定 API。</p></td>
</tr>
<tr class="odd">
<td align="left">**第一次登入設定**</td>
<td align="left"><p>**FirstSignInSettings** 受限制的功能讓 app 能夠存取使用者首次登入其裝置時所設定的使用者設定。</p></td>
</tr>
<tr class="even">
<td align="left">**Windows 小組體驗**</td>
<td align="left"><p>**TeamEditionExperience** 受限制的功能讓 app 能夠存取控制 Windows 小組工作階段各個體驗層面的內部 API。 Windows 小組工作階段極有可能會在小組裝置 (如 Microsoft Surface 中樞) 上執行。</p></td>
</tr>
<tr class="odd">
<td align="left">**遠端解除鎖定**</td>
<td align="left"><p>**remotePassportAuthentication** 受限制的功能讓應用程式能夠存取可以用來解除遠端電腦鎖定的認證。</p></td>
</tr>
<tr class="even">
<td align="left">**預覽組合**</td>
<td align="left"><p>**previewUiComposition** 受限制的功能讓應用程式能夠預覽其使用者介面的 [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Dn706878) 命名空間，以便他們可以在完成 API 之前提供意見反應。 如需詳細資訊，請連絡 wincomposition@microsoft.com。</p></td>
</tr>
<tr class="odd">
<td align="left">**安全評定鎖定**</td>
<td align="left"><p>**secureAssessment** 受限制的功能讓應用程式能夠將 Windows 鎖定為單一應用程式模式以進行安全性評估。</p></td>
</tr>
<tr class="even">
<td align="left">**連線管理員佈建**</td>
<td align="left"><p>**connectionManagerProvisioning** 受限制的功能讓應用程式能夠定義可將裝置連線到 WWAN 和 WLAN 介面的原則。 電信業者會建立使用這個功能的應用程式，以便管理其行動網路的裝置連線。</p></td>
</tr>
<tr class="odd">
<td align="left">**資料計劃佈建**</td>
<td align="left"><p>**networkDataPlanProvisioning** 受限制的功能讓 app 能夠收集裝置上資料計劃的相關資訊，並讀取網路使用量。 電信業者會建立使用這個功能的 app，以便將其客戶的實際數據使用量整合到 OS 數據使用量設定。</p></td>
</tr>
<tr class="even">
<td align="left">**軟體授權**</td>
<td align="left"><p>**SlapiQueryLicenseValue** 受限制的功能讓 app 能夠查詢軟體授權原則。</p></td>
</tr>
<tr class="odd">
<td align="left">**延伸執行**</td>
<td align="left"><p>**ExtendedExecutionBackgroundAudio** 受限制的功能讓 app 能夠在背景執行時也能播放音訊。</p>
<p>**ExtendedExecutionCritical** 受限制的功能讓 app 能夠開始嚴重延伸執行工作階段。</p>
<p>**extendedExecutionUnconstrained** 受限制的功能讓 app 能夠開始不受限制的延伸執行工作階段。</p></td>
</tr>
<tr class="even">
<td align="left">**行動裝置管理**</td>
<td align="left"><p>**DeviceManagementDmAccount** 受限制的功能讓 app 能夠佈建和設定 [電信業者開放行動通訊聯盟 - 裝置管理 (MO OMA-DM)] 帳戶。</p>
<p>**DeviceManagementFoundation** 受限制的功能讓 app 能夠基本存取裝置上的行動裝置管理 (MDM) 設定服務提供者 (CSP) 基礎結構。 請注意，需有其他功能才能存取特定 CSP。</p>
<p>**DeviceManagementWapSecurityPolicies** 受限制的功能讓 app 能夠設定無線應用通訊協定 (WAP) 服務，例如 MMs、服務指示/服務載入 (SI/SL) 及開放行動通訊聯盟 - 用戶端佈建 (OMA CP)。</p>
<p>**DeviceManagementEmailAccount** 受限制的功能讓 app 能夠由電信業者建立，以便在他們佈建給使用者的裝置上新增和管理電子郵件帳戶。</p></td>
</tr>
<tr class="odd">
<td align="left">**套件原則控制項**</td>
<td align="left"><p>**PackagePolicySystem** 受限制的功能讓 app 能夠掌控裝置上所安裝之 app 的相關系統原則。</p></td>
</tr>
<tr class="even">
<td align="left">**遊戲清單**</td>
<td align="left"><p>**gameList** 受限制的功能讓 app 能夠取得一份系統上已安裝的知名遊戲清單。</p></td>
</tr>
<tr class="odd">
<td align="left">**Xbox 配件**</td>
<td align="left"><p>**xboxAccessoryManagement** 受限制的功能讓 app 能夠直接管理符合 Xbox 硬體規格的 Xbox 裝置。</p></td>
</tr>
<tr class="even">
<td align="left">**附屬應用程式的語音辨識**</td>
<td align="left"><p>**cortanaSpeechAccessory** 受限制的功能讓 app 能夠叫用並傳遞命令到 Cortana。</p></td>
</tr>
<tr class="odd">
<td align="left">**配件管理**</td>
<td align="left"><p>**accessoryManager** 受限制的功能讓應用程式能夠註冊為配件專屬應用程式，並選擇加入特定應用程式通知，以便將它們轉送到配件並對使用者顯示。</p></td>
</tr>
<tr class="even">
<td align="left">**驅動程式存取**</td>
<td align="left"><p>**interopServices** 受限制的功能讓應用程式能夠直接與驅動程式互動。</p></td>
</tr>
<tr class="odd">
<td align="left">**前景觀察**</td>
<td align="left"><p>**inputForegroundObservation** 受限制的功能讓前景的應用程式能夠攔截鍵盤輸入，並繞過所有非應用程式鍵盤輸入的處理。 此功能無法攔截 SAS 組合。 要存取 [**KeyboardDeliveryInterceptor**](https://msdn.microsoft.com/library/windows/apps/Mt608395) 類別的成員，必須要有這個功能。</p></td>
</tr>
<tr class="even">
<td align="left">**OEM 和 MO 合作夥伴 app**</td>
<td align="left"><p>**oemDeployment** 受限制的功能讓 Microsoft 合作夥伴建立的應用程式可以在裝置上安裝新的應用程式和查詢目前安裝的應用程式。</p>
<p>**oemPublicDirectory** 受限制的功能讓 Microsoft 合作夥伴建立的 app 可以存取共用 app 資料夾。</p></td>
</tr>
<tr class="odd">
<td align="left">**應用程式授權模型**</td>
<td align="left"><p>**appLicensing** 限制功能可允許 app 在不需要授權的情況下執行。 如果您在資訊清單中宣告此功能，您就無法將 app 提交至市集。 任何要求存取此功能以進行市集提交的要求將會一律被拒絕。</p></td>
</tr>
<tr class="even">
<td align="left">**位置系統**</td>
<td align="left"><p>**locationSystem** 限制功能可允許 app 執行特定需要特殊權限的位置設定，例如設定裝置的預設位置。 如果您在資訊清單中宣告此功能，您就無法將 app 提交至市集。 任何要求存取此功能以進行市集提交的要求將會一律被拒絕。</p></td>
</tr>
</tbody>
</table>

**注意**  
此文章適用於撰寫 UWP app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

## 相關主題

* [資訊清單設計工具](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230259.aspx)
* [隱私權感知 app 的指導方針](https://msdn.microsoft.com/library/windows/apps/Hh768223)
* [如何在套件資訊清單中指定功能](https://msdn.microsoft.com/library/windows/apps/BR211477)
* [如何在套件資訊清單中指定裝置功能](https://msdn.microsoft.com/library/windows/apps/Dn263092)
 


<!--HONumber=Mar16_HO5-->


