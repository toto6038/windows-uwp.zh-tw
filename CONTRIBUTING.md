# <a name="contributing-to-uwp-conceptual-documentation"></a>參與 UWP 概念文件

感謝您對通用 Windows 平台 (UWP) 文件感興趣！ 我們非常感謝您對我們文件的意見、編輯和新增。

此頁面涵蓋參與我們開發人員文件的基本步驟。

## <a name="public-and-private-repos"></a>公用和私人存放庫

UWP 概念性文件裝載於兩個不同的存放庫中，接著這兩個存放庫會合併並更新為單一網站：一個存放庫是任何人的參與，另一個則是 Microsoft 員工的參與。

如果您***不***是 Microsoft 員工，請使用[公用內容存放庫](https://github.com/MicrosoftDocs/windows-uwp)。

如果您***是***Microsoft 員工，則可以使用公用存放庫或[私人內容存放庫](https://cpubwin.visualstudio.com/_git/windows-uwp)。 員工可以透過稍微快速的方式即時推送變更，方法是參與私人存放庫，或將[特定分支](https://review.docs.microsoft.com/en-us/windows-authoring-guide/uwp/conceptual/setup-local-repo-for-large-changes#what-branch-should-i-use-for-my-authoring)用於需要嚴密控制的變更，直到某個未來日期為止。

## <a name="editing-topics-on-the-public-repo"></a>編輯公用存放庫的主題

我們已嘗試盡可能簡單地對現有檔案進行編輯。 
- 如果您已經在存放庫中，則只需要導覽至該檔案，然後按一下 **[編輯]** 按鈕。  
- 或者，如果您正在瀏覽器中檢視 Docs.microsoft.com 頁面，請按一下頁面右上方的 **[編輯]** 按鈕。 系統會將您重新導向至存放庫中的正確 Markdown 來源檔案，您可以在其中按一下**[編輯]** 按鈕。 

GitHub 會自動將官方存放庫分為個人 GitHub 帳戶，您可以在其中進行變更。 當您完成時，請將提取要求提交回 "docs" 分支。 建立提取要求之後，UWP 文件小組的成員會檢閱您的變更。 如果接受您的要求，則會將更新發行至 https://docs.microsoft.com/windows/。

您只需要幾分鐘的時間就可以了解 Markdown 的基本概念。  若要開始，請查看[使用 Markdown](https://guides.github.com/features/mastering-markdown/)(英文)。

## <a name="making-more-substantial-changes"></a>進行更多的變更

若要對現有的文件進行更多的變更、新增或變更影像，或提供新的文件，您需要建立我們私人內容存放庫的本機複本。 請遵循 [Windows 製作手冊中的指示](https://review.docs.microsoft.com/en-us/windows-authoring-guide/uwp/conceptual/)(英文)。 如果您尚未設定 GitHub 帳戶以及加入網域的 Microsoft 別名，[請從這裡開始](https://review.docs.microsoft.com/en-us/windows-authoring-guide/github-account)(英文)。

## <a name="using-issues-to-provide-feedback-on-uwp-conceptual-documentation"></a>使用 Issues 提供對 UWP 概念文件的意見反應

如果您只想要提供意見反應，而不是直接修改實際的文件頁面，則可以[在公用存放庫中建立問題](https://github.com/MicrosoftDocs/windows-uwp/issues)(英文)。 按一下 [問題] 索引標籤，然後按一下**[新問題]** 按鈕。 請務必包括主題標題和頁面 URL。

UWP 文件小組的成員會定期檢閱問題，以及適當地對其進行分級、指派和處理。

*針對內部問題，請使用 [http://aka.ms/pubrequest](http://aka.ms/pubrequest) 上的 WDG Content Request Tool。 
