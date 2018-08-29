---
title: 在應用程式之間共用憑證
description: 針對需要比使用者識別碼和密碼組合更安全之驗證方式的通用 Windows 平台 (UWP) 應用程式，即可使用憑證來進行驗證。
ms.assetid: 159BA284-9FD4-441A-BB45-A00E36A386F9
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp 安全性
ms.localizationpriority: medium
ms.openlocfilehash: 863658438ce53f2c74faddb845a7d17c6ec3130c
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/29/2018
ms.locfileid: "2911893"
---
# <a name="share-certificates-between-apps"></a>在應用程式之間共用憑證




針對需要比使用者識別碼和密碼組合更安全之驗證方式的通用 Windows 平台 (UWP) 應用程式，即可使用憑證來進行驗證。 憑證驗證可在驗證使用者時提供高階的信任層級。 在某些情況下，會有一組服務想驗證多個 app 的某位使用者。 本文說明如何使用相同的憑證來驗證多個 app，以及如何提供便利的程式碼，讓使用者匯入用來存取受保護 Web 服務的憑證。

app 可使用憑證來向 Web 服務驗證，而且多個 app 可使用憑證存放區中的單一憑證來驗證同一個使用者。 如果憑證不存在存放區中，您可以新增程式碼到 app 以匯入來自 PFX 檔案的憑證。

## <a name="enable-microsoft-internet-information-services-iis-and-client-certificate-mapping"></a>啟用 Microsoft 網際網路資訊服務 (IIS) 與用戶端憑證對應


本文使用 Microsoft 網際網路資訊服務 (IIS)，例如用途。 預設不會啟用 IIS。 您可以使用控制台來啟用 IIS。

1.  開啟 [控制台]，然後選取 **\[程式集\]**。
2.  選取 **\[開啟或關閉 Windows 功能\]**。
3.  展開 **\[Internet Information Services\]**，然後展開 **\[World Wide Web 服務\]**。 展開 **\[應用程式開發功能\]**，選取 **\[ASP.NET 3.5\]** 與 **\[ASP.NET 4.5\]**。 選擇這些項目會自動啟用 **Internet Information Services**。
4.  按一下 **\[確定\]** 套用變更。

## <a name="create-and-publish-a-secured-web-service"></a>建立與發行受保護的 Web 服務


