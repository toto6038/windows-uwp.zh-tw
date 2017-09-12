---
author: drewbatgit
ms.assetid: 79C284CA-C53A-4C24-807E-6D4CE1A29BFA
description: "本節說明如何修改 PlayReady Web app，以支援從舊版 Windows 8.1 到 Windows 10 版本所做的變更。"
title: "PlayReady 加密媒體延伸"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: a8cc35115b2805b2191424edca671c53c252c549
ms.sourcegitcommit: cd9b4bdc9c3a0b537a6e910a15df8541b49abf9c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/21/2017
---
# <a name="playready-encrypted-media-extension"></a>PlayReady 加密媒體延伸

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本節說明如何修改 PlayReady Web app，以支援從舊版 Windows 8.1 到 Windows 10 版本所做的變更。

使用 Internet Explorer 中的 PlayReady 媒體元素讓開發人員可以建立 Web App，能夠為使用者提供 PlayReady 內容，同時強制執行內容提供者所定義的存取規則。 本節說明如何只使用 HTML5 和 JavaScript，將 PlayReady 媒體元素新增到現有的 Web app。

## <a name="whats-new-in-playready-encrypted-media-extension"></a>PlayReady 加密媒體延伸的新功能

本節提供對於 PlayReady 加密媒體延伸 (EME) 所做的變更清單，可用來在 Windows 10 上啟用 PlayReady 內容保護。

下列清單會針對適用於 Windows 10 的 PlayReady 加密媒體延伸說明新功能及所做的變更：

-   已新增硬體數位版權管理 (DRM)。

    硬體式內容保護支援能夠在多個裝置的平台上安全播放高畫質 (HD) 和超高畫質 (UHD) 的內容。 金鑰內容 (包括私密金鑰、內容金鑰，以及其他用來衍生或解除鎖定上述金鑰的金鑰內容)，以及已解密的壓縮和未壓縮的視訊範例會利用硬體安全性來進行保護。

