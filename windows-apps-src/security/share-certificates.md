---
title: Freigabe von Zertifikaten zwischen Apps
description: "UWP-Apps, die über eine Kombination aus Benutzer-ID und Kennwort hinaus eine sichere Authentifizierung benötigen, können Zertifikate für die Authentifizierung verwenden."
ms.assetid: 159BA284-9FD4-441A-BB45-A00E36A386F9
author: awkoren
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: 2bb1b601e1ab35115c88692f6c36dccc70836541

---

# Freigabe von Zertifikaten zwischen Apps


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


UWP-Apps, die über eine Kombination aus Benutzer-ID und Kennwort hinaus eine sichere Authentifizierung benötigen, können Zertifikate für die Authentifizierung verwenden. Die Zertifikatauthentifizierung bietet eine hohe Vertrauenswürdigkeit bei der Benutzerauthentifizierung. Es kann vorkommen, dass eine Gruppe von Diensten einen Benutzer für mehrere Apps authentifizieren möchte. In diesem Artikel wird veranschaulicht, wie Sie mehrere Apps mit demselben Zertifikat authentifizieren und für einen Benutzer geeigneten Code zum Importieren eines Zertifikats bereitstellen können, das für den Zugriff auf sichere Webdienste bestimmt ist.

Apps können für die Authentifizierung bei einem Webdienst ein Zertifikat verwenden, und mehrere Apps können ein einzelnes Zertifikat aus dem Zertifikatspeicher verwenden, um denselben Benutzer zu authentifizieren. Falls im Speicher kein Zertifikat vorhanden ist, können Sie der App Code für den Import eines Zertifikats aus einer PFX-Datei hinzufügen.

## Aktivieren von Microsoft Internet Information Services (IIS) und Clientzertifikatszuordnungen


In diesem Artikel werden die Microsoft-Internetinformationsdienste (Microsoft Internet Information Services, IIS) als Beispiel verwendet. IIS ist standardmäßig nicht aktiviert. Sie können IIS über die Systemsteuerung aktivieren.

1.  Öffnen Sie die Systemsteuerung, und wählen Sie **Programme** aus.
2.  Wählen Sie die Option **Windows-Features aktivieren oder deaktivieren** aus.
3.  Erweitern Sie **Internetinformationsdienste** und dann **WWW-Dienste**. Erweitern Sie **Anwendungsentwicklungsfeatures**, und wählen Sie **ASP.NET3.5** und **ASP.NET4.5**. Das Auswählen führt dazu, dass **Internetinformationsdienste** automatisch aktiviert wird.
4.  Klicken Sie auf **OK**, um die Änderungen zu übernehmen.

## Erstellen und Veröffentlichen eines sicheren Webdiensts


1.  Führen Sie Microsoft Visual Studio als Administrator aus, und wählen Sie auf der Startseite die Option **Neues Projekt** aus. Für die Veröffentlichung eines Webdiensts auf einem IIS-Server ist Administratorzugriff erforderlich. Ändern Sie im Dialogfeld „Neues Projekt“ das Framework in **.NET Framework 3.5**. Wählen Sie **Visual C#** -&gt;**Web** -&gt;**Visual Studio** -&gt;**ASP.NET-Webdienstanwendung**. Geben Sie der Anwendung den Namen „FirstContosoBank“. Klicken Sie auf **OK**, um das Projekt zu erstellen.
2.  Ersetzen Sie in der Datei **Service1.asmx.cs** die standardmäßige Webmethode **HelloWorld** durch die folgende "Login"-Methode.
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

3.  Speichern Sie die Datei **Service1.asmx.cs**.
4.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die App "FirstContosoBank", und wählen Sie **Veröffentlichen**.
5.  Erstellen Sie im Dialogfeld **Web veröffentlichen** ein neues Profil, und geben Sie ihm den Namen "ContosoProfile". Klicken Sie auf **Weiter**.
6.  Geben Sie auf der nächsten Seite den Servernamen für Ihren IIS-Server ein, und geben Sie dann den Websitenamen "Standardwebsite/FirstContosoBank" an. Klicken Sie auf **Veröffentlichen**, um den Webdienst zu veröffentlichen.

## Konfigurieren des Webdiensts für die Verwendung der Clientzertifikatauthentifizierung


