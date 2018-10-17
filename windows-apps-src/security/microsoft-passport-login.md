---
title: 建立 Windows Hello 登入應用程式
description: 這是一份完整逐步解說的第 1 部分，將說明如何建立會利用 Windows Hello 來取代傳統的使用者名稱及密碼驗證系統的 Windows 10 UWP (通用 Windows 平台) 應用程式。
ms.assetid: A9E11694-A7F5-4E27-95EC-889307E0C0EF
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp 安全性
ms.localizationpriority: medium
ms.openlocfilehash: 106ea458502a95c53ecbf02d9118f3c31ff43978
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2018
ms.locfileid: "4691371"
---
# <a name="create-a-windows-hello-login-app"></a>建立 Windows Hello 登入應用程式

這是一份完整逐步解說的第 1 部分，將說明如何建立會利用 Windows Hello 來取代傳統的使用者名稱及密碼驗證系統的 Windows 10 UWP (通用 Windows 平台) 應用程式。 應用程式利用使用者名稱來進行登入作業，並為每個帳戶建立 Hello 金鑰。 這些帳戶會受到 PIN 碼的保護；而該 PIN 碼是在 Windows 設定中針對 Windows Hello 組態所設定的。

這個逐步解說分成兩個部分：建置應用程式，以及連線至後端服務。 當您讀完這篇文章之後，請繼續參閱第 2 部分：[Windows Hello 登入服務](microsoft-passport-login-auth-service.md)。

在您開始之前，您應先閱讀 [Windows Hello](microsoft-passport.md) 概觀，讓自己對 Windows Hello 的運作方式有大致的概念。

## <a name="get-started"></a>入門


為了能順利建置這個專案，您需要有 C# 及 XAML 方面的經驗。 您也需要使用 Visual Studio 2015 (Community 版或更高版本)，或更新版本的 Visual Studio 中，Windows 10 電腦上。 Visual Studio 2015 時所需的最低版本，我們建議您針對最新的開發人員和安全性更新使用最新版的 Visual Studio。

