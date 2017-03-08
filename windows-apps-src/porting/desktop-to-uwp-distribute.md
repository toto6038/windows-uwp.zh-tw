---
author: awkoren
Description: "散發使用傳統型轉 UWP 橋接器轉換的 UWP App"
Search.Product: eADQiWindows 10XVcnh
title: "散佈使用傳統型轉 UWP 橋接器轉換的 UWP 應用程式"
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 0acb144d79c1d05d68cc7430f4cc99efeadc7c6b
ms.lasthandoff: 02/08/2017

---

# <a name="distribute-apps-converted-with-the-desktop-bridge"></a>散佈使用傳統型橋接器轉換的應用程式

有三種主要方式可用來部署您轉換的應用程式：Windows 市集、側載和鬆散檔案註冊。  

## <a name="windows-store"></a>Windows 市集

Windows 市集是客戶取得您 App 最簡便的方式。 若要開始使用，請填寫[透過傳統型橋接器將現有的 App 和遊戲帶入 Windows 市集](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge)的表單。 Microsoft 將會與您連絡以啟動上架程序。 

請注意，您必須是 App 或遊戲的開發人員和/或發行者，才能將 App 或遊戲帶入 Windows 市集。 因此，請確定您的名稱與電子郵件地址和下方提交的網站 URL 相符，我們才能驗證您是否為開發人員和/或發行者。

## <a name="sideloading"></a>側載

側載提供一種簡單的方式跨多部電腦部署。 這在企業/企業營運 (LOB) 案例中特別有用，尤其是在您想要更精細地控制發佈體驗，並且不想牽涉市集憑證的時候。

透過側載部署您的 App 之前，您必須以憑證簽署它。 如需建立憑證的詳細資訊，請參閱[簽署您的 .Appx 套件](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter#deploy-your-converted-appx)。 

以下是如何匯入您先前建立之憑證的方式。 您可以使用 CERTUTIL 直接安裝憑證，或者可以從您已簽署的 appx 中安裝它，就像客戶會做的一樣。 

若要透過 CERTUTIL 安裝憑證，請從系統管理員的命令提示字元執行下列命令：

```cmd
Certutil -addStore TrustedPeople <testcert.cer>
```

從 Appx 匯入憑證，就像客戶會做的一樣：

1.    在 [檔案總管] 中，以滑鼠右鍵按一下您使用測試憑證簽署的 appx，然後從操作功能表選擇 [屬性]****。
2.    按一下或點選 [數位簽章]**** 索引標籤。
3.    按一下或點選憑證，然後選擇 [詳細資料]****。
4.    按一下或點選 [檢視憑證]****。
5.    按一下或點選 [安裝憑證]****。
6.    在 [存放區位置]**** 群組中，選取 [本機電腦]****。
7.    按一下或點選 [下一步]**** 和 [確定]****，以確認 UAC 對話方塊。
8.    在 [憑證匯入精靈] 的下一個畫面中，將選取的選項變更為 [將所有憑證放入以下的存放區]****。
9.    按一下或點選 [瀏覽]****。 在 [選取憑證存放區] 視窗中，向下捲動並選取 [受信任的人]****，然後按一下或點選 [確定]****。
10.    按一下或點選 [下一步]****。 新畫面隨即顯示。 按一下或點選 [完成]****。
11.    應該會顯示確認對話方塊。 出現時，按一下 [確定]****。 如果出現其他對話方塊，指出憑證發生問題，您可能需要執行一些憑證疑難排解。

注意：若要使 Windows 信任憑證，憑證必須位於 [憑證 (本機電腦)] &gt; [信任的根憑證授權單位] &gt; [憑證]**** 節點或 [憑證 (本機電腦)] &gt; [受信任的人] &gt; [憑證]**** 節點上。 只有這兩個位置中的憑證可以驗證本機電腦內容中的憑證信任。 否則，會出現類似下列字串的錯誤訊息︰

```CMD
"Add-AppxPackage : Deployment failed with HRESULT: 0x800B0109, A certificate chain processed,
but terminated in a rootcertificate which is not trusted by the trust provider.
(Exception from HRESULT: 0x800B0109) error 0x800B0109: The root certificate of the signature
in the app package must be trusted."
```

現已信任憑證，您有 2 種方式可以用來安裝套件 – 透過 PowerShell 或只要按兩下 appx 套件檔案來安裝。  若要透過 PowerShell 安裝，請執行下列 Cmdlet：

```powershell
Add-AppxPackage <MyApp>.appx
```

### <a name="loose-file-registration"></a>鬆散檔案註冊

若檔案配置在位於您可以輕鬆存取及更新之位置的磁碟上，且不需要簽署或憑證，鬆散檔案註冊就非常適合用於偵錯用途。  

若要在開發期間部署您的 App，請執行下列 PowerShell Cmdlet： 

```Add-AppxPackage –Register AppxManifest.xml```

若要更新應用程式的 .exe 或.dll 檔案，只需使用新檔案來取代套件中現有的檔案、提高 AppxManifest.xml 的版本號碼，然後再次執行上述命令即可。

請注意下列事項： 

* 您必須將已轉換應用程式安裝所在的任意磁碟機格式設定為 NTFS 格式。
* 已轉換的 App 一律會以互動式使用者身分執行。