1.  Führen Sie den **Internetinformationsdienste (IIS)-Manager** aus.
2.  Erweitern Sie die Websites für Ihren IIS-Server. Wählen Sie unter **Standardwebsite** den neuen Webdienst "FirstContosoBank". Wählen Sie im Abschnitt **Aktionen** die Option **Erweiterte Einstellungen...**.
3.  Legen Sie den **Anwendungspool** auf **.NET v2.0** fest, und klicken Sie auf **OK**.
4.  Wählen Sie im **Internetinformationsdienste (IIS)-Manager** Ihren IIS-Server aus, und doppelklicken Sie anschließend auf **Serverzertifikate**. Wählen Sie im Abschnitt **Aktionen** die Option **Selbstsigniertes Zertifikat erstellen...**. Geben Sie "ContosoBank" als Anzeigenamen für das Zertifikat ein, und klicken Sie auf **OK**. Es wird ein neues Zertifikat zur Verwendung durch den IIS-Server im Format „&lt;Servername&gt;.&lt;Domänenname&gt;“ erstellt.
5.  Wählen Sie im **Internetinformationsdienste (IIS)-Manager** die Standardwebsite aus. Wählen Sie im Abschnitt **Aktionen** die Option **Bindung**, und klicken Sie anschließend auf **Hinzufügen...**. Wählen Sie als Typ https, legen Sie den Port auf 443 fest, und geben Sie den vollständigen Hostnamen für Ihren IIS-Server ein („&lt;Servername&gt;.&lt;Domänenname&gt;“). Legen Sie das SSL-Zertifikat auf „ContosoBank“ fest. Klicken Sie auf **OK**. Klicken Sie im Fenster **Sitebindungen** auf **Schließen**.
6.  Wählen Sie im **Internetinformationsdienste (IIS)-Manager** den Webdienst "FirstContosoBank" aus. Doppelklicken Sie auf **SSL-Einstellungen**. Aktivieren Sie **SSL erforderlich**. Wählen Sie unter **Clientzertifikate** die Option **Erforderlich**. Klicken Sie im Abschnitt **Aktionen** auf **Übernehmen**.
7.  Sie können überprüfen, ob der Webdienst richtig konfiguriert ist, indem Sie den Webbrowser öffnen und die folgende Webadresse eingeben: https://&lt;server-name&gt;.&lt;domain-name&gt;/FirstContosoBank/Service1.asmx. Beispiel: „https://myserver.example.com/FirstContosoBank/Service1.asmx“. Wenn der Webdienst richtig konfiguriert ist, werden Sie zum Auswählen eines Clientzertifikats für den Zugriff auf den Webdienst aufgefordert.

Sie können diese Schritte wiederholen, um mehrere Webdienste zu erstellen, auf die mit demselben Clientzertifikat zugegriffen werden kann.

## Erstellen einer Windows Store-App mit Verwendung der Authentifizierung per Zertifikat


Nachdem Sie nun über mindestens einen sicheren Webdienst verfügen, können Ihre Apps Zertifikate für die Authentifizierung bei diesen Webdiensten verwenden. Wenn Sie mithilfe des [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)-Objekts eine Anforderung an einen authentifizierten Webdienst senden, enthält die erste Anforderung kein Clientzertifikat. Der authentifizierte Webdienst antwortet mit einer Anforderung der Clientauthentifizierung. Daraufhin fragt der Windows-Client den Zertifikatspeicher automatisch nach verfügbaren Clientzertifikaten ab. Der Benutzer kann für die Authentifizierung beim Webdienst eine Auswahl aus diesen Zertifikaten treffen. Da einige Zertifikate kennwortgeschützt sind, müssen Sie es Benutzern ermöglichen, das Kennwort für ein Zertifikat einzugeben.

