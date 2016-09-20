---
title: Erstellen einer Microsoft Passport-Anmelde-App
description: "Dies ist der erste Teil der umfassenden exemplarischen Vorgehensweise zum Erstellen einer Windows 10-App für die Universelle Windows-Plattform (UWP), die Microsoft Passport als Alternative zu herkömmlichen Authentifizierungssystemen mit Benutzername und Kennwort verwendet."
ms.assetid: A9E11694-A7F5-4E27-95EC-889307E0C0EF
author: awkoren
ms.sourcegitcommit: af8ae79f67d77195d5ed4801d040b2f1aafe8a97
ms.openlocfilehash: d28f0b9ea08d35a220cdb58367f03af95966e282

---

# Erstellen einer Microsoft Passport-Anmelde-App


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt keine Garantie, weder ausdrücklicher noch impliziter Art, für die hier bereitgestellten Informationen.\]

Dies ist der erste Teil der umfassenden exemplarischen Vorgehensweise zum Erstellen einer Windows 10-App für die Universelle Windows-Plattform (UWP), die Microsoft Passport als Alternative zu herkömmlichen Authentifizierungssystemen mit Benutzername und Kennwort verwendet. Die App verwendet einen Benutzernamen für die Anmeldung und erstellt einen Passport-Schlüssel für jedes Konto. Diese Konten werden durch die PIN geschützt, die in den Windows-Einstellungen beim Konfigurieren von Microsoft Passport eingerichtet wird.

Diese exemplarischen Vorgehensweise ist in zwei Teile aufgeteilt: Erstellen der App und Herstellen der Verbindung mit dem Back-End-Dienst. Fahren Sie nach Abschluss dieses Artikels mit Teil2 fort: [Microsoft Passport-Anmeldedienst](microsoft-passport-login-auth-service.md).

Machen Sie sich in der Übersicht [Microsoft Passport und Windows Hello](microsoft-passport.md) zunächst mit grundlegenden Informationen zur Funktionsweise von Microsoft Passport vertraut.

## Erste Schritte


Die Erstellung dieses Projekts setzt Erfahrung mit C# und XAML voraus. Außerdem muss Visual Studio2015 (mindestens Community Edition) auf einem Computer unter Windows10 verwendet werden.

-   Öffnen Sie Visual Studio2015, und wählen Sie „Datei“ > „Neu“ > „Projekt“ aus.
-   Din Fenster für ein neues Projekt wird geöffnet. Navigieren Sie zu „Vorlagen“ > „Visual C#“.
-   Wählen Sie „ Leere App (Universelle Windows-App)“ aus, und nennen Sie die Anwendung „PassportLogin“.
-   Erstellen Sie die neue Anwendung, und führen Sie sie aus (F5). Daraufhin erscheint ein leeres Fenster auf dem Bildschirm. Schließen Sie die Anwendung.

![](images/passport-login-1.png)

## Übung1: Anmelden mit Microsoft Passport


In dieser Übung lernen Sie, wie Sie ermitteln, ob Microsoft Passport auf dem Computer eingerichtet ist, und wie Sie sich mit Microsoft Passport bei einem Konto anmelden.

-   Erstellen Sie in dem neuen Projekt einen neuen Projektmappenordner namens „Views“. Dieser Ordner enthält die Seiten, zu denen in diesem Beispiel navigiert wird. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, wählen Sie „Hinzufügen“ > „Neuer Ordner“ aus, und benennen Sie den Ordner anschließend in „Views“ um.

    ![](images/passport-login-2.png)

-   Klicken Sie mit der rechten Maustaste auf den neuen Ordner „Views“. Wählen Sie „Hinzufügen“ > „Neues Element“ und anschließend die Option „Leere Seite“ aus. Nennen Sie die Seite „Login.xaml“.

    ![](images/passport-login-3.png)

-   Fügen Sie den folgenden XAML-Code hinzu, um die Benutzeroberfläche für die neue Anmeldeseite zu definieren. Dieser XAML-Code definiert ein StackPanel-Element zur Ausrichtung folgender untergeordneter Elemente:

    -   TextBlock-Element für einen Titel.
    -   TextBlock-Element für Fehlermeldungen.
    -   TextBox-Element für die Eingabe des Benutzernamens.
    -   Button-Element für die Navigation zu einer Registrierungsseite.
    -   TextBlock-Element für den Status von Microsoft Passport.
    -   TextBlock-Element zur Erläuterung der Anmeldeseite, da kein Back-End und keine konfigurierten Benutzer vorhanden sind.

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

