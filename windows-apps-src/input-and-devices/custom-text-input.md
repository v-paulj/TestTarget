---
author: Karl-Bridge-Microsoft
Description: "Mit den Core-Text-APIs im Windows.UI.Text.Core-Namespace kann eine UWP-App (Universelle Windows-Plattform) Texteingaben von beliebigen Textdiensten empfangen, die auf Windows-Geräten unterstützt werden."
title: "Übersicht über benutzerdefinierte Texteingabe"
ms.assetid: 58F5F7AC-6A4B-45FC-8C2A-942730FD7B74
label: Custom text input
template: detail.hbs
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: 31f10b862ba53f2ba51f3936a73e874466590b30

---

# Benutzerdefinierte Texteingabe

Mit den Core-Text-APIs im [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238)-Namespace kann eine UWP-App (Universelle Windows-Plattform) Texteingaben von beliebigen Textdiensten empfangen, die auf Windows-Geräten unterstützt werden. Die APIs sind den [Textdienstframework](https://msdn.microsoft.com/library/windows/desktop/ms629032)-APIs dahingehend ähnlich, dass die App keine detaillierten Kenntnisse über die Textdienste benötigt. Auf diese Weise kann die App Text in einer beliebigen Sprache und von einem beliebigen Eingabegerät empfangen, wie Tastatur, Sprache oder Stift.


**Wichtige APIs**

-   [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238)
-   [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158)


## Gründe für die Verwendung von Core-Text-APIs


Für zahlreiche Apps sind die XAML- oder HTML-Textfeld-Steuerelemente für Texteingabe und Textbearbeitung ausreichend. Wenn Ihre App jedoch komplexe Textszenarien behandelt, beispielsweise eine Textverarbeitungs-App ist, benötigen Sie vielleicht die Flexibilität eines benutzerdefinierten Textbearbeitungssteuerelements. Sie könnten die [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Tastatur APIs verwenden, um das Textbearbeitungssteuerelement zu erstellen. Diese ermöglichen jedoch keinen Empfang von kompositionsbasierten Texteingaben, die für die Unterstützung ostasiatischer Sprachen erforderlich sind.

Verwenden Sie stattdessen die [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238)-APIs, wenn Sie ein benutzerdefiniertes Textbearbeitungssteuerelement erstellen müssen. Diese APIs sind so konzipiert, dass sie Ihnen ein hohes Maß an Flexibilität bei der Verarbeitung von Texteingaben in allen Sprachen bieten. Sie können daher Textverarbeitung so gestalten, wie dies für Ihre App am besten ist. Texteingabe- und Textbearbeitungssteuerelemente, die mit den Core-Text-APIs erstellt wurde, können Texteingaben von allen vorhandenen Texteingabemethoden auf Windows-Geräten empfangen, von [Textdienstframework](https://msdn.microsoft.com/library/windows/desktop/ms629032)-basierten Eingabemethoden-Editoren (IMEs) und Handschrift auf PCs bis hin zu der WordFlow-Tastatur (die AutoKorrektur, Vorhersage und Diktat bereitstellt) auf mobilen Geräten.

## Architektur


Im Folgenden finden Sie eine einfache Darstellung des Texteingabesystems.

-   „Anwendung“ ist eine UWP-App, die ein benutzerdefiniertes Bearbeitungssteuerelement mit den Core-Text-APIs hostet.
-   Die [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238)-APIs ermöglichen die Kommunikation mit Textdiensten über Windows. Die Kommunikation zwischen dem Textbearbeitungssteuerelement und den Textdiensten erfolgt in erster Linie über ein [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158)-Objekt, das die Methoden und Ereignisse für die Kommunikation bereitstellt.

![Diagramm für die Kerntextarchitektur](images/coretext/architecture.png)

## Textbereiche und Auswahl


Bearbeitungssteuerelemente bieten Platz für die Eingabe von Text, und Benutzer erwarten, dass Text an einer beliebigen Stelle in diesem Bereich bearbeitet werden kann. Hier wird das von den Core-Text-APIs verwendete Textpositionierungssystem erläutert und wie Bereiche und Auswahlen in diesem System dargestellt werden.

### Textcursorposition der Anwendung

Textbereiche, die mit den Core-Text-APIs verwendet werden, werden mittels Textcursorpositionen ausgedrückt. Eine „Textcursorposition der Anwendung“ (Application Caret Position, ACP) ist eine nullbasierte Zahl, die die Anzahl der Zeichen vom Anfang des Textstreams bis unmittelbar vor dem Textcursor angibt, wie hier gezeigt.

![Beispieldiagramm für einen Textstream](images/coretext/stream-1.png)
### Textbereiche und Auswahl

Textbereiche und -auswahlen werden anhand der [**CoreTextRange**](https://msdn.microsoft.com/library/windows/apps/dn958201)-Struktur dargestellt, die zwei Felder enthält:

| Feld                  | Datentyp                                                                 | Beschreibung                                                                      |
|------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **StartCaretPosition** | **Number** \[JavaScript\] | **System.Int32** \[.NET\] | **int32** \[C++\] | Die Startposition eines Bereichs ist die Textcursorposition der Anwendung unmittelbar vor dem ersten Zeichen. |
| **EndCaretPosition**   | **Number** \[JavaScript\] | **System.Int32** \[.NET\] | **int32** \[C++\] | Die Endposition eines Bereichs ist Textcursorposition der Anwendung unmittelbar nach dem letzten Zeichen.     |

 

In dem zuvor dargestellten Textbereich gibt der Bereich \[0, 5\] beispielsweise das Wort „Hello“ an. **StartCaretPosition** muss stets kleiner oder gleich der **EndCaretPosition** sein. Der Bereich \[5, 0\] ist ungültig.

### Einfügemarke

Die aktuelle Textcursorposition, häufig als Einfügemarke bezeichnet, wird durch dargestellt, indem **StartCaretPosition** als gleich **EndCaretPosition** festgelegt wird.

### Nicht zusammenhängende Auswahl

Einige Bearbeitungssteuerelemente unterstützen nicht zusammenhängende Auswahlen. Microsoft Office-Apps unterstützen z. B. eine beliebige Mehrfachauswahl, und viele Quellcode-Editoren unterstützen die Spaltenauswahl. Von Core-Text-APIs wird jedoch eine nicht zusammenhängende Auswahl nicht unterstützt. Bearbeitungssteuerelemente müssen nur eine einzige zusammenhängende Auswahl melden; meistens ist dies der aktive Unterbereich der nicht zusammenhängenden Auswahlen.

Betrachten Sie beispielsweise den folgenden Textstream:

![Beispieldiagramm für einen Textstream](images/coretext/stream-2.png) Es gibt zwei Auswahlen: \[0, 1\] und \[6, 11\]. Das Bearbeitungssteuerelement muss nur eine von diesen melden: \[0, 1\] oder \[6, 11\].

## Arbeiten mit Text


Die [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158)-Klasse ermöglicht einen Textfluss zwischen Windows und Bearbeitungssteuerelementen über das [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176)-Ereignis, das [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175)-Ereignis und die [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172)-Methode.

Das Bearbeitungssteuerelement empfängt Text über die [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176)-Ereignisse, die generiert werden, wenn Benutzer mit Texteingabemethoden wie Tastaturen, Sprache oder IMEs interagieren.

Wenn Sie Text im Bearbeitungssteuerelement ändern, beispielsweise durch Einfügen von Text in das Steuerelement, müssen Sie Windows benachrichtigen, indem Sie [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) aufrufen.

Wenn der Textdienst den neuen Text erfordert, wird ein [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175)-Ereignis ausgelöst. Sie müssen den neuen Text in den **TextRequested**-Ereignishandler eingeben.

### Akzeptieren von Textupdates

Ihr Bearbeitungssteuerelement sollte in der Regel Textaktualisierungsanforderungen akzeptieren, da diese den Text darstellen, den der Benutzer eingeben möchte. Im [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176)-Ereignishandler werden die folgenden Aktionen vom Bearbeitungssteuerelement erwartet:

1.  Einfügen des Texts, der in [**CoreTextTextUpdatingEventArgs.Text**](https://msdn.microsoft.com/library/windows/apps/dn958236) an der Position angegeben wurde, die in [**CoreTextTextUpdatingEventArgs.Range**](https://msdn.microsoft.com/library/windows/apps/dn958234) angegeben wurde
2.  Platzieren der Auswahl an der in [**CoreTextTextUpdatingEventArgs.NewSelection**](https://msdn.microsoft.com/library/windows/apps/dn958233) angegebenen Position
3.  Benachrichtigen des Systems, dass das Update erfolgreich war, indem [**CoreTextTextUpdatingEventArgs.Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) auf [**CoreTextTextUpdatingResult.Succeeded**](https://msdn.microsoft.com/library/windows/apps/dn958237) festgelegt wird

Dies ist beispielsweise der Zustand eines Bearbeitungssteuerelements, bevor der Benutzer „d“ eingibt. Die Einfügemarke befindet sich bei \[10, 10\].

![Beispieldiagramm für einen Textstream](images/coretext/stream-3.png) Wenn der Benutzer „d“ eingibt, wird ein [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176)-Ereignis mit den folgenden [**CoreTextTextUpdatingEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn958229)-Daten ausgelöst:

-   [
              **Range**
            ](https://msdn.microsoft.com/library/windows/apps/dn958234) = \[10, 10\]
-   [
              **Text**
            ](https://msdn.microsoft.com/library/windows/apps/dn958236) = "d"
-   [
              **NewSelection**
            ](https://msdn.microsoft.com/library/windows/apps/dn958233) = \[11, 11\]

Wenden Sie in Ihrem Bearbeitungssteuerelement die angegebenen Änderungen an, und legen Sie [**Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) auf **Succeeded** fest. Hier sehen Sie den Zustand des Steuerelements, nachdem die Änderungen angewendet wurden.

![Beispieldiagramm für einen Textstream](images/coretext/stream-4.png)
### Ablehnen von Textupdates

Manchmal können Textaktualisierungen nicht angewendet werden, da sich der angeforderte Bereich in einem Bereich des Bearbeitungssteuerelements befindet, der nicht geändert werden darf. In diesem Fall sollten Sie keine Änderungen anwenden. Benachrichtigen Sie stattdessen das System, dass die Aktualisierung fehlgeschlagen ist, indem Sie [**CoreTextTextUpdatingEventArgs.Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) auf [**CoreTextTextUpdatingResult.Failed**](https://msdn.microsoft.com/library/windows/apps/dn958237) festlegen.

Angenommen, Sie haben ein Bearbeitungssteuerelement, das nur eine E-Mail-Adresse akzeptiert. Leerzeichen sollten zurückgewiesen werden, da E-Mail-Adressen keine Leerzeichen enthalten dürfen. Wenn daher [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176)-Ereignisse für die Leertaste ausgelöst werden, können Sie [**Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) im Bearbeitungssteuerelement einfach auf **Failed** festlegen.

### Benachrichtigen über Textänderungen

Manchmal nimmt das Bearbeitungssteuerelement Änderungen am Text vor, wenn beispielsweise Text eingefügt oder automatische korrigiert wird. In diesen Fällen müssen Sie die Textdienste über diese Änderungen benachrichtigen, indem Sie die [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172)-Methode aufrufen.

Dies ist beispielsweise der Zustand eines Bearbeitungssteuerelements, bevor der Benutzer „World“ einfügt. Die Einfügemarke befindet sich bei \[6, 6\].

![Beispieldiagramm für einen Textstream](images/coretext/stream-5.png) Der Benutzer führt die Einfügeaktion durch, und das Bearbeitungssteuerelement endet mit dem folgenden Text:

![Beispieldiagramm für einen Textstream](images/coretext/stream-4.png) In diesem Fall sollten Sie [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) mit diesen Argumenten aufrufen:

-   *modifiedRange* = \[6, 6\]
-   *newLength* = 5
-   *newSelection* = \[11, 11\]

Es folgt mindestens ein [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175)-Ereignis, das Sie zum Aktualisieren des Texts behandeln, mit dem die Textdienste arbeiten.

### Überschreiben von Textaktualisierungen

Vielleicht möchten Sie in Ihrem Bearbeitungssteuerelement eine Textaktualisierung überschreiben, um AutoKorrektur-Funktionen bereitzustellen.

Angenommen, Sie haben ein Bearbeitungssteuerelement, das eine Korrekturfunktion bereitstellt, das kontrahierte Schreibweisen formalisiert. Dies ist der Zustand des Bearbeitungssteuerelements, bevor der Benutzer die Leertaste drückt, um die Korrektur auszulösen. Die Einfügemarke befindet sich bei \[3, 3\].

![Beispieldiagramm für einen Textstream](images/coretext/stream-6.png) Der Benutzer drückt die Leertaste, und ein entsprechendes [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176)-Ereignis wird ausgelöst. Das Bearbeitungssteuerelement akzeptiert die Textaktualisierung. Dies ist der Zustand des Bearbeitungssteuerelements für einen kurzen Moment, bevor die Korrektur abgeschlossen ist. Die Einfügemarke befindet sich bei \[4, 4\].

![Beispieldiagramm für einen Textstream](images/coretext/stream-7.png) Außerhalb des [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176)-Ereignishandlers nimmt das Bearbeitungssteuerelement die folgende Korrektur vor. Dies ist der Zustand des Bearbeitungssteuerelements nach Abschluss der Korrektur. Die Einfügemarke befindet sich bei \[5, 5\].

![Beispieldiagramm für einen Textstream](images/coretext/stream-8.png) In diesem Fall sollten Sie [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) mit diesen Argumenten aufrufen:

-   *modifiedRange* = \[1, 2\]
-   *newLength* = 2
-   *newSelection* = \[5, 5\]

Es folgt mindestens ein [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175)-Ereignis, das Sie zum Aktualisieren des Texts behandeln, mit dem die Textdienste arbeiten.

### Bereitstellen von angefordertem Text

Es ist wichtig, dass Textdienste über den richtigen Text verfügen, damit Funktionen wie AutoKorrektur oder Vorhersage bereitgestellt werden können, insbesondere bei Text, der im Bearbeitungssteuerelement bereits durch Laden eines Dokuments vorhanden war, oder bei Text, der vom Bearbeitungssteuerelement eingefügt wird, wie in den vorherigen Abschnitten erläutert. Wenn ein [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175)-Ereignis ausgelöst wird, müssen Sie daher stets den Text bereitstellen, der sich zurzeit im Bearbeitungssteuerelement für den angegebenen Bereich befindet.

Gelegentlich gibt [**Range**](https://msdn.microsoft.com/library/windows/apps/dn958227) in [**CoreTextTextRequest**](https://msdn.microsoft.com/library/windows/apps/dn958221) einen Bereich an, den das Bearbeitungssteuerelement nicht in der Form aufnehmen kann, in der dieser vorliegt. Beispielsweise ist **Range** zum Zeitpunkt des [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175)-Ereignisses größer als das Bearbeitungssteuerelement, oder das Ende von **Range** liegt außerhalb des zulässigen Bereichs. In diesen Fällen sollten Sie zu einem Bereich zurückkehren, der sinnvoll ist. In der Regel ist dies eine Untergruppe des angeforderten Bereichs.

## Verwandte Artikel


**Archivbeispiele**
* [Beispiel für die XAML-Textbearbeitung](http://go.microsoft.com/fwlink/p/?LinkID=251417)
 

 







<!--HONumber=Jun16_HO4-->


