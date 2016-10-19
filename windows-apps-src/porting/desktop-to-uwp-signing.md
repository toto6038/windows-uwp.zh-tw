# 簽署已轉換的傳統型應用程式

本文說明如何簽署已轉換至通用 Windows 平台 (UWP) 的傳統型應用程式。 部署 .appx 套件之前，您必須以憑證簽署它。

## 使用 Desktop App Converter (DAC) 自動簽署

執行 DAC 以自動簽署您的 .appx 套件時，請使用 ```-Sign``` 旗標。 如需詳細資訊，請參閱 [Desktop App Converter 預覽](desktop-to-uwp-run-desktop-app-converter.md)。

## 使用 SignTool.exe 手動簽署

首先，使用 MakeCert.exe 建立憑證。 如果系統要求您輸入密碼，請選取「無」。 

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
```

接下來，使用 pvk2pfx.exe 將您的公開金鑰和私密金鑰的資訊複製到憑證中。 

```cmd
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
```
最後，使用 SignTool.exe 來使用憑證簽署您的 .appx。

```cmd
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
``` 

如需其他詳細資訊，請參閱[如何使用 SignTool 簽署應用程式套件](https://msdn.microsoft.com/library/windows/desktop/jj835835.aspx)。 

Microsoft Windows 10 SDK 已包含上述三個工具。 若要直接呼叫它們，請從命令提示字元呼叫 ```C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\Tools\VsDevCmd.bat``` 指令碼。

## 常見錯誤

### 發行者和憑證不符會導致 Signtool 錯誤「錯誤：SignerSign() 失敗」(-2147024885/0x8007000b)

Appx 資訊清單中的發行者項目必須符合您用來簽署的憑證主體。  您可以使用下列任一種方法來檢視憑證的主體。 

**選項 1：Powershell**

執行下列 PowerShell 命令。 .cer 或.pfx 可以用來做為憑證檔案，因為它們具有相同的發行者資訊。

```ps
(Get-PfxCertificate <cert_file>).Subject
```

**選項 2：檔案總管**

按兩下 [檔案總管] 中的憑證、選取 [詳細資料]** 索引標籤，然後選取清單中的 [主體]** 欄位。 您接著可以複製內容。 

**選項 3：CertUtil**

從 PFX 檔案的命令列執行 **certutil**，然後從輸出複製 [主體]** 欄位。 

```cmd
certutil -dump <cert_file.pfx>
```

### 毀損或格式錯誤的 Authenticode 簽章

本節中的詳細資訊包括如何識別 AppX 套件中可攜式執行 (PE) 檔的問題，且套件中可能包含損毀或格式錯誤的 Authenticode 簽章。 PE 檔 (exe、.dll、.chm 等任何二進位檔案格式) 上無效的 Authenticode 簽章，會導致無法正確簽署您的套件，進而造成無法從 AppX 套件部署它。 

PE 檔的 Authenticode 簽章的位置是由「選用標頭資料目錄」中的「憑證表格」及相關的「屬性憑證表格」來指定。 驗證簽章的期間，會使用這些結構中的資訊來找出 PE 檔上的簽章。 如果這些值已損毀，就可能使檔案的簽署看似無效。 

為了使 Authenticode 簽章正確無誤，Authenticode 簽章必須符合下列條件：

- PE 檔中 **WIN_CERTIFICATE** 項目的開頭不可延伸超過可執行檔的結尾
- **WIN_CERTIFCATE** 項目應位在映像的結尾
- **WIN_CERTIFICATE** 項目的大小必須是正值
- 32 位元可執行檔的 **WIN_CERTIFICATE** 項目必須在 **IMAGE_NT_HEADERS32** 結構之後開始，64 位元可執行檔則是在 IMAGE_NT_HEADERS64 結構之後開始

如需詳細資訊，請參閱 [Authenticode 可攜式可執行檔規格](http://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx) (英文) 和 [PE 檔格式規格](https://msdn.microsoft.com/windows/hardware/gg463119.aspx)。 

請注意，嘗試簽署 AppX 套件時，SignTool.exe 可以輸出損毀或格式錯誤之二進位檔案的清單。 若要這樣做，請將環境變數 APPXSIP_LOG 設定為 1 (如 ```set APPXSIP_LOG=1```) 以啟用詳細資訊記錄，然後重新執行 SignTool.exe。

若要修正這些格式錯誤的二進位檔，請確定它們符合上述需求。

## 另請參閱

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)
- [SignTool.exe (簽署工具)](https://msdn.microsoft.com/library/8s9b9yaz.aspx)
- [如何使用 SignTool 簽署應用程式套件](https://msdn.microsoft.com/library/windows/desktop/jj835835.aspx)

<!--HONumber=Sep16_HO2-->