-   Dem CodeBehind müssen einige Methoden hinzugefügt werden, damit die Lösung erstellt werden kann. Drücken Sie entweder F7, oder verwenden Sie den Projektmappen-Explorer, um zu „Login.xaml.cs“ zu gelangen. Fügen Sie zur Behandlung des Anmelde- und Registrierungsereignisses die beiden folgenden Ereignismethoden hinzu. Durch diese Methoden wird „ErrorMessage.Text“ vorerst auf eine leere Zeichenfolge festgelegt.

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

-   Bearbeiten Sie zum Rendern der Anmeldeseite den MainPage-Code so, dass beim Laden der Hauptseite zur Anmeldeseite navigiert wird. Öffnen Sie die Datei „MainPage.xaml.cs“. Doppelklicken Sie im Projektmappen-Explorer auf „MainPage.xaml.cs“. Sollten Sie das Element nicht finden, klicken Sie auf den kleinen Pfeil neben „MainPage.xaml“, um das CodeBehind anzuzeigen. Erstellen Sie eine geladene Ereignishandlermethode für die Navigation zur Anmeldeseite. Sie müssen einen Verweis auf den Views-Namespace hinzufügen.

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

-   Auf der Anmeldeseite müssen Sie durch Behandeln des OnNavigatedTo-Ereignisses überprüfen, ob Microsoft Passport auf diesem Computer verfügbar ist. Implementieren Sie Folgendes in „Login.xaml.cs“. Das MicrosoftPassportHelper-Objekt gibt einen Fehler aus. Das liegt daran, dass es noch nicht implementiert wurde.

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

-   Klicken Sie zum Erstellen der MicrosoftPassportHelper-Klasse mit der rechten Maustaste auf die Projektmappe „PassportLogin (Universelle Windows-App)“, und wählen Sie „Hinzufügen“ > „Neuer Ordner“ aus. Nennen Sie den Ordner „Utils“.

    ![](images/passport-login-5.png)

-   Klicken Sie mit der rechten Maustaste auf den Ordner „Utils“, und wählen Sie „Hinzufügen“ > „Klasse“ aus. Nennen Sie die Klasse „MicrosoftPassportHelper.cs“.
-   Definieren Sie „MicrosoftPassportHelper“ als öffentliche, statische Klasse, und fügen Sie anschließend die folgende Methode hinzu, um den Benutzer darüber zu informieren, ob Microsoft Passport verwendungsbereit ist. Sie müssen die erforderlichen Namespaces hinzufügen.

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

-   Fügen Sie in „Login.xaml.cs“ einen Verweis auf den Utils-Namespace hinzu. Dadurch wird der Fehler in der OnNavigatedTo-Methode behoben.

    ```cs
    using PassportLogin.Utils;
    ```

-   Erstellen Sie die Anwendung, und führen Sie sie aus (F5). Sie werden auf die Anmeldeseite weitergeleitet, und das MicrosoftPassport-Banner gibt Aufschluss darüber, ob Passport verwendungsbereit ist. Das Banner ist entweder grün oder blau und gibt so den Status von Microsoft Passport auf Ihrem Computer an.

    ![](images/passport-login-6.png)

    ![](images/passport-login-7.png)

-   Als Nächstes muss die Anmeldelogik erstellt werden. Erstellen Sie einen neuen Ordner namens „Models“.
-   Erstellen Sie im Ordner „Models“ eine neue Klasse namens „Account.cs“. Diese Klasse fungiert als Modell für Ihr Konto. Da es sich hierbei um ein Beispiel handelt, enthält es lediglich einen Benutzernamen. Definieren Sie die Klasse als öffentliche Klasse, und fügen Sie die Username-Eigenschaft hinzu.
    
    ```cs
    namespace PassportLogin.Models
    {
        public class Account
        {
            public string Username { get; set; }
        }
    }
    ```

-   Sie benötigen eine Möglichkeit zur Behandlung von Konten. Da in dieser praktischen Übung weder ein Server noch eine Datenbank vorhanden ist, wird eine Liste mit Benutzern lokal gespeichert und geladen. Klicken Sie mit der rechten Maustaste auf den Ordner „Utils“, und fügen Sie eine neue Klasse namens „AccountHelper.cs“ hinzu. Definieren Sie die Klasse als öffentliche, statische Klasse. „AccountHelper“ ist eine statische Klasse mit allen erforderlichen Methoden zum lokalen Speichern und Laden der Kontenliste. Zum Speichern und Laden wird ein XmlSerializer-Element verwendet. Darüber hinaus müssen Sie sich die gespeicherte Datei und ihren Speicherort merken. Dazu werden Verweise auf weitere Namespaces benötigt.
    
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

