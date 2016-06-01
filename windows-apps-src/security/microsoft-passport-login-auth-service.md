---
title: 建立 Microsoft Passport 登入服務
description: 這是一份完整逐步解說的第 2 部分，將說明如何在 Windows 10 UWP (通用 Windows 平台) 應用程式中，使用 Microsoft Passport 來取代傳統的使用者名稱及密碼驗證系統。
ms.assetid: ECC9EF3D-E0A1-4BC4-94FA-3215E6CFF0E4
author: awkoren
---

# 建立 Microsoft Passport 登入服務


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[正式發行前可能會進行大幅度修改之預先發行的產品的一些相關資訊。 Microsoft 對此處提供的資訊，不提供任何明確或隱含的瑕疵擔保。\]

這是一份完整逐步解說的第 2 部分，將說明如何在 Windows 10 UWP (通用 Windows 平台) 應用程式中，使用 Microsoft Passport 來取代傳統的使用者名稱及密碼驗證系統。 這篇文章將從第 1 部分 ([Microsoft Passport 登入應用程式](microsoft-passport-login.md)) 結束的地方延續下去，擴充應用程式的功能來示範如何將 Microsoft Passport 整合到您現有的應用程式中。

為了能順利建置這個專案，您需要有 C# 及 XAML 方面的經驗。 您也需要使用安裝在 Windows 10 電腦上的 Visual Studio 2015 (Community 版或更新版本)。

## 練習 1：伺服器端的邏輯


您將在這個練習中，從您在第一個實驗室所建置的 Passport 應用程式開始，建立本機模擬伺服器和資料庫。 這個實習實驗室的目的，是要讓您了解如何將 Microsoft Passport 整合到現有的系統中。 由於我們使用模擬伺服器及模擬資料庫，因此會去除許多不相關的設定。 而在您自己的應用程式中，您必須用實際的服務及資料庫取代模擬的物件。

-   首先，請開啟您在第一個 Passport 實習實驗室所建立的 PassportLogin 解決方案。
-   您將從實作模擬伺服器及模擬資料庫開始。 請建立名為「AuthService」的新資料夾， 方法是在方案總管中，用滑鼠右鍵按一下 [PassportLogin (通用 Windows)] 方案，然後選取 [加入] &gt; [新增資料夾]。
-   請建立 UserAccount 和 PassportDevices 類型，來做為將在模擬資料庫中儲存的資料模型。 UserAccount 類似在傳統的驗證伺服器上實作的使用者模型。 請用滑鼠右鍵按一下 [AuthService] 資料夾，然後加入名為「UserAccount.cs」的新類別。

    ![](images/passport-auth-1.png)

    ![](images/passport-auth-2.png)

