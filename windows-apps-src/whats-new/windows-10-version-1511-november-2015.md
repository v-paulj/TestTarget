---
Description: Windows 10, Version 1511, und Updates für Entwicklertools stellen weiterhin Tools, Features und Umgebungen bereit, die von der universellen Windows-Plattform unterstützt werden.
title: Neuigkeiten für Entwickler in Windows 10, Version 1511: November 2015
---

# Neuigkeiten für Entwickler in Windows 10, Version 1511: November 2015

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Windows 10, Version 1511, und Updates für Entwicklertools stellen weiterhin Tools, Features und Umgebungen bereit, die von der universellen Windows-Plattform unterstützt werden. Nach der [Installation der Tools und des SDKs](https://dev.windows.com/downloads) unter Windows 10, Version 1511, können Sie entweder [eine neue universelle Windows-App erstellen](https://msdn.microsoft.com/library/windows/apps/bg124288) oder lesen, wie Sie Ihren vorhandenen [App-Code unter Windows verwenden](https://msdn.microsoft.com/library/windows/apps/mt238321) können.

## Benutzerfreundlichkeit

Die neue <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.aspx">Windows.UI.StartScreen.JumpList</a>-Klasse und <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.aspx">Windows.UI.StartScreen.JumpListItem</a>-Klasse bieten Apps die Möglichkeit, den Typ der gewünschten systemverwalteten Sprungliste programmgesteuert auszuwählen und der Sprungliste Einstiegspunkte für benutzerdefinierte Aufgaben sowie benutzerdefinierte Gruppen hinzuzufügen.

## Eingabe
                                        
* <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.input.keyboarddeliveryinterceptor.aspx">Interceptor für die Tastaturbereitstellung</a>
                                        
    Ermöglicht es einer App, die Systemverarbeitung der Rohdateneingabe außer Kraft zu setzen – z. B. Tastaturkürzel, Zugriffstasten (oder Abkürzungstasten), Tastenkombinationen und Anwendungstasten. Ausgenommen sind SAS-Tastenkombinationen (Secure Attention Sequence, SAS).

    SAS-Tastenkombinationen, einschließlich Strg+Alt+Entf und Windows-L, werden weiterhin vom System verarbeitet.
                                        
* Prozessübergreifendes Verketten von Zeigereingaben für <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.core.corewindow.aspx">UWP-Apps</a> und <a href="https://msdn.microsoft.com/library/windows/desktop/hh454903(v=vs.85).aspx">klassische Windows-Apps</a>.
                                        
    Neue Zeigerereignisse, die prozessübergreifendes Verketten von Eingaben aktivieren.    
                                        
* <a href="https://msdn.microsoft.com/library/windows/desktop/mt622165(v=vs.85).aspx">InkPresenter für klassische Desktop-Apps</a>
                                        
    Die InkPresenter-APIs ermöglichen Microsoft Win32-Apps die Verwaltung von Eingabe, Verarbeitung und Rendering der Freihandeingabe (Standard und verändert) über ein <a href="https://msdn.microsoft.com/library/windows/desktop/windows.ui.input.inking.inkpresenter.aspx">InkPresenter</a>-Objekt in der visuellen <a href="https://msdn.microsoft.com/library/windows/desktop/hh437371(v=vs.85).aspx">DirectComposition</a>-Struktur der App.    
                                    
## Netzwerke
                                                                        
Für Benutzer von WebSockets: <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">MessageWebSocket.OutputStream.FlushAsync</a> und <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">StreamWebSocket.OutputStream.FlushAsync</a> wurden vollständig implementiert und warten darauf, zuvor ausgestellte WriteAsync-Aufrufe abzuschließen. Beachten Sie, dass dadurch in vorhandenem Code möglicherweise eine Ausnahme ausgelöst wird, wenn das WebSocket beim Aufrufen von <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">FlushAsync</a> einen ungültigen Zustand aufweist.    

Die neue Eigenschaft <a href="https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx">CookieUsageBehavior</a> wurde der vorhandenen <a href="https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx">Windows.Web.Http.Filters.HttpBaseProtocolFilter -Klasse</a> hinzugefügt. Dadurch können Entwickler bestimmen, wie Cookies vom System behandelt werden.    
                                    
## ORTC
                                    
Microsoft Edge ermöglicht durch <a href="https://msdn.microsoft.com/library/mt433097(v=vs.85).aspx">ORTC (Object Real-Time Communications)</a> über systemeigene Javascript-APIs das direkte Echtzeit-Audio/Videoanrufe zwischen Webbrowsern, Mobilgeräten und Servern. Entwickler können jetzt erweiterte Echtzeit-Anwendungen für die Audio-/Videokommunikation im Microsoft Edge-Browser entwickeln, indem sie die ORTC-API mit Unterstützung für Gruppenvideoanrufe, Simulcast, skalierbare Videocodierung (SVC) und vieles mehr verwenden.    

Eine Demo eines 1:1-Audio-/Videoanrufs über die ORTC-API zwischen Microsoft Edge-Browsern finden Sie auf den <a href="/microsoft-edge/testdrive/demos/ortcdemo/">Websites mit Testversionen und Demos</a>. Eine Übersicht und eine exemplarische Vorgehensweise für Codebeispiele finden Sie im <a href="https://msdn.microsoft.com/library/mt588497(v=vs.85).aspx">ORTC-Entwicklerhandbuch</a>.
                                        
## Microsoft Edge F12-Entwicklungstools
                                                                        
Microsoft Edge bietet großartige neue Verbesserungen für F12-Entwicklungstools, einschließlich einiger der am häufigsten gewünschten Features aus <a href="https://wpdev.uservoice.com/forums/257854-microsoft-edge-developer">UserVoice</a>. Entdecken Sie neue Features in DOM Explorer, Konsole, Debugger, Netzwerk, Leistung, Arbeitsspeicher, Emulation und ein neues Experimente-Tool, mit dem Sie leistungsstarke neue Features vorab testen können. Die neuen Tools sind in TypeScript integriert und werden immer ausgeführt, sodass kein Neuladen erforderlich ist. Darüber hinaus ist die F12-Tools Entwicklerdokumentation nun Bestandteil der <a href="http://dev.modern.ie/">Microsoft Edge Dev Website</a> und vollständig auf <a href="https://github.com/MicrosoftEdge/MicrosoftEdge-Documentation">GitHub</a> verfügbar. Von nun an werden die Dokumente nicht nur durch Ihr Feedback beeinflusst, sondern Sie können dazu beitragen, unsere Dokumentation weiterzuentwickeln. Eine kurze Videoeinführung in die F12-Entwicklungstools finden Sie in <a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/Microsoft-Edge-F12-tools">Channel9s One Dev Minute</a>.    
                                    
## Windows Hello
                                    
Windows Hello bietet Ihrer App die Möglichkeit, die Gesichtserkennung oder Fingerabdruckerkennung für die Anmeldung bei einem Windows-System oder Gerät zu aktivieren.

Die Anbieter-APIs ermöglichen IHVs und OEMs die Verwendung von Tiefen-, Infrarot- und Farbkameras (und zugehörigen Metadaten) für Einblicke in UWP und die Auswahl einer Kamera für die Windows Hello-Gesichtserkennung. Der <a href="http://go.microsoft.com/fwlink/?LinkId=691697">Windows.Devices.Perception</a>-Namespace enthält die Client-APIs, die einer UWP-Anwendung den Zugriff auf die Farb-, Tiefe- oder Infrarotdaten von Computerkameras ermöglichen.
                                    
## Neue Spiele-API

Verwenden Sie die neue Windows.Gaming.UI.GameBar-Klasse, um Benachrichtigungen zu erhalten, wenn die Spieleleiste ein- oder ausgeblendet wird.    
                            
                                    
## Bluetooth-APIs
                                    
Verschiedene APIs wurden hinzugefügt und aktualisiert, um die Unterstützung für Bluetooth LE-Geräteenumeration und andere Bluetooth-Features zu erweitern. Siehe <a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.aspx">Windows.Devices.Bluetooth</a>-Namespace.    
                                   
## Smartcard-APIs ## 

Verschiedene SmartCardCryptogram-APIs wurden dem <a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.aspx">Windows.Devices.SmartCards</a>-Namespace hinzugefügt, um die sichere Verschlüsselung von Zahlungsprotokollen zu unterstützen. Zahlungs-Apps mit hostbasierter Kartenemulation für „Zum Zahlen Berühren“ können diese APIs für zusätzliche Sicherheit und Leistung verwenden. Apps können einen Schlüssel erstellen und eingeschränkte Transaktionsschlüssel durch TPM schützen. Apps können auch das NGC-Framework (Next Generation Credentials) nutzen, um die Tasten mit der PIN des Benutzers zu schützen. Diese APIs delegieren die Kryptogrammerstellung an das System, um die Leistung zu verbessern. Dies verhindert auch den Zugriff auf die Schlüssel und Kryptogramme von anderen Apps.    
                                    
## Aktualisierte Speicher-APIs ## 
    
<a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.aspx">Windows.Storage.DownloadsFolder-Klasse</a><br />
Ihre App kann nun <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.createfileforuserasync.aspx">eine Datei</a> oder <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.createfolderforuserasync.aspx">einen Ordner</a> im Downloads-Order für einen bestimmten <a href="https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx">Benutzer</a> erstellen.
                                            
<a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.storagelibrary.aspx">Windows.Storage.StorageLibrary-Klasse</a><br />
Ihre App kann nun <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.storagelibrary.getlibraryforuserasync.aspx">eine angegebene Bibliothek</a> für einen bestimmten <a href="https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx">Benutzer</a> abrufen.
                                    
## Zertifizierungskit für Windows-Apps ## 
                                    
Das Zertifizierungskits für Windows-Apps wurde mit verbesserten Tests aktualisiert. Eine vollständige Liste der Updates finden Sie auf der Website zum <a href="/develop/app-certification-kit">Zertifizierungskit für Windows-Apps</a>.    
                                    
## Entwurfsvorlagen zum Herunterladen ## 

Entdecken Sie unsere neuen UWP-App-Entwurfsvorlagen für Adobe Photoshop. Zudem haben wir unsere Microsoft PowerPoint- und Adobe Illustrator-Vorlagen aktualisiert und eine PDF-Version unserer Richtlinien zur Verfügung gestellt. <a href="/design/assets">Besuchen Sie die Seite zum Herunterladen von Entwurfsvorlagen</a>.    




<!--HONumber=Mar16_HO5-->


