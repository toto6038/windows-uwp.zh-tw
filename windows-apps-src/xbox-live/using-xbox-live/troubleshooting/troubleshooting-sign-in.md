---
title: Xbox Live 登入進行疑難排解
description: 了解如何使用 Xbox Live 登入問題進行疑難排解。
ms.assetid: 87b70b4c-c9c1-48ba-bdea-b922b0236da4
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 一個登入 xbox 疑難排解
ms.localizationpriority: medium
ms.openlocfilehash: ff8d66105d8a1a44708bf23a681767a3044cb654
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630323"
---
# <a name="troubleshooting-xbox-live-sign-in"></a>Xbox Live 登入進行疑難排解

有幾個問題，可能會導致無法登入。  如果您遵循的步驟[開始使用 Visual Studio for UWP 遊戲](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)任何未預期的錯誤的機會降到最低。

## <a name="common-issues"></a>常見問題

### <a name="sandbox-problems"></a>沙箱的問題

一般而言，您應該熟悉沙箱和 Xbox live 如何與相關的概念。  中的詳細資訊請參閱 < [Xbox Live 沙箱](../../xbox-live-sandboxes.md)指南。

簡單地說，沙箱會強制執行內容的隔離和存取控制在零售版之前。  無法存取您的開發沙箱的使用者無法執行任何讀取或寫入作業屬於您的標題。  您也可以到不同的沙箱來測試發佈的服務組態的變化。

下面會討論一些需要注意有關沙箱。

#### <a name="developer-account-doesnt-have-access-to-the-right-sandbox-for-run-time-access"></a>開發人員帳戶沒有存取執行階段存取正確的沙箱

