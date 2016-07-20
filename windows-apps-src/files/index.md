---
author: TylerMSFT
ms.assetid: 1901c4c2-5161-435d-bc7b-f40c69cdb138
title: Dateien, Ordner und Bibliotheken
description: Hier erhalten Sie Informationen zum Lesen und Schreiben von App-Einstellungen, zur Datei- und Ordnerauswahl und zu speziellen Speicherorten im Sandkasten, wie z.B. Video-/Musikbibliothek.
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: ea52cafab084f202705282d63f4a37ec0d65bd03

---
 # Dateien, Ordner und Bibliotheken

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Verwenden Sie die APIs im [Windows.Storage](https://msdn.microsoft.com/library/windows/apps/br227346), [Windows.Storage.Streams](https://msdn.microsoft.com/library/windows/apps/br241791) und [Windows.Storage.Pickers](https://msdn.microsoft.com/library/windows/apps/br207928)-Namespace, um Text und andere Datenformate in Dateien zu lesen und zu schreiben sowie Dateien und Ordner zu verwalten. In diesem Abschnitt erhalten Sie Informationen zum Lesen und Schreiben von App-Einstellungen, zur Datei- und Ordnerauswahl und zu speziellen Speicherorten im Sandkasten, z.B. die Video-/Musikbibliothek.

| Thema | Beschreibung  |
|-------|--------------|
| [Aufzählen und Abfragen von Dateien und Ordnern](quickstart-listing-files-and-folders.md) | Greifen Sie auf Dateien und Ordner zu, die sich in einem Ordner, in einer Bibliothek, auf einem Gerät oder an einer Netzwerkadresse befinden. Sie können auch durch Erstellen von Datei- und Ordnerabfragen Dateien und Ordner an bestimmten Speicherorten abrufen. |
| [Erstellen, Schreiben und Lesen einer Datei](quickstart-reading-and-writing-files.md) | Lesen und Schreiben Sie eine Datei mithilfe eines [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171)-Objekts. |
| [Abrufen von Dateieigenschaften](quickstart-getting-file-properties.md) | Es werden Eigenschaften – oberste Ebene, grundlegend und erweitert – für eine Datei abgerufen, die durch ein [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171)-Objekt dargestellt wird. |
| [Öffnen von Dateien und Ordnern mit einer Auswahl](quickstart-using-file-and-folder-pickers.md) | Greifen Sie auf Dateien und Ordner zu, indem Sie Benutzern die Interaktion mit einer Auswahl ermöglichen. Sie können über [FolderPicker](https://msdn.microsoft.com/library/windows/apps/br207881) auf einen Ordner zugreifen. |
| [Speichern einer Datei mit einer Auswahl](quickstart-save-a-file-with-a-picker.md) | Mithilfe von [FileSavePicker](https://msdn.microsoft.com/library/windows/apps/br207871) können Benutzer den Namen und Speicherort zum Speichern einer Datei durch die App angeben. |
| [Schützen von Dateien mithilfe des Unternehmensdatenschutzes (EDP)](protect-your-enterprise-data-with-edp.md) | Dieses Thema enthält Beispiele für Programmieraufgaben, die in einigen der gängigsten, dateibezogenen EDP-Szenarien durchgeführt werden müssen. |
| [Schützen von Datenströmen und Puffern mithilfe des Unternehmensdatenschutzes (Enterprise Data Protection, EDP)](use-edp-to-protect-streams-and-buffers.md) | Dieses Thema enthält Beispiele für Programmieraufgaben, die in einigen der gängigsten EDP-Szenarien im Zusammenhang mit Datenströmen und Puffern durchgeführt werden müssen. |
| [Zugreifen auf Inhalte in der Heimnetzgruppe](quickstart-accessing-homegroup-content.md) | Greifen Sie auf Inhalte zu, die sich im Heimnetzgruppenordner des Benutzers befinden, einschließlich Bildern, Musik und Videos. |
| [Ermitteln der Verfügbarkeit von Microsoft OneDrive-Dateien](quickstart-determining-availability-of-microsoft-onedrive-files.md) | Ermitteln Sie mithilfe der [StorageFile.IsAvailable](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx)-Eigenschaft, ob eine Microsoft OneDrive-Datei verfügbar ist. |
| [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md) | Fügen Sie vorhandene Musik-, Bilder- oder Video-Ordner den entsprechenden Bibliotheken hinzu. Sie können auch Ordner aus Bibliotheken entfernen, die Liste der Ordner in einer Bibliothek abrufen und gespeicherte Fotos, Musik und Videos untersuchen. |
| [Nachverfolgen kürzlich verwendeter Dateien und Ordner](how-to-track-recently-used-files-and-folders.md) | Sie können Dateien nachverfolgen, auf die häufig zugegriffen wird, indem Sie sie der Liste mit den zuletzt verwendeten Elementen (MRU) der App hinzufügen. Die Plattform verwaltet die MRU für Sie. Dabei werden Elemente nach der Zeit und dem Ort des letzten Zugriffs sortiert und die ältesten Elemente entfernt, wenn das Limit von 25Elementen erreicht ist. Alle Apps besitzen eine eigene MRU. |
| [Zugreifen auf die SD-Karte](access-the-sd-card.md) | Sie können weniger wichtige Daten auf einer optionalen microSD-Karte speichern und auf diese zugreifen. Dies gilt besonders für kostengünstige Geräte, die nur über einen begrenzten internen Speicher verfügen. |
| [Berechtigungen für den Dateizugriff](file-access-permissions.md) | Apps können standardmäßig auf bestimmte Dateisystemspeicherorte zugreifen. Apps können darüber hinaus mithilfe der Dateiauswahl oder über die Deklaration von Funktionen auf weitere Speicherorte zugreifen. |

## Verwandte Beispiele
[Beispiel für Ordnerenumeration](http://go.microsoft.com/fwlink/p/?linkid=619993)

[Beispiel zum Dateizugriff](http://go.microsoft.com/fwlink/p/?linkid=619995)

[Beispiel zur Dateiauswahl](http://go.microsoft.com/fwlink/p/?linkid=619994)
 

 







<!--HONumber=Jul16_HO2-->