-   請將類型定義變更為 Public，然後加入下列的公用屬性。 您將需要下列的參考。

    ```cs
    using System.ComponentModel.DataAnnotations;
     
    namespace PassportLogin.AuthService
    {
        public class UserAccount
        {
            [Key, Required]
            public Guid UserId { get; set; }
            [Required]
            public string Username { get; set; }
            public string Password { get; set; }
            // public List<PassportDevice> PassportDevices = new List<PassportDevice>();
        }
    }
    ```

    您可能已經注意到，PassportDevices 清單已標記為註解。 這就是您需要針對目前實作中現有使用者所做的修改。 PassportDevices 清單將包含 deviceID、透過 Microsoft Passport 產生的公開金鑰，以及 [**KeyCredentialAttestationResult**](https://msdn.microsoft.com/library/windows/apps/dn973034)。 而對於這個實習實驗室，您將必須實作 keyAttestationResult，因為只有在擁有 TPM (信賴平台模組) 晶片之裝置上的 Microsoft Passport 才會提供 keyAttestationResult。 **KeyCredentialAttestationResult** 是多重屬性的組合，必須分割才能利用資料庫來儲存及載入。

-   請在 [AuthService] 資料夾中，建立名為「PassportDevice.cs」的新類別。 這是上述的 Passport 裝置所用的模型。 請將類型定義變更為 Public，然後加入下列屬性。

    ```cs
    namespace PassportLogin.AuthService
    {
        public class PassportDevice
        {
            // These are the new variables that will need to be added to the existing UserAccount in the Database
            // The DeviceName is used to support multiple devices for the one user.
            // This way the correct public key is easier to find as a new public key is made for each device.
            // The KeyAttestationResult is only used if the User device has a TPM (Trusted Platform Module) chip, 
            // in most cases it will not. So will be left out for this hands on lab.
            public Guid DeviceId { get; set; }
            public byte[] PublicKey { get; set; }
            // public KeyCredentialAttestationResult KeyAttestationResult { get; set; }
        }
    }
    ```

-   請返回 UserAccount.cs，然後取消 Passport 裝置清單的註解。

    ```cs
    using System.Collections.Generic;
     
    namespace PassportLogin.AuthService
    {
        public class UserAccount
        {
            [Key, Required]
            public Guid UserId { get; set; }
            [Required]
            public string Username { get; set; }
            public string Password { get; set; }
            public List<PassportDevice> PassportDevices = new List<PassportDevice>();
        }
    }
    ```

-   UserAccount 及 PassportDevice 所用的模型已經建立，現在您需要在 [AuthService] 資料夾中，建立另一個新類別來做為模擬資料庫。 這就是您將用來儲存及載入使用者帳戶清單的本機模擬資料庫。 在真實世界中，這就是您的資料庫實作。 請在 [AuthService] 資料夾中，建立名為「MockStore.cs」的新類別， 並將類別定義變更為 Public。
-   由於模擬存放區將在本機儲存及載入使用者帳戶清單，您可以利用 XmlSerializer 來實作儲存及載入該清單的邏輯。 您也需要記住檔案名稱及檔案的儲存位置。 請在 MockStore.cs 中實作下列程式碼：
-   

    ```cs
    using System.IO;
    using System.Linq;
    using System.Xml.Serialization;
    using Windows.Storage;

    namespace PassportLogin.AuthService
    {
        public class MockStore
        {
            private const string USER_ACCOUNT_LIST_FILE_NAME = "userAccountsList.txt";
            // This cannot be a const because the LocalFolder is accessed at runtime
            private string _userAccountListPath = Path.Combine(
                ApplicationData.Current.LocalFolder.Path, USER_ACCOUNT_LIST_FILE_NAME);
            private List<UserAccount> _mockDatabaseUserAccountsList;
     
#region Save and Load Helpers
            /// <summary>
            /// Create and save a useraccount list file. (Replacing the old one)
            /// </summary>
            private async void SaveAccountListAsync()
            {
                string accountsXml = SerializeAccountListToXml();
     
                if (File.Exists(_userAccountListPath))
                {
                    StorageFile accountsFile = await StorageFile.GetFileFromPathAsync(_userAccountListPath);
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
            private async void LoadAccountListAsync()
            {
                if (File.Exists(_userAccountListPath))
                {
                    StorageFile accountsFile = await StorageFile.GetFileFromPathAsync(_userAccountListPath);
     
                    string accountsXml = await FileIO.ReadTextAsync(accountsFile);
                    DeserializeXmlToAccountList(accountsXml);
                }
     
                // If the UserAccountList does not contain the sampleUser Initialize the sample users
                // This is only needed as it in a Hand on Lab to demonstrate a user migrating
                // In the real world user accounts would just be in a database
                if (!_mockDatabaseUserAccountsList.Any(f => f.Username.Equals("sampleUsername")))
                {
                    //If the list is empty InitializeSampleAccounts and return the list
                    //InitializeSampleUserAccounts();
                }
            }
     
            /// <summary>
            /// Uses the local list of accounts and returns an XML formatted string representing the list
            /// </summary>
            /// <returns>XML formatted list of accounts</returns>
            private string SerializeAccountListToXml()
            {
                XmlSerializer xmlizer = new XmlSerializer(typeof(List<UserAccount>));
                StringWriter writer = new StringWriter();
                xmlizer.Serialize(writer, _mockDatabaseUserAccountsList);
                return writer.ToString();
            }
     
            /// <summary>
            /// Takes an XML formatted string representing a list of accounts and returns a list object of accounts
            /// </summary>
            /// <param name="listAsXml">XML formatted list of accounts</param>
            /// <returns>List object of accounts</returns>
            private List<UserAccount> DeserializeXmlToAccountList(string listAsXml)
            {
                XmlSerializer xmlizer = new XmlSerializer(typeof(List<UserAccount>));
                TextReader textreader = new StreamReader(new MemoryStream(Encoding.UTF8.GetBytes(listAsXml)));
                return _mockDatabaseUserAccountsList = (xmlizer.Deserialize(textreader)) as List<UserAccount>;
            }
#endregion
        }
    }
    ```

-   您可能已經注意到在 Load 方法中，有個 InitializeSampleUserAccounts 方法已加上註解。 您將必須在 MockStore.cs 中建立這個方法。 這個方法會填入使用者帳戶清單，讓登入能夠發生。 而在真實世界中，使用者資料庫會已經遭到填入。 您也將在這個步驟中建立建構函式，來初始化使用者清單並呼叫 Load。

    ```cs
    namespace PassportLogin.AuthService
    {
        public class MockStore
        {
            private const string USER_ACCOUNT_LIST_FILE_NAME = "userAccountsList.txt";
            // This cannot be a const because the LocalFolder is accessed at runtime
            private string _userAccountListPath = Path.Combine(
                ApplicationData.Current.LocalFolder.Path, USER_ACCOUNT_LIST_FILE_NAME);
            private List<UserAccount> _mockDatabaseUserAccountsList;
     
            public MockStore()
            {
                _mockDatabaseUserAccountsList = new List& lt; UserAccount & gt; ();
                LoadAccountListAsync();
            }

            private void InitializeSampleUserAccounts()
            {
                // Create a sample Traditional User Account that only has a Username and Password
                // This will be used initially to demonstrate how to migrate to use Microsoft Passport

                UserAccount sampleUserAccount = new UserAccount()
                {
                    UserId = Guid.NewGuid(),
                    Username = "sampleUsername",
                    Password = "samplePassword",
                };

                // Add the sampleUserAccount to the _mockDatabase
                _mockDatabaseUserAccountsList.Add(sampleUserAccount);
                SaveAccountListAsync();
            }
        }
    }
    ```

-   現在 InitializeSampleUserAccounts 方法已經存在，請取消 LoadAccountListAsync 方法中方法呼叫的註釋。

    ```cs
    private async void LoadAccountListAsync()
    {
        if (File.Exists(_userAccountListPath))
        {
            StorageFile accountsFile = await StorageFile.GetFileFromPathAsync(_userAccountListPath);

            string accountsXml = await FileIO.ReadTextAsync(accountsFile);
            DeserializeXmlToAccountList(accountsXml);
        }

        // If the UserAccountList does not contain the sampleUser Initialize the sample users
        // This is only needed as it in a Hand on Lab to demonstrate a user migrating
        // In the real world user accounts would just be in a database
        if (!_mockDatabaseUserAccountsList.Any(f = > f.Username.Equals("sampleUsername")))
                {
            //If the list is empty InitializeSampleAccounts and return the list
            InitializeSampleUserAccounts();
        }
    }
    ```

-   現在，模擬存放區中的使用者帳戶清單可以儲存及載入了。 應用程式的其他部分將需要有這份清單的存取權，因此我們需要有某些方法來擷取此資料。 請在 InitializeSampleUserAccounts 方法下方，加入下列的 Get 方法。 這些方法會讓您取得一個 UserID、單一使用者，以及特定 Passport 裝置的使用者清單，還會讓您取得特定裝置上使用者的公開金鑰。

    ```cs
    public Guid GetUserId(string username)
    {
        if (_mockDatabaseUserAccountsList.Any())
        {
            UserAccount account = _mockDatabaseUserAccountsList.FirstOrDefault(f => f.Username.Equals(username));
            if (account != null)
            {
                return account.UserId;
            }
        }
        return Guid.Empty;
    }

    public UserAccount GetUserAccount(Guid userId)
    {
        return _mockDatabaseUserAccountsList.FirstOrDefault(f => f.UserId.Equals(userId));
    }

    public List<UserAccount> GetUserAccountsForDevice(Guid deviceId)
    {
        List<UserAccount> usersForDevice = new List<UserAccount>();

        foreach (UserAccount account in _mockDatabaseUserAccountsList)
        {
            if (account.PassportDevices.Any(f => f.DeviceId.Equals(deviceId)))
            {
                usersForDevice.Add(account);
            }
        }

        return usersForDevice;
    }

    public byte[] GetPublicKey(Guid userId, Guid deviceId)
    {
        UserAccount account = _mockDatabaseUserAccountsList.FirstOrDefault(f => f.UserId.Equals(userId));
        if (account != null)
        {
            if (account.PassportDevices.Any())
            {
                return account.PassportDevices.FirstOrDefault(p => p.DeviceId.Equals(deviceId)).PublicKey;
            }
        }
        return null;
    }
    ```

-   下一個要實作的方法，會處理簡單的作業來加入帳戶、移除帳戶，還會移除裝置。 Microsoft Passport 是裝置專用的，因此您需要移除裝置。 Microsoft Passport 會為您登入的每個裝置建立新的公開及私密金鑰組。 這就像是您登入的每個裝置都有不同的密碼，但您並不需要記錄所有這些密碼，伺服器才需要這麼做。 請將下列方法加入 MockStore.cs

    ```cs
    public UserAccount AddAccount(string username)
    {
        UserAccount newAccount = null;
        try
        {
            newAccount = new UserAccount()
            {
                UserId = Guid.NewGuid(),
                Username = username,
            };

            _mockDatabaseUserAccountsList.Add(newAccount);
            SaveAccountListAsync();
        }
        catch (Exception)
        {
            throw;
        }
        return newAccount;
    }

    public bool RemoveAccount(Guid userId)
    {
        UserAccount userAccount = GetUserAccount(userId);
        if (userAccount != null)
        {
            _mockDatabaseUserAccountsList.Remove(userAccount);
            SaveAccountListAsync();
            return true;
        }
        return false;
    }

    public bool RemoveDevice(Guid userId, Guid deviceId)
    {
        UserAccount userAccount = GetUserAccount(userId);
        PassportDevice deviceToRemove = null;
        if (userAccount != null)
        {
            foreach (PassportDevice device in userAccount.PassportDevices)
            {
                if (device.DeviceId.Equals(deviceId))
                {
                    deviceToRemove = device;
                    break;
                }
            }
        }

        if (deviceToRemove != null)
        {
            //Remove the PassportDevice
            userAccount.PassportDevices.Remove(deviceToRemove);
            SaveAccountListAsync();
        }

        return true;
    }
    ```

-   請在 MockStore 類別中，加入方法來把 Passport 相關的資料加入現有的 UserAccount。 這個方法會呼叫 PassportUpdateDetails，並使用參數來識別使用者及 Passport 詳細資料。 在建立 PassportDevice 時，KeyAttestationResult 已被標記為註釋；但在真實世界的應用程式中，您會需要 KeyAttestationResult。

   ```cs
   using Windows.Security.Credentials;

    public void PassportUpdateDetails(Guid userId, Guid deviceId, byte[] publicKey, 
        KeyCredentialAttestationResult keyAttestationResult)
    {
        UserAccount existingUserAccount = GetUserAccount(userId);
        if (existingUserAccount != null)
        {
            if (!existingUserAccount.PassportDevices.Any(f => f.DeviceId.Equals(deviceId)))
            {
                existingUserAccount.PassportDevices.Add(new PassportDevice()
                {
                    DeviceId = deviceId,
                    PublicKey = publicKey,
                    // KeyAttestationResult = keyAttestationResult,
                });
            }
        }
        SaveAccountListAsync();
    }
    ```

-   現在 MockStore 類別已經完成，但由於它代表資料庫，因此應該被視為私用。 為了要存取 MockStore，您需要 AuthService 類別來管理資料庫的資料。 請在 [AuthService] 資料夾中，建立名為「AuthService.cs」的新類別， 並將類別定義變更為 Public，然後加入單一執行個體模式，來確保執行過程中只會建立一個執行個體。

    ```cs
    namespace PassportLogin.AuthService
    {
        public class AuthService
        {
            // Singleton instance of the AuthService
            // The AuthService is a mock of what a real world server and service implementation would be
            private static AuthService _instance;
            public static AuthService Instance
            {
                get
                {
                    if (null == _instance)
                    {
                        _instance = new AuthService();
                    }
                    return _instance;
                }
            }

            private AuthService()
            { }
        }
    }
    ```

-   AuthService 類別將需要建立 MockStore 類別的執行個體，並提供 MockStore 物件的屬性存取權。

    ```cs
    namespace PassportLogin.AuthService
    {
        public class AuthService
        {
            //Singleton instance of the AuthService
            //The AuthService is a mock of what a real world server and database implementation would be
            private static AuthService _instance;
            public static AuthService Instance
            {
                get
                {
                    if (null == _instance)
                    {
                        _instance = new AuthService();
                    }
                    return _instance;
                }
            }
     
            private MockStore _mockStore = new MockStore();
     
            public Guid GetUserId(string username)
            {
                return _mockStore.GetUserId(username);
            }
     
            public UserAccount GetUserAccount(Guid userId)
            {
                return _mockStore.GetUserAccount(userId);
            }
     
            public List<UserAccount> GetUserAccountsForDevice(Guid deviceId)
            {
                return _mockStore.GetUserAccountsForDevice(deviceId);
            }
        }
    }
    ```

-   您需要 AuthService 類別中的方法，來存取 MockStore 物件中新增、移除及更新 Passport 詳細資料的方法。 請在 AuthService 類別檔案的結尾加入下列方法。

    ```cs
    using Windows.Security.Credentials;

    public void Register(string username)
    {
        _mockStore.AddAccount(username);
    }

    public bool PassportRemoveUser(Guid userId)
    {
        return _mockStore.RemoveAccount(userId);
    }

    public bool PassportRemoveDevice(Guid userId, Guid deviceId)
    {
        return _mockStore.RemoveDevice(userId, deviceId);
    }

    public void PassportUpdateDetails(Guid userId, Guid deviceId, byte[] publicKey, 
        KeyCredentialAttestationResult keyAttestationResult)
    {
        _mockStore.PassportUpdateDetails(userId, deviceId, publicKey, keyAttestationResult);
    }
    ```

-   AuthService 類別將需要提供方法來驗證認證。 這個方法會取得使用者名稱及密碼，並確保帳戶確實存在，且密碼是有效的。 現有的系統可能會有對等的方法，來查看使用者是否已通過授權。 請將下列 ValidateCredentials 加入 AuthService.cs 檔案。

    ```cs
    public bool ValidateCredentials(string username, string password)
    {
        if (!string.IsNullOrEmpty(username) && !string.IsNullOrEmpty(password))
        {
            // This would be used for existing accounts migrating to use Passport
            Guid userId = GetUserId(username);
            if (userId != Guid.Empty)
            {
                UserAccount account = GetUserAccount(userId);
                if (account != null)
                {
                    if (string.Equals(password, account.Password))
                    {
                        return true;
                    }
                }
            }
        }
        return false;
    }
    ```

-   AuthService 類別必須有要求查問方法，以便把查問傳回給用戶端，來驗證使用者是否為他們聲稱的那個人。 然後 AuthService 類別需要方法，以便接收來自用戶端的已簽署查問。 而對於這個實習實驗室來說，判斷已簽署查問是否已完成的方法並不完整。 Microsoft Passport 在某個現有驗證系統中的每個實作，都會有少許的差異。 儲存在伺服器上的公開金鑰，必須與用戶端傳回給伺服器的結果相符。 請將這兩個方法加入 AuthService.cs。

    ```cs
    using Windows.Security.Cryptography;
    using Windows.Storage.Streams;

    public IBuffer PassportRequestChallenge()
    {
        return CryptographicBuffer.ConvertStringToBinary("ServerChallenge", BinaryStringEncoding.Utf8);
    }

    public bool SendServerSignedChallenge(Guid userId, Guid deviceId, byte[] signedChallenge)
    {
        // Depending on your company polices and procedures this step will be different
        // It is at this point you will need to validate the signedChallenge that is sent back from the client.
        // Validation is used to ensure the correct user is trying to access this account. 
        // The validation process will use the signedChallenge and the stored PublicKey 
        // for the username and the specific device signin is called from.
        // Based on the validation result you will return a bool value to allow access to continue or to block the account.

        // For this sample validation will not happen as a best practice solution does not apply and will need to 
           // be configured for each company.
        // Simply just return true.

        // You could get the User's Public Key with something similar to the following:
        byte[] userPublicKey = _mockStore.GetPublicKey(userId, deviceId);
        return true;
    }
    ```

## 練習 2：用戶端的邏輯


您將在這個練習中，將第一個實驗室的用戶端 Views 及 Helper 類別，變更使用 AuthService 類別。 在真實世界中，AuthService 就是驗證伺服器，且您需要使用 Web API 來傳送及接收伺服器的資料。 對於這個實習實驗室來說，為了讓事情保持簡單，戶端和伺服器都在本機上。 而這個練習的目標，是了解如何使用 Microsoft Passport API。

-   在 MainPage.xaml.cs 中，由於 AuthService 類別會建立 MockStore 執行個體來載入帳戶清單，因此您可以移除 Loaded 方法中的 AccountHelper.LoadAccountListAsync 方法。 現在 Loaded 方法看起來應該像下方的內容。 請注意，由於沒有任何項目在等待中，因此我們移除了 Async 方法定義。

    ```cs
    private void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        Frame.Navigate(typeof(UserSelection));
    }
    ```

-   請更新 Login 頁面的介面，來要求使用者輸入 Passport。 這個實習實驗室將示範如何移轉現有的系統來使用 Microsoft Passport，且現有的帳戶將會有使用者名稱及密碼。 同時請更新 XAML 底部的說明，來包含預設的密碼。 請在 Login.xaml 中更新下列 XAML。

    ```xml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <StackPanel Orientation="Vertical">
        <TextBlock Text="Login" FontSize="36" Margin="4" TextAlignment="Center"/>

        <TextBlock x:Name="ErrorMessage" Text="" FontSize="20" Margin="4" Foreground="Red" TextAlignment="Center"/>

        <TextBlock Text="Enter your credentials below" Margin="0,0,0,20"
                   TextWrapping="Wrap" Width="300"
                   TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>

        <StackPanel Orientation="Horizontal" HorizontalAlignment="Center">
          <!-- Username Input -->
          <TextBlock x:Name="UserNameTextBlock" Text="Username: "
                 FontSize="20" Margin="4" Width="100"/>
          <TextBox x:Name="UsernameTextBox" PlaceholderText="sampleUsername" Width="200" Margin="4"/>
        </StackPanel>

        <StackPanel Orientation="Horizontal" HorizontalAlignment="Center">
          <!-- Password Input -->
          <TextBlock x:Name="PasswordTextBlock" Text="Password: "
                 FontSize="20" Margin="4" Width="100"/>
          <PasswordBox x:Name="PasswordBox" PlaceholderText="samplePassword" Width="200" Margin="4"/>
        </StackPanel>

        <Button x:Name="PassportSignInButton" Content="Login" Background="DodgerBlue" Foreground="White"
            Click="PassportSignInButton_Click" Width="80" HorizontalAlignment="Center" Margin="0,20"/>

        <TextBlock Text="Don't have an account?"
                    TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>
        <TextBlock x:Name="RegisterButtonTextBlock" Text="Register now"
                   PointerPressed="RegisterButtonTextBlock_OnPointerPressed"
                   Foreground="DodgerBlue"
                   TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>

        <Border x:Name="PassportStatus" Background="#22B14C"
                   Margin="0,20" Height="100">
          <TextBlock x:Name="PassportStatusText" Text="Microsoft Passport is ready to use!"
                 Margin="4" TextAlignment="Center" VerticalAlignment="Center" FontSize="20"/>
        </Border>

        <TextBlock x:Name="LoginExplaination" FontSize="24" TextAlignment="Center" TextWrapping="Wrap" 
            Text="Please Note: To demonstrate a login, validation will only occur using the default username 
            'sampleUsername' and default password 'samplePassword'"/>

      </StackPanel>
    </Grid>
    ```

-   在 Login 類別程式碼後置中，您將需要把類別頂端的 Account 私用變數，變更為 UserAccount。 請變更 OnNavigatedTo 事件，來把類型轉換成 UserAccount。 您將需要下列的參考。

    ```cs
    using PassportLogin.AuthService;

    namespace PassportLogin.Views
    {
        public sealed partial class Login : Page
        {
            private UserAccount _account;
            private bool _isExistingAccount;

            public Login()
            {
                this.InitializeComponent();
            }

            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                //Check Microsoft Passport is setup and available on this machine
                if (await MicrosoftPassportHelper.MicrosoftPassportAvailableCheckAsync())
                {
                    if (e.Parameter != null)
                    {
                        _isExistingAccount = true;
                        //Set the account to the existing account being passed in
                        _account = (UserAccount)e.Parameter;
                        UsernameTextBox.Text = _account.Username;
                        SignInPassport();
                    }
                }
            }
        }
    }
    ```

-   由於 Login 頁面使用 UserAccount 物件，而非先前的 Account 物件，MicrosoftPassportHelper.cs 將需要更新來把 UserAccount 當做某些方法的參數。 您將需要變更 CreatePassportKeyAsync、RemovePassportAccountAsync 及 GetPassportAuthenticationMessageAsync 方法的下列參數。 由於 UserAccount 類別有某個 UserId 的 Guid，您將開始在更多地方使用 Id 來讓資料更明確。

    ```cs
    public static async Task<bool> CreatePassportKeyAsync(Guid userId, string username)
    {
        KeyCredentialRetrievalResult keyCreationResult = await KeyCredentialManager.RequestCreateAsync(username, KeyCredentialCreationOption.ReplaceExisting);
    }

    public static async void RemovePassportAccountAsync(UserAccount account)
    {

    }
    public static async Task<bool> GetPassportAuthenticationMessageAsync(UserAccount account)
    {
        KeyCredentialRetrievalResult openKeyResult = await KeyCredentialManager.OpenAsync(account.Username);
        //Calling OpenAsync will allow the user access to what is available in the app and will not require user credentials again.
        //If you wanted to force the user to sign in again you can use the following:
        //var consentResult = await Windows.Security.Credentials.UI.UserConsentVerifier.RequestVerificationAsync(account.Username);
        //This will ask for the either the password of the currently signed in Microsoft Account or the PIN used for Microsoft Passport.

        if (openKeyResult.Status == KeyCredentialStatus.Success)
        {
            //If OpenAsync has succeeded, the next thing to think about is whether the client application requires access to backend services.
            //If it does here you would Request a challenge from the Server. The client would sign this challenge and the server
            //would check the signed challenge. If it is correct it would allow the user access to the backend.
            //You would likely make a new method called RequestSignAsync to handle all this
            //e.g. RequestSignAsync(openKeyResult);
            //Refer to the second Microsoft Passport sample for information on how to do this.

            //For this sample there is not concept of a server implemented so just return true.
            return true;
        }
        else if (openKeyResult.Status == KeyCredentialStatus.NotFound)
        {
            //If the _account is not found at this stage. It could be one of two errors. 
            //1. Microsoft Passport has been disabled
            //2. Microsoft Passport has been disabled and re-enabled cause the Microsoft Passport Key to change.
            //Calling CreatePassportKey and passing through the account will attempt to replace the existing Microsoft Passport Key for that account.
            //If the error really is that Microsoft Passport is disabled then the CreatePassportKey method will output that error.
            if (await CreatePassportKeyAsync(account.UserId, account.Username))
            {
                //If the Passport Key was again successfully created, Microsoft Passport has just been reset.
                //Now that the Passport Key has been reset for the _account retry sign in.
                return await GetPassportAuthenticationMessageAsync(account);
            }
        }

        // Can't use Passport right now, try again later
        return false;
    }
    ```

-   Login.xaml.cs 檔案中的 SignInPassport 方法將需要更新來使用 AuthService，而不是 AccountHelper。 認證的驗證過程會透過 AuthService 來進行。 而對於這個實習實驗室來說，唯一的已設定帳戶是「sampleUsername」。 這個帳戶是在 MockStore.cs 的 InitializeSampleUserAccounts 方法中所建立的。 現在，請更新 Login.xaml.cs 的 SignInPassport 方法，來反映以下的程式碼片段 。

    ```cs
    private async void SignInPassportAsync()
    {
        if (_isExistingLocalAccount)
        {
            if (await MicrosoftPassportHelper.GetPassportAuthenticationMessageAsync(_account))
            {
                Frame.Navigate(typeof(Welcome), _account);
            }
        }
        else if (AuthService.AuthService.Instance.ValidateCredentials(UsernameTextBox.Text, PasswordBox.Password))
        {
            Guid userId = AuthService.AuthService.Instance.GetUserId(UsernameTextBox.Text);

            if (userId != Guid.Empty)
            {
                //Now that the account exists on server try and create the necessary passport details and add them to the account
                bool isSuccessful = await MicrosoftPassportHelper.CreatePassportKeyAsync(userId, UsernameTextBox.Text);
                if (isSuccessful)
                {
                    Debug.WriteLine("Successfully signed in with Microsoft Passport!");
                    //Navigate to the Welcome Screen. 
                    _account = AuthService.AuthService.Instance.GetUserAccount(userId);
                    Frame.Navigate(typeof(Welcome), _account);
                }
                else
                {
                    //The passport account creation failed.
                    //Remove the account from the server as passport details were not configured
                    AuthService.AuthService.Instance.PassportRemoveUser(userId);

                    ErrorMessage.Text = "Account Creation Failed";
                }
            }
        }
        else
        {
            ErrorMessage.Text = "Invalid Credentials";
        }
    }
    ```

-   Microsoft Passport 會為每個裝置上的每個帳戶建立不同的公開金鑰及私密金鑰組，因此 Welcome 頁面將需要為每個已登入帳戶顯示已註冊的裝置清單，以及讓使用者能刪除裝置。 請在 Welcome.xaml 中，加入下列程式碼中 ForgetButton 下方的 XAML 。 這將會實作能忘記裝置按鈕、錯誤文字區域，以及顯示所有裝置的清單。

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

        <Button x:Name="ForgetDeviceButton" Content="Forget Device" Click="Button_Forget_Device_Click"
               Foreground="White"
               Background="Gray"
               Margin="0,40,0,20"
               HorizontalAlignment="Center"/>

        <TextBlock x:Name="ForgetDeviceErrorTextBlock" Text="Select a device first"
                  TextWrapping="Wrap" Width="300" Foreground="Red"
                  TextAlignment="Center" VerticalAlignment="Center" FontSize="16" Visibility="Collapsed"/>

        <ListView x:Name="UserListView" MaxHeight="500" MinWidth="350" Width="350" HorizontalAlignment="Center">
          <ListView.ItemTemplate>
            <DataTemplate>
              <Grid Background="Gray" Height="50" Width="350" HorizontalAlignment="Center" VerticalAlignment="Stretch" >
                <TextBlock Text="{Binding DeviceId}" HorizontalAlignment="Center" TextAlignment="Center" VerticalAlignment="Center"
                          Foreground="White"/>
              </Grid>
            </DataTemplate>
          </ListView.ItemTemplate>
        </ListView>
      </StackPanel>
    </Grid>
    ```

-   在 Welcome.xaml.cs 檔案中，您將需要把類別頂端的私用 Account 變數變更為私用 UserAccount 變數。 然後請更新 OnNavigatedTo 方法來使用 AuthService，以及擷取目前的帳戶資訊。 當您擁有帳戶資訊時，可以設定清單的 ItemSource 來顯示裝置。 您將需要加入對 AuthService 命名空間的參考。

   ```cs
   using PassportLogin.AuthService;

    namespace PassportLogin.Views
    {
        public sealed partial class Welcome : Page
        {
            private UserAccount _activeAccount;

            public Welcome()
            {
                InitializeComponent();
            }

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
                _activeAccount = (UserAccount)e.Parameter;
                if (_activeAccount != null)
                {
                    UserAccount account = AuthService.AuthService.Instance.GetUserAccount(_activeAccount.UserId);
                    if (account != null)
                    {
                        UserListView.ItemsSource = account.PassportDevices;
                        UserNameText.Text = account.Username;
                    }
                }
            }
        }
    }
    ```

-   由於您將在移除帳戶時使用 AuthService，您可以移除 Button\_Forget\_User\_Click 方法中對 AccountHelper 的參考。 現在方法看起來應該像下面這樣。

    ```cs
    private void Button_Forget_User_Click(object sender, RoutedEventArgs e)
    {
        //Remove it from Microsoft Passport
        MicrosoftPassportHelper.RemovePassportAccountAsync(_activeAccount);

        Debug.WriteLine("User " + _activeAccount.Username + " deleted.");

        //Navigate back to UserSelection page.
        Frame.Navigate(typeof(UserSelection));
    }
    ```

-   MicrosoftPassportHelper 方法並沒有使用 AuthService 來移除帳戶。 您必須呼叫 AuthService，並傳遞 userId。

    ```cs
    public static async void RemovePassportAccountAsync(UserAccount account)
    {
        //Open the account with Passport
        KeyCredentialRetrievalResult keyOpenResult = await KeyCredentialManager.OpenAsync(account.Username);

        if (keyOpenResult.Status == KeyCredentialStatus.Success)
        {
            // In the real world you would send key information to server to unregister
            AuthService.AuthService.Instance.PassportRemoveUser(account.UserId);
        }

        //Then delete the account from the machines list of Passport Accounts
        await KeyCredentialManager.DeleteAsync(account.Username);
    }
    ```

-   您在完成 Welcome 頁面類別的實作之前，必須在 MicrosoftPassportHelper.cs 中建立方法來讓裝置可以遭到移除。 請建立新的方法來呼叫 AuthService 中的 PassportRemoveDevice。

   ```cs
   public static void RemovePassportDevice(UserAccount account, Guid deviceId)
    {
        AuthService.AuthService.Instance.PassportRemoveDevice(account.UserId, deviceId);
    }
    ```

-   請在 Welcome.xaml.cs 中，實作 Forget Device Click 事件。 這將會使用裝置清單中的已選取裝置，然後使用 Passport Helper 來移除裝置。

    ```cs
    private void Button_Forget_Device_Click(object sender, RoutedEventArgs e)
    {
        PassportDevice selectedDevice = UserListView.SelectedItem as PassportDevice;
        if (selectedDevice != null)
        {
            //Remove it from Microsoft Passport
            MicrosoftPassportHelper.RemovePassportDevice(_activeAccount, selectedDevice.DeviceId);

            Debug.WriteLine("User " + _activeAccount.Username + " deleted.");

            if (!UserListView.Items.Any())
            {
                //Navigate back to UserSelection page.
                Frame.Navigate(typeof(UserSelection));
            }
        }
        else
        {
            ForgetDeviceErrorTextBlock.Visibility = Visibility.Visible;
        }
    }
    ```

-   您要更新的下一個頁面就是 UserSelection 頁面。 UserSelection 頁面將需要使用 AuthService 來擷取目前裝置的所有使用者帳戶。 目前您完全無法把裝置識別碼傳遞給 AuthService，讓它能傳回該裝置的使用者帳戶。 請在 [Utils] 資料夾中建立名為「Helpers.cs」的新類別， 並把類別定義變更為 Public Static，然後加入下列方法來讓您擷取目前的裝置識別碼。

    ```cs
    using Windows.Security.ExchangeActiveSyncProvisioning;

    namespace PassportLogin.Utils
    {
        public static class Helpers
        {
            public static Guid GetDeviceId()
            {
                //Get the Device ID to pass to the server
                EasClientDeviceInformation deviceInformation = new EasClientDeviceInformation();
                return deviceInformation.Id;
            }
        }
    }
    ```

-   在 UserSelection 頁面類別中，只有程式碼後置需要變更，而非使用者介面。 請在 UserSelection.xaml.cs 中，更新 Loaded 方法及 UserSelection 方法來使用 UserAccount 類別，而不是 Account 類別。 您也需要透過 AuthService 來取得此裝置的所有使用者。

    ```cs
    using System.Linq;
    using PassportLogin.AuthService;

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
                List<UserAccount> accounts = AuthService.AuthService.Instance.GetUserAccountsForDevice(Helpers.GetDeviceId());

                if (accounts.Any())
                {
                    UserListView.ItemsSource = accounts;
                    UserListView.SelectionChanged += UserSelectionChanged;
                }
                else
                {
                    //If there are no accounts navigate to the LoginPage
                    Frame.Navigate(typeof(Login));
                }
            }

            /// <summary>
            /// Function called when an account is selected in the list of accounts
            /// Navigates to the Login page and passes the chosen account
            /// </summary>
            private void UserSelectionChanged(object sender, RoutedEventArgs e)
            {
                if (((ListView)sender).SelectedValue != null)
                {
                    UserAccount account = (UserAccount)((ListView)sender).SelectedValue;
                    if (account != null)
                    {
                        Debug.WriteLine("Account " + account.Username + " selected!");
                    }
                    Frame.Navigate(typeof(Login), account);
                }
            }
        }
    }
    ```

-   PassportRegister 頁面需要更新程式碼後置，但使用者介面不需要變更。 請在 PassportRegister.xaml.cs 中，移除類別頂端的私用 Account 變數，因為您不再需要它。 請更新 RegisterButton Click 事件來使用 AuthService。 這個方法會建立新的 UserAccount，然後嘗試並更新該帳戶的密碼詳細資料。 如果 Passport 無法建立 Passport 金鑰，代表註冊程序失敗了，因此帳戶將會遭到移除。

    ```cs
    private async void RegisterButton_Click_Async(object sender, RoutedEventArgs e)
    {
        ErrorMessage.Text = "";

        //Validate entered credentials are acceptable
        if (!string.IsNullOrEmpty(UsernameTextBox.Text))
        {
            //Register an Account on the AuthService so that we can get back a userId
            AuthService.AuthService.Instance.Register(UsernameTextBox.Text);
            Guid userId = AuthService.AuthService.Instance.GetUserId(UsernameTextBox.Text);

            if (userId != Guid.Empty)
            {
                //Now that the account exists on server try and create the necessary passport details and add them to the account
                bool isSuccessful = await MicrosoftPassportHelper.CreatePassportKeyAsync(userId, UsernameTextBox.Text);
                if (isSuccessful)
                {
                    //Navigate to the Welcome Screen. 
                    Frame.Navigate(typeof(Welcome), AuthService.AuthService.Instance.GetUserAccount(userId));
                }
                else
                {
                    //The passport account creation failed.
                    //Remove the account from the server as passport details were not configured
                    AuthService.AuthService.Instance.PassportRemoveUser(userId);

                    ErrorMessage.Text = "Account Creation Failed";
                }
            }
        }
        else
        {
            ErrorMessage.Text = "Please enter a username";
        }
    }
    ```

-   請建置並執行應用程式 (F5)。 請登入範例使用者帳戶，其認證為「sampleUsername」及「samplePassword」。 您可能會在歡迎畫面中看到 \[Forget Device\] \(忘記裝置\) 按鈕，但頁面上沒有任何裝置。 當您建立或移轉使用者來使用 Microsoft Passport 時，系統並不會把 Passport 資訊發送到 AuthService。

    ![](images/passport-auth-3.png)

    ![](images/passport-auth-4.png)

-   如要把 Passport 資訊傳遞給 AuthService，您必須更新 MicrosoftPassportHelper.cs。 在 CreatePassportKeyAsync 方法中，您將需要呼叫新方法來嘗試取得 KeyAttestation，而不是只在成功建立金鑰時傳回 True。 雖然這個實習實驗室不會把這個資訊記錄在 AuthService 中，您將必須了解如何在用戶端取得此資訊。 請更新 CreatePassportKeyAsync 方法。

    ```cs
    public static async Task<bool> CreatePassportKeyAsync(Guid userId, string username)
    {
        KeyCredentialRetrievalResult keyCreationResult = await KeyCredentialManager.RequestCreateAsync(username, KeyCredentialCreationOption.ReplaceExisting);

        switch (keyCreationResult.Status)
        {
            case KeyCredentialStatus.Success:
                Debug.WriteLine("Successfully made key");
                await GetKeyAttestationAsync(userId, keyCreationResult);
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

-   請在 MicrosoftPassportHelper.cs 中，建立這個 GetKeyAttestationAsync 方法。 這個方法將示範如何取得 Microsoft Passport 能為特定裝置上的每個帳戶提供的所有必要資訊。

    ```cs
    using Windows.Storage.Streams;

    private static async Task GetKeyAttestationAsync(Guid userId, KeyCredentialRetrievalResult keyCreationResult)
    {
        KeyCredential userKey = keyCreationResult.Credential;
        IBuffer publicKey = userKey.RetrievePublicKey();
        KeyCredentialAttestationResult keyAttestationResult = await userKey.GetAttestationAsync();
        IBuffer keyAttestation = null;
        IBuffer certificateChain = null;
        bool keyAttestationIncluded = false;
        bool keyAttestationCanBeRetrievedLater = false;
        KeyCredentialAttestationStatus keyAttestationRetryType = 0;

        if (keyAttestationResult.Status == KeyCredentialAttestationStatus.Success)
        {
            keyAttestationIncluded = true;
            keyAttestation = keyAttestationResult.AttestationBuffer;
            certificateChain = keyAttestationResult.CertificateChainBuffer;
            Debug.WriteLine("Successfully made key and attestation");
        }
        else if (keyAttestationResult.Status == KeyCredentialAttestationStatus.TemporaryFailure)
        {
            keyAttestationRetryType = KeyCredentialAttestationStatus.TemporaryFailure;
            keyAttestationCanBeRetrievedLater = true;
            Debug.WriteLine("Successfully made key but not attestation");
        }
        else if (keyAttestationResult.Status == KeyCredentialAttestationStatus.NotSupported)
        {
            keyAttestationRetryType = KeyCredentialAttestationStatus.NotSupported;
            keyAttestationCanBeRetrievedLater = false;
            Debug.WriteLine("Key created, but key attestation not supported");
        }

        Guid deviceId = Helpers.GetDeviceId();
        //Update the Pasport details with the information we have just gotten above.
        //UpdatePassportDetails(userId, deviceId, publicKey.ToArray(), keyAttestationResult);
    }
    ```

-   您可能已經注意到，您剛才加入的 GetKeyAttestationAsync 方法中最後一行被標記為註解。 這最後一行將會是您建立的新方法，它會把所有 Microsoft Passport 資訊傳送到 AuthService。 在真實世界中，您需要利用 Web API 把該資訊傳送給實際的伺服器。

    ```cs
    using System.Runtime.InteropServices.WindowsRuntime;

    public static bool UpdatePassportDetails(Guid userId, Guid deviceId, byte[] publicKey, KeyCredentialAttestationResult keyAttestationResult)
    {
        //In the real world you would use an API to add Passport signing info to server for the signed in _account.
        //For this tutorial we do not implement a WebAPI for our server and simply mock the server locally 
        //The CreatePassportKey method handles adding the Microsoft Passport account locally to the device using the KeyCredential Manager

        //Using the userId the existing account should be found and updated.
        AuthService.AuthService.Instance.PassportUpdateDetails(userId, deviceId, publicKey, keyAttestationResult);
        return true;
    }
    ```

-   請取消 GetKeyAttestationAsync 方法中最後一行的註解，讓 Microsoft Passport 資訊能傳送到 AuthService。
-   請建置並執行應用程式，然後跟之前一樣，使用預設認證登入應用程式。 您將會在歡迎畫面上看到裝置識別碼。 如果您曾經登入另一個裝置，該裝置的識別碼也會在這裡顯示 (如果您曾經有雲端代管的驗證服務)。 對於這個實習實驗室來說，這裡會顯示實際的裝置識別碼。 在真實的實作中，您會想要顯示易記的名稱，讓使用者能輕易了解，並用來辨識每個裝置。

    ![](images/passport-auth-5.png)

-   21. 如要完成這個實習實驗室，在使用者選取使用者選取頁面中的項目並重新登入時，您需要針對使用者的要求及查問。 您先前為 AuthService 建立了兩種方法來要求查問，其中一個使用已簽署的查問。 請在 MicrosoftPassportHelper.cs 中建立名為「RequestSignAsync」的新方法。它將會從 AuthService 要求查問、使用 Passport API 在本機簽署查問，以及把已簽署的查問傳送到 AuthService。 在這個實習實驗室中，AuthService 將會接收已簽署的查問，並傳回 True。 在實際的實作中，您會需要實作驗證機制，來確認查問是否是由正確裝置上的正確使用者所簽署的。 請把下列方法加入 MicrosoftPassportHelper.cs。

    ```cs
    private static async Task<bool> RequestSignAsync(Guid userId, KeyCredentialRetrievalResult openKeyResult)
    {
        // Calling userKey.RequestSignAsync() prompts the uses to enter the PIN or use Biometrics (Windows Hello).
        // The app would use the private key from the user account to sign the sign-in request (challenge)
        // The client would then send it back to the server and await the servers response.
        IBuffer challengeMessage = AuthService.AuthService.Instance.PassportRequestChallenge();
        KeyCredential userKey = openKeyResult.Credential;
        KeyCredentialOperationResult signResult = await userKey.RequestSignAsync(challengeMessage);

        if (signResult.Status == KeyCredentialStatus.Success)
        {
            // If the challenge from the server is signed successfully
            // send the signed challenge back to the server and await the servers response
            return AuthService.AuthService.Instance.SendServerSignedChallenge(
                userId, Helpers.GetDeviceId(), signResult.Result.ToArray());
        }
        else if (signResult.Status == KeyCredentialStatus.UserCanceled)
        {
            // User cancelled the Passport PIN entry.
        }
        else if (signResult.Status == KeyCredentialStatus.NotFound)
        {
            // Must recreate Passport key
        }
        else if (signResult.Status == KeyCredentialStatus.SecurityDeviceLocked)
        {
            // Can't use Passport right now, remember that hardware failed and suggest restart
        }
        else if (signResult.Status == KeyCredentialStatus.UnknownError)
        {
            // Can't use Passport right now, try again later
        }

        return false;
    }
    ```

-   22. 在 MicrosoftPassportHelper 類別中，從 GetPassportAuthenticationMessageAsync 方法呼叫 RequestSignAsync 方法。

    ```cs
    public static async Task<bool> GetPassportAuthenticationMessageAsync(UserAccount account)
    {
        KeyCredentialRetrievalResult openKeyResult = await KeyCredentialManager.OpenAsync(account.Username);
        // Calling OpenAsync will allow the user access to what is available in the app and will not require user credentials again.
        // If you wanted to force the user to sign in again you can use the following:
        // var consentResult = await Windows.Security.Credentials.UI.UserConsentVerifier.RequestVerificationAsync(account.Username);
        // This will ask for the either the password of the currently signed in Microsoft Account or the PIN used for Microsoft Passport.

        if (openKeyResult.Status == KeyCredentialStatus.Success)
        {
            //If OpenAsync has succeeded, the next thing to think about is whether the client application requires access to backend services.
            //If it does here you would Request a challenge from the Server. The client would sign this challenge and the server
            //would check the signed challenge. If it is correct it would allow the user access to the backend.
            //You would likely make a new method called RequestSignAsync to handle all this
            //e.g. RequestSignAsync(openKeyResult);
            //Refer to the second Microsoft Passport sample for information on how to do this.

            return await RequestSignAsync(account.UserId, openKeyResult);
        }
        else if (openKeyResult.Status == KeyCredentialStatus.NotFound)
        {
            //If the _account is not found at this stage. It could be one of two errors. 
            //1. Microsoft Passport has been disabled
            //2. Microsoft Passport has been disabled and re-enabled cause the Microsoft Passport Key to change.
            //Calling CreatePassportKey and passing through the account will attempt to replace the existing Microsoft Passport Key for that account.
            //If the error really is that Microsoft Passport is disabled then the CreatePassportKey method will output that error.
            if (await CreatePassportKeyAsync(account.UserId, account.Username))
            {
                //If the Passport Key was again successfully created, Microsoft Passport has just been reset.
                //Now that the Passport Key has been reset for the _account retry sign in.
                return await GetPassportAuthenticationMessageAsync(account);
            }
        }

        // Can't use Passport right now, try again later
        return false;
    }
    ```

-   在這個練習的過程中，您已經更新用戶端的應用程式來使用 AuthService。 這麼做能讓您不再需要 Account 類別及 AccountHelper 類別。 請刪除 [Utils] 資料夾中的 Account 類別、[Models] 資料夾，以及 AccountHelper 類別。 您將需要移除整個應用程式中對 Models 命名空間的所有參考，才能順利建置解決方案。
-   請建置並執行應用程式，現在您可以搭配模擬服務及資料庫來使用 Microsoft Passport 了！

在這個實習實驗室中，您已經了解如何使用 Passport API，來取代從 Windows 10 電腦使用驗證時的密碼需求。 當您考慮到在現有的系統中，人員需要耗費多少精力來維護密碼，以及支援遺失密碼的功能，您應該就會發現改用這個新的 Microsoft Passport 驗證系統的好處。

我們已經透過練習，提供您如何在伺服器端及用戶端實作的詳細資料。 我們預期這篇文章大多數的讀者都有需要移轉的現有系統，以便開始搭配 Microsoft Passport 來運作，而每個系統的詳細資料都將有所不同。

## 相關主題

* [Microsoft Passport 及 Windows Hello](microsoft-passport.md)
* [Microsoft Passport 登入應用程式](microsoft-passport-login.md)

<!--HONumber=May16_HO2-->