* 測試帳戶 （也稱為開發帳戶） 或已獲授權的開發人員帳戶必須用於登入正在開發中的標題。  請確定您嘗試使用其中一個登入，或在 XDP 上建立其他的測試帳戶[ https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts ](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts)。 您可以授權在合作夥伴中心上的 xbox 即時相關聯的開發人員帳戶 [https://partner.microsoft.com/en-us/xboxconfig/TestAccounts/Creator](https://partner.microsoft.com/en-us/xboxconfig/TestAccounts/Creator)
* 請確定帳戶具有存取您的標題會發佈至沙箱。  您在 XDP 中建立測試帳戶繼承 XDP 帳戶建立它們的權限

#### <a name="your-device-is-not-on-the-correct-sandbox"></a>您的裝置不在正確的沙箱

您正在開發的裝置必須設定為開發沙箱。  Xbox One 上您可以設定您的沙箱 using *Xbox One Manager*。  適用於 Windows 10 桌面，您可以使用 Xbox Live SDK 安裝的工具目錄位於 SwitchSandbox.cmd 指令碼。

#### <a name="your-titles-service-configuration-is-not-published-to-the-correct-development-sandbox"></a>項目的服務組態不會發行到正確的開發沙箱

請確定在開發沙箱中發行項目的服務組態。  您無法登入 Xbox Live 中指定的開發沙箱的標題，除非標題發行至相同的沙箱。  請參閱[XDP 文件](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_03_31_16.aspx#PublishServiceConfig)如需如何執行這項操作的資訊。 您可以閱讀[合作夥伴中心文件](../../get-started-with-creators/xbox-live-service-configuration-creators.md#publish-your-xbox-live-service-configuration)以了解如何發佈您的合作夥伴中心組態。

### <a name="ids-configured-incorrectly"></a>設定不正確的識別碼

有幾個部分的設定您的遊戲所需的識別碼。  中的詳細資訊請參閱 <<c0> [ 開始使用 Visual Studio for UWP 遊戲](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)並[開始使用跨玩遊戲](../../get-started-with-partner/get-started-with-cross-play-games.md)標題何種根據您所建立。

要注意一些事項如下：

* 確保您的應用程式識別碼正確的輸入到 XDP 或合作夥伴中心
* 確定您 PFN 已正確輸入到 XDP 或合作夥伴中心
* 請仔細檢查中所述，您在 Visual Studio 專案相同的目錄中建立 xboxservices.config[加入新的或現有的 UWP 專案 Xbox Live](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)指南。
* 請確定 「 套件中的身分識別 」 您 appxmanifest 正確無誤。  這會顯示在合作夥伴中心 」 封裝/身分識別/Name"應用程式身分識別 區段中。

### <a name="title-id-or-scid-not-configured-correctly"></a>標題的識別碼或 SCID 未正確設定

* UWP 標題的識別碼和 SCID 標題必須設定為正確值 xboxservices.config 檔案中。  也請確定此檔案已正確格式化為 UTF8。  中的詳細資訊請參閱 <<c0> [ 開始使用 Visual Studio for UWP 遊戲](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)。 Xboxservices.config 檔案是區分大小寫。
* XDK 項目的，在您的 package.appxmanifest 中設定這些值。
* 您可以看到 UWP 和 XDK 標題組態的 Samples 目錄中的 Xbox Live SDK 的範例。

## <a name="test-using-the-xbox-app"></a>使用 Xbox 應用程式測試

如果您正在開發的 UWP 應用程式，您可以偵錯使用 Xbox 應用程式的一些問題：

1. 將您的裝置沙箱設定為使用 SwitchSandbox.cmd 指令碼的開發沙箱
2. 開啟 Xbox 應用程式，並嘗試存取相同的沙箱中使用測試帳戶登入。

如果您能夠成功登入，然後這會確認您的開發沙箱已在您的裝置上正確設定，和您的測試帳戶具有存取權。

如果您仍然收到登入錯誤，很可能您的服務組態不會發行到您的沙箱，或您 xboxservices.config 未設定正確。 Xboxservices.config 檔案是區分大小寫。

## <a name="debug-based-on-error-code"></a>根據錯誤的程式碼的偵錯

以下是一些您可能會看到登入和偵錯時可採取的步驟時的錯誤碼。  中所示，您會看到錯誤碼以下螢幕擷取畫面。

![0x8015DC12 登入錯誤螢幕擷取畫面](../../images/troubleshooting/sign_in_error.png)

### <a name="0x80860003-the-application-is-either-disabled-or-incorrectly-configured"></a>0x80860003 應用程式停用或未正確設定

1. 請嘗試刪除您的 PFX 檔案。

![在 [方案總管] 中的 pfx 檔案](../../images/troubleshooting/pfx_file.png)

如果您未登入時用來佈建在合作夥伴中心內的應用程式的 Microsoft 帳戶的 Visual studio，Visual Studio 就會自動產生簽章的 pfx 檔案，根據您的個人 Microsoft 帳戶或您的網域帳戶。 當建置 appx 套件，Visual Studio 會使用自動產生的 pfx，來簽署封裝與封裝識別碼在 package.appxmanifest 中的 「 發行者 」 部分。 如此一來，產生的位元 (具體而言，appxmanifest.xml) 會有比您想要使用不同的封裝識別碼。 

2. 請仔細檢查您 package.appxmanifest 設定為相同的應用程式身分識別，做為您的標題，在合作夥伴中心。 您可以以滑鼠右鍵按一下專案，並選擇 市集-> 將應用程式與市集建立關聯...中所示以下螢幕擷取畫面。 或以手動方式編輯您 package.appxmanifest。 請參閱[開始使用 Visual Studio for UWP 遊戲](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)如需詳細資訊。

![與市集建立關聯](../../images/troubleshooting/appxmanifest_binding.png)

### <a name="0x8015dc12-content-isolation-error"></a>0x8015DC12 內容隔離時發生錯誤

總而言之，這表示裝置或使用者不具有存取指定的標題。

1. 這可能表示您嘗試登入時，不使用測試帳戶，或您的測試帳戶無法存取已登入為相同的沙箱。 請仔細檢查上建立測試帳戶中的指示[XDP 文件](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/creating_development_accounts_03_31_16.aspx)和[合作夥伴中心文件。](../../xbox-live-test-accounts.md) 必要時建立新的測試帳戶的存取權適當的沙箱。

您可能需要從 Windows 10 中移除舊的帳戶，您可以從 開始 功能表中，移至 設定，並然後移至帳戶

2. 請仔細檢查，您的標題會發佈至沙箱您嘗試使用。 請參閱[XDP 文件](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_03_31_16.aspx#PublishServiceConfig)或是[合作夥伴中心文件](../../xbox-live-service-configuration.md#sandbox-ids)如需如何執行這項操作的資訊。

### <a name="0x87dd0005-unexpected-or-unknown-title"></a>0x87DD0005 非預期或無法辨識的標題

請仔細檢查應用程式識別碼設定和開發人員中心中 XDP 繫結。 您可以檢視中的指示[將 Xbox Live 支援加入新的或現有 Visual Studio UWP](https://docs.microsoft.com/windows-hardware/drivers/devapps/step-1--create-a-uwp-device-app#span-idassociateyourappwiththewindowsstorespanspan-idassociateyourappwiththewindowsstorespanspan-idassociateyourappwiththewindowsstorespanassociate-your-app-with-the-microsoft-store)

### <a name="0x87dd000e-title-not-authorized"></a>0x87DD000E 標題未授權

請仔細檢查您的裝置設為適當的開發沙箱和使用者具有存取權的沙箱。 請參閱[使用 Xbox 應用程式測試](#test-using-the-xbox-app)一節以取得指導您如何驗證這些項目與 Xbox 應用程式的詳細資訊。

如果這無法解決此問題，然後也檢查繫結開發人員中心和應用程式識別碼設定，如上面所述。

如果您收到錯誤這裡沒有描述，請參閱以取得有關錯誤碼的詳細資訊的 xbox::services::xbox_live_error_code 文件中的錯誤清單。 您也可以參考在 XSAPI errors.h 包含。

之後所有的這些步驟中，如果您仍然無法使用登入您的標題，請在支援執行緒上公佈[論壇](https://forums.xboxlive.com)或連絡您的補西牆。
