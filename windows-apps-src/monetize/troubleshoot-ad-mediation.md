---
author: mcleanbyron
Description: "以下是與廣告流量分配有關的幾個一般開發問題的解決方案。"
title: "疑難排解廣告流量分配"
ms.assetid: 8728DE4F-E050-4217-93D3-588DD3280A3A
translationtype: Human Translation
ms.sourcegitcommit: 10dcf3c2b8ea530b94e9c17ada80aaa98e9418fe
ms.openlocfilehash: f32dc28c9b199c11a1932639f49ab4c29d3e1e8f

---

# 疑難排解廣告流量分配


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

以下是與廣告流量分配有關的幾個一般開發問題的解決方案。

**您無法將 AdMediatorControl 新增到設計介面**  
當您在使用 C# 或 Visual Basic 與 XAML 的通用 Windows 平台 (UWP)、Windows 8.1 或 Windows Phone 8.1 專案中首次將 **AdMediatorControl** 控制項拖曳到設計工具時，Visual Studio 會將必要的廣告流量分配者組件參考新增到您的專案，但還不會將該控制項新增到設計工具。 若要新增控制項，請按一下 Visual Studio 所顯示之訊息中的 \[確定\]，接著等候設計工具重新整理，然後將控制項再次拖曳回設計工具。

如果您仍然無法順利將控制項新增到設計工具，請確定您的專案目標為應用程式適用的處理器架構 (例如，**x86**)，而不是**任何 CPU**。 如果專案建置平台的目標是 **任何 CPU**，則控制項無法新增到設計工具。


            *
              *AdMediatorControl 會在執行階段提供來自 Microsoft 的廣告時顯示下列錯誤：「不支援 &lt;*width*
            &gt; x &lt;*height*&gt;」**Microsoft Advertising 只支援 [Interactive Advertising Bureau (IAB) 建議的特定廣告大小](add-and-use-the-ad-mediator-control.md#supported-ad-sizes-for-microsoft-advertising)。在某些情況下，即使您在設計工具或 XAML 中將廣告流量分配控制項的高度和寬度設定為這其中一個支援的廣告大小，縮放比例和進位問題仍然可能阻止廣告流量分配架構提供廣告。若要避免這個問題，請在程式碼中將 Microsoft Advertising 的** Width **和** Height** 選擇性參數設定為其中一個支援的廣告大小。

下列程式碼範例示範如何將 Microsoft Advertising 的 **Width** 和 **Height** 選擇性參數指派為 728 x 90。

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 728;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 90;
```

**廣告流量分配不包含廣告網路的位置 (緯度/經度)**  
如果您啟用 App 中的定位功能，廣告流量分配者控制項會自動抓取緯度/經度座標，並提供給支援的廣告網路。

**Smaato 廣告控制項沒有正確對齊**  
請嘗試使用選擇性參數設定 SDK 控制項的值：

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Margin"] = new Thickness(0, -20, 0, 0);
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 50d;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 320d;
```

**AdDuplex 廣告控制項沒有以正確的大小顯示 (它是顯示為 250x250)**  
廣告流量分配不會設定任何大小的值，因此您應使用選擇性參數 **Size** 加以變更。 例如：

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.AdDuplex]["Size"] = "160x600";
```

**您收到「廣告控制項被覆蓋」錯誤**  
只要廣告在您的 App 中被以任何方式遮蔽，Ad Duplex 就會顯示錯誤。 
            [閱讀解決方式](http://blog.adduplex.com/2014/01/solving-something-is-covering-ad.mdl)以解決此錯誤。

**您收到「兩個檔案之間發生衝突」的錯誤**  
您在應用程式中的某一個地方參考了 Microsoft Advertising 組件。 廣告流量分配的設計是只能在您的應用程式中運作，如果使用了對 Microsoft Advertising 組件的其他參考，它將不會運作。 請手動移除 Microsoft Advertising 參考，並重新安裝 Microsoft Store Engagement and Monetization SDK 來清除此錯誤。

**變更 AdMediator.config 檔案中的 RefreshRate 值後，發生非預期的行為**

在 Visual Studio 中的 \[加入已連接服務\] 對話方塊內執行 Ad Mediator 元件來設定您的廣告網路之後，預設設定資訊會儲存到您專案中的 AdMediator.config。 您無法直接修改此檔案。 而是在上傳應用程式套件到 Windows 開發人員中心儀表板後，[為 App 設定廣告流量分配設定](submit-your-app-and-configure-ad-mediation.md)時修改此資訊 (包含新廣告重新整理的頻率)。

如果您變更 AdMediator.config 檔案中的 **RefreshRate** 值，請注意這個值必須包含 30 到 120 之間的整數，代表以秒為單位的重新整理頻率。 如果您設定小於 30 或大於 120 的整數，廣告流量分配架構將會自動使用 60 秒的重新整理頻率。

## 相關主題

* [選取和管理廣告網路](select-and-manage-your-ad-networks.md)
* [新增和使用廣告流量分配控制項](add-and-use-the-ad-mediator-control.md)
* [測試您的廣告流量分配實作](test-your-ad-mediation-implementation.md)
* [送出您的應用程式並設定廣告流量分配](submit-your-app-and-configure-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


