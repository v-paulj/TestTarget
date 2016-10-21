---
author: Mtoepke
title: Erste Schritte bei der Entwicklung von UWP-Apps auf Xbox One
description: "Einrichten des Computers und der Xbox One für die UWP-Entwicklung"
translationtype: Human Translation
ms.sourcegitcommit: d4ef0da606c98c5eb024f349720fe9641e4a541e
ms.openlocfilehash: 33b8369be9cc0fd54ee044ec2aa6ea38b352c908

---

#Erste Schritte bei der Entwicklung von UWP-Apps auf Xbox One

Führen Sie die folgenden Schritte **sorgfältig** aus, um Ihren PC und Xbox One erfolgreich für die Entwicklung für die Universelle Windows-Plattform einzurichten. Lesen Sie nach der Einrichtung die Seite [UWP für Xbox One](index.md), um mehr über den Entwicklermodus auf Xbox One und das Erstellen von UWP-Apps zu erfahren. 

## Vorbereitung
Bevor Sie beginnen, müssen Sie die folgenden Schritte ausführen:
-   Richten Sie einen PC mit Windows10 ein.
-   Installieren Sie Microsoft Visual Studio 2015 Update 3.
- Sorgen Sie für mindestens 5GB freien Speicherplatz auf Ihrer Xbox One.

## Einrichten des Entwicklungs-PCs
1.  Installieren Sie Visual Studio 2015 Update. Wählen Sie die **benutzerdefinierte** Installation aus, und aktivieren Sie das Kontrollkästchen **Entwicklungstools für universelle Windows-Apps** (dieses gehört nicht zur standardmäßigen Installation). Achten Sie als C++-Entwickler darauf, **Benutzerdefinierte Installation** auszuwählen. Wählen Sie **C++** aus. Weitere Informationen finden Sie unter [Einrichtung der Entwicklungsumgebung](development-environment-setup.md). 

2.  Installieren Sie das aktuelle Windows10 SDK. Dieses können Sie unter [https://developer.microsoft.com/de-de/windows/downloads/windows-10-sdk](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk) abrufen.

3.  Aktivieren Sie den Entwicklermodus für Ihren Entwicklungs-PC (Einstellungen > Update und Sicherheit > Für Entwickler > Entwicklermodus).

## Einrichten Ihrer Xbox One-Konsole
1.  Aktivieren Sie den Entwicklermodus auf der Xbox One. Laden Sie die App herunter, und geben Sie dann den erhaltenen Aktivierungscode im Dev Center-Konto auf der xboxactivate-Seite ein. Weitere Informationen finden Sie unter [Aktivieren des Entwicklermodus auf Xbox One](devkit-activation.md). 

2.  Öffnen Sie die DevMode-Aktivierungsapp, und wählen Sie **Wechseln und neu starten** aus. Herzlichen Glückwunsch! Ihre Xbox One befindet sich nun im Entwicklermodus.
  
  > [!NOTE]
  > Ihre Einzelhandelsspiele und -Apps werden im Entwicklermodus nicht ausgeführt, die von Ihnen erstellten Apps oder Spiele werden jedoch ausgeführt. Wechseln Sie zurück in den Einzelhandelsmodus, um Ihre Lieblingsspiele und -Apps auszuführen.
    
  > [!NOTE]
  > Damit Sie eine App auf der Xbox One-Konsole im Entwicklermodus bereitstellen können, muss ein Benutzer an der Konsole angemeldet sein. Sie können entweder ein vorhandenes Xbox Live-Konto verwenden oder ein neues Konto für Ihre Konsole im Entwicklermodus erstellen. 

## Erstellen Ihres ersten Projekts in Visual Studio 2015

Ausführliche Informationen finden Sie unter [Einrichtung der Entwicklungsumgebung](development-environment-setup.md).

1.  **Für C#**: Erstellen Sie ein neues universelles Windows-Projekt, öffnen Sie die Projekteigenschaften, und wählen Sie die Registerkarte **Debuggen** aus. Ändern Sie **Zielgerät** in **Remotecomputer**, geben Sie die IP-Adresse oder den Hostnamen Ihrer Xbox One im Feld **Remotecomputer** ein, und wählen Sie die Option **Universell (unverschlüsseltes Protokoll)** in der Dropdownliste **Authentifizierungsmodus** aus.   

    Die IP-Adresse Ihrer Xbox One finden Sie, indem Sie Dev Home auf der Konsole starten (die große Kachel auf der rechten Seite der Startseite) und in der oberen linken Ecke suchen. Weitere Informationen zu Dev Home finden Sie unter [Einführung in Xbox One-Tools](introduction-to-xbox-tools.md).  

2.  **Für C++- und HTML-/Javascript-Projekte**: Sie folgen einem ähnlichen Pfad, wechseln aber in den Projekteigenschaften zur Registerkarte **Debuggen**, wählen die Option **Remotecomputer** aus, um die Dropdownliste zu öffnen, geben die IP-Adresse oder den Hostnamen der Konsole im Feld **Computername** ein, und wählen die Option **Universell (unverschlüsseltes Protokoll)** im Feld **Authentifizierungstyp** aus.
   
3.  Wenn Sie F5 drücken, wird Ihre App erstellt, und die Bereitstellung auf der Xbox One wird gestartet.
  
4.  Wenn Sie diesen Vorgang zum ersten Mal durchführen, fordert Visual Studio Sie zur Eingabe einer PIN für Ihre Xbox One auf. Eine PIN erhalten Sie, indem Sie Dev Home auf der Xbox One starten und die Schaltfläche **Mit Visual Studio koppeln** auswählen.
  
5.  Nach der Kopplung wird die Bereitstellung der App gestartet. Beim ersten Mal kann diese Bereitstellung ein wenig langsam sein (da alle Tools auf Ihre Xbox kopiert werden müssen). Wenn dieser Vorgang jedoch länger als wenige Minuten dauert, ist wahrscheinlich ein Problem aufgetreten. Stellen Sie sicher, dass Sie alle oben genannten Schritte ausgeführt haben – haben Sie den **Authentifizierungsmodus** auf **Universell** festgelegt? Vergewissern Sie sich außerdem, dass Sie eine drahtgebundene Netzwerkverbindung mit der Xbox One verwenden.  

6. Lehnen Sie sich nun ganz entspannt zurück. Viel Spaß mit Ihrer ersten App auf der Konsole!  

## Das war's.

![Hello World](images/getting-started-hello-world.png)

## Weitere Informationen  
- [FAQ](frequently-asked-questions.md)  
- [Bekannte Probleme](known-issues.md)
- [UWP auf XboxOne](index.md) 



<!--HONumber=Aug16_HO4-->