-   提供主動取得非永久性授權。
-   提供在一則訊息中取得多個授權的功能。

    您可以使用 PlayReady 物件搭配多個金鑰識別碼 (KeyID) (就像在 Windows 8.1 中)，或者使用[內容解密模型資料 (CDMData)](https://go.microsoft.com/fwlink/p/?LinkID=626819) 搭配多個 KeyID。

    > [!NOTE]
    > 在 Windows 10 中，CDMData 中的 &lt;KeyID&gt; 下方支援多個金鑰識別碼。

-   已新增即時到期支援或限時授權 (LDL)。

    能夠設定授權的即時到期。

-   已新增 HDCP 類型 1 (版本 2.2) 原則的支援。
-   現已內含 Miracast 做為輸出。
-   已新增安全停止功能。

    安全停止功能讓 PlayReady 裝置能夠確實向媒體串流處理服務宣告任何指定內容的媒體播放已停止。

-   已新增音訊與視訊各別授權。

    分離曲目可防止視訊被解碼為音訊，進而使內容保護能力更為健全。 新的標準要求音訊與視覺曲目要個別提供金鑰。

-   新增 MaxResDecode。

    即使擁有功能更強的金鑰 (但非授權)，這項新功能仍限制以最高解析度來播放內容。 其支援使用單一金鑰編碼多個資料流大小的情況。

## <a name="encrypted-media-extension-support-in-playready"></a>PlayReady 的加密媒體延伸支援

本節說明 PlayReady 支援的 W3C 加密媒體延伸版本。

適用於 Web apps 的 PlayReady 目前已繫結至 [2013 年 5 月 10 日的 W3C 加密媒體延伸 (EME) 草稿](http://www.w3.org/TR/2013/WD-encrypted-media-20130510/)。 在未來的 Windows 版本中，此支援將變更為更新的 EME 規格。

## <a name="use-hardware-drm"></a>使用硬體 DRM

本節說明 Web app 如何使用 PlayReady 硬體 DRM，以及如果受保護的內容不支援硬體 DRM，該如何停用它。

若要使用 PlayReady 硬體 DRM，您的 JavaScript Web app 應該使用 **isTypeSupported** EME 方法搭配 `com.microsoft.playready.hardware` 的金鑰系統識別碼，從瀏覽器查詢 PlayReady 硬體 DRM 支援。

硬體 DRM 有時不支援某些內容。 硬體 DRM 從未支援混合式內容 (雞尾酒內容)；若想要播放混合式內容，您必須選擇不使用硬體 DRM。 部分硬體 DRM 會支援 HEVC，部分則不支援；若想要播放 HEVC 內容，但硬體 DRM 不支援，則您可能也會選擇不使用。

> [!NOTE]
> 若要判斷是否支援 HEVC 內容，在具現化 `com.microsoft.playready` 之後，請使用 [**PlayReadyStatics.CheckSupportedHardware**](https://msdn.microsoft.com/library/windows/apps/dn986441) 方法。

## <a name="add-secure-stop-to-your-web-app"></a>在 Web app 中新增安全停止功能

本節說明如何將安全停止功能新增到您的 Web app。

安全停止功能讓 PlayReady 裝置能夠確實向媒體串流處理服務宣告任何指定內容的媒體播放已停止。 此功能確保您的媒體串流處理服務能在不同裝置上，針對指定帳戶正確地強制執行和報告使用限制。

傳送安全停止挑戰的主要案例有兩種：

-   當媒體呈現因已到達內容結尾，或當使用者在中間的某處停止呈現媒體而停止。
-   當先前的工作階段意外結束 (例如，因為系統或 App 當機)。 App 將需要針對任何待處理的安全停止工作階段進行查詢 (可能是在開機或關機時)，並從任何其他媒體播放個別傳送挑戰。

下列程序說明如何針對各種案例設定安全停止功能。

設定安全停止以使呈現能夠正常結束：

1.  播放開始之前，先登錄 **onEnded** 事件。
2.  **onEnded** 事件處理常式必須從視訊/音訊元素物件呼叫 `removeAttribute(“src”)`，將來源設定為 **NULL**，這將觸發媒體基礎來中斷拓撲、毀損解碼程式，以及設定停止狀態。
3.  您可以在處理常式內部啟動安全停止 CDM 工作階段，將安全停止挑戰傳送到伺服器，通知目前已停止播放，但它也能在稍後完成。

若要在使用者離開該網頁瀏覽或者關閉索引標籤或瀏覽器時設定安全停止：

-   不需使用任何 App 動作來記錄停止狀態；系統會為您記錄此狀態。

若要針對自訂頁面控制項或使用者動作設定安全停止 (例如，自訂瀏覽按鈕或在目前版面完成之前啟動新的呈現)：

-   發生自訂的使用者動作時，app 需要將來源設定為 **NULL**，這將觸發媒體基礎來中斷拓撲、毀損解碼程式，以及設定停止狀態。

下列範例示範如何在 Web app 中使用安全停止：

```JavaScript
// JavaScript source code

var g_prkey = null;
var g_keySession = null;
var g_fUseSpecificSecureStopSessionID = false;
var g_encodedMeteringCert = 'Base64 encoded of your metering cert (aka publisher cert)';

// Note: g_encodedLASessionId is the CDM session ID of the proactive or reactive license acquisition 
//       that we want to initiate the secure stop process.
var g_encodedLASessionId = null;

function main()
{
    ...

    g_prkey = new MSMediaKeys("com.microsoft.playready");

    ...

    // add 'onended' event handler to the video element
    // Assume 'myvideo' is the ID of the video element
    var videoElement = document.getElementById("myvideo");
    videoElement.onended = function (e) { 

        //
        // Calling removeAttribute("src") will set the source to null
        // which will trigger the MF to tear down the topology, destroy the
        // decryptor(s) and set the stop state.  This is required in order
        // to set the stop state.
        //
        videoElement.removeAttribute("src");
        videoElement.load();

        onEndOfStream();
    };
}

function onEndOfStream()
{
    ...

    createSecureStopCDMSession();

    ...    
}

function createSecureStopCDMSession()
{
    try{    
        var targetMediaCodec = "video/mp4";
        var customData = "my custom data";

        var encodedSessionId = g_encodedLASessionId;
        if( !g_fUseSpecificSecureStopSessionID )
        {
            // Use "*" (wildcard) as the session ID to include all secure stop sessions
            // TODO: base64 encode "*" and place encoded result to encodedSessionId
        }

        var int8ArrayCDMdata = formatSecureStopCDMData( encodedSessionId, customData,  g_encodedMeteringCert );
        var emptyArrayofInitData = new Uint8Array();

        g_keySession = g_prkey.createSession(targetMediaCodec, emptyArrayofInitData, int8ArrayCDMdata);

        addPlayreadyKeyEventHandler();

    } catch( e )
    {
        // TODO: Handle exception
    }
}

function addPlayreadyKeyEventHandler()
{
    // add 'keymessage' eventhandler   
    g_keySession.addEventListener('mskeymessage', function (event) {

        // TODO: Get the keyMessage from event.message.buffer which contains the secure stop challenge
        //       The keyMessage format for the secure stop is similar to LA as below:
        //
        //            <PlayReadyKeyMessage type="SecureStop" >
        //              <SecureStop version="1.0" >
        //                <Challenge encoding="base64encoded">
        //                    secure stop challenge
        //                </Challenge>
        //                <HttpHeaders>
        //                    <HttpHeader>
        //                      <name>Content-Type</name>
        //                         <value>"content type data"</value>
        //                    </HttpHeader>
        //                    <HttpHeader>
        //                         <name>SOAPAction</name>
        //                         <value>soap action</value>
        //                     </HttpHeader>
        //                    ....
        //                </HttpHeaders>
        //              </SecureStop>
        //            </PlayReadyKeyMessage>
                
        // TODO: send the secure stop challenge to a server that handles the secure stop challenge

        // TODO: Recevie and response and call event.target.Update() to proecess the response
    });
    
    // add 'keyerror' eventhandler
    g_keySession.addEventListener('mskeyerror', function (event) {
        var session = event.target;
        
        ...

        session.close();
    });
    
    // add 'keyadded' eventhandler
    g_keySession.addEventListener('mskeyadded', function (event) {
        
        var session = event.target;

        ...

        session.close();             
    });
}

/**
* desc@ formatSecureStopCDMData
*   generate playready CDMData
*   CDMData is in xml format:
*   <PlayReadyCDMData type="SecureStop">
*     <SecureStop version="1.0">
*       <SessionID>B64 encoded session ID</SessionID>
*       <CustomData>B64 encoded custom data</CustomData>
*       <ServerCert>B64 encoded server cert</ServerCert>
*     </SecureCert>
* </PlayReadyCDMData>        
*/
function formatSecureStopCDMData(encodedSessionId, customData, encodedPublisherCert) 
{
    var encodedCustomData = null;

    // TODO: base64 encode the custom data and place the encoded result to encodedCustomData

    var CDMDataStr = "<PlayReadyCDMData type=\"SecureStop\">" +
                     "<SecureStop version=\"1.0\" >" +
                     "<SessionID>" + encodedSessionId + "</SessionID>" +
                     "<CustomData>" + encodedCustomData + "</CustomData>" +
                     "<ServerCert>" + encodedPublisherCert + "</ServerCert>" +
                     "</SecureStop></PlayReadyCDMData>";
    
    var int8ArrayCDMdata = null

    // TODO: Convert CDMDataStr to Uint8 byte array and palce the converted result to int8ArrayCDMdata

    return int8ArrayCDMdata;
}
```

> [!NOTE]
> 在上述範例中，安全停止資料的 `<SessionID>B64 encoded session ID</SessionID>` 可以是星號 (\*)，這是適用於所記錄之所有安全停止工作階段的萬用字元。 也就是說，**SessionID** 標記可以是特定的工作階段，或是用來選取所有安全停止工作階段的萬用字元 (\*)。

## <a name="programming-considerations-for-encrypted-media-extension"></a>適用於加密媒體延伸的程式設計考量

本節列出您在建立適用於 Windows 10 且已啟用 PlayReady 的 Web app 時應納入考慮的程式設計考量。

在您的 app 關閉之前，該 app 建立的 **MSMediaKeys** 和 **MSMediaKeySession** 物件必須保持運作。 確保這些物件會保持運作的一種方式，是將它們指派為全域變數 (如果將變數宣告為函式內的區域變數，變數就會變成超出範圍且受限於記憶體回收)。 例如，下列範例會將變數 *g\_msMediaKeys* 和 *g\_mediaKeySession* 指派為全域變數，接著將其指派給函式中的 **MSMediaKeys** 和 **MSMediaKeySession** 物件。

``` syntax
var g_msMediaKeys;
var g_mediaKeySession;

function foo() {
  ...
  g_msMediaKeys = new MSMediaKeys("com.microsoft.playready");
  ...
  g_mediaKeySession = g_msMediaKeys.createSession("video/mp4", intiData, null);
  g_mediaKeySession.addEventListener(this.KEYMESSAGE_EVENT, function (e) 
  {
    ...
    downloadPlayReadyKey(url, keyMessage, function (data) 
    {
      g_mediaKeySession.update(data);
    });
  });
  g_mediaKeySession.addEventListener(this.KEYADDED_EVENT, function () 
  {
    ...
    g_mediaKeySession.close();
    g_mediaKeySession = null;
  });
}
```

如需詳細資訊，請參閱[範例應用程式](https://code.msdn.microsoft.com/windowsapps/PlayReady-samples-for-124a3738)。

## <a name="see-also"></a>另請參閱
- [PlayReady DRM](playready-client-sdk.md)




