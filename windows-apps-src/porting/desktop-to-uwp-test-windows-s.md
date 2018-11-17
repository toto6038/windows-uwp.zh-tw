---
author: normesta
Description: Test your app for Windows 10 in S mode.
Search.Product: eADQiWindows 10XVcnh
title: 針對 Windows 10 S 測試您的 Windows 應用程式
ms.author: normesta
ms.date: 05/11/2017
ms.topic: article
keywords: windows 10 S, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3307506c5cf62d04cd19fbc302ad14bfcedd0045
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2018
ms.locfileid: "7167750"
---
# <a name="test-your-windows-app-for-windows-10-in-s-mode"></a>針對 Windows 10 S 模式測試您的 Windows 應用程式

您可以測試您的 Windows 應用程式，以確定此應用程式會在執行 Windows S 模式的裝置上正常運作。 事實上，如果要將應用程式發行至 Microsoft Store，您必須執行此動作，因為它是 Microsoft Store 的要求。 若要測試您的應用程式，您可以在執行 Windows 10 專業版的裝置上套用 Device Guard 程式碼完整性原則。

> [!NOTE]
> 套用 Device Guard 程式碼完整性原則的裝置必須執行 Windows 10 Creators 版本 (10.0；組建 15063) 或更新版本。

Device Guard 程式碼完整性原則會強制應用程式必須符合該原則，才能在 Windows 10 S 上執行。

> [!IMPORTANT]
>雖然我們建議您將這些項原則套用到虛擬機器，但如果您想要套用到您的本機電腦，請務必在您套用原則之前，先查看在〈下一步，安裝原則並重新啟動您的系統〉章節中我們提供的最佳做法指導。

<a id="choose-policy" />

## <a name="first-download-the-policies-and-then-choose-one"></a>首先，下載原則然後挑選一個

在[這裡](https://go.microsoft.com/fwlink/?linkid=849018)下載 Device Guard 程式碼完整性原則。

然後，選擇最適合您的其中一項。 以下是每個原則的摘要。

|原則 |強制執行 |簽署憑證 |檔案名稱 |
|--|--|--|--|
|稽核模式原則 |記錄檔問題/沒有封鎖 |市集 |SiPolicy_Audit.p7b |
|運作模式原則 |是 |市集 |SiPolicy_Enforced.p7b |
|帶有自我簽署應用程式的運作模式原則 |是 |AppX 測試憑證  |SiPolicy_DevModeEx_Enforced.p7b |

我們建議您從稽核模式原則開始。 您可以檢閱程式碼完整性事件記錄檔，並使用該資訊協助您調整您的應用程式。 然後，當您準備好進行最後一項測試時，套用運作模式原則。

以下是每個原則的更多資訊。

### <a name="audit-mode-policy"></a>稽核模式原則
在此模式下，即使不受 Windows 10 S 支援，您的應用程式還是會執行工作。Windows 會記錄下任何會被封鎖並進入程式碼完整性事件記錄檔的可執行檔。

您可以透過開啟 **\[事件檢視器\]**，並瀏覽至此位置：\[應用程式及服務記錄檔\] -> \[Microsoft]] -> \[Windows\] -> \[CodeIntegrity\] -> \[操作\]，尋找那些記錄檔。

![code-integrity-event-logs](images/desktop-to-uwp/code-integrity-logs.png)

此模式很安全，它不會導致系統無法啟動。

#### <a name="optional-find-specific-failure-points-in-the-call-stack"></a>(選擇性) 在呼叫堆疊中尋找特定失敗點
若要在發生封鎖問題的呼叫堆疊中尋找特定失敗點，請新增此登錄機碼，然後[設定核心模式偵錯環境](https://docs.microsoft.com/windows-hardware/drivers/debugger/getting-started-with-windbg--kernel-mode-#span-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanset-up-a-kernel-mode-debugging)。

|機碼|名稱|類型|值|
|--|---|--|--|
|HKEY_LOCAL_MACHINE\SYSTEM\CurentControlSet\Control\CI| DebugFlags |REG_DWORD | 1 |


![reg-setting](images/desktop-to-uwp/ci-debug-setting.png)

### <a name="production-mode-policy"></a>運作模式原則
這項原則會執行符合 Windows 10 S 的程式碼完整性規則，讓您可以模擬執行 Windows 10 S。由於這是最嚴格的原則，因此相當適合用於最終運作測試。 在此模式下，您的應用程式會受到與使用者裝置上相同的規則限制。 若要使用此模式，您的應用程式必須先經 Microsoft Store 簽署。

### <a name="production-mode-policy-with-self-signed-apps"></a>帶有自我簽署應用程式的運作模式原則
此模式與運作模式原則相似，但它允許經由包含在 zip 檔案中之測試憑證簽署過的應用程式執行。 安裝包含在此 zip 檔案中 **AppxTestRootAgency** 資料夾下的 PFX 檔案。 然後，使用它登入您的應用程式。 如此一來，您可以快速的逐一查看，而不需要市集的簽署。

因為您憑證的發行者名稱必須符合您應用程式的發行者名稱，因此必須將 **Identity** 元素 **Publisher** 屬性的值暫時變更為「CN=Appx Test Root Agency Ex」。 測試完成之後，可以將該屬性變更回其原始值。

## <a name="next-install-the-policy-and-restart-your-system"></a>下一步，安裝原則並重新啟動您的系統

我們建議您將這些項原則套用至虛擬機器，因為這些原則可能會導致開機失敗。 因為這些原則會封鎖未經 Microsoft Store 簽署之程式碼的執行，包括驅動程式。

若您想要將這些項原則套用至您的本機電腦，建議您最好先從稽核模式原則開始。 透過使用這項原則，您可以檢閱程式碼完整性事件記錄檔，以確保強制執行原則不會封鎖任何重要的項目。

當您準備好要套用原則之後，尋找您選擇之原則的 .P7B 檔案，將它重新命名為 **SIPolicy.P7B**，然後將該檔案儲存到您系統上的這個位置：**C:\Windows\System32\CodeIntegrity\\**。

然後，請重新啟動您的系統。

>[!NOTE]
>若要從您的系統移除原則，請刪除 .P7B 檔案，然後重新啟動系統。

## <a name="next-steps"></a>後續步驟

**尋找您的問題解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。

**檢視應用程式諮詢小組公布的詳細部落格文章**

請參閱[使用傳統型橋接器在 Windows 10 S 上移植並測試您的傳統桌面應用程式](https://blogs.msdn.microsoft.com/appconsult/2017/06/15/porting-and-testing-your-classic-desktop-applications-on-windows-10-s-with-the-desktop-bridge/) (英文)。

**深入了解可讓您輕鬆測試 Windows S 模式的工具**

請參閱[解除封裝、修改、重新封裝、簽署 APPX](https://blogs.msdn.microsoft.com/appconsult/2017/08/07/unpack-modify-repack-sign-appx/) (英文)。