Falls keine Clientzertifikate verfügbar sind, muss der Benutzer dem Zertifikatspeicher ein Zertifikat hinzufügen. Sie können Code in die App einfügen, der Benutzern die Auswahl einer PFX-Datei mit einem Clientzertifikat und das Importieren dieses Zertifikats in den Clientzertifikatspeicher ermöglicht.


            **Tipp**  Verwenden Sie „makecert.exe“ zum Erstellen einer PFX-Datei zur Verwendung für diese Schnellstartanleitung. Informationen zur Verwendung von „makecert.exe“ finden Sie unter [MakeCert](https://msdn.microsoft.com/library/windows/desktop/aa386968).

 

1.  Öffnen Sie Visual Studio, und erstellen Sie auf der Startseite ein neues Projekt. Geben Sie dem neuen Projekt den Namen "FirstContosoBankApp". Klicken Sie auf **OK**, um das neue Projekt zu erstellen.
2.  Fügen Sie in der Datei "MainPage.xaml" dem standardmäßigen **Grid**-Element den folgenden XAML-Code hinzu. Dieser XAML-Code enthält Folgendes: eine Schaltfläche zum Suchen nach einer zu importierenden PFX-Datei, ein Textfeld zum Eingeben eines Kennworts für eine kennwortgeschützte PFX-Datei, eine Schaltfläche zum Importieren einer ausgewählten PFX-Datei, eine Schaltfläche zum Anmelden beim sicheren Webdienst und einen Textblock zum Anzeigen des Zustands der aktuellen Aktion.
    ```xml
    <Button x:Name="Import" Content="Import Certificate (PFX file)" HorizontalAlignment="Left" Margin="352,305,0,0" VerticalAlignment="Top" Height="77" Width="260" Click="Import_Click" FontSize="16"/>
    <Button x:Name="Login" Content="Login" HorizontalAlignment="Left" Margin="611,305,0,0" VerticalAlignment="Top" Height="75" Width="240" Click="Login_Click" FontSize="16"/>
    <TextBlock x:Name="Result" HorizontalAlignment="Left" Margin="355,398,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Height="153" Width="560"/>
    <PasswordBox x:Name="PfxPassword" HorizontalAlignment="Left" Margin="483,271,0,0" VerticalAlignment="Top" Width="229"/>
    <TextBlock HorizontalAlignment="Left" Margin="355,271,0,0" TextWrapping="Wrap" Text="PFX password" VerticalAlignment="Top" FontSize="18" Height="32" Width="123"/>
    <Button x:Name="Browse" Content="Browse for PFX file" HorizontalAlignment="Left" Margin="352,189,0,0" VerticalAlignment="Top" Click="Browse_Click" Width="499" Height="68" FontSize="16"/>
    <TextBlock HorizontalAlignment="Left" Margin="717,271,0,0" TextWrapping="Wrap" Text="(Optional)" VerticalAlignment="Top" Height="32" Width="83" FontSize="16"/>
    ```
    
3.  Speichern Sie die Datei "MainPage.xaml".
4.  Fügen Sie in der Datei "MainPage.xaml.cs" die folgenden using-Anweisungen hinzu:
    ```cs
    using Windows.Web.Http;
    using System.Text;
    using Windows.Security.Cryptography.Certificates;
    using Windows.Storage.Pickers;
    using Windows.Storage;
    using Windows.Storage.Streams;
    ```

5.  Fügen Sie in der Datei "MainPage.xaml.cs" der **MainPage**-Klasse die folgenden Variablen hinzu. Damit werden die Adresse für die sichere Login-Methode des Webdiensts „FirstContosoBank“ sowie eine globale Variable, die ein PFX-Zertifikat zum Importieren in den Zertifikatspeicher enthält, angegeben. Ersetzen Sie &lt;Servername&gt; durch den vollqualifizierten Servernamen Ihres IIS-Servers (Microsoft Internet Information Server).
    ```cs
    private Uri requestUri = new Uri("https://<server-name>/FirstContosoBank/Service1.asmx?op=Login");
    private string pfxCert = null;
    ```

6.  Fügen Sie in der Datei "MainPage.xaml.cs" den folgenden Klickhandler für die Anmeldeschaltfläche und -methode zum Zugreifen auf den sicheren Webdienst hinzu.
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

7.  Fügen Sie in der Datei „MainPage.xaml.cs“ die folgenden Klickhandler für die Schaltfläche zum Suchen nach einer PFX-Datei und die Schaltfläche zum Importieren einer ausgewählten PFX-Datei in den Zertifikatspeicher hinzu.
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

8.  Führen Sie Ihre App aus, und melden Sie sich beim sicheren Webdienst an. Importieren Sie eine PFX-Datei in den lokalen Zertifikatspeicher.

Mithilfe dieser Schritte können Sie mehrere Apps erstellen, für die dasselbe Benutzerzertifikat verwendet wird, um auf dieselben oder unterschiedliche sichere Webdienste zuzugreifen.


<!--HONumber=Jun16_HO4-->


