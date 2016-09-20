---
author: drewbatgit
ms.assetid: 
description: "In diesem Artikel wird beschrieben, wie Medien von einem Gerät importiert werden, darunter die Suche nach verfügbaren Medienquellen, das Importieren von Dateien wie Fotos und Sidecardateien und das Löschen der importierten Dateien vom Quellgerät."
title: Importieren von Medien
translationtype: Human Translation
ms.sourcegitcommit: 599e7dd52145d695247b12427c1ebdddbfc4ffe1
ms.openlocfilehash: b1b8467e181d1a04317a8a0b8688ffb02621730c

---

# Importieren von Medien von einem Gerät

In diesem Artikel wird beschrieben, wie Medien von einem Gerät importiert werden, darunter die Suche nach verfügbaren Medienquellen, das Importieren von Dateien wie Videos, Fotos und Sidecardateien und das Löschen der importierten Dateien vom Quellgerät.

> [!NOTE] 
> Der Code in diesem Artikel wurde aus dem [**MediaImport UWP-App-Beispiel**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport) übernommen und angepasst. Sie können dieses Beispiel klonen oder aus dem [**Git-Repository für universelle Windows-App-Beispiele**](https://github.com/Microsoft/Windows-universal-samples) herunterladen, um den Code im Kontext anzuzeigen oder ihn als Ausgangspunkt für Ihre eigene App zu verwenden.

## Erstellen einer einfachen Medienimport-Benutzeroberfläche
Für das Beispiel in diesem Artikel wird eine minimale Benutzeroberfläche verwendet, um die grundlegenden Medienimportszenarien zu ermöglichen. Informationen zum Erstellen einer stabileren Benutzeroberfläche für eine Medienimport-App finden Sie im [**MediaImport-Beispiel**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport). Der folgende XAML-Code erstellt ein StackPanel-Element mit den folgenden Steuerelementen:
* Ein [**Button**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Button)-Steuerelement zum Initiieren der Suche nach Quellen, aus denen Medien importiert werden können.
* Ein [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox)-Steuerelement zum Auflisten und Auswählen der Medienimportquellen, die gefunden werden.
* Ein [**ListView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ListView)-Steuerelement zum Anzeigen und Auswählen der Medienelemente aus der ausgewählten Importquelle.
* Ein **Button**-Steuerelement zum Initiieren des Imports von Medienelementen aus der ausgewählten Quelle.
* Ein **Button**-Steuerelement zum Initiieren des Löschens der Elemente, die aus der ausgewählten Quelle importiert wurden.
* Ein **Button**-Steuerelement zum Abbrechen eines asynchronen Medienimportvorgangs.

[!code-xml[ImportXAML](./code/PhotoImport_Win10/cs/MainPage.xaml#SnippetImportXAML)]

## Einrichten der CodeBehind-Datei
Fügen Sie *using*-Direktiven hinzu, um die in diesem Beispiel verwendeten Namespaces einzubeziehen, die noch nicht in der Standardprojektvorlage enthalten sind.

[!code-cs[Using](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetUsing)]

## Einrichten des Abbruchs von Aufgaben für Medienimportvorgänge

Da Medienimportvorgänge lange dauern können, werden sie mit [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/en-us/library/windows/apps/br206594.aspx) asynchron ausgeführt. Deklarieren Sie eine Klassenmembervariable vom Typ [**CancellationTokenSource**](https://msdn.microsoft.com/en-us/library/system.threading.cancellationtokensource), mit der ein laufender Vorgang abgebrochen wird, wenn der Benutzer auf die Schaltfläche zum Abbrechen klickt.

[!code-cs[DeclareCts](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareCts)]

Implementieren Sie einen Handler für die Schaltfläche zum Abbrechen. In den Beispielen weiter unten in diesem Artikel wird **CancellationTokenSource** initialisiert, wenn ein Vorgang beginnt, und auf Null festgelegt, wenn der Vorgang abgeschlossen ist. Überprüfen Sie im Handler für die Schaltfläche zum Abbrechen, ob das Token Null ist, und rufen Sie andernfalls [**Cancel**](https://msdn.microsoft.com/en-us/library/dd321955) auf, um den Vorgang abzubrechen.

[!code-cs[OnCancel](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetOnCancel)]

## Datenbindungs-Hilfsklassen

In einem typischen Medienimportszenario wird dem Benutzer eine Liste der zum Import verfügbaren Medienelemente angezeigt. Es kann eine große Anzahl von Mediendateien zur Auswahl vorhanden sein, und in der Regel soll eine Miniaturansicht für jedes Medienelement angezeigt werden. Aus diesem Grund werden in diesem Beispiel drei Hilfsklassen zum inkrementellen Laden von Einträgen in das ListView-Steuerelement verwendet, wenn der Benutzer in der Liste nach unten scrollt.

* **IncrementalLoadingBase**-Klasse – Implementiert [**IList**](https://msdn.microsoft.com/en-us/library/system.collections.ilist), [**ISupportIncrementalLoading**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading) und [**INotifyCollectionChanged**](https://msdn.microsoft.com/en-us/library/windows/apps/system.collections.specialized.inotifycollectionchanged(v=vs.105).aspx), um das grundlegende inkrementelle Ladeverhalten bereitzustellen.
* **GeneratorIncrementalLoadingClass**-Klasse – Stellt die Implementierung der Basisklasse für das inkrementelle Laden bereit.
* **ImportableItemWrapper**-Klasse – Ein dünner Wrapper um die [**PhotoImportItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem)-Klasse zum Hinzufügen einer bindbaren [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.BitmapImage)-Eigenschaft für das Miniaturbild für jedes importierte Element.

Diese Klassen werden im [**MediaImport-Beispiel**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport) bereitgestellt und können dem Projekt ohne Änderungen hinzugefügt werden. Deklarieren Sie nach dem Hinzufügen der Hilfsklassen zum Projekt eine Klassenmembervariable vom Typ **GeneratorIncrementalLoadingClass**, die weiter unten in diesem Beispiel verwendet wird.

[!code-cs[GeneratorIncrementalLoadingClass](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetGeneratorIncrementalLoadingClass)]


# Suchen nach verfügbaren Quellen, aus denen Medien importiert werden können

Rufen Sie im Klickhandler für die Schaltfläche zur Suche nach Quellen die statische Methode [**PhotoImportManager.FindAllSourcesAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportManager.FindAllSourcesAsync) auf, um die Systemsuche nach Geräten, von denen Medien importiert werden können, zu starten. Durchlaufen Sie nach dem Warten auf den Abschluss des Vorgangs jedes [**PhotoImportSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource)-Objekt in der zurückgegebenen Liste, fügen Sie einen Eintrag zu **ComboBox** hinzu, und legen Sie die **Tag**-Eigenschaft direkt auf das Quellobjekt fest, damit es einfach abgerufen werden kann, wenn der Benutzer eine Auswahl trifft.

[!code-cs[FindSourcesClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindSourcesClick)]

Deklarieren Sie eine Klassenmembervariable zum Speichern der ausgewählten Importquelle des Benutzers.

[!code-cs[DeclareImportSource](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportSource)]

Legen Sie im [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged)-Handler für **ComboBox** für die Importquelle die Klassenmembervariable auf die ausgewählte Quelle fest, und rufen Sie dann die **FindItems**-Hilfsmethode auf, die weiter unten in diesem Artikel erläutert wird. 

[!code-cs[SourcesSelectionChanged](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetSourcesSelectionChanged)]

# Suchen nach zu importierenden Elementen

Fügen Sie Klassenmembervariablen vom Typ [**PhotoImportSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSession) und [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult) hinzu, die in den folgenden Schritten verwendet werden.

[!code-cs[DeclareImport](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImport)]

Initialisieren Sie in der FindItems-Methode die **CancellationTokenSource**-Variable, sodass sie zum Abbrechen des Suchvorgangs verwendet werden kann, falls dies erforderlich ist. Erstellen Sie in einem **try**-Block eine neue Importsitzung durch Aufrufen von [**CreateImportSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource.CreateImportSession) für das vom Benutzer ausgewählte [**PhotoImportSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource)-Objekt. Erstellen Sie ein neues [**Progress**](https://msdn.microsoft.com/en-us/library/hh193692.aspx)-Objekt, um einen Rückruf zum Anzeigen des Status des Suchvorgangs bereitzustellen. Rufen Sie als Nächstes [**FindItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSession.FindItemsAsync(Windows.Media.Import.PhotoImportContentTypeFilter,Windows.Media.Import.PhotoImportItemSelectionMode) auf, um den Suchvorgang zu starten. Stellen Sie einen [**PhotoImportContentTypeFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportContentTypeFilter)-Wert bereit, um anzugeben, ob Fotos, Videos oder beides zurückgegeben werden sollen. Stellen Sie einen [**PhotoImportItemSelectionMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItemSelectionMode)-Wert bereit, um anzugeben, ob alle, keine oder nur die neuen Medienelemente zurückgegeben werden, deren [**IsSelected**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem.IsSelected)-Eigenschaft auf „true“ festgelegt ist. Diese Eigenschaft ist an ein Kontrollkästchen für jedes Medienelement in der ListBox-Elementvorlage gebunden.

**FindItemsAsync** gibt [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/en-us/library/windows/apps/br206594.aspx) zurück. Die Erweiterungsmethode [**AsTask**](https://msdn.microsoft.com/en-us/library/hh779750.aspx) wird verwendet, um eine Aufgabe zu erstellen, auf die gewartet werden kann, die mit dem Abbruchtoken abgebrochen werden kann und den Status mit dem bereitgestellten **Progress**-Objekt meldet.

Als Nächstes wird die Datenbindungs-Hilfsklasse **GeneratorIncrementalLoadingClass** initialisiert. **FindItemsAsync** gibt bei der Rückkehr aus dem Wartezustand ein [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult)-Objekt zurück. Dieses Objekt enthält Statusinformationen über den Suchvorgang, darunter den Erfolg des Vorgangs und die Anzahl der verschiedenen Typen von Medienelementen, die gefunden wurden. Die [**FoundItems**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.FoundItems)-Eigenschaft enthält eine Liste von [**PhotoImportItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem)-Objekten, die die gefundenen Medienelemente darstellen. Der **GeneratorIncrementalLoadingClass**-Konstruktor verwendet als Argumente die Gesamtzahl der Elemente, die inkrementell geladen werden, und eine Funktion, die je nach Bedarf zu ladende neue Elemente generiert. In diesem Fall erstellt der bereitgestellten Lambda-Ausdruck eine neue Instanz von **ImportableItemWrapper**, der **PhotoImportItem** umschließt und eine Miniaturansicht für jedes Element enthält. Nachdem die Klasse zum inkrementellen Laden initialisiert wurde, legen Sie diese auf die [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ItemsControl.ItemsSource)-Eigenschaft des **ListView**-Steuerelements in der UI fest. Jetzt werden die gefundenen Medienelemente inkrementell geladen und in der Liste angezeigt.

Als Nächstes werden die Statusinformationen für den Suchvorgang ausgegeben. Eine typische App zeigt diese Informationen dem Benutzer in der UI an, aber in diesem Beispiel werden die Informationen einfach in der Debugkonsole ausgegeben. Legen Sie zum Schluss das Abbruchtoken auf Null fest, da der Vorgang abgeschlossen ist.

[!code-cs[FindItems](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindItems)]

## Importieren von Medienelementen

Deklarieren Sie vor dem Implementieren des Importvorgangs ein [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult)-Objekt zur Speicherung der Ergebnisse des Importvorgangs. Dieses wird später zum Löschen von Medienelementen verwendet, die erfolgreich aus der Quelle importiert wurden.

[!code-cs[DeclareImportResult](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportResult)]

Initialisieren Sie vor dem Starten der Medienimportvorgangs die **CancellationTokenSource**-Variable, und legen Sie den Wert des [**ProgressBar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ProgressBar)-Steuerelements auf 0 fest.

Wenn keine ausgewählten Elemente im **ListView**-Steuerelement vorhanden sind, gibt es nichts zu importieren. Initialisieren Sie andernfalls ein [**Progress**](https://msdn.microsoft.com/en-us/library/hh193692.aspx)-Objekt, um einen Statusrückruf bereitzustellen, der den Wert des Steuerelements für die Statusanzeige aktualisiert. Registrieren Sie einen Handler für das [**ItemImported**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ItemImported)-Ereignis von [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult), das vom Suchvorgang zurückgegeben wird. Dieses Ereignis wird ausgelöst, wenn ein Element importiert wird, und gibt in diesem Beispiel den Namen jeder importierten Datei in der Debugkonsole aus.

Rufen Sie [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync) auf, um den Importvorgang zu starten. Genau wie bei dem Suchvorgang wird die [**AsTask**](https://msdn.microsoft.com/en-us/library/hh779750.aspx)-Erweiterungsmethode verwendet, um den zurückgegebenen Vorgang in eine Ausgabe zu konvertieren, auf die gewartet werden kann, die den Status meldet und abgebrochen werden kann.

Nach Abschluss des Importvorgangs kann der Vorgangsstatus aus dem [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult)-Objekt abgerufen werden, das von [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync) zurückgegeben wird. In diesem Beispiel werden die Statusinformationen an die Debugkonsole ausgegeben und abschließend das Abbruchtoken auf Null festlegt.

[!code-cs[ImportClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetImportClick)]

## Löschen von importierten Elementen
Um erfolgreich importierte Elemente aus der Quelle zu löschen, aus der sie importiert wurden, initialisieren Sie zuerst das Abbruchtoken so, dass der Löschvorgang abgebrochen werden kann, und legen Sie den Wert der Statusanzeige auf 0 fest. Stellen Sie sicher, dass das [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult), das von [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync) zurückgegeben wurde, nicht Null ist. Erstellen Sie andernfalls erneut ein [**Progress**](https://msdn.microsoft.com/en-us/library/hh193692.aspx)-Objekt, um einen Statusrückruf für den Löschvorgang bereitzustellen. Rufen Sie [**DeleteImportedItemsFromSourceAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult.DeleteImportedItemsFromSourceAsync) auf, um den Löschvorgang für die importierten Elemente zu starten. Verwenden Sie **AsTask**, um das Ergebnis in eine awaitable-Aufgabe mit Status- und Abbruchfunktionen zu konvertieren. Nach dem Warten kann das zurückgegebene [**PhotoImportDeleteImportedItemsFromSourceResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportDeleteImportedItemsFromSourceResult)-Objekt zum Abrufen und Anzeigen von Statusinformationen über den Löschvorgang verwendet werden.

[!code-cs[DeleteClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeleteClick)]








 





<!--HONumber=Aug16_HO3-->