-   Implementieren Sie als Nächstes eine Möglichkeit zum Hinzufügen und Entfernen eines Kontos aus der lokalen Kontenliste. Durch diese Aktionen wird die Liste jeweils gespeichert. Zum Abschluss wird für diese praktische Übung noch eine Überprüfungsmethode benötigt. Da weder ein Authentifizierungsserver noch eine Benutzerdatenbank zur Verfügung steht, erfolgt die Überprüfung anhand eines einzelnen, hartcodierten Benutzers. Die folgenden Methoden müssen der AccountHelper-Klasse hinzugefügt werden:
    
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
            }<
    ```

-   Als Nächstes müssen Sie eine Anmeldeanforderung des Benutzers behandeln. Erstellen Sie in „Login.xaml.cs“ eine neue private Variable, die das aktuelle Konto für die Anmeldung enthält. Fügen Sie dann einen neuen SignInPassport-Methodenaufruf hinzu. Dadurch werden die Anmeldeinformationen des Kontos mithilfe der AccountHelper.ValidateAccountCredentials-Methode überprüft. Diese Methode gibt einen booleschen Wert zurück, wenn der eingegebene Benutzername dem hartcodierten Zeichenfolgenwert entspricht, den Sie im vorherigen Schritt festgelegt haben. Der hartcodierte Wert für dieses Beispiel lautet „sampleUsername“.

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

-   Sie haben wahrscheinlich den auskommentierten Code bemerkt, der auf eine Methode in „MicrosoftPassportHelper“ verweist. Fügen Sie in „MicrosoftPassportHelper.cs“ eine neue Methode namens „CreatePassportKeyAsync“ hinzu. Diese Methode verwendet die Microsoft Passport-API in [**KeyCredentialManager**](https://msdn.microsoft.com/library/windows/apps/dn973043). Durch Aufrufen von [**RequestCreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn973048) wird ein spezifischer Passport-Schlüssel für die *accountId* und den lokalen Computer erstellt. Beachten Sie die Kommentare in der switch-Anweisung, falls Sie dies in der Praxis umsetzen möchten.

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

-   Nachdem Sie nun die CreatePassportKeyAsync-Methode erstellt haben, kehren Sie zur Datei „Login.xaml.cs“ zurück, und heben Sie die Auskommentierung des Codes in der SignInPassport-Methode auf.

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

-   Erstellen Sie die Anwendung, und führen Sie sie aus. Sie werden auf die Anmeldeseite weitergeleitet. Geben Sie „sampleUsername“ ein, und klicken Sie auf „Anmelden“. Daraufhin werden Sie von Microsoft Passport zur Eingabe Ihrer PIN aufgefordert. Nach korrekter PIN-Eingabe kann von der CreatePassportKeyAsync-Methode ein Passport-Schlüssel erstellt werden. Überwachen Sie die Ausgabefenster auf Erfolgsmeldungen.

    ![](images/passport-login-8.png)

## Übung2: Willkommens- und Benutzerauswahlseite


Diese Übung baut direkt auf der vorherigen Übung auf. Nach erfolgreicher Anmeldung soll der Benutzer auf eine Willkommensseite weitergeleitet werden, auf der er sich abmelden oder sein Konto löschen kann. Da von Passport ein computerspezifischer Schlüssel erstellt wird, kann ein Benutzerauswahlbildschirm erstellt werden, der alle Benutzer anzeigt, die auf diesem Computer angemeldet waren. Ein Benutzer kann dann eines dieser Konten auswählen, um direkt und ohne erneute Kennworteingabe zur Willkommensseite zu gelangen, da er bereits für den Zugriff auf den Computer authentifiziert wurde.

-   Fügen Sie im Ordner „Views“ eine neue leere Seite namens „Welcome.xaml“ hinzu. Fügen Sie zur Vervollständigung der Benutzeroberfläche den folgenden XAML-Code hinzu. Dadurch werden ein Titel, der Benutzername des angemeldeten Benutzers und zwei Schaltflächen angezeigt. Über eine der Schaltflächen gelangt der Benutzer wieder zur Benutzerliste (die Sie später noch erstellen), die andere Schaltfläche dient zum Löschen des Benutzers.

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

-   Fügen Sie in der CodeBehind-Datei „Welcome.xaml.cs“ eine neue private Variable hinzu, die das angemeldete Konto enthält. Sie müssen eine Methode zum Überschreiben des OnNavigatedTo-Ereignisses implementieren. Dadurch wird das an die Willkommensseite übergebene Konto gespeichert. Außerdem müssen Sie das Klickereignis für die beiden im XAML-Code definierten Schaltflächen implementieren. Sie benötigen einen Verweis auf die Ordner „Models“ und „Utils“.

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

-   Wahrscheinlich haben Sie bemerkt, dass im Klickereignis zum Löschen des Benutzers eine Zeile auskommentiert ist. Das Konto wird aus Ihrer lokalen Liste entfernt, kann derzeit aber nicht aus Passport entfernt werden. Sie müssen in „MicrosoftPassportHelper.cs“ eine neue Methode implementieren, die das Entfernen eines Passport-Benutzers behandelt. Diese Methode verwendet andere MicrosoftPassport-APIs, um das Konto zu öffnen und zu löschen. In der Praxis muss beim Löschen eines Kontos der Server oder die Datenbank benachrichtigt werden, damit die Benutzerdatenbank gültig bleibt. Sie benötigen einen Verweis auf den Ordner „Models“.

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

-   Heben Sie in „Welcome.xaml.cs“ die Auskommentierung der Zeile mit dem RemovePassportAccountAsync-Aufruf auf.

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

-   Nach erfolgreicher Ausführung von „CreatePassportKeyAsync“ navigiert die SignInPassport-Methode (in „Login.xaml.cs“) zur Willkommensseite und übergibt das Konto.

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

-   Erstellen Sie die Anwendung, und führen Sie sie aus. Geben Sie „sampleUsername“ ein, und klicken Sie auf „Anmelden“. Geben Sie Ihre PIN ein. Bei erfolgreicher Eingabe werden Sie auf die Willkommensseite weitergeleitet. Klicken Sie auf die Schaltfläche zum Löschen des Benutzers, und prüfen Sie im Ausgabefenster, ob der Benutzer gelöscht wurde. Beachten Sie, dass nach dem Löschen des Benutzers weiterhin die Willkommensseite angezeigt wird. Sie müssen eine Benutzerauswahlseite erstellen, zu der die App navigieren kann.

    ![](images/passport-login-9.png)

-   Erstellen Sie im Ordner „Views“ eine neue leere Seite namens „UserSelection.xaml“, und fügen Sie den folgenden XAML-Code hinzu, um die Benutzeroberfläche zu definieren. Diese Seite enthält ein [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878)-Element, das alle Benutzer in der lokalen Kontenliste anzeigt, sowie eine Schaltfläche, über die der Benutzer zur Anmeldeseite navigieren und ein weiteres Konto hinzufügen kann.

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
    </Grid><
    ```

