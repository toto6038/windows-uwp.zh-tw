---
保留檔案和 URI 配置名稱
您可以使用 URI 關聯，在另一個 app 啟動特定 URI 配置時自動啟動您的 app。
ms.assetid: 7428C4A2-1380-4EBB-9C2A-7DF7B5C468AE
---
# 保留檔案和 URI 配置名稱


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


您可以使用 URI 關聯，在另一個 app 啟動特定 URI 配置時自動啟動您的 app。 但是有些 URI 關聯是您無法使用的，即保留的關聯。 如果您的 app 登錄為保留的關聯，該登錄將會被忽略。 此主題列出並不適用於您的 app 的保留檔案和 URI 配置名稱。

## 保留的檔案類型


保留的檔案類型有兩種類型：保留給內建 app 的檔案類型，和保留給作業系統的檔案類型。 當保留給內建 app 的檔案類型啟動時，將只會啟動內建 app。 您的 app 若嘗試對該檔案類型登錄，將一律被忽略。 同樣地，您的 app 若嘗試對保留給作業系統的檔案類型進行登錄，也將一律被忽略。

保留給內建 app 的檔案類型

| .aac  | .icon    | .pem  | .wdp   |
|-------|----------|-------|--------|
| .aetx | .jpeg    | .png  | .wmv   |
| .asf  | .jxr     | .pptm | .xap   |
| .bmp  | .m4a     | .pptx | .xht   |
| .cer  | .m4r     | .qcp  | .xhtml |
| .dotm | .m4v     | .rtf  | .xltm  |
| .dotx | .mov     | .tif  | .xltx  |
| .gif  | .mp3     | .tiff | .xml   |
| .hdp  | .mp4     | .txt  | .xsl   |
| .htm  | .one     | .url  | .zip   |
| .html | .onetoc2 | .vcf  |        |
| .ico  | .p7b     | .wav  |        |
 

## 保留給作業系統的檔案類型


以下是保留給作業系統的檔案類型

| .accountpicture-ms | its      | .ops           | .url      |
|--------------------|----------|----------------|-----------|
| .ade               | .jar     | .pcd           | .vb       |
| .adp               | .js      | .pif           | .vbe      |
| .app               | .jse     | .pl            | .vbp      |
| .appx              | .ksh     | .plg           | .vbs      |
| .application       | .lnk     | .plsc          | .vhd      |
| .appref-ms         | .mad     | .prf           | .vhdx     |
| .asp               | .maf     | .prg           | .vsmacros |
| .bas               | .mag     | .printerexport | .vsw      |
| .bat               | .mam     | .provxml       | .webpnp   |
| .cab               | .maq     | .ps1           | .ws       |
| .chm               | .mar     | .ps1xml        | .wsc      |
| .cmd               | .mas     | .ps2           | .wsf      |
| .cnt               | .mat     | .ps2xml        | .wsh      |
| .com               | .mau     | .psc1          | .xaml     |
| .cpf               | .mav     | .psc2          | .xdp      |
| .cpl               | .maw     | .psm1          | .xip      |
| .crd               | .mcf     | .pst           | .xnk      |
| .crds              | .mda     | .pvw           |           |
| .crt               | .mdb     | .py            |           |
| .csh               | .mde     | .pyc           |           |
| .der               | .mdt     | .pyo           |           |
| .dll               | .mdw     | .rb            |           |
| .drv               | .mdz     | .rbw           |           |
| .exe               | .msc     | .rdp           |           |
| .fxp               | .msh     | .reg           |           |
| .fon               | .msh1    | .rgu           |           |
| .gadget            | .msh1xml | .scf           |           |
| .grp               | .msh2    | .scr           |           |
| .hlp               | .msh2xml | .shb           |           |
| .hme               | .mshxml  | .shs           |           |
| .hpj               | .msi     | .sys           |           |
| .hta               | .msp     | .theme         |           |
| .inf               | .mst     | .tmp           |           |
| .ins               | .msu     | .tsk           |           |
| .isp               | .ocx     | .ttf           |           |
 

## 保留 URI 配置名稱


| application.manifest                        | internetshortcut                      | ms-settings:network-mobilehotspot | shbfile                 |
|---------------------------------------------|---------------------------------------|-----------------------------------|-------------------------|
| application.reference                       | javascript                            | ms-settings:network-proxy         | shcmdfile               |
| batfile                                     | jscript                               | ms-settings:network-wifi          | shsfile                 |
| Bing                                        | jsefile                               | ms-settings:nfctransactions       | smb                     |
| blob                                        | ldap                                  | ms-settings:notifications         | stickynotes             |
| callto                                      | lnkfile                               | ms-settings:personalization       | sysfile                 |
| cerfile                                     | mailto                                | ms-settings:privacy-calendar      | tel                     |
| chm.filecmdfilecomfile                      | maps                                  | ms-settings:privacy-contacts      | telnet                  |
| cplfile                                     | microsoft.powershellscript.1          | ms-settings:privacy-customdevices | tn3270                  |
| dllfile                                     | ms-accountpictureprovider             | ms-settings:privacy-feedback      | ttffile                 |
| drvfile                                     | ms-appdata                            | ms-settings:privacy-location      | unknown                 |
| dtmf                                        | ms-appx                               | ms-settings:privacy-messaging     | usertileprovider        |
| exefile                                     | ms-autoplay                           | ms-settings:privacy-microphone    | vbefile                 |
| explorer.assocactionid.burnselection        | ms-excel                              | ms-settings:privacy-speechtyping  | vbscript                |
| explorer.assocactionid.closesession         | msi.package                           | ms-settings:privacy-webcam        | vbsfile                 |
| explorer.assocactionid.erasedisc            | msi.patch                             | ms-settings:proximity             | wallet                  |
| explorer.assocactionid.zipselection         | ms-powerpoint                         | ms-settings:regionlanguage        | windows.gadget          |
| explorer.assocprotocol.search-ms            | ms-settings                           | ms-settings:screenrotation        | windowsmediacenterapp   |
| explorer.burnselection                      | ms-settings:batterysaver              | ms-settings:speech                | windowsmediacenterssl   |
| explorer.burnselectionexplorer.closesession | ms-settings:batterysaver-settings     | ms-settings:storagesense          | windowsmediacenterweb   |
| explorer.closesession                       | ms-settings:batterysaver-usagedetails | ms-settings:windowsupdate         | wmp11.assocprotocol.mms |
| explorer.erasedisc                          | ms-settings:bluetooth                 | ms-settings:workplace             | wsffile                 |
| explorer.zipselection                       | ms-settings:connecteddevices          | ms-windows-store                  | wsfile                  |
| file                                        | ms-settings:cortanasearch             | ms-word                           | wshfile                 |
| fonfile                                     | ms-settings:datasense                 | ocxfile                           | xbls                    |
| hlpfile                                     | ms-settings:dateandtime               | office                            | zune                    |
| htafile                                     | ms-settings:display                   | onenote                           |                         |
| http                                        | ms-settings:lockscreen                | piffile                           |                         |
| https                                       | ms-settings:maps                      | regfile                           |                         |
| iehistory                                   | ms-settings:network-airplanemode      | res                               |                         |
| ierss                                       | ms-settings:network-cellular          | rlogin                            |                         |
| inffile                                     | ms-settings:network-dialup            | scrfile                           |                         |
| insfile                                     | ms-settings:network-ethernet          | scriptletfile                     |                         |

 

 

 





<!--HONumber=Mar16_HO1-->


