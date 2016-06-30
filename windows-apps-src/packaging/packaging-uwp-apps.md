---
author: msatranjr
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: Verpacken von UWP-Apps
description: "Um Ihre UWP-App (Universelle Windows-Plattform) zu verkaufen oder an andere Benutzer zu verteilen, müssen Sie ein APPXUPLOAD-Paket erstellen."
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: a77e441cbd1b6826e06064dbd4be449813754b25

---
# Verpacken von UWP-Apps

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Um Ihre UWP-App (Universelle Windows-Plattform) zu verkaufen oder an andere Benutzer zu verteilen, müssen Sie ein APPXUPLOAD-Paket erstellen. Beim Erstellen des APPXUPLOAD-Pakets wird ein weiteres APPX-Paket für Testzwecke und das Querladen generiert. Sie können Ihre App direkt verteilen, indem Sie das APPX-Paket durch Querladen auf einem Gerät installieren. In diesem Artikel wird das Konfigurieren, Erstellen und Testen von UWP-App-Paketen beschrieben. Weitere Informationen über das Querladen finden Sie unter [Querladen von Apps mit DISM](http://go.microsoft.com/fwlink/?LinkID=231020).

Für Windows 10 generieren Sie ein Paket („.appxupload“), das in den Windows Store hochgeladen werden kann. Ihre App kann dann auf beliebigen Windows 10-Geräten installiert und ausgeführt werden. Im Folgenden finden Sie die Schritte zum Erstellen eines App-Pakets.

1.  [Vor dem Verpacken der App](#before-packaging-your-app). Führen Sie die folgenden Schritte aus, um sicherzustellen, dass die App verpackt und an den Store übermittelt werden kann.
2.  [Konfigurieren eines App-Pakets](#configure-an-app-package). Verwenden Sie den Manifest-Designer, um das Paket zu konfigurieren. Fügen Sie beispielsweise Kachelbilder hinzu, und wählen Sie die von Ihrer App unterstützten Ausrichtungen aus.
3.  [Erstellen eines App-Pakets](#create-an-app-package). Verwenden Sie den Assistenten in Microsoft Visual Studio, um ein App-Paket zu erstellen und Ihr Paket mit dem Zertifizierungskit für Windows-Apps zu zertifizieren.
4.  [Querladen des App-Pakets](#sideload-your-app-package). Nach dem Querladen Ihrer App auf ein Gerät können Sie testen, ob es ordnungsgemäß funktioniert.

Nachdem Sie die vorangehenden Schritte abgeschlossen haben, können Sie Ihre App im Store zum Kauf anbieten. Eine branchenspezifische App, die Sie nicht verkaufen, sondern nur internen Benutzern zur Verfügung stellen möchten, können Sie querladen, um sie auf einem beliebigen Windows 10-Gerät zu installieren.

## Vor dem Verpacken der App

1.  Testen der App. Bevor Sie Ihre App für die Übermittlung an den Store verpacken, stellen Sie sicher, dass sie auf allen Gerätefamilien, die unterstützt werden sollen, erwartungsgemäß funktioniert. Diese Gerätefamilien umfassen Desktop-, Mobile-, Surface Hub-, XBOX-, IoT- und andere Geräte.
2.  Optimieren der App. Sie können die Profilerstellungs- und Debugtools von Visual Studio verwenden, um die Leistung Ihrer UWP-App zu optimieren. Zu diesen Tools gehören das Zeitachsentool für „Reaktionsfähigkeit der Benutzeroberfläche“, das Speichernutzungstool, das CPU-Auslastungstool und viele mehr. Weitere Informationen zu diesen Tools finden Sie unter [Ausführen von Diagnosetools ohne Debugging](https://msdn.microsoft.com/library/dn957936.aspx).
3.  Überprüfen der .NET Native-Kompatibilität (für VB- und C#-Apps). Mit UWP wurde ein neuer systemeigener Compiler eingeführt, der die Laufzeitleistung Ihrer App verbessert. Diese Änderung macht es dringend erforderlich, dass Sie Ihre App in dieser Kompilierungsumgebung testen. Standardmäßig aktiviert die **Release**-Buildkonfiguration die .NET Native-Toolkette. Daher ist es wichtig, die App und das erwartete Verhalten mit dieser **Release**-Konfiguration zu testen. Einige häufige Debugprobleme, die bei .NET Native auftreten können, werden [hier](http://blogs.msdn.com/b/visualstudioalm/archive/2015/07/29/debugging-net-native-windows-universal-apps.aspx) ausführlich erläutert.

## Konfigurieren eines App-Pakets

Die App-Manifestdatei („package.appxmanifest.xml“) verfügt über die Eigenschaften und Einstellungen, die für die Erstellung des App-Pakets erforderlich sind. Die Eigenschaften in der Manifestdatei beschreiben z. B. das Bild, das als App-Kachel verwendet wird, und die Ausrichtungen, die von der App beim Drehen des Geräts unterstützt werden.

Visual Studio verfügt über einen Manifest-Designer, mit dem Sie die Manifestdatei ohne Bearbeitung der XML-Rohdaten der Datei aktualisieren können.

Visual Studio kann Ihr Paket dem Store zuordnen. Dabei werden einige Felder der Registerkarte „Verpacken“ im Manifest-Designer automatisch aktualisiert.

**Konfigurieren eines Pakets mit dem Manifest-Designer**

1.  Erweitern Sie im **Projektmappen-Explorer** den Projektknoten Ihrer UWP-App.
2.  Doppelklicken Sie auf die Datei **Package.appxmanifest**. Wenn die Manifestdatei bereits in der XML-Codeansicht geöffnet ist, werden Sie von Visual Studio zum Schließen der Datei aufgefordert.
3.  Jetzt können Sie entscheiden, wie Sie Ihre App konfigurieren möchten. Jede Registerkarte enthält Informationen, die Sie für Ihre App konfigurieren können, sowie Links zu weiteren ggf. erforderlichen Informationen.<br/>
    ![](images/packaging-screen1.jpg)

    Überprüfen Sie auf der Registerkarte **Visuelle Anlagen**, ob Sie über alle Bilder verfügen, die für eine UWP-App erforderlich sind.

    Auf der Registerkarte **Verpacken** können Sie Veröffentlichungsdaten eingeben. An dieser Stelle können Sie auswählen, welches Zertifikat zur Signierung der App verwendet werden soll. Alle UWP-Apps müssen anhand eines Zertifikats signiert werden. Wenn Sie ein App-Paket querladen möchten, müssen Sie dem Paket vertrauen. Das Zertifikat muss auf diesem Gerät installiert sein, damit das Paket vertrauenswürdig ist. Weitere Informationen zum Querladen finden Sie unter [Aktivieren Ihres Geräts für die Entwicklung](https://msdn.microsoft.com/library/windows/apps/Dn706236).

4.  Speichern Sie Ihre Datei, nachdem Sie die erforderlichen Bearbeitungsschritte für die App vorgenommen haben.

## Erstellen eines App-Pakets

Wenn Sie eine App über den Store verteilen möchten, müssen Sie ein APPXUPLOAD-Paket erstellen. Verwenden Sie dazu den Assistenten **App-Pakete erstellen**. Führen Sie die folgenden Schritte aus, um ein Paket zu erstellen, das für die Store-Übermittlung in Microsoft Visual Studio 2015 geeignet ist.

**So erstellen Sie das App-Paket**

1.  Öffnen Sie im **Projektmappen-Explorer** die Projektmappe für Ihr UWP-App-Projekt.
2.  Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Store**->**App-Pakete erstellen** aus. Wenn diese Option deaktiviert ist oder nicht angezeigt wird, überprüfen Sie, ob es sich bei dem Projekt um ein UWP-Projekt handelt.<br/>
    ![](images/packaging-screen2.jpg)

    Der Assistent **App-Pakete erstellen** wird angezeigt.

3.  Wählen Sie im ersten Dialogfeld, in dem Sie gefragt werden, ob Sie Pakete zum Hochladen in den Windows Store erstellen möchten, „Ja“ aus, und klicken Sie auf „Weiter“.<br/>
    ![](images/packaging-screen3.jpg)

    Wenn Sie hier „Nein“ auswählen, wird das für die Store-Übermittlung benötigte APPXUPLOAD-Paket von Visual Studio nicht generiert. Sie können diese Option auswählen, wenn Sie die App lediglich für die Ausführung auf internen Geräten querladen möchten. Weitere Informationen zum Querladen finden Sie unter [Aktivieren Ihres Geräts für die Entwicklung](https://msdn.microsoft.com/library/windows/apps/Dn706236).

4.  Melden Sie sich mit Ihrem Entwicklerkonto beim Windows Dev Center an. (Wenn Sie noch kein Entwicklerkonto besitzen, hilft Ihnen der Assistent bei der Erstellung.)
5.  Wählen Sie den App-Namen für das Paket aus, oder reservieren Sie einen neuen Namen, wenn Sie noch kein Paket im Windows Dev Center-Portal reserviert haben.<br/>
    ![](images/packaging-screen4.jpg)
6.  Stellen Sie sicher, dass Sie im Dialogfeld **Auswählen und Konfigurieren von Paketen** alle drei Architekturkonfigurationen (x86, x64 und ARM) auswählen. Auf diese Weise kann Ihre App auf einer breiten Palette von Geräten bereitgestellt werden. Wählen Sie im Listenfeld **App-Bundle erstellen** die Option **Immer**. Dadurch wird die Übermittlung an den Store erheblich vereinfacht, weil nur eine Datei („.appxupload“) hochgeladen werden muss. Dieses einzelne Bündel enthält alle erforderlichen Pakete für die Bereitstellung auf Geräten mit jeder Prozessorarchitektur.<br/>
    ![](images/packaging-screen5.jpg)
7.  Es empfiehlt sich, vollständige PDB-Symboldateien einzuschließen, um die beste [Absturzanalyse](http://blogs.windows.com/buildingapps/2015/07/13/crash-analysis-in-the-unified-dev-center/) im Windows Dev Center zu erhalten. Weitere Informationen zum Debuggen mit Symbolen finden Sie unter [Debuggen mit Symbolen](https://msdn.microsoft.com/library/windows/desktop/Ee416588).
8.  Jetzt können Sie die Details für die Paketerstellung konfigurieren. Sobald Sie für die Veröffentlichung der App bereit sind, laden Sie die Pakete aus dem Ausgabepfad hoch.
9.  Klicken Sie auf **Erstellen**, um das APPXUPLOAD-Paket zu generieren.
10. Daraufhin wird dieses Dialogfeld angezeigt.<br/>
    ![](images/packaging-screen6.jpg)

    Überprüfen Sie Ihre App, bevor Sie sie zur Zertifizierung an den Store übermitteln, auf einem lokalen oder Remotecomputer. (Versionsbuilds können nur für Ihr App-Paket, nicht aber für Debugbuilds überprüft werden.)

11. Für eine lokale Überprüfung lassen Sie die Option **Lokaler Computer** aktiviert und klicken auf **Zertifizierungskit für Windows-Apps starten**. Weitere Informationen zum Testen der App mit dem Zertifizierungskit für Windows-Apps finden Sie unter [Zertifizierungskit für Windows-Apps](https://msdn.microsoft.com/library/windows/apps/Mt186449).

    Das Zertifizierungskit für Windows-Apps führt die Tests aus und zeigt die Ergebnisse an. Weitere Informationen finden Sie unter [Tests im Zertifizierungskit für Windows-Apps](https://msdn.microsoft.com/library/windows/apps/mt186450).

    Wenn Sie über ein Windows 10-Remotegerät verfügen, das Sie zum Testen verwenden möchten, müssen Sie das Zertifizierungskit für Windows-Apps manuell auf dem Gerät installieren. Im nächsten Abschnitt werden die erforderlichen Schritte beschrieben. Nachdem Sie damit fertig sind, wählen Sie **Remotecomputer** und klicken auf **Zertifizierungskit für Windows-Apps starten**, um eine Verbindung zum Remotegerät herzustellen und die Überprüfungen ausführen.

12. Nachdem das WACK abgeschlossen ist und die App erfolgreich getestet wurde, können Sie sie in den Store hochladen. Stellen Sie sicher, dass Sie die richtige Datei hochladen. Sie befindet sich im Stammordner der Projektmappe „\\\[AppName\]\\AppPackages“ und endet mit der Dateierweiterung „.appxupload“. Der Name hat das Format „\[AppName\]\_\[AppVersion\]\_x86\_x64\_arm\_bundle.appxupload“.

**Überprüfen des App-Pakets auf einem Windows 10-Remotegerät**

1.  Aktivieren Sie das Windows 10-Gerät für die Entwicklung, indem Sie die Anweisungen unter [Aktivieren Ihres Geräts für die Entwicklung](https://msdn.microsoft.com/library/windows/apps/Dn706236) befolgen.
    **Wichtig**  Sie können das App-Paket nicht auf einem ARM-Remotegerät für Windows 10 überprüfen.
2.  Laden Sie die Remotetools für Visual Studio herunter, und installieren Sie sie. Diese Tools werden verwendet, um das Zertifizierungskit für Windows-Apps remote auszuführen. Weitere Informationen zu diesen Tools einschließlich der Downloadseite finden Sie unter [Ausführen von Windows Store-Apps auf einem Remotecomputer](https://msdn.microsoft.com/library/hh441469.aspx#BKMK_Starting_the_Remote_Debugger_Monitor).
3.  Laden Sie das erforderliche [Zertifizierungskit für Windows-Apps](http://go.microsoft.com/fwlink/p/?LinkID=309666) herunter, und installieren Sie es auf Ihrem Windows 10-Remotegerät.
4.  Aktivieren Sie auf der Seite **Paketerstellung abgeschlossen** des Assistenten das Optionsfeld **Remotecomputer**. Klicken Sie anschließend neben der Schaltfläche **Testverbindung** auf die Schaltfläche mit den Auslassungszeichen.
    **Hinweis**  Das Optionsfeld **Remotecomputer** ist nur verfügbar, wenn Sie mindestens eine Projektmappenkonfiguration ausgewählt haben, die die Überprüfung unterstützt. Weitere Informationen zum Testen der App mit dem WACK finden Sie unter [Zertifizierungskit für Windows-Apps](https://msdn.microsoft.com/library/windows/apps/Mt186449).
5.  Geben Sie ein Gerät vom Subnetz aus an, oder geben Sie den DNS-Namen (Domain Name Server) oder die IP-Adresse eines Geräts an, das sich außerhalb des Subnetzes befindet.
6.  Wählen Sie in der Liste **Authentifizierungsmodus** die Option **Keiner** aus, wenn Ihr Gerät keine Anmeldung mittels Windows-Anmeldeinformationen erfordert.
7.  Klicken Sie auf die Schaltfläche **Auswählen** und anschließend auf die Schaltfläche **Zertifizierungskit für Windows-Apps starten**. Wenn die Remotetools auf diesem Gerät ausgeführt werden, stellt Visual Studio eine Verbindung her und führt die Überprüfungstests aus. Weitere Informationen finden Sie unter [Tests im Zertifizierungskit für Windows-Apps](https://msdn.microsoft.com/library/windows/apps/mt186450).

## Querladen des App-Pakets

Bei UWP-App-Paketen können Sie Apps nicht wie eine Desktop-App auf dem Gerät installieren. In der Regel laden Sie diese Apps aus dem Store herunter und installieren sie auf diese Weise auf dem Gerät. Sie können Apps jedoch auch auf das Gerät querladen, ohne sie an den Store zu übermitteln. Bei dieser Vorgehensweise können Sie die App nach der Installation mit dem erstellten App-Paket („.appx“) testen. Wenn Sie über eine App verfügen, die Sie nicht im Store anbieten möchten (z. B. eine branchenspezifische App), können Sie diese App querladen, um sie für Kollegen im Unternehmen bereitzustellen.

Die folgende Liste enthält die Anforderungen für das Querladen von Apps.

-   Sie müssen [Ihr Gerät für die Entwicklung aktivieren](https://msdn.microsoft.com/library/windows/apps/Dn706236).
-   Wenn Sie Ihre App auf ein Windows 10 Mobile-Gerät querladen möchten, verwenden Sie das Tool [WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md).

**Querladen einer App auf einen Desktop, Laptop oder Tablet**

1.  Kopieren Sie die Ordner der zu installierenden Version auf das Zielgerät.

    Wenn Sie ein App-Bündel erstellt haben, verfügen Sie über einen Ordnernamen bestehend aus Versionsnummer und dem Zusatz „\_test“. Zum Beispiel die beiden folgenden Ordner (wobei die Version 1.0.2 installiert wird):

    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0
    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_Test

    Wenn Sie über kein App-Bündel verfügen, können Sie den Ordner einfach kopieren, um die richtige Architektur und den entsprechenden Testordner zu übernehmen. Zum Beispiel die beiden folgenden Ordner.

    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64
    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64\_Test
2.  Öffnen Sie den Testordner auf dem Zielgerät. Beispielsweise: C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_Test
3.  Klicken Sie mit der rechten Maustaste auf die Datei **Add-AppDevPackage.ps1**, wählen Sie **Mit PowerShell ausführen**, und befolgen Sie die Eingabeaufforderungen.<br/>
    ![](images/packaging-screen7.jpg)

    Nachdem das App-Paket installiert wurde, sehen Sie die folgende Meldung im PowerShell-Fenster: Ihre App wurde erfolgreich installiert.

    **Hinweis**  Wenn Sie das Kontextmenü auf einem Tablet öffnen möchten, berühren Sie den Bildschirm an der Stelle, an der Sie mit der rechten Maustaste klicken möchten. Drücken Sie so lange, bis ein vollständiger Kreis angezeigt wird, und lassen Sie dann wieder los. Das Kontextmenü wird angezeigt, sobald Sie loslassen.
4.  Klicken Sie auf die Schaltfläche „Start“, und geben Sie den Namen der App ein, um sie zu starten.

 

 







<!--HONumber=Jun16_HO4-->