-   開啟 Visual Studio，然後選取檔案 > 新增 > 專案。
-   這將會開啟 \[新增專案\] 視窗。 瀏覽至 \[範本\] &gt; \[Visual C#\]。
-   選擇 \[空白應用程式 (通用 Windows)\]，然後把您的應用程式命名為「PassportLogin」。
-   建置並執行新的應用程式 (F5)，您應該會看到畫面出現空白的視窗。 關閉應用程式。

![Windows Hello 新專案](images/passport-login-1.png)

## <a name="exercise-1-login-with-microsoft-passport"></a>練習 1：使用 Microsoft Passport 登入


您將在這個練習中，了解如何查看電腦是否已設定 Windows Hello，以及如何使用 Windows Hello 來登入帳戶。

-   您將在新的專案中，為新的解決方案建立名為「Views」的新資料夾。 這個資料夾將包含會在這個範例中瀏覽的頁面。 在方案總管中，用滑鼠右鍵按一下專案、選取 \[加入\] > \[新增資料夾\]，然後將資料夾重新命為 Views。

    ![Windows Hello 新增資料夾](images/passport-login-2.png)

-   用滑鼠右鍵按一下新的 \[Views\] 資料夾、選取 \[加入\] > \[新增項目\]，然後選取 \[空白頁\]。 請將此頁面命名為 "Login.xaml"。

    ![Windows Hello 新增空白頁](images/passport-login-3.png)

-   如要定義新登入頁面的使用者介面，請加入下列 XAML。 這個 XAML 將定義 StackPanel 來對齊下列子系：

    -   將包含標題的 TextBlock。
    -   錯誤訊息用的 TextBlock。
    -   用來輸入使用者名稱的 TextBox。
    -   用來前往註冊頁面的 Button。
    -   用來包含 Windows Hello 狀態的 TextBlock。
    -   由於沒有後端，也沒有已設定的使用者，因此用 TextBlock 來說明登入頁面。

    ```xml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <StackPanel Orientation="Vertical">
        <TextBlock Text="Login" FontSize="36" Margin="4" TextAlignment="Center"/>
        <TextBlock x:Name="ErrorMessage" Text="" FontSize="20" Margin="4" Foreground="Red" TextAlignment="Center"/>
        <TextBlock Text="Enter your username below" Margin="0,0,0,20"
                   TextWrapping="Wrap" Width="300"
                   TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>
        <TextBox x:Name="UsernameTextBox" Margin="4" Width="250"/>
        <Button x:Name="PassportSignInButton" Content="Login" Background="DodgerBlue" Foreground="White"
            Click="PassportSignInButton_Click" Width="80" HorizontalAlignment="Center" Margin="0,20"/>
        <TextBlock Text="Don't have an account?"
                    TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>
        <TextBlock x:Name="RegisterButtonTextBlock" Text="Register now"
                   PointerPressed="RegisterButtonTextBlock_OnPointerPressed"
                   Foreground="DodgerBlue"
                   TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>
        <Border x:Name="PassportStatus" Background="#22B14C"
                   Margin="0,20" Height="100" >
          <TextBlock x:Name="PassportStatusText" Text="Microsoft Passport is ready to use!"
                 Margin="4" TextAlignment="Center" VerticalAlignment="Center" FontSize="20"/>
        </Border>
        <TextBlock x:Name="LoginExplaination" FontSize="24" TextAlignment="Center" TextWrapping="Wrap" 
            Text="Please Note: To demonstrate a login, validation will only occur using the default username 'sampleUsername'"/>
      </StackPanel>
    </Grid>
    ```

-   您需要把幾個方法加入程式碼後置，才能建置解決方案。 請按下 F7，或使用方案總管來前往 Login.xaml.cs， 然後加入下列兩個事件方法來處理 Login 及 Register 事件。 這些方法會暫時將 ErrorMessage.Text 設定為空字串。

    ```cs
    namespace PassportLogin.Views
    {
        public sealed partial class Login : Page
        {
            public Login()
            {
                this.InitializeComponent();
            }
     
            private void PassportSignInButton_Click(object sender, RoutedEventArgs e)
            {
                ErrorMessage.Text = "";
            }
            private void RegisterButtonTextBlock_OnPointerPressed(object sender, PointerRoutedEventArgs e)
            {
                ErrorMessage.Text = "";
            }
        }
    }
    ```

-   為了要轉譯 Login 頁面，您必須編輯 MainPage 程式碼，以便在 MainPage 載入時瀏覽至 Login 頁面。 請開啟 MainPage.xaml.cs 檔案， 方法是在方案總管中按兩下 MainPage.xaml.cs。 如果您找不到這個檔案，請按一下 MainPage.xaml 旁邊的小箭號來顯示程式碼後置。 請建立會瀏覽至登入頁面的 Loaded 事件處理常式方法， 您將需要加入對 Views 命名空間的參考。

    ```cs
    using PassportLogin.Views;
     
    namespace PassportLogin
    {
        public sealed partial class MainPage : Page
        {
            public MainPage()
            {
                this.InitializeComponent();
                Loaded += MainPage_Loaded;
            }
     
            private void MainPage_Loaded(object sender, RoutedEventArgs e)
            {
                Frame.Navigate(typeof(Login));
            }
        }
    }
    ```

-   您必須在 Login 頁面中處理 OnNavigatedTo 事件，以便驗證這台電腦上是否有 Windows Hello。 請在 Login.xaml.cs 中實作下列程式碼。 您會注意到 MicrosoftPassportHelper 物件顯示錯誤， 這是因為我們還沒有實作該物件。

    ```cs
    public sealed partial class Login : Page
    {
        public Login()
        {
            this.InitializeComponent();
        }
     
        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            // Check Microsoft Passport is setup and available on this machine
            if (await MicrosoftPassportHelper.MicrosoftPassportAvailableCheckAsync())
            {
            }
            else
            {
                // Microsoft Passport is not setup so inform the user
                PassportStatus.Background = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 50, 170, 207));
                PassportStatusText.Text = "Microsoft Passport is not setup!\n" + 
                    "Please go to Windows Settings and set up a PIN to use it.";
                PassportSignInButton.IsEnabled = false;
            }
        }
    }
    ```

-   如要建立 MicrosoftPassportHelper 類別，請用滑鼠右鍵按一下解決方案 \[PassportLogin (通用 Windows)\]，然後按一下 \[加入\] &gt; \[新增資料夾\]， 將這個資料夾命名為 Utils。

    ![passport 建立協助程式類別](images/passport-login-5.png)

-   用滑鼠右鍵按一下 \[Utils\] 資料夾，並按一下 \[加入\] &gt; \[類別\]。 然後把這個類別命名為「MicrosoftPassportHelper.cs」。
-   請將 MicrosoftPassportHelper 的類別定義變更為 Public Static，然後加入下列方法，以便讓使用者知道是否已準備好使用 Windows Hello。 您將需要新增必要的命名空間。

    ```cs
    using System;
    using System.Diagnostics;
    using System.Threading.Tasks;
    using Windows.Security.Credentials;
     
    namespace PassportLogin.Utils
    {
        public static class MicrosoftPassportHelper
        {
            /// <summary>
            /// Checks to see if Passport is ready to be used.
            /// 
            /// Passport has dependencies on:
            ///     1. Having a connected Microsoft Account
            ///     2. Having a Windows PIN set up for that _account on the local machine
            /// </summary>
            public static async Task<bool> MicrosoftPassportAvailableCheckAsync()
            {
                bool keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
                if (keyCredentialAvailable == false)
                {
                    // Key credential is not enabled yet as user 
                    // needs to connect to a Microsoft Account and select a PIN in the connecting flow.
                    Debug.WriteLine("Microsoft Passport is not setup!\nPlease go to Windows Settings and set up a PIN to use it.");
                    return false;
                }
     
                return true;
            }
        }
    }
    ```

-   請在 Login.xaml.cs 中，加入對 Utils 命名空間的參考。 這將會解決 OnNavigatedTo 方法中的錯誤。

    ```cs
    using PassportLogin.Utils;
    ```

-   請建置並執行應用程式 (F5)。 系統將讓您瀏覽至登入頁面，而該頁面的 Windows Hello 橫幅會指出您的 Hello 是否已準備好使用。 您應該會看到綠色或藍色的橫幅，指出您電腦上的 Windows Hello 狀態。

    ![Windows Hello 登入畫面就緒](images/passport-login-6.png)

    ![Windows Hello 登入畫面未設定](images/passport-login-7.png)

-   接下來，您必須建置登入用的邏輯。 請建立名為「Models」的新資料夾。
-   請在 \[Models\] 資料夾中建立名為「Account.cs」的新類別。 這個類別將做為您的帳戶模型。 由於這是範例，這個帳戶模型將只會包含一個使用者名稱。 請將類別定義變更為 Public，然後加入 Username 屬性。
    
    ```cs
    namespace PassportLogin.Models
    {
        public class Account
        {
            public string Username { get; set; }
        }
    }
    ```

-   您將需要處理帳戶的方法。 這個實習實驗室沒有伺服器或資料庫，因此您必須讓系統在本機上儲存及載入使用者清單。 請用滑鼠右鍵按一下 \[Utils\] 資料夾，然後加入名為「AccountHelper.cs」的新類別。 並將類別定義變更為 Public Static。 AccountHelper 是靜態類別，它將包含所有儲存及載入本機帳戶清單的必要方法。 您可以利用 XmlSerializer 來儲存及載入清單， 但您也必須記住您儲存的檔案名稱及儲存位置。 您還需要提供對其他命名空間的參考。
    
    ```cs
    using System.IO;
    using System.Xml.Serialization;
    using Windows.Storage;
    using PassportLogin.Models;

    namespace PassportLogin.Utils
    {
        public static class AccountHelper
        {
            // In the real world this would not be needed as there would be a server implemented that would host a user account database.
            // For this tutorial we will just be storing accounts locally.
            private const string USER_ACCOUNT_LIST_FILE_NAME = "accountlist.txt";
            private static string _accountListPath = Path.Combine(ApplicationData.Current.LocalFolder.Path, USER_ACCOUNT_LIST_FILE_NAME);
            public static List<Account> AccountList = new List<Account>();
     
            /// <summary>
            /// Create and save a useraccount list file. (Updating the old one)
            /// </summary>
            private static async void SaveAccountListAsync()
            {
                string accountsXml = SerializeAccountListToXml();
     
                if (File.Exists(_accountListPath))
                {
                    StorageFile accountsFile = await StorageFile.GetFileFromPathAsync(_accountListPath);
                    await FileIO.WriteTextAsync(accountsFile, accountsXml);
                }
                else
                {
                    StorageFile accountsFile = await ApplicationData.Current.LocalFolder.CreateFileAsync(USER_ACCOUNT_LIST_FILE_NAME);
                    await FileIO.WriteTextAsync(accountsFile, accountsXml);
                }
            }
     
            /// <summary>
            /// Gets the useraccount list file and deserializes it from XML to a list of useraccount objects.
            /// </summary>
            /// <returns>List of useraccount objects</returns>
            public static async Task<List<Account>> LoadAccountListAsync()
            {
                if (File.Exists(_accountListPath))
                {
                    StorageFile accountsFile = await StorageFile.GetFileFromPathAsync(_accountListPath);
     
                    string accountsXml = await FileIO.ReadTextAsync(accountsFile);
                    DeserializeXmlToAccountList(accountsXml);
                }
     
                return AccountList;
            }
     
            /// <summary>
            /// Uses the local list of accounts and returns an XML formatted string representing the list
            /// </summary>
            /// <returns>XML formatted list of accounts</returns>
            public static string SerializeAccountListToXml()
            {
                XmlSerializer xmlizer = new XmlSerializer(typeof(List<Account>));
                StringWriter writer = new StringWriter();
                xmlizer.Serialize(writer, AccountList);
     
                return writer.ToString();
            }
     
            /// <summary>
            /// Takes an XML formatted string representing a list of accounts and returns a list object of accounts
            /// </summary>
            /// <param name="listAsXml">XML formatted list of accounts</param>
            /// <returns>List object of accounts</returns>
            public static List<Account> DeserializeXmlToAccountList(string listAsXml)
            {
                XmlSerializer xmlizer = new XmlSerializer(typeof(List<Account>));
                TextReader textreader = new StreamReader(new MemoryStream(Encoding.UTF8.GetBytes(listAsXml)));
     
                return AccountList = (xmlizer.Deserialize(textreader)) as List<Account>;
            }
        }
    }
    ```

-   接下來，您需要實作能將帳戶加入本機帳戶清單，以及從該清單移除帳戶的方法。 這些動作分別都會儲存清單。 您在這個實習實驗室中需要的最後一個方法，就是驗證方法。 由於沒有驗證伺服器，也沒有使用者資料庫，因此會針對硬式編碼的單一使用者來進行驗證。 您應該要把這些方法加入 AccountHelper 類別。
    
    ```cs
    public static Account AddAccount(string username)
            {
                // Create a new account with the username
                Account account = new Account() { Username = username };
                // Add it to the local list of accounts
                AccountList.Add(account);
                // SaveAccountList and return the account
                SaveAccountListAsync();
                return account;
            }
     
            public static void RemoveAccount(Account account)
            {
                // Remove the account from the accounts list
                AccountList.Remove(account);
                // Re save the updated list
                SaveAccountListAsync();
            }
     
            public static bool ValidateAccountCredentials(string username)
            {
                // In the real world, this method would call the server to authenticate that the account exists and is valid.
                // For this tutorial however we will just have a existing sample user that is just "sampleUsername"
                // If the username is null or does not match "sampleUsername" it will fail validation. In which case the user should register a new passport user
     
                if (string.IsNullOrEmpty(username))
                {
                    return false;
                }
     
                if (!string.Equals(username, "sampleUsername"))
                {
                    return false;
                }
     
                return true;
            }
    ```

-   接下來，您必須處理使用者的登入要求。 請在 Login.xaml.cs 中，建立新的私用變數來保存目前登入的帳戶。 然後，加入名為 SignInPassport 的新方法。 它將會使用 AccountHelper.ValidateAccountCredentials 方法來驗證帳戶認證。 如果輸入的使用者名稱與您在上一個步驟中設定的硬式編碼字串值相同，這個方法就會傳回布林值。 而這個範例的硬式編碼值是「sampleUsername」。

    ```cs
    using PassportLogin.Models;
    using PassportLogin.Utils;
    using System.Diagnostics;
     
    namespace PassportLogin.Views
    {
        public sealed partial class Login : Page
        {
            private Account _account;
     
            public Login()
            {
                this.InitializeComponent();
            }
     
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // Check Microsoft Passport is setup and available on this machine
                if (await MicrosoftPassportHelper.MicrosoftPassportAvailableCheckAsync())
                {
                }
                else
                {
                    // Microsoft Passport is not setup so inform the user
                    PassportStatus.Background = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 50, 170, 207));
                    PassportStatusText.Text = "Microsoft Passport is not setup!\nPlease go to Windows Settings and set up a PIN to use it.";
                    PassportSignInButton.IsEnabled = false;
                }
            }
     
            private void PassportSignInButton_Click(object sender, RoutedEventArgs e)
            {
                ErrorMessage.Text = "";
                SignInPassport();
            }
     
            private void RegisterButtonTextBlock_OnPointerPressed(object sender, PointerRoutedEventArgs e)
            {
                ErrorMessage.Text = "";
            }
     
            private async void SignInPassport()
            {
                if (AccountHelper.ValidateAccountCredentials(UsernameTextBox.Text))
                {
                    // Create and add a new local account
                    _account = AccountHelper.AddAccount(UsernameTextBox.Text);
                    Debug.WriteLine("Successfully signed in with traditional credentials and created local account instance!");
     
                    //if (await MicrosoftPassportHelper.CreatePassportKeyAsync(UsernameTextBox.Text))
                    //{
                    //    Debug.WriteLine("Successfully signed in with Microsoft Passport!");
                    //}
                }
                else
                {
                    ErrorMessage.Text = "Invalid Credentials";
                }
            }
        }
    }
    ```

-   您可能已經注意到參考 MicrosoftPassportHelper 中之方法的已加上註解標記的程式碼。 請在 MicrosoftPassportHelper.cs 中，加入名為 CreatePassportKeyAsync 的新方法。 這個方法在 [**KeyCredentialManager**](https://msdn.microsoft.com/library/windows/apps/dn973043) 中使用 Windows Hello API。 呼叫 [**RequestCreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn973048) 將會建立 *accountId* 及本機電腦專屬的 Passport 金鑰。 如果您想在真實世界的案例中實作 Switch 陳述式，請留意 Switch 陳述式中的註解。

    ```cs
    /// <summary>
    /// Creates a Passport key on the machine using the _account id passed.
    /// </summary>
    /// <param name="accountId">The _account id associated with the _account that we are enrolling into Passport</param>
    /// <returns>Boolean representing if creating the Passport key succeeded</returns>
    public static async Task<bool> CreatePassportKeyAsync(string accountId)
    {
        KeyCredentialRetrievalResult keyCreationResult = await KeyCredentialManager.RequestCreateAsync(accountId, KeyCredentialCreationOption.ReplaceExisting);

        switch (keyCreationResult.Status)
        {
            case KeyCredentialStatus.Success:
                Debug.WriteLine("Successfully made key");

                // In the real world authentication would take place on a server.
                // So every time a user migrates or creates a new Microsoft Passport account Passport details should be pushed to the server.
                // The details that would be pushed to the server include:
                // The public key, keyAttesation if available, 
                // certificate chain for attestation endorsement key if available,  
                // status code of key attestation result: keyAttestationIncluded or 
                // keyAttestationCanBeRetrievedLater and keyAttestationRetryType
                // As this sample has no concept of a server it will be skipped for now
                // for information on how to do this refer to the second Passport sample

                //For this sample just return true
                return true;
            case KeyCredentialStatus.UserCanceled:
                Debug.WriteLine("User cancelled sign-in process.");
                break;
            case KeyCredentialStatus.NotFound:
                // User needs to setup Microsoft Passport
                Debug.WriteLine("Microsoft Passport is not setup!\nPlease go to Windows Settings and set up a PIN to use it.");
                break;
            default:
                break;
        }

        return false;
    }
    ```

-   現在您已經建立 CreatePassportKeyAsync 方法，請返回 Login.xaml.cs 檔案，然後取消 SignInPassport 方法中程式碼的註解。

    ```cs
    private async void SignInPassport()
    {
        if (AccountHelper.ValidateAccountCredentials(UsernameTextBox.Text))
        {
            //Create and add a new local account
            _account = AccountHelper.AddAccount(UsernameTextBox.Text);
            Debug.WriteLine("Successfully signed in with traditional credentials and created local account instance!");

            if (await MicrosoftPassportHelper.CreatePassportKeyAsync(UsernameTextBox.Text))
            {
                Debug.WriteLine("Successfully signed in with Microsoft Passport!");
            }
        }
        else
        {
            ErrorMessage.Text = "Invalid Credentials";
        }
    }
    ```

-   請建置並執行應用程式。 系統將帶您前往 Login 頁面。 請輸入「sampleUsername」，然後按一下 \[Login\] \(登入\)。 系統會以 Windows Hello 提示要求您輸入 PIN 碼。 在您正確輸入自己的 PIN 碼之後，CreatePassportKeyAsync 方法就能建立 Windows Hello 金鑰。 請監視輸出視窗，看看是否顯示已成功登入的訊息。

    ![Windows Hello 登入 PIN 提示](images/passport-login-8.png)

## <a name="exercise-2-welcome-and-user-selection-pages"></a>練習 2：歡迎使用頁面和使用者選取頁面


您將在這個練習中，繼續先前的練習。 當使用者成功登入之後，他們應該會看見歡迎頁面，而該頁面會有能讓使用者登出或刪除自己帳戶的選項。 由於 Windows Hello 會為每台電腦建立金鑰，您可以建立使用者選取畫面，來顯示所有曾經登入該電腦的使用者。 然後使用者就能選取其中一個帳戶，不用重新輸入密碼就能直接前往歡迎畫面，原因是該使用者已通過驗證來存取該電腦。

-   在 \[Views\] 資料夾中，加入名為「Welcome.xaml」的新空白頁。 請加入下列 XAML 來完成使用者介面。 使用者介面將會顯示標題、已登入的使用者名稱，以及兩個按鈕。 其中一個按鈕會讓使用者回到使用者清單 (您會在稍後建立)，而另一個按鈕將處理忘記這位使用者的作業。

    ```xml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <StackPanel Orientation="Vertical">
        <TextBlock x:Name="Title" Text="Welcome" FontSize="40" TextAlignment="Center"/>
        <TextBlock x:Name="UserNameText" FontSize="28" TextAlignment="Center" Foreground="Black"/>

        <Button x:Name="BackToUserListButton" Content="Back to User List" Click="Button_Restart_Click"
                HorizontalAlignment="Center" Margin="0,20" Foreground="White" Background="DodgerBlue"/>

        <Button x:Name="ForgetButton" Content="Forget Me" Click="Button_Forget_User_Click"
                Foreground="White"
                Background="Gray"
                HorizontalAlignment="Center"/>
      </StackPanel>
    </Grid>
    ```

-   請在 Welcome.xaml.cs 程式碼後置檔案中，加入新的私用變數來保存已登入的帳戶。 您將需要實作會覆寫 OnNavigateTo 事件的方法，它將會儲存已傳遞給歡迎頁面的帳戶。 您也必須針對在 XAML 中定義的兩個按鈕實作 Click 事件。 您將需要對 \[Models\] 及 \[Utils\] 資料夾的參考。

    ```cs
    using PassportLogin.Models;
    using PassportLogin.Utils;
    using System.Diagnostics;
     
    namespace PassportLogin.Views
    {
        public sealed partial class Welcome : Page
        {
            private Account _activeAccount;
     
            public Welcome()
            {
                InitializeComponent();
            }
     
            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
                _activeAccount = (Account)e.Parameter;
                if (_activeAccount != null)
                {
                    UserNameText.Text = _activeAccount.Username;
                }
            }
     
            private void Button_Restart_Click(object sender, RoutedEventArgs e)
            {
            }
     
            private void Button_Forget_User_Click(object sender, RoutedEventArgs e)
            {
                // Remove it from Microsoft Passport
                // MicrosoftPassportHelper.RemovePassportAccountAsync(_activeAccount);
     
                // Remove it from the local accounts list and resave the updated list
                AccountHelper.RemoveAccount(_activeAccount);
     
                Debug.WriteLine("User " + _activeAccount.Username + " deleted.");
            }
        }
    }
    ```

-   您可能已經注意到，在 Forget User Click 事件中有一行已加上註解的程式碼。 帳戶已經從本機清單中移除，但目前沒有辦法從 Windows Hello 中移除帳戶。 您需要在 MicrosoftPassportHelper.cs 中實作新方法，處理 Windows Hello 使用者的移除作業。 這個方法將使用其他 Windows Hello API 開啟並刪除帳戶。 在真實世界中，當您刪除帳戶時，伺服器或資料庫會收到通知，讓使用者資料庫能保持其正確性。 您將需要對 \[Models\] 資料夾的參考。

    ```cs
    using PassportLogin.Models;

    /// <summary>
    /// Function to be called when user requests deleting their account.
    /// Checks the KeyCredentialManager to see if there is a Passport for the current user
    /// Then deletes the local key associated with the Passport.
    /// </summary>
    public static async void RemovePassportAccountAsync(Account account)
    {
        // Open the account with Passport
        KeyCredentialRetrievalResult keyOpenResult = await KeyCredentialManager.OpenAsync(account.Username);

        if (keyOpenResult.Status == KeyCredentialStatus.Success)
        {
            // In the real world you would send key information to server to unregister
            //e.g. RemovePassportAccountOnServer(account);
        }

        // Then delete the account from the machines list of Passport Accounts
        await KeyCredentialManager.DeleteAsync(account.Username);
    }
    ```

-   請回到 Welcome.xaml.cs，將呼叫 RemovePassportAccountAsync 的那一行程式碼取消註解。

    ```cs
    private void Button_Forget_User_Click(object sender, RoutedEventArgs e)
    {
        // Remove it from Microsoft Passport
        MicrosoftPassportHelper.RemovePassportAccountAsync(_activeAccount);
     
        // Remove it from the local accounts list and resave the updated list
        AccountHelper.RemoveAccount(_activeAccount);
     
        Debug.WriteLine("User " + _activeAccount.Username + " deleted.");
    }
    ```

-   在 (Login.xaml.cs 的) SignInPassport 方法中，當CreatePassportKeyAsync 成功時，它應該會瀏覽至 Welcome 畫面並傳遞 Account。

    ```cs
    private async void SignInPassport()
    {
        if (AccountHelper.ValidateAccountCredentials(UsernameTextBox.Text))
        {
            // Create and add a new local account
            _account = AccountHelper.AddAccount(UsernameTextBox.Text);
            Debug.WriteLine("Successfully signed in with traditional credentials and created local account instance!");

            if (await MicrosoftPassportHelper.CreatePassportKeyAsync(UsernameTextBox.Text))
            {
                Debug.WriteLine("Successfully signed in with Microsoft Passport!");
                Frame.Navigate(typeof(Welcome), _account);
            }
        }
        else
        {
            ErrorMessage.Text = "Invalid Credentials";
        }
    }
    ```

-   請建置並執行應用程式。 然後使用「sampleUsername」登入，並按一下 \[Login\] \(登入\)。 請輸入您的 PIN 碼；如果登入成功，您應該會看到歡迎畫面。 請嘗試按一下能忘記使用者的按鈕，然後監視輸出視窗，看看使用者是否遭到刪除。 請注意，當使用者遭到刪除時，您仍舊會在歡迎頁面上。 您必須建立應用程式可以瀏覽的使用者選取頁面。

    ![Windows Hello 歡迎畫面](images/passport-login-9.png)

-   在 [Views] 資料夾中，建立名為 "UserSelection.xaml" 的新空白頁，並加入下列 XAML 來定義使用者介面。 此頁面將包含會顯示本機帳戶清單中的所有使用者的 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878)，以及會瀏覽至登入頁面來讓使用者加入另一個帳戶的按鈕。

    ```xml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <StackPanel Orientation="Vertical">
        <TextBlock x:Name="Title" Text="Select a User" FontSize="36" Margin="4" TextAlignment="Center" HorizontalAlignment="Center"/>

        <ListView x:Name="UserListView" Margin="4" MaxHeight="200" MinWidth="250" Width="250" HorizontalAlignment="Center">
          <ListView.ItemTemplate>
            <DataTemplate>
              <Grid Background="DodgerBlue" Height="50" Width="250" HorizontalAlignment="Stretch" VerticalAlignment="Stretch">
                <TextBlock Text="{Binding Username}" HorizontalAlignment="Center" TextAlignment="Center" VerticalAlignment="Center" Foreground="White"/>
              </Grid>
            </DataTemplate>
          </ListView.ItemTemplate>
        </ListView>

        <Button x:Name="AddUserButton" Content="+" FontSize="36" Width="60" Click="AddUserButton_Click" HorizontalAlignment="Center"/>
      </StackPanel>
    </Grid>
    ```

-   請在 UserSelection.xaml.cs 中實作 Loaded 方法，它將會在本機清單中沒有帳戶時瀏覽至登入頁面。 同時，請實作 ListView 的 SelectionChanged 事件，以及 Button 的 Click 事件。

    ```cs
    using System.Diagnostics;
    using PassportLogin.Models;
    using PassportLogin.Utils;

    namespace PassportLogin.Views
    {
        public sealed partial class UserSelection : Page
        {
            public UserSelection()
            {
                InitializeComponent();
                Loaded += UserSelection_Loaded;
            }

            private void UserSelection_Loaded(object sender, RoutedEventArgs e)
            {
                if (AccountHelper.AccountList.Count == 0)
                {
                    //If there are no accounts navigate to the LoginPage
                    Frame.Navigate(typeof(Login));
                }


                UserListView.ItemsSource = AccountHelper.AccountList;
                UserListView.SelectionChanged += UserSelectionChanged;
            }

            /// <summary>
            /// Function called when an account is selected in the list of accounts
            /// Navigates to the Login page and passes the chosen account
            /// </summary>
            private void UserSelectionChanged(object sender, RoutedEventArgs e)
            {
                if (((ListView)sender).SelectedValue != null)
                {
                    Account account = (Account)((ListView)sender).SelectedValue;
                    if (account != null)
                    {
                        Debug.WriteLine("Account " + account.Username + " selected!");
                    }
                    Frame.Navigate(typeof(Login), account);
                }
            }

            /// <summary>
            /// Function called when the "+" button is clicked to add a new user.
            /// Navigates to the Login page with nothing filled out
            /// </summary>
            private void AddUserButton_Click(object sender, RoutedEventArgs e)
            {
                Frame.Navigate(typeof(Login));
            }
        }
    }
    ```

<!-- -->

-   應用程式中有幾個您會想要瀏覽至 UserSelection 頁面的地方。 在 MainPage.xaml.cs 中，您應該要瀏覽至 UserSelection 頁面，而不是 Login 頁面。 當您在 MainPage 中的 Loaded 事件時，將會需要載入帳戶清單，以便讓 UserSelection 頁面能查看清單中是否有任何帳戶。 這將需要將 Loaded 方法變更為 Async，同時還要加入對 Utils 資料夾的參考。

    ```cs
    using PassportLogin.Utils;

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        // Load the local Accounts List before navigating to the UserSelection page
        await AccountHelper.LoadAccountListAsync();
        Frame.Navigate(typeof(UserSelection));
    }
    ```

-   接下來，您會想要從 \[歡迎\] 畫面瀏覽至 UserSelection 頁面。 在這兩個 Click 事件中，您應該會回到 UserSelection 頁面。

    ```cs
    private void Button_Restart_Click(object sender, RoutedEventArgs e)
    {
        Frame.Navigate(typeof(UserSelection));
    }

    private void Button_Forget_User_Click(object sender, RoutedEventArgs e)
    {
        // Remove it from Microsoft Passport
        MicrosoftPassportHelper.RemovePassportAccountAsync(_activeAccount);

        // Remove it from the local accounts list and resave the updated list
        AccountHelper.RemoveAccount(_activeAccount);

        Debug.WriteLine("User " + _activeAccount.Username + " deleted.");

        // Navigate back to UserSelection page.
        Frame.Navigate(typeof(UserSelection));
    }
    ```

-   在 \[登入\] 頁面中，您需要程式碼來登入您在 UserSelection 頁面的清單中所選取的帳戶。 請在 OnNavigatedTo 事件中，儲存已傳遞至導覽的帳戶。 首先，請加入新的私用變數來辨識該帳戶是否為現有的帳戶。 然後，請處理 OnNavigatedTo 事件。

    ```cs
    namespace PassportLogin.Views
    {
        public sealed partial class Login : Page
        {
            private Account _account;
            private bool _isExistingAccount;

            public Login()
            {
                InitializeComponent();
            }

            /// <summary>
            /// Function called when this frame is navigated to.
            /// Checks to see if Microsoft Passport is available and if an account was passed in.
            /// If an account was passed in set the "_isExistingAccount" flag to true and set the _account
            /// </summary>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // Check Microsoft Passport is setup and available on this machine
                if (await MicrosoftPassportHelper.MicrosoftPassportAvailableCheckAsync())
                {
                    if (e.Parameter != null)
                    {
                        _isExistingAccount = true;
                        // Set the account to the existing account being passed in
                        _account = (Account)e.Parameter;
                        UsernameTextBox.Text = _account.Username;
                        SignInPassport();
                    }
                }
                else
                {
                    // Microsoft Passport is not setup so inform the user
                    PassportStatus.Background = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 50, 170, 207));
                    PassportStatusText.Text = "Microsoft Passport is not setup!\n" + 
                        "Please go to Windows Settings and set up a PIN to use it.";
                    PassportSignInButton.IsEnabled = false;
                }
            }
        }
    }
    ```

-   SignInPassport 方法將需要更新，才能登入已選取的帳戶。 MicrosoftPassportHelper 將需要另一個方法來搭配 Passport 開啟帳戶，原因是該帳戶已經有專屬的 Passport 金鑰。 請在 MicrosoftPassportHelper.cs 中實作新的方法，以便利用 Passport 來登入現有的使用者。 如需程式碼各個部分的資訊，請參閱程式碼註解。

    ```cs
    /// <summary>
    /// Attempts to sign a message using the Passport key on the system for the accountId passed.
    /// </summary>
    /// <returns>Boolean representing if creating the Passport authentication message succeeded</returns>
    public static async Task<bool> GetPassportAuthenticationMessageAsync(Account account)
    {
        KeyCredentialRetrievalResult openKeyResult = await KeyCredentialManager.OpenAsync(account.Username);
        // Calling OpenAsync will allow the user access to what is available in the app and will not require user credentials again.
        // If you wanted to force the user to sign in again you can use the following:
        // var consentResult = await Windows.Security.Credentials.UI.UserConsentVerifier.RequestVerificationAsync(account.Username);
        // This will ask for the either the password of the currently signed in Microsoft Account or the PIN used for Microsoft Passport.

        if (openKeyResult.Status == KeyCredentialStatus.Success)
        {
            // If OpenAsync has succeeded, the next thing to think about is whether the client application requires access to backend services.
            // If it does here you would Request a challenge from the Server. The client would sign this challenge and the server
            // would check the signed challenge. If it is correct it would allow the user access to the backend.
            // You would likely make a new method called RequestSignAsync to handle all this
            // e.g. RequestSignAsync(openKeyResult);
            // Refer to the second Microsoft Passport sample for information on how to do this.

            // For this sample there is not concept of a server implemented so just return true.
            return true;
        }
        else if (openKeyResult.Status == KeyCredentialStatus.NotFound)
        {
            // If the _account is not found at this stage. It could be one of two errors. 
            // 1. Microsoft Passport has been disabled
            // 2. Microsoft Passport has been disabled and re-enabled cause the Microsoft Passport Key to change.
            // Calling CreatePassportKey and passing through the account will attempt to replace the existing Microsoft Passport Key for that account.
            // If the error really is that Microsoft Passport is disabled then the CreatePassportKey method will output that error.
            if (await CreatePassportKeyAsync(account.Username))
            {
                // If the Passport Key was again successfully created, Microsoft Passport has just been reset.
                // Now that the Passport Key has been reset for the _account retry sign in.
                return await GetPassportAuthenticationMessageAsync(account);
            }
        }

        // Can't use Passport right now, try again later
        return false;
    }
    ```

-   請更新 Login.xaml.cs 中的 SignInPassport 方法，以處理現有的帳戶。 這將會使用 MicrosoftPassportHelper.cs 中的新方法。 如果成功，帳戶將登入，而使用者會看到歡迎畫面。

    ```cs
    private async void SignInPassport()
    {
        if (_isExistingAccount)
        {
            if (await MicrosoftPassportHelper.GetPassportAuthenticationMessageAsync(_account))
            {
                Frame.Navigate(typeof(Welcome), _account);
            }
        }
        else if (AccountHelper.ValidateAccountCredentials(UsernameTextBox.Text))
        {
            //Create and add a new local account
            _account = AccountHelper.AddAccount(UsernameTextBox.Text);
            Debug.WriteLine("Successfully signed in with traditional credentials and created local account instance!");

            if (await MicrosoftPassportHelper.CreatePassportKeyAsync(UsernameTextBox.Text))
            {
                Debug.WriteLine("Successfully signed in with Microsoft Passport!");
                Frame.Navigate(typeof(Welcome), _account);
            }
        }
        else
        {
            ErrorMessage.Text = "Invalid Credentials";
        }
    }
    ```

-   請建置並執行應用程式。 然後使用 "sampleUsername" 登入。 請輸入您的 PIN；如果登入成功，您將會看到歡迎畫面。 請按一下 Back to User List \(返回使用者清單\)。 現在，您應該會看到清單中有一位使用者。 如果您按一下該使用者，Passport 就會讓您重新登入，但不必重新輸入任何密碼等資料。

    ![Windows Hello 選取使用者清單](images/passport-login-10.png)

## <a name="exercise-3-registering-a-new-windows-hello-user"></a>練習 3︰ 登錄新 Windows Hello 使用者


您將在這個練習中建立新的頁面，以便利用 Windows Hello 來建立新帳戶。 該頁面的運作方式與 \[登入\] 頁面類似。 \[登入\] 頁面會針對正移轉去使用 Windows Hello 的現有使用者實作。 PassportRegister 頁面將為新的使用者建立 Windows Hello 註冊。

-   請在 views 資料夾中，建立名為 "PassportRegister.xaml" 的新空白頁。 然後在 XAML 中新增下列程式碼來設定使用者介面。 這裡的介面與 \[登入\] 頁面很類似。

    ```xml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <StackPanel Orientation="Vertical">
        <TextBlock x:Name="Title" Text="Register New Passport User" FontSize="24" Margin="4" TextAlignment="Center"/>

        <TextBlock x:Name="ErrorMessage" Text="" FontSize="20" Margin="4" Foreground="Red" TextAlignment="Center"/>

        <TextBlock Text="Enter your new username below" Margin="0,0,0,20"
                   TextWrapping="Wrap" Width="300"
                   TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>

        <TextBox x:Name="UsernameTextBox" Margin="4" Width="250"/>

        <Button x:Name="PassportRegisterButton" Content="Register" Background="DodgerBlue" Foreground="White"
            Click="RegisterButton_Click_Async" Width="80" HorizontalAlignment="Center" Margin="0,20"/>

        <Border x:Name="PassportStatus" Background="#22B14C"
                   Margin="4" Height="100">
          <TextBlock x:Name="PassportStatusText" Text="Microsoft Passport is ready to use!" FontSize="20"
                 Margin="4" TextAlignment="Center" VerticalAlignment="Center"/>
        </Border>
      </StackPanel>
    </Grid>
    ```

-   請在 PassportRegister.xaml.cs 程式碼後置檔案中，實作私用 Account 變數，以及註冊按鈕用的 Click 事件。 這將會加入新的本機帳戶，並建立 Passport 金鑰。

    ```cs
    using PassportLogin.Models;
    using PassportLogin.Utils;

    namespace PassportLogin.Views
    {
        public sealed partial class PassportRegister : Page
        {
            private Account _account;

            public PassportRegister()
            {
                InitializeComponent();
            }

            private async void RegisterButton_Click_Async(object sender, RoutedEventArgs e)
            {
                ErrorMessage.Text = "";

                //In the real world you would normally validate the entered credentials and information before 
                //allowing a user to register a new account. 
                //For this sample though we will skip that step and just register an account if username is not null.

                if (!string.IsNullOrEmpty(UsernameTextBox.Text))
                {
                    //Register a new account
                    _account = AccountHelper.AddAccount(UsernameTextBox.Text);
                    //Register new account with Microsoft Passport
                    await MicrosoftPassportHelper.CreatePassportKeyAsync(_account.Username);
                    //Navigate to the Welcome Screen. 
                    Frame.Navigate(typeof(Welcome), _account);
                }
                else
                {
                    ErrorMessage.Text = "Please enter a username";
                }
            }
        }
    }
    ```

-   當註冊按鈕遭到點擊時，您必須從 \[登入\] 頁面瀏覽至這個頁面。

    ```cs
    private void RegisterButtonTextBlock_OnPointerPressed(object sender, PointerRoutedEventArgs e)
    {
        ErrorMessage.Text = "";
        Frame.Navigate(typeof(PassportRegister));
    }
    ```

-   請建置並執行應用程式。 然後嘗試為新的使用者註冊。 接著返回使用者清單，並驗證您可以選取該使用者來登入。

    ![Windows Hello 登錄新的使用者](images/passport-login-11.png)

在這個實驗室中，您已經學會必要的基本技巧，利用新的 Windows Hello API 來驗證現有的使用者，以及為新使用者建立帳戶。 您可以利用這個新知識，來開始消除使用者必須記住您應用程式所用密碼的需求，且依然有信心自己的應用程式會受到使用者驗證的保護。 Windows 10 使用 Windows Hello 的新驗證技術支援其生物登入選項。

## <a name="related-topics"></a>相關主題

* [Windows Hello](microsoft-passport.md)
* [Windows Hello 登入服務](microsoft-passport-login-auth-service.md)
