---
author: Mtoepke
title: Xbox 開發人員計畫上的 UWP 已知問題
description: 列出 UWP 在 Xbox 開發人員計畫上的已知問題。
ms.author: mstahl
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: a7b82570-1f99-4bc3-ac78-412f6360e936
ms.localizationpriority: medium
ms.openlocfilehash: 798192dd898af5a7107087b4a9708e1a1d0cb9b5
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6974517"
---
# <a name="known-issues-with-uwp-on-xbox-developer-program"></a>Xbox 開發人員計畫上的 UWP 已知問題

本主題說明 Xbox One 開發人員計畫上的 UWP 已知問題。 如需此計畫的詳細資訊，請參閱 [Xbox 上的 UWP](index.md)。 

\[如果您是透過 API 參考主題中的連結來到這裡，並在尋找通用裝置系列 API 的資訊，請參閱[尚未在 Xbox 上支援的 UWP 功能](http://go.microsoft.com/fwlink/?LinkID=760755) (英文)。\]

下列清單將針對您可能遇到的一些已知問題進行重點提示，但這並不是完整的清單。 

**我們想要您的意見反應**，因此請在[開發通用 Windows 平台應用程式](https://social.msdn.microsoft.com/forums/windowsapps/home?forum=wpdevelop) 論壇上報告您發現的任何問題。 

如果您遇到困難，請閱讀本主題中的資訊、參閱[常見問題集](frequently-asked-questions.md)，並使用論壇以尋求協助。

 
## <a name="deploying-from-vs-fails-with-parental-controls-turned-on"></a>家長監護開啟時，從 VS 進行部署將會失敗

若主機開啟 [設定] 中的 [家長監護] 功能，從 VS 啟用應用程式將會失敗。

若要解決此問題，可以先暫時停用家長監護，或：
1. 將應用程式部署到已關閉家長監護的主機。
2. 開啟家長監護。
3. 從主機啟動應用程式。
4. 輸入 PIN 或密碼來允許啟動應用程式。
5. 應用程式將會啟動。
6. 關閉應用程式。
7. 使用 F5 從 VS 啟動，應用程式將會以無提示方式啟動。

此時，直到您將使用者登出為止，該權限將會「持續生效」__，即使您將應用程式解除安裝並重新安裝也一樣。
 
有另外一種免除類型，僅適用於子女帳戶。 子女帳戶需要家長登入以授與權限，但當他們這樣做時，家者可以選擇 **\[一律\]** 允許子女啟動該應用程式。 該免除項目儲存於雲端，即使子女登出後又重新登入，也會保留。

## <a name="storagefilecopyasync-fails-to-copy-encrypted-files-to-unencrypted-destination"></a>StorageFile.CopyAsync 無法將加密的檔案複製到未加密的目的地 

使用 StorageFile.CopyAsync 將已加密檔案複製到沒有加密的目的地時，這個呼叫會失敗並發生下列例外狀況：

```
System.UnauthorizedAccessException: Access is denied. (Excep_FromHResult 0x80070005)
```

這可能會影響想要將部署為應用程式套件一部分之檔案複製到其他位置的 Xbox 開發人員。 之所以如此的原因是，Xbox 加密套件內容時所用的是零售模式，而不是開發人員模式。 因此，App 或許在開發和測試期間看起來會依預期正常運作，但是發佈並安裝至零售 Xbox 後，就會失敗。
 

## <a name="blocked-networking-ports-on-xbox-one"></a>Xbox One 上的已封鎖網路連接埠

Xbox One 裝置上的通用 Windows 平台 (UWP) 應用程式具有無法繫結至 [57344, 65535] (包含此項) 範圍中之連接埠的限制。 雖然於執行階段期間繫結至這些連接埠時，看起來可能已成功，不過網路流量在抵達應用程式之前可能便已無訊息地被捨棄。 您的應用程式應該在可行的情況下繫結至連接埠 0，這將能允許系統選取本機連接埠。 如果您需要使用特定的連接埠，連接埠號碼必須位於 [1025, 49151] 的範圍內，而您也應該檢查並避免與 IANA 登錄發生衝突。 如需詳細資訊，請參閱[服務名稱與傳輸通訊協定連接埠號碼登錄 (英文)](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)。

## <a name="uwp-api-coverage"></a>UWP API 涵蓋範圍

並非所有的 UWP API 都在 Xbox 上受到支援。 如需取得已知無法運作的 API 清單，請參閱[尚未在 Xbox 上支援的 UWP 功能 (英文)](http://go.microsoft.com/fwlink/p/?LinkId=760755)。 如果您發現其他 API 問題，請在論壇上報告它們。 


## <a name="navigating-to-wdp-causes-a-certificate-warning"></a>瀏覽到 WDP 會導致憑證警告

您將收到有關所提供憑證的警告 (與下列螢幕擷取畫面類似)，因為 Xbox One 主機所簽署的安全性憑證並不被視為已知的受信任發行者。 若要存取 Windows Device Portal，請按一下 **\[繼續瀏覽此網站\]**。

![網站安全性憑證警告](images/security_cert_warning.jpg)


## <a name="knownfoldersmediaserverdevices-caveat-on-xbox"></a>在 Xbox 上 KnownFolders.MediaServerDevices 附加說明

在桌面上，媒體伺服器與電腦「搭配」，而裝置關聯服務會不斷地追蹤哪些伺服器目前在線上，如此，初始檔案系統查詢便可立即傳回目前在線上的已配對伺服器清單。

在 Xbox 上，沒有新增或移除伺服器的 UI，因此初始檔案系統查詢將永遠傳回空白。 您必須建立查詢，並訂閱至 ContentsChanged 事件，每當收到通知便重新整理查詢。 伺服器會魚貫而至，大部分在 3 秒鐘內即會被發現。

簡單的範例程式碼︰

```
namespace TestDNLA {

    public sealed partial class MainPage : Page {
        public MainPage() {
            this.InitializeComponent();
        }

        private async void FindFiles_Click(object sender, RoutedEventArgs e) {
            try {
                StorageFolder library = KnownFolders.MediaServerDevices;
                var folderQuery = library.CreateFolderQuery();
                folderQuery.ContentsChanged += FolderQuery_ContentsChanged;
                IReadOnlyList<StorageFolder> rootFolders = await folderQuery.GetFoldersAsync();
                if (rootFolders.Count == 0) {
                    Debug.WriteLine("No Folders found");
                } else {
                    Debug.WriteLine("Folders found");
                }
            } catch (Exception ex) {
                Debug.WriteLine("Error: " + ex.Message);
            } finally {
                Debug.WriteLine("Done");
            }
        }

        private async void FolderQuery_ContentsChanged(Windows.Storage.Search.IStorageQueryResultBase sender, object args) {
            Debug.WriteLine("Folder added " + sender.Folder.Name);
            IReadOnlyList<StorageFolder> topLevelFolders = await sender.Folder.GetFoldersAsync();
            foreach (StorageFolder topLevelFolder in topLevelFolders) {
                Debug.WriteLine(topLevelFolder.Name);
            }
        }
    }
}
```

## <a name="see-also"></a>請參閱
- [常見問題集](frequently-asked-questions.md)
- [Xbox One 上的 UWP](index.md)