1.  以系統管理員的身分執行 Microsoft Visual Studio，然後從起始畫面選取 **\[新增專案\]**。 必須要有系統管理員存取權才能將 Web 服務發行到 IIS 伺服器。 在 [新增專案] 對話方塊中，將 Framework 變更為 **\[.NET Framework 3.5\]**。 選取 [Visual C#]**** -&gt; [Web]**** -&gt; [Visual Studio]**** -&gt; [ASP.NET Web Service 應用程式]****。 將應用程式命名為 "FirstContosoBank"。 按一下 **\[確定\]** 來建立專案。
2.  在 **Service1.asmx.cs** 檔案中，將預設的 **HelloWorld** Web 方法取代為下列 "Login" 方法。
    ```cs
            [WebMethod]
            public string Login()
            {
                // Verify certificate with CA
                var cert = new System.Security.Cryptography.X509Certificates.X509Certificate2(
                    this.Context.Request.ClientCertificate.Certificate);
                bool test = cert.Verify();
                return test.ToString();
            }
    ```

3.  儲存 **Service1.asmx.cs** 檔案。
4.  在 **\[方案總管\]** 中，用滑鼠右鍵按一下 "FirstContosoBank" app，然後選取 **\[發行\]**。
5.  在 **\[發行 Web\]** 對話方塊中建立新的設定檔，將它命名為 "ContosoProfile"。 按 **\[下一步\]**。
6.  在下一頁中，輸入 IIS 伺服器的伺服器名稱，然後指定網站名稱為 "Default Web Site/FirstContosoBank"。 按一下 **\[發行\]** 以發行您的 Web 服務。

## <a name="configure-your-web-service-to-use-client-certificate-authentication"></a>設定您的 Web 服務以使用用戶端憑證驗證


1.  執行 **\[網際網路資訊服務 (IIS) 管理員\]**。
2.  展開 IIS 伺服器的網站。 在 **\[預設的網站\]** 底下選取新的 \[FirstContosoBank\] Web 服務。 在 **\[動作\]** 區段中，選取 **\[進階設定\]**。
3.  將 **\[應用程式集區\]** 設為 **\[.NET v2.0\]**，然後按一下 **\[確定\]**。
4.  在 **\[網際網路資訊服務 (IIS) 管理員\]** 中選取您的 IIS 伺服器，然後按兩下 **\[伺服器憑證\]**。 在 **\[動作\]** 區段中，選取 **\[建立自我簽署憑證...\]**。輸入 "ContosoBank" 做為憑證的易記名稱，然後按一下 **\[確定\]**。 這樣會建立下列格式的新憑證供 IIS 伺服器使用："&lt;伺服器名稱&gt;.&lt;網域名稱&gt;"。
5.  在 **\[網際網路資訊服務 (IIS) 管理員\]** 中選取預設網站。 在 **\[動作\]** 區段中，選取 **\[繫結\]**，然後按一下 **\[新增...\]**。選取 \[https\] 做為類型，將連接埠設為 "443"，然後輸入 IIS 伺服器的完整主機名稱 (&lt;伺服器名稱&gt;.&lt;網域名稱&gt;)。 將 SSL 憑證設為 "ContosoBank"。 按一下 **\[確定\]**。 在 **\[站台繫結\]** 視窗中按一下 **\[關閉\]**。
6.  在 **\[網際網路資訊服務 (IIS) 管理員\]** 中，選取 \[FirstContosoBank\] Web 服務。 按兩下 **\[SSL 設定\]**。 選取 **\[需要 SSL\]**。 選取 **\[用戶端憑證\]** 下方的 **\[需要\]**。 在 **\[動作\]** 區段中，按一下 **\[套用\]**。
7.  您可以開啟網頁瀏覽器並輸入下列網址，以確認 Web 服務是否已正確設定："https://&lt;伺服器名稱&gt;.&lt;網域名稱&gt;/FirstContosoBank/Service1.asmx"。 例如，"https://myserver.example.com/FirstContosoBank/Service1.asmx"。 如果您的 Web 服務已正確設定，系統會提示您選取用戶端憑證以存取 Web 服務。

您可以重複上述步驟來建立多個 Web 服務，並使用相同的用戶端憑證來存取這些服務。

## <a name="create-a-uwp-app-that-uses-certificate-authentication"></a>建立使用憑證驗證的 UWP app


現在您已有一或多個受保護的 Web 服務後，您的 app 可以使用憑證向那些 Web 服務進行驗證。 當您使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 物件向驗證的 Web 服務提出要求時，初始要求將不會包含用戶端憑證。 已驗證的 Web 服務將以用戶端驗證要求回應。 發生此情況時，Windows 用戶端將自動查詢憑證存放區是否有可用的用戶端憑證。 您的使用者可從這些憑證選取以向 Web 服務驗證。 某些憑證受密碼保護，因此您必須提供使用者輸入密碼的方式以取得憑證。

如果沒有可用的用戶端憑證，則使用者必須新增憑證到憑證存放區。 您可以在 app 中包含程式碼，讓使用者選取包含用戶端憑證的 PFX 檔案，然後將該憑證匯入用戶端憑證存放區。

**秘訣**  您可以使用 makecert.exe 來建立 PFX 檔案以配合此快速入門使用。 如需使用 makecert.exe 的資訊，請參閱 [MakeCert](https://msdn.microsoft.com/library/windows/desktop/aa386968)。

 

1.  開啟 Visual Studio，然後從開始頁面建立新專案。 將新專案命名為 "FirstContosoBankApp"。 按一下 **\[確定\]** 以建立新的專案。
2.  在 MainPage.xaml 檔案中，將下列 XAML 新增至預設的 **Grid** 元素。 這個 XAML 包含一個瀏覽要匯入之 PFX 檔案的按鈕、一個輸入受密碼保護之 PFX 檔案的密碼的文字方塊、一個匯入所選 PFX 檔案的按鈕、一個登入受保護的 Web 服務的按鈕，以及一個顯示目前動作狀態的文字區塊。
    ```xml
    <Button x:Name="Import" Content="Import Certificate (PFX file)" HorizontalAlignment="Left" Margin="352,305,0,0" VerticalAlignment="Top" Height="77" Width="260" Click="Import_Click" FontSize="16"/>
    <Button x:Name="Login" Content="Login" HorizontalAlignment="Left" Margin="611,305,0,0" VerticalAlignment="Top" Height="75" Width="240" Click="Login_Click" FontSize="16"/>
    <TextBlock x:Name="Result" HorizontalAlignment="Left" Margin="355,398,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Height="153" Width="560"/>
    <PasswordBox x:Name="PfxPassword" HorizontalAlignment="Left" Margin="483,271,0,0" VerticalAlignment="Top" Width="229"/>
    <TextBlock HorizontalAlignment="Left" Margin="355,271,0,0" TextWrapping="Wrap" Text="PFX password" VerticalAlignment="Top" FontSize="18" Height="32" Width="123"/>
    <Button x:Name="Browse" Content="Browse for PFX file" HorizontalAlignment="Left" Margin="352,189,0,0" VerticalAlignment="Top" Click="Browse_Click" Width="499" Height="68" FontSize="16"/>
    <TextBlock HorizontalAlignment="Left" Margin="717,271,0,0" TextWrapping="Wrap" Text="(Optional)" VerticalAlignment="Top" Height="32" Width="83" FontSize="16"/>
    ```
    
3.  儲存 MainPage.xaml 檔案。
4.  在 MainPage.xaml.cs 檔案中，新增下列 using 陳述式。
    ```cs
    using Windows.Web.Http;
    using System.Text;
    using Windows.Security.Cryptography.Certificates;
    using Windows.Storage.Pickers;
    using Windows.Storage;
    using Windows.Storage.Streams;
    ```

5.  在 MainPage.xaml.cs 檔案中，將下列變數新增至 **MainPage** 類別。 它們會指定 "FirstContosoBank" Web 服務的受保護 "Login" 方法的位址，以及存放要匯入憑證存放區之 PFX 憑證的全域變數。 將 &lt;伺服器名稱&gt; 更新為 Microsoft Internet Information Server (IIS) 伺服器的完整伺服器名稱。
    ```cs
    private Uri requestUri = new Uri("https://<server-name>/FirstContosoBank/Service1.asmx?op=Login");
    private string pfxCert = null;
    ```

6.  在 MainPage.xaml.cs 檔案中，為登入按鈕新增下列 Click 處理常式並新增存取受保護的 Web 服務的方法。
    ```cs
    private void Login_Click(object sender, RoutedEventArgs e)
    {
        MakeHttpsCall();
    }

    private async void MakeHttpsCall()
    {

        StringBuilder result = new StringBuilder("Login ");
        HttpResponseMessage response;
        try
        {
            Windows.Web.Http.HttpClient httpClient = new Windows.Web.Http.HttpClient();
            response = await httpClient.GetAsync(requestUri);
            if (response.StatusCode == HttpStatusCode.Ok)
            {
                result.Append("successful");
            }
            else
            {
                result = result.Append("failed with ");
                result = result.Append(response.StatusCode);
            }
        }
        catch (Exception ex)
        {
            result = result.Append("failed with ");
            result = result.Append(ex.Message);
        }

        Result.Text = result.ToString();
    }
    ```

7.  在 MainPage.xaml.cs 檔案中，為瀏覽 PFX 檔案的按鈕和將所選 PFX 檔案匯入憑證存放區的按鈕新增下列 Click 處理常式。
    ```cs
    private async void Import_Click(object sender, RoutedEventArgs e)
    {
        try
        {
            Result.Text = "Importing selected certificate into user certificate store....";
            await CertificateEnrollmentManager.UserCertificateEnrollmentManager.ImportPfxDataAsync(
                pfxCert,
                PfxPassword.Password,
                ExportOption.Exportable,
                KeyProtectionLevel.NoConsent,
                InstallOptions.DeleteExpired,
                "Import Pfx");

            Result.Text = "Certificate import succeded";
        }
        catch (Exception ex)
        {
            Result.Text = "Certificate import failed with " + ex.Message;
        }
    }

    private async void Browse_Click(object sender, RoutedEventArgs e)
    {

        StringBuilder result = new StringBuilder("Pfx file selection ");
        FileOpenPicker pfxFilePicker = new FileOpenPicker();
        pfxFilePicker.FileTypeFilter.Add(".pfx");
        pfxFilePicker.CommitButtonText = "Open";
        try
        {
            StorageFile pfxFile = await pfxFilePicker.PickSingleFileAsync();
            if (pfxFile != null)
            {
                IBuffer buffer = await FileIO.ReadBufferAsync(pfxFile);
                using (DataReader dataReader = DataReader.FromBuffer(buffer))
                {
                    byte[] bytes = new byte[buffer.Length];
                    dataReader.ReadBytes(bytes);
                    pfxCert = System.Convert.ToBase64String(bytes);
                    PfxPassword.Password = string.Empty;
                    result.Append("succeeded");
                }
            }
            else
            {
                result.Append("failed");
            }
        }
        catch (Exception ex)
        {
            result.Append("failed with ");
            result.Append(ex.Message); ;
        }

        Result.Text = result.ToString();
    }
    ```

8.  執行您的 app 並登入受保護的 Web 服務，以及將 PFX 檔案匯入本機憑證存放區。

您可以使用這些步驟來建立多個應用程式，它們會使用相同的使用者憑證來存取相同或不同的受保護 Web 服務。