-   Implementieren Sie in „UserSelection.xaml.cs“ die geladene Methode, die zur Anmeldeseite navigiert, falls die lokale Liste keine Konten enthält. Implementieren Sie außerdem das SelectionChanged-Ereignis für das ListView-Element und ein Klickereignis für die Schaltfläche.

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

-   Es gibt einige Orte in der App, an denen eine Navigation zur UserSelection-Seite erfolgen soll. In „MainPage.xaml.cs“ müssen Sie anstatt zur Anmeldeseite zur UserSelection-Seite navigieren. Während Sie sich in „MainPage“ im geladenen Ereignis befinden, müssen Sie die Kontenliste laden, damit von der UserSelection-Seite geprüft werden kann, ob Konten vorhanden sind. Dies erfordert eine asynchrone Lademethode sowie einen Verweis auf den Ordner „Utils“.

    ```cs
    using PassportLogin.Utils;

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        // Load the local Accounts List before navigating to the UserSelection page
        await AccountHelper.LoadAccountListAsync();
        Frame.Navigate(typeof(UserSelection));
    }
    ```

-   Als Nächstes möchten Sie von der Willkommensseite zur UserSelection-Seite navigieren. In beiden Klickereignissen müssen Sie wieder zur UserSelection-Seite navigieren.

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

-   Auf der Anmeldeseite benötigen Sie Code für die Anmeldung bei dem Konto, das in der Liste der UserSelection-Seite ausgewählt wurde. Speichern Sie im OnNavigatedTo-Ereignis das an die Navigation übergebene Konto. Fügen Sie zunächst eine neue private Variable hinzu, die angibt, ob es sich bei dem Konto um ein vorhandenes Konto handelt. Behandeln Sie dann das OnNavigatedTo-Ereignis.

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

