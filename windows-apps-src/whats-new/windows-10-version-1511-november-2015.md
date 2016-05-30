---
Description&#58; 作者：QuinnRadich Windows 10 1511 版和開發人員工具的更新持續提供通用 Windows 平台所支援的工具、功能及使用經驗。
title&#58; Windows 10 1511 版提供給開發人員的新功能：2015 年 11 月
---

# Windows 10 1511 版提供給開發人員的新功能：2015 年 11 月

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Windows 10 1511 版和開發人員工具的更新持續提供通用 Windows 平台所支援的工具、功能及使用經驗。 在 Windows 10 1511 版上[安裝工具和 SDK](https://dev.windows.com/downloads) 之後，您即已經準備好可以[建立新的通用 Windows 應用程式](https://msdn.microsoft.com/library/windows/apps/bg124288)，或探索如何使用您在[Windows 上的現有應用程式程式碼](https://msdn.microsoft.com/library/windows/apps/mt238321)。

## 使用者經驗

新的 <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.aspx">Windows.UI.StartScreen.JumpList</a> 和 <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.aspx">Windows.UI.StartScreen.JumpListItem</a> 類別可讓 app 以程式設計方式選取其所需使用之系統管理的捷徑清單類型、將自訂工作進入點新增至其捷徑清單，以及將自訂群組新增至其捷徑清單。

## 輸入
                                        
* <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.input.keyboarddeliveryinterceptor.aspx">鍵盤傳遞攔截器</a>
                                        
    可讓 App 覆寫系統對原始鍵盤輸入的處理，包括鍵盤快速鍵、便捷鍵 (或熱鍵)、快速鍵和應用程式鍵，但排除 Secure Attention Sequence (SAS) 按鍵組合。

    Secure Attention Sequence (SAS) 按鍵組合 (包括 Ctrl-Alt-Del 和 Windows-L) 將繼續由系統處理。
                                        
* <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.core.corewindow.aspx">UWP app</a> 和<a href="https://msdn.microsoft.com/library/windows/desktop/hh454903(v=vs.85).aspx">傳統 Windows 應用程式</a>之指標輸入的交叉處理鏈結。
                                        
    啟用輸入之跨處理程序鏈結的新指標事件。    
                                        
* <a href="https://msdn.microsoft.com/library/windows/desktop/mt622165(v=vs.85).aspx">適用於傳統型應用程式的筆跡呈現器</a>
                                        
    筆跡展示器 API 可讓 Microsoft Win32 應用程式透過在 App 的 <a href="https://msdn.microsoft.com/library/windows/desktop/hh437371(v=vs.85).aspx">DirectComposition</a> 視覺化樹狀結構中插入的 <a href="https://msdn.microsoft.com/library/windows/desktop/windows.ui.input.inking.inkpresenter.aspx">InkPresenter</a> 物件，管理筆跡輸入 (標準和修改過的) 輸入、處理和轉譯。    
                                    
## 網路功能
                                                                        
Websocket 使用者︰<a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">MessageWebSocket.OutputStream.FlushAsync</a> 和 <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">StreamWebSocket.OutputStream.FlushAsync</a> 已完整實作，並等候先前發出 WriteAsync 呼叫完成。 請注意，如果在您呼叫 <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">FlushAsync</a> 時，WebSocket 處於無效狀態，這可能會造成現有的程式碼擲回例外狀況。    

新的屬性 <a href="https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx">CookieUsageBehavior</a> 已新增到現有的 <a href="https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx">Windows.Web.Http.Filters.HttpBaseProtocolFilter 類別</a>。 這可讓開發人員控制系統處理 Cookie 的方式。    
                                    
## ORTC
                                    
Microsoft Edge 現在會實作 <a href="https://msdn.microsoft.com/library/mt433097(v=vs.85).aspx">ORTC (物件即時通訊)</a>，可讓您直接在瀏覽器、行動裝置和伺服器之間，透過原生 Javascript API 在 Web 上執行即時音訊/視訊通話。 透過對群組視訊通話、同時聯播、可調式視訊編碼 (SVC) 等功能的支援，開發人員現在可以使用 ORTC API，在 Microsoft Edge 瀏覽器之上建置進階的即時音訊/視訊通訊應用程式。    

如需在 Microsoft Edge 瀏覽器之間透過 ORTC API 進行 1:1 音訊/視訊通話的示範，請造訪<a href="/microsoft-edge/testdrive/demos/ortcdemo/">試用網站和示範</a>。 如需概觀和程式碼範例逐步解說，請造訪 <a href="https://msdn.microsoft.com/library/mt588497(v=vs.85).aspx">ORTC 開發人員指南入門</a>。
                                        
## Microsoft Edge F12 開發人員工具
                                                                        
Microsoft Edge 針對 F12 開發人員工具導入了絕佳的全新改良功能，包括一些最常從 <a href="https://wpdev.uservoice.com/forums/257854-microsoft-edge-developer">UserVoice</a> 要求的功能。 探索 DOM 總管、主控台、偵錯工具、網路、效能、記憶體、模擬中的新功能，以及新的實驗工具，您可以在這些強大的新功能完成之前加以試用。 新工具內建於 TypeScript 中，並且會持續執行，因此不需要重新載入。 此外，F12 開發人員工具文件現在是 <a href="http://dev.modern.ie/">Microsoft Edge 開發人員網站</a>的一部分，並且完整提供於 <a href="https://github.com/MicrosoftEdge/MicrosoftEdge-Documentation">GitHub</a> 上。 從現在起，這些文件不僅需要您的寶貴意見，我們也邀請您對文件的修訂提供貢獻與協助。 如需 F12 開發人員工具的簡短影片介紹，請造訪 <a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/Microsoft-Edge-F12-tools">Channel9 的開發人員短片</a>。    
                                    
## Windows Hello
                                    
Windows Hello 可讓您的應用程式啟用臉部或指紋辨識登入 Windows 系統或裝置的功能。

提供者 API 可讓 IHV 和 OEM 對 UWP 公開電腦視覺的深度、紅外線和色彩相機 (和相關的中繼資料)，並將相機指定為參與 Windows Hello 臉部驗證的相機。 <a href="http://go.microsoft.com/fwlink/?LinkId=691697">Windows.Devices.Perception</a> 命名空間包含用戶端 API，可讓 UWP 應用程式存取電腦願景相機的色彩、深度或紅外線資料。
                                    
## 新的遊戲 API

使用新的 Windows.Gaming.UI.GameBar 類別，可在顯示或關閉遊戲列時收到通知。    
                            
                                    
## 藍牙 API
                                    
現已新增數個 API 並進行更新，以擴充對藍牙 LE、裝置列舉及藍牙中其他功能的支援。 請參閱 <a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.aspx">Windows.Devices.Bluetooth</a> 命名空間。    
                                   
## 智慧卡 API ## 

已有數個 SmartCardCryptogram API 新增到 <a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.aspx">Windows.Devices.SmartCards</a> 命名空間，以支援安全密碼付款通訊協定。 使用主機卡模擬支援輕觸付款的付款應用程式，可以利用這些 API 獲得更高的安全性和效能。 應用程式可以使用 TPM 建立金鑰，並保護使用受限的交易金鑰。 應用程式也可以利用 NGC (下一代認證) 架構，透過使用者的 PIN 來保護金鑰。 這些 API 會將產生密碼的工作委派給系統，以提高效能。 這樣也可防止其他應用程式存取金鑰和密碼。    
                                    
## 更新的儲存 API ## 
    
<a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.aspx">Windows.Storage.DownloadsFolder 類別</a><br />
現在，您的 App 可以在 [下載] 資料夾內，為特定的<a href="https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx">使用者</a><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.createfileforuserasync.aspx">建立檔案</a>或<a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.createfolderforuserasync.aspx">建立資料夾</a>。
                                            
<a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.storagelibrary.aspx">Windows.Storage.StorageLibrary 類別</a><br />
現在，您的 App 可以為特定的<a href="https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx">使用者</a><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.storagelibrary.getlibraryforuserasync.aspx">取得指定的媒體櫃</a>。
                                    
## Windows 應用程式認證套件 ## 
                                    
Windows 應用程式認證套件已經由更嚴格的測試進行更新。 如需更新的完整清單，請造訪 <a href="/develop/app-certification-kit">Windows 應用程式認證套件</a>頁面。    
                                    
## 設計下載 ## 

看看我們針對 Adobe Photoshop 新推出的 UWP 應用程式設計範本。 我們也會更新我們的 Microsoft PowerPoint 和 Adobe Illustrator 範本，並提供我們 PDF 版的指導方針。 <a href="/design/assets">造訪設計下載頁面</a>。    




<!--HONumber=May16_HO2-->


