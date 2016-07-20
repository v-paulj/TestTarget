---
author: Mtoepke
title: Erste Schritte bei der Entwicklung von UWP-Apps auf Xbox One
description: "Einrichten des Computers und der Xbox One für die UWP-Entwicklung"
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 5f050eee9430dc7aaa2738a4610a2c4f5083839d
ms.openlocfilehash: 92a9cc54c6257c35b1e7ae19838b01c8452c4b36

---

#Erste Schritte bei der Entwicklung von UWP-Apps auf Xbox One


              Führen Sie die folgenden Schritte **sorgfältig** aus, um Ihren PC und Xbox One erfolgreich für die UWP-Entwicklung einzurichten. Lesen Sie nach der Einrichtung die Seite [UWP für Xbox One](index.md), um mehr über den Entwicklermodus auf Xbox One und das Erstellen von UWP-Apps zu erfahren. 

## Vorbereitung
Bevor Sie beginnen, müssen Sie die folgenden Schritte ausführen:
-   Erstellen Sie ein [Windows Dev Center](https://dev.windows.com)-Konto.
-   Nehmen Sie am [Windows-Insider-Programm](https://insider.windows.com/) teil. Dies ist erforderlich, um das Preview Windows SDK zu erhalten.
-   Richten Sie einen Windows 10-PC für diese Vorschau ein (jede Version ist geeignet, einschließlich des aktuellen Test-Flights für Windows10-Insider). Für unsere Entwicklungstools muss Windows10 ausgeführt werden. 
-   Verbinden Sie Ihre Xbox One-Konsole mit einem Netzwerk. Verwenden Sie für eine optimale Leistung eine Kabelverbindung.
- Sorgen Sie für mindestens 5GB freien Speicherplatz auf Ihrer Xbox One-Konsole.

## Einrichten des Entwicklungs-PCs
1.  Installieren Sie Visual Studio 2015 Update 2. Wählen Sie die **benutzerdefinierte** Installation aus, und aktivieren Sie das Kontrollkästchen **Entwicklungstools für universelle Windows-Apps** (dieses gehört nicht zur standardmäßigen Installation). Weitere Informationen finden Sie unter [Einrichtung der Entwicklungsumgebung](development-environment-setup.md) (als C++-Entwickler sollten Sie außerdem darauf achten, die benutzerdefinierte Installation sowie C++ auszuwählen).

2.  Installieren Sie das aktuelle Windows 10 SDK Preview Build. Diese Version erhalten Sie über das [Windows-Insider-Programm](http://go.microsoft.com/fwlink/p/?LinkId=780552).
  
  > 
              **Wichtig**&nbsp;&nbsp;Nach der Installation dieses Preview SDK auf Ihrem PC können Sie Apps, die auf diesem PC erstellt werden, nicht mehr an den Store übermitteln. Daher sollten Sie diesen Schritt nicht auf Ihrem Entwicklungscomputer für die Produktion ausführen. 

## Einrichten Ihrer Xbox One-Konsole
1.  Aktivieren Sie den Entwicklermodus auf der Xbox One. Laden Sie die App herunter, und geben Sie den erhaltenen Aktivierungscode im Dev Center-Konto auf der xboxactivate-Seite ein. Weitere Informationen finden Sie unter [Aktivieren des Entwicklermodus auf Xbox One](devkit-activation.md). 

2.  Warten Sie, bis die Xbox One ein Systemupdate durchführt, sodass die Developer Preview ausgeführt wird. Dies kann bis zu vier Stunden dauern. Wenn Sie nicht warten möchten, führen Sie einen „harten“ Neustart der Konsole durch, indem Sie den Netzschalter 10 Sekunden gedrückt halten und dann wieder einschalten. Dadurch wird das Update ausgelöst.  

3.  Öffnen Sie die DevMode-Aktivierungsapp, und wählen Sie **Switch and restart** aus. Herzlichen Glückwunsch! Ihre Xbox One befindet sich nun im Entwicklermodus.
  
  > 
              **Hinweis**&nbsp;&nbsp;Ihre Einzelhandelsspiele und -Apps werden im Entwicklermodus nicht ausgeführt, die von Ihnen erstellten Apps oder Spiele werden jedoch ausgeführt. Wechseln Sie zurück in den Einzelhandelsmodus, um Ihre Lieblingsspiele und -Apps auszuführen.
  
  > 
              **Hinweis**&nbsp;&nbsp;Damit Sie eine App auf der Xbox One-Konsole im Entwicklermodus bereitstellen können, muss ein Benutzer an der Konsole angemeldet sein. Sie können entweder ein vorhandenes Xbox Live-Konto verwenden oder ein neues Konto für Ihre Konsole im Entwicklermodus erstellen. 

## Erstellen Ihres ersten Projekts in Visual Studio 2015

Ausführliche Informationen finden Sie unter [Einrichtung der Entwicklungsumgebung](development-environment-setup.md).

1.  Für C#: Erstellen Sie ein neues universelles Windows-Projekt, öffnen Sie die Projekteigenschaften, und wählen Sie die Registerkarte **Debuggen** aus. Ändern Sie **Zielgerät** in **Remotecomputer**, geben Sie die IP-Adresse oder den Hostnamen Ihrer Xbox One im Feld **Remotecomputer** ein, und wählen Sie die Option **Universell (unverschlüsseltes Protokoll)** in der Dropdownliste **Authentifizierungsmodus** aus.   

    Die IP-Adresse Ihrer Xbox One finden Sie, indem Sie Dev Home auf der Konsole starten (die große Kachel auf der rechten Seite der Startseite), und in der oberen linken Ecke suchen. Weitere Informationen zu Dev Home finden Sie unter [Einführung in Xbox One-Tools](introduction-to-xbox-tools.md).  

2.  Für C++- und HTML-/Javascript-Projekte: Sie folgen einem ähnlichen Pfad, wechseln aber in den Projekteigenschaften zur Registerkarte **Debuggen**, wählen die Option **Remotecomputer** aus, um die Dropdownliste zu öffnen, geben die IP-Adresse oder den Hostnamen der Konsole im Feld **Computername** ein, und wählen die Option **Universell (unverschlüsseltes Protokoll)** im Feld **Authentifizierungstyp** aus.
   
3.  Wenn Sie F5 drücken, wird Ihre App erstellt, und die Bereitstellung auf der Xbox One wird gestartet.
  
4.  Wenn Sie diesen Vorgang zum ersten Mal durchführen, fordert Visual Studio Sie zur Eingabe einer PIN für Ihre Xbox One auf. Eine PIN erhalten Sie, indem Sie Dev Home auf der Xbox One starten und auf die Schaltfläche **Pair with Visual Studio** klicken.
  
5.  Nach der Kopplung wird die Bereitstellung der App gestartet. Beim ersten Mal kann diese Bereitstellung ein wenig langsam sein (da alle Tools auf Ihre Xbox kopiert werden müssen). Wenn dieser Vorgang jedoch länger als wenige Minuten dauert, ist wahrscheinlich ein Problem aufgetreten. Stellen Sie sicher, dass Sie alle oben genannten Schritte ausgeführt haben – haben Sie den **Authentifizierungsmodus** auf **Universell** festgelegt? Vergewissern Sie sich außerdem, dass Sie eine drahtgebundene Netzwerkverbindung mit der Xbox One verwenden.  

6. Lehnen Sie sich nun ganz entspannt zurück. Viel Spaß mit Ihrer ersten App auf der Konsole!  

## Das war's.

![Hello World](images/getting-started-hello-world.png)

## Siehe auch  
- [FAQ](frequently-asked-questions.md)  
- [Bekannte Probleme](known-issues.md)
- [UWP auf XboxOne](index.md)



<!--HONumber=Jul16_HO2-->