-   Die SignInPassport-Methode muss für die Anmeldung bei dem ausgewählten Konto aktualisiert werden. Da für das Konto bereits ein Passport-Schlüssel erstellt wurde, benötigt „MicrosoftPassportHelper“ eine weitere Methode, um das Konto mit Passport zu öffnen. Implementieren Sie die neue Methode in „MicrosoftPassportHelper.cs“, um einen vorhandenen Benutzer mit Passport anzumelden. Informationen zu den einzelnen Teilen des Codes finden Sie in den Codekommentaren.

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

-   Aktualisieren Sie die SignInPassport-Methode in „Login.xaml.cs“, um das vorhandene Konto zu behandeln. Dadurch wird die neue Methode in „MicrosoftPassportHelper.cs“ verwendet. Ist der Vorgang erfolgreich, wird das Konto angemeldet, und der Benutzer wird auf die Willkommensseite weitergeleitet.

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

-   Erstellen Sie die Anwendung, und führen Sie sie aus. Melden Sie sich mit „sampleUsername“ an. Geben Sie Ihre PIN ein. Bei erfolgreicher Eingabe werden Sie auf die Willkommensseite weitergeleitet. Klicken Sie auf die Schaltfläche für die Rückkehr zur Benutzerliste. Daraufhin wird ein einzelner Benutzer in der Liste angezeigt. Wenn Sie auf diesen Benutzer klicken, können Sie sich dank Passport direkt und ohne erneute Kennworteingabe wieder anmelden.

    ![](images/passport-login-10.png)

## Übung3: Registrieren eines neuen Passport-Benutzers


In dieser Übung erstellen Sie eine neue Seite für die Erstellung eines neuen Passport-Kontos. Das funktioniert ähnlich wie bei der Anmeldeseite. Die Anmeldeseite wird für einen vorhandenen Benutzer implementiert, der für die Verwendung von Passport migriert wird. Eine PassportRegister-Seite erstellt die Passport-Registrierung für einen neuen Benutzer.

-   Erstellen Sie im Ordner „Views“ eine neue leere Seite namens „PassportRegister.xaml“. Fügen Sie dem XAML-Code Folgendes hinzu, um die Benutzeroberfläche einzurichten. Diese Benutzeroberfläche ähnelt der Benutzeroberfläche für die Anmeldeseite.

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

-   Implementieren Sie in der CodeBehind-Datei „PassportRegister.xaml.cs“ eine private Kontovariable und ein Klickereignis für die Registrierungsschaltfläche. Dadurch wird ein neues lokales Konto hinzugefügt und ein Passport-Schlüssel erstellt.

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

-   Beim Klicken auf die Registrierungsschaltfläche muss von der Anmeldeseite zu dieser Seite navigiert werden.

    ```cs
    private void RegisterButtonTextBlock_OnPointerPressed(object sender, PointerRoutedEventArgs e)
    {
        ErrorMessage.Text = "";
        Frame.Navigate(typeof(PassportRegister));
    }
    ```

-   Erstellen Sie die Anwendung, und führen Sie sie aus. Versuchen Sie, einen neuen Benutzer zu registrieren. Kehren Sie anschließend zur Benutzerliste zurück, und vergewissern Sie sich, dass Sie den Benutzer auswählen und sich anmelden können.

    ![](images/passport-login-11.png)

In dieser Übung wurden die Grundlagen für die Verwendung der neuen Microsoft Passport-API zur Authentifizierung vorhandener Benutzer sowie zur Erstellung von Konten für neue Benutzer vermittelt. Dank dieser neuen Kenntnisse müssen sich die Benutzer bald keine Kennwörter für Ihre Anwendung mehr merken, während gleichzeitig der Schutz durch die Benutzerauthentifizierung erhalten bleibt. Windows10 nutzt die Passport-Technologie zur Unterstützung der biometrischen Anmeldung von Windows Hello. Wenn Sie einen Computer verwendet haben, der Windows Hello unterstützt, haben Sie gesehen, dass diese Übungen bereits Windows Hello unterstützen.

Neben der Implementierung der Microsoft Passport-Unterstützung sind für Entwickler keine weiteren Schritte zur Unterstützung von Windows Hello erforderlich.

## Verwandte Themen

* [Microsoft Passport und Windows Hello](microsoft-passport.md)
* [Microsoft Passport-Anmeldedienst](microsoft-passport-login-auth-service.md)


<!--HONumber=Jun16_HO4-->